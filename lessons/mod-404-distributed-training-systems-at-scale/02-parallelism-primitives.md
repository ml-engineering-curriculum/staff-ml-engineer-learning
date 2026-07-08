# The Parallelism Primitives

## Motivation

A recipe is a composition of primitives. Before this module can walk the recipe-choice decision tree (chapter 3), it needs to name each primitive precisely, state what it costs and what it buys, and give the learner the vocabulary to read Megatron-LM, DeepSpeed, and FSDP configuration files without translating in their head. This chapter is that reference.

A learner arriving from a Senior tech-lead track has almost certainly used DDP and read about FSDP. Fewer have touched tensor-parallel or pipeline-parallel in production. Almost none have configured expert-parallel. The chapter is deliberately dense on the primitives that are new-at-Staff (TP, PP, SP, EP) and lighter on DDP where the reader is already fluent.

The frame that ties the six primitives together is a single trade-off triangle:

```
                           communication
                              /       \
                             /         \
                            /   RECIPE   \
                           /             \
                          memory ─────── compute
```

Every primitive spends one axis to buy another. DDP spends communication (all-reduce of full gradients per step) to buy memory (no sharding). FSDP-3 spends more communication (all-gather of params on the forward, reduce-scatter of grads on the backward) to buy memory (sharded weights). TP spends *very fast* communication (all-reduce inside a matmul) for larger effective per-device compute. Understanding what each primitive gives up and what it gains is the load-bearing skill of chapter 3's recipe choice.

## The three memory footprints you are managing

Before the primitives, the memory model. A training step's peak accelerator memory has three roughly-additive contributions:

1. **Weights.** N parameters × precision-in-bytes. BF16 weights: 2 bytes per parameter. FP32 master copy (Adam optimizer): +4 bytes per parameter.
2. **Optimizer state.** For Adam-family: 2 momentum buffers × 4 bytes = 8 bytes per parameter. For SGD with momentum: 4 bytes. For sharded (ZeRO-1) or offloaded state, this bucket shrinks or moves off-device.
3. **Activations.** Per-layer, per-microbatch, roughly proportional to `batch × seq_len × hidden × num_layers`. Recomputation trades activation memory for extra forward compute; sequence-parallel further reduces the activation footprint.

For a rough sizing sanity check: a 70B model in BF16 with FP32 optimizer state and Adam consumes roughly **70B × (2 + 4 + 8) = ~1 TB** just for weights and optimizer state, before activations. An H100 has 80 GB HBM3 (H200 has 141 GB HBM3e). The 1 TB against 80 GB tells you why FSDP-3 or 3D-parallel is not optional at 70B — the model does not fit on any single accelerator with unsharded state.

Every primitive below is, at some level, a strategy for making that arithmetic tractable.

## Primitive 1 — Data-parallel (DDP)

**What it does.** Replicate the model on every accelerator. Split the global batch across ranks. On the backward, all-reduce the gradients so every rank has the same averaged gradient. Optimiser step runs identically on every rank.

**Reference implementation.** `torch.nn.parallel.DistributedDataParallel` (PyTorch, canonical). Alternative: Horovod (Sergeev and Del Balso 2018, *"Horovod: fast and easy distributed deep learning in TensorFlow"*).

**Communication cost.** One `all_reduce` of size 2·N bytes per step (in BF16 gradient units) if you use the `bucket_cap_mb` overlap, roughly amortised across the backward pass. AllReduce over K ranks completes in ~2·(K-1)/K of the tensor size (ring-AllReduce, Baidu 2017), so the wall-clock is roughly `2·N / (K · bandwidth_per_link)`.

**Memory cost.** Full model + full optimizer state on every rank. This is the primitive's ceiling: DDP requires the entire model *and* Adam state to fit on one accelerator.

**When it is the right primitive.** Model + Adam state + activations fit on one accelerator with the target batch size. Concretely: anything up to a few-B parameters on a modern GPU with modest activation memory, plus every ranking model, plus most vision models, plus most fine-tuned encoder models. If the model fits, DDP has the cleanest MFU story because there is no weight or activation sharding overhead.

**When it is the wrong primitive.** Any modern LLM at N ≥ ~10B, where the raw weights + optimizer state already blow past a single accelerator's memory. DDP with `offload_optimizer_state` is not DDP any more — it is ZeRO-1 by another name.

**The subtle failure mode.** DDP quietly performs the reduce during the backward pass. If the backward is fast (small model, big cluster), the AllReduce becomes the wall-clock even though the arithmetic looks compute-bound. Chapter 4 walks the diagnostic.

## Primitive 2 — Fully-Sharded Data-Parallel (FSDP) / ZeRO

**What it does.** Shard the weights, gradients, and optimizer state across data-parallel ranks. On the forward pass, gather the shard for each layer *just before* using it, then release. On the backward, gather again for the backward-pass compute, then reduce-scatter the gradients (each rank keeps only its shard). The optimizer step touches only the local shard.

**Reference implementations.** DeepSpeed ZeRO (Rajbhandari et al. 2020, *"ZeRO: Memory Optimizations Toward Training Trillion Parameter Models"*). PyTorch FSDP (Zhao et al. 2023, *"PyTorch FSDP: Experiences on Scaling Fully Sharded Data Parallel"*). PyTorch 2.x introduced FSDP2, which fixes several ergonomics and performance issues of the original.

**The three ZeRO stages.**

- **ZeRO-1.** Shard optimizer state only. Full weights and full gradients live on every rank. Memory saving: ~50% for Adam (the 8 bytes/param optimizer state was the biggest bucket).
- **ZeRO-2.** Shard optimizer state + gradients. Weights still replicated. Memory saving: an additional ~25% for Adam-with-FP32-master.
- **ZeRO-3 / FSDP.** Shard optimizer state + gradients + weights. Memory scales as `total / DP_degree` (plus a small constant for the currently-in-use layer's gathered weights).

Practically, when a program says "FSDP" they almost always mean ZeRO-3 semantics, and when they say "ZeRO-2" they mean the DeepSpeed configuration that stops short of weight sharding.

**Communication cost.** Higher than DDP by roughly 1.5×. ZeRO-3 does an all-gather in the forward, an all-gather in the backward, and a reduce-scatter of gradients — roughly 3× the DDP data volume. FSDP overlap is what gets this hidden behind compute; when the overlap fails, MFU tanks.

**Memory cost.** Model memory divided by data-parallel degree. This is the primitive's whole point.

**When it is the right primitive.** The most common Staff-scope call in 2024-2025 at model sizes where DDP is out but 3D-parallel is overkill. 7B — 70B models on 32 — 512 accelerators. FSDP with careful `min_num_params` sharding units, prefetching, and mixed-precision plumbing regularly hits 40-50% MFU in published programs (LLaMA-2 reported ~40% on A100; LLaMA-3 and Mixtral programs reported similar bands on H100).

**When it is the wrong primitive.** Once the DP degree becomes so large that per-shard weights are tiny and communication dominates, FSDP degrades. Beyond ~1024 accelerators, most published programs move to 3D-parallel (FSDP + TP + PP) rather than pure FSDP.

**The subtle failure mode.** FSDP's overlap depends on the *unit* of sharding matching the forward-and-backward computation graph. Sharding at too fine a granularity (per-parameter) collapses the overlap and communication dominates; sharding at too coarse a granularity (per-block-of-layers) makes the peak activation memory unmanageable. Modern practice is to shard at the transformer-block granularity — PyTorch FSDP's `auto_wrap_policy` and DeepSpeed's `stage3_module_granularity_threshold` — and the recipe should state this.

## Primitive 3 — Tensor-parallel (TP)

**What it does.** Partition individual matrix multiplications across accelerators. For a linear layer `Y = X · W`, split `W` column-wise across TP ranks: rank *i* holds `W[:, i·M/TP : (i+1)·M/TP]` and produces `Y[:, i·M/TP : (i+1)·M/TP]` from the full `X`. Concatenation happens at the boundary of the layer via an `all_gather` or `all_reduce`, depending on the split direction.

For transformer attention and MLP blocks, the Megatron-LM pattern is: split QKV projections column-wise, split the attention output projection row-wise, and the two split directions combine to give a **single** all-reduce per attention block and a **single** all-reduce per MLP block — two per transformer layer.

**Reference implementation.** Megatron-LM (Shoeybi et al. 2019, *"Megatron-LM: Training Multi-Billion Parameter Language Models Using Model Parallelism"*). The paper is the canonical read; the split pattern above is section 3 verbatim. Modern alternatives: NVIDIA NeMo Megatron, DeepSpeed's tensor-parallel wrappers, PyTorch's native `torch.distributed.tensor.parallel`, and Alibaba's `torch.distributed.tensor.parallel.parallelize_module`.

**Communication cost.** Two `all_reduce`s per transformer layer per forward *and* two per backward, each of size `batch × seq × hidden` in BF16. The communication is *inside* the layer, meaning it cannot be overlapped with the layer's own compute — it is on the critical path. This is why TP demands very fast interconnect.

**Memory cost.** Weights per rank scale as `1/TP`. Activations for the tensor-split dimension also scale as `1/TP`. LayerNorm and dropout activations are *not* split (unless sequence-parallel is added; see below), which is a specific and load-bearing detail.

**When it is the right primitive.** When one transformer layer's weights are large enough that FSDP's per-block gather becomes painful, and when there is a fast intra-node interconnect to hide the all-reduce latency. Practically: 30B+ dense models on H100/H200 nodes with NVLink, at TP degrees that fit inside a single NVLink domain (typically ≤8).

**When it is the wrong primitive.** Across nodes over InfiniBand. The two all-reduces per layer at BF16 sizes typical of a 70B model are hundreds of KB to MB each, and IB latency (~2-5 µs per RTT plus bandwidth-proportional cost) is far higher than NVLink's ~600 ns. TP-across-IB routinely kills MFU by a factor of 2-3. Every serious 3D-parallel program keeps TP within one NVLink domain.

**The load-bearing detail.** The Megatron pattern reduces communication *within a single block*, not the total data volume. Increasing TP degree does *not* reduce the size of the collectives — the tensors stay the same size, they are just cut across more ranks. This is why TP has a natural cap around the NVLink domain size.

## Primitive 4 — Pipeline-parallel (PP)

**What it does.** Partition the model's *layers* across pipeline stages, each stage on its own group of accelerators. A microbatch flows forward stage 0 → stage 1 → ... → stage P-1, then backward P-1 → 0. Multiple microbatches are in flight simultaneously to keep the pipeline full.

**Reference implementations.**

- **GPipe** (Huang et al. 2019, *"GPipe: Efficient Training of Giant Neural Networks using Pipeline Parallelism"*). Simple all-forward-then-all-backward schedule. Bubble = (P-1)/M microbatches wasted, where M is the number of microbatches per pipeline pass.
- **PipeDream / 1F1B** (Narayanan et al. 2019, *"PipeDream: Generalized Pipeline Parallelism for DNN Training"*). Interleave one forward and one backward per stage after the pipeline fills. Same bubble as GPipe but lower peak activation memory.
- **Interleaved 1F1B** (Narayanan et al. 2021, *"Efficient Large-Scale Language Model Training on GPU Clusters Using Megatron-LM"*). Each stage holds multiple non-contiguous layer chunks, reducing the bubble to (P-1)/(M · V), where V is the number of chunks per stage. This is what most modern 3D-parallel programs use.
- **Zero Bubble Pipeline Parallelism** (Qi et al. 2023) and **DualPipe** (DeepSeek-V3 report 2024) further reduce or eliminate the bubble by splitting the backward pass into weight-grad and input-grad components and reordering.

**Communication cost.** Only the *boundary activation* between adjacent stages travels across accelerators — usually a single `batch × seq × hidden` BF16 tensor per microbatch per stage boundary. This is dramatically less than TP's all-reduce cost. PP is the primitive that scales well across InfiniBand.

**Memory cost.** Weights per stage scale as `1/PP` (each stage holds only its layer subset). Activation memory per stage scales linearly with the number of in-flight microbatches (roughly `M/P` for GPipe, less for 1F1B).

**When it is the right primitive.** When TP has been maxed out to the NVLink domain and the model is still too large to fit per-node. Adding PP across nodes over InfiniBand works well because the boundary-activation communication is small.

**When it is the wrong primitive.** When P is small (bubble dominates), or when the pipeline is unbalanced (some stages have longer per-microbatch time — usually the embedding and unembedding stages). Balanced-stage partitioning is a non-trivial recipe consideration and one of the specialist's implementation tasks the Staff engineer plans around.

**The load-bearing detail.** The pipeline bubble is real compute waste. A recipe with GPipe, 8 pipeline stages, and 16 microbatches per pass wastes `(8-1)/16 = 44%` of the pipeline stages' compute in the bubble. Interleaved 1F1B with V=4 reduces the same bubble to `(8-1)/(16·4) = 11%`. This is why V — the number of interleaved chunks per stage — is a first-class recipe parameter, not an implementation detail.

## Primitive 5 — Sequence-parallel (SP)

**What it does.** In a Megatron-TP setup, the LayerNorm and dropout operations are *not* tensor-parallelised because they operate along the hidden dimension. Their activations remain `batch × seq × hidden` and are replicated across all TP ranks — a significant fraction of the total activation memory. Sequence-parallel splits *these specific activations* along the sequence dimension across the same TP ranks, so each rank now holds `batch × (seq/TP) × hidden` for the LayerNorm-and-dropout sections.

**Reference implementation.** Korthikanti et al. 2022, *"Reducing Activation Recomputation in Large Transformer Models"* (NVIDIA / Megatron-LM). The primitive is often called "Megatron sequence-parallel" or just "SP" in recipe docs.

**Communication cost.** SP adds `all_gather` at the LayerNorm-to-attention transition and `reduce_scatter` at the attention-to-LayerNorm transition, but it *removes* one all-reduce per section. Net communication is roughly the same as TP-only; the win is activation memory.

**Memory cost.** LayerNorm and dropout activation memory drops by a factor of TP. In an activation-memory-bound recipe, this can be the difference between fitting the target microbatch and having to recompute activations.

**When it is the right primitive.** Whenever TP is present. SP is essentially a free win once TP is set up: same communication, less activation memory, less recomputation. Modern Megatron-LM recipes turn SP on by default with TP.

**When it is the wrong primitive.** DDP-only or FSDP-only recipes without TP — SP has no meaning outside a TP setup.

**The subtle detail.** SP + selective activation recomputation (also from the same Korthikanti et al. paper) together reduce activation memory enough that many programs can turn *off* the aggressive "recompute-everything" activation-checkpointing that was standard practice pre-2022. Chapter 4 walks the trade.

## Primitive 6 — Expert-parallel (EP), for Mixture-of-Experts

**What it does.** In a Mixture-of-Experts (MoE) layer, the router assigns each token to K out of E experts (typically K=1 or K=2). Rather than materialising all E expert weights on every rank, expert-parallel places different experts on different accelerators. Token-to-expert routing then becomes an `all_to_all` collective: each rank sends the tokens routed to remote experts and receives the tokens routed to its local expert.

**Reference implementations.**

- **GShard** (Lepikhin et al. 2020, *"GShard: Scaling Giant Models with Conditional Computation and Automatic Sharding"*). Google's original at-scale MoE recipe.
- **Switch Transformer** (Fedus et al. 2021, *"Switch Transformers: Scaling to Trillion Parameter Models with Simple and Efficient Sparsity"*). The K=1 (single-expert) simplification.
- **Megablocks** (Gale et al. 2023, *"MegaBlocks: Efficient Sparse Training with Mixture-of-Experts"*). Block-sparse-kernel-based MoE that eliminates the padding overhead of prior implementations.
- Recent programs (Mixtral, DeepSeek-V2 / V3, Qwen2-MoE) provide public recipe descriptions and are the current canonical references for what MoE looks like in production.

**Communication cost.** Two `all_to_all` collectives per MoE layer (one to dispatch, one to gather). AllToAll is expensive relative to AllReduce at the same data volume, and its cost grows roughly linearly with the EP degree.

**Memory cost.** Expert weights per rank scale as `1/EP`. The router and shared parameters (attention, embeddings, LayerNorm) are typically replicated or FSDP-sharded on top of EP.

**When it is the right primitive.** MoE architectures where the total parameter count is large but active parameters per token are small (Mixtral 8x7B → 47B total, ~13B active; DeepSeek-V3 → 671B total, 37B active). EP is the mechanism that makes the total parameter count tractable.

**When it is the wrong primitive.** Dense models. There is nothing to expert-parallel.

**The load-bearing detail.** MoE load balance is a first-class recipe concern. The router's routing distribution across experts is trained; if it collapses (all tokens go to a few experts), the AllToAll becomes a straggler and MFU tanks. Auxiliary load-balancing loss terms, expert-choice routing (Zhou et al. 2022), and expert-capacity limits are all mitigations the recipe should specify. Chapter 5 handles this as a failure mode.

## The trade-off triangle, made explicit

Each primitive spends one axis of the triangle to buy another. The recipe designer's job is to pick a combination whose total spend lands inside the cluster's actual constraints.

| Primitive       | Communication cost     | Memory saving           | Compute overhead                     |
| --------------- | ---------------------- | ----------------------- | ------------------------------------ |
| DDP             | Low (1× AllReduce/step)| None                    | None                                 |
| FSDP / ZeRO-3   | ~3× DDP                | Full weights + state / DP | Small (overlap dependent)          |
| Tensor-parallel | Very high, on critical path | Weights + activations / TP | Modest (extra collectives)      |
| Pipeline-parallel | Low (boundary-activation only) | Weights / PP     | Pipeline bubble ((P-1)/M or /(M·V)) |
| Sequence-parallel | Same as TP (replaces one AR) | LayerNorm/dropout activations / TP | None       |
| Expert-parallel | Very high (AllToAll)   | Expert weights / EP     | Router + load-balance overhead       |

Two shape rules the table hides that chapter 3's recipe choice will lean on:

**Rule 1.** *TP within one NVLink domain; PP across nodes.* This is why the 3D-parallel recipe is TP × PP × DP (in that nesting): TP wants NVLink, PP tolerates IB, DP replicates across the remaining axes.

**Rule 2.** *SP is free once TP is present.* Almost every real 2024-era recipe pairs them.

## Reading a recipe config

Every recipe end-state can be written as a tuple:

```
(TP_degree, PP_degree, DP_degree, SP_enabled, EP_degree,
 precision, recompute_policy, batch, seq)
```

Where `TP_degree · PP_degree · DP_degree · EP_degree = total_accelerators` (with EP_degree = 1 for dense models). For example:

- LLaMA-2 7B fine-tune on 32 H100s: `(TP=1, PP=1, DP=32, SP=off, EP=1, BF16, no-recompute, batch=256, seq=4096)`. Pure DDP or pure FSDP.
- Hypothetical 70B pretrain on 512 H100s (64 nodes × 8 GPUs): `(TP=8, PP=4, DP=16, SP=on, EP=1, BF16, selective-recompute, batch=1024, seq=8192)`. 3D-parallel with SP.
- A 400B-total MoE on 1024 accelerators: `(TP=4, PP=8, DP=16, SP=on, EP=8, BF16, selective-recompute, batch=2048, seq=8192)`. 3D + EP.

The tuple is the artifact chapter 3 produces. This chapter has given you the vocabulary to fill it in defensibly.

## Summary

- Six primitives, each a trade of one axis of the communication-memory-compute triangle for another: **DDP, FSDP/ZeRO, TP, PP, SP, EP**.
- **DDP** is right when the model fits with all optimizer state on one accelerator; wrong at modern-LLM scale.
- **FSDP / ZeRO-3** is the mainstream Staff-scope choice at 7B — 70B on 32 — 512 accelerators.
- **TP** is right when a single layer's weights are large enough that FSDP's gather is painful *and* there is a fast intra-node interconnect. Cap TP at the NVLink domain size.
- **PP** scales across InfiniBand because its only cross-node traffic is the boundary activation. Modern recipes use interleaved 1F1B with V ≥ 4 chunks to shrink the pipeline bubble.
- **SP** is a nearly-free memory saving when TP is present. Modern recipes turn it on by default.
- **EP** is the primitive that makes MoE tractable; its `all_to_all` cost and load-balance sensitivity are the failure modes to plan around.
- The recipe is a tuple `(TP, PP, DP, SP, EP, precision, recompute, batch, seq)`. Filling it in is chapter 3.

The next chapter uses the six primitives above to walk the decision tree for picking a specific recipe for a specific (model, cluster, dataset) point.
