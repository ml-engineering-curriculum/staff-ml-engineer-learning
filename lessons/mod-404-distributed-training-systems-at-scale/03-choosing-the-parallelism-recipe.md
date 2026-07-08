# Choosing the Parallelism Recipe

## Motivation

Chapter 2 gave you the primitives; chapter 3 picks the combination. The recipe is the top-of-page artifact this module produces: given a (model shape, cluster, dataset) triple, name the specific `(TP, PP, DP, SP, EP)` mesh, the precision, the recompute policy, and the batch and sequence shape — and defend the choice against the two or three alternate recipes a review will raise.

The core mistake is treating recipe design as a menu selection: *"we picked FSDP because everybody's using FSDP."* A recipe defended that way falls apart in review because the reviewer's next question is *"what would you have picked at 2× the cluster size, or 2× the model size, or half the interconnect bandwidth?"* — and the "everybody's using it" defense has nothing to offer. The chapter's technique is a decision tree that answers those questions structurally.

## The five inputs to the recipe

The decision tree needs five inputs. Read them off the program brief and the cluster spec before you start.

1. **Model shape.** N (parameters), architecture family (dense vs. MoE), per-layer size (hidden × attention heads × MLP intermediate), depth (num_layers), sequence length, tokenizer vocabulary size.
2. **Cluster shape.** Accelerator count, accelerator model (H100 / H200 / A100 / TPU vX / B200 / MI300X), per-accelerator HBM capacity, intra-node interconnect (NVLink generation, NVSwitch topology, TPU ICI), inter-node interconnect (InfiniBand HDR / NDR / XDR, or Ethernet RoCE, or TPU Pod).
3. **Batch shape.** Target global batch size (in tokens), target microbatch size (per-rank per forward), and any hard constraint (e.g., "we want at least X tokens/second wall-clock throughput for the ablation cadence to close by date Y").
4. **Precision plan.** BF16 activations + FP32 master (standard). FP8 with Transformer Engine (~2× throughput uplift, small quality cost). Fully-FP32 (rare, small-scale programs only).
5. **Recompute plan.** No recomputation (fastest, most memory). Selective recomputation (SP + Korthikanti-style). Full activation checkpointing (slowest, most memory-frugal).

The decision tree turns the five inputs into a `(TP, PP, DP, SP, EP)` mesh. The rest of this chapter is the tree.

## The decision tree

### Step 1 — Does the model + Adam state + activation fit on one accelerator at the target microbatch?

Compute the peak memory:

- **Weights.** 2 · N bytes (BF16).
- **Adam state.** 12 · N bytes (FP32 master + two FP32 momentums), or 8 · N if you use pure-BF16 Adam.
- **Activations.** Roughly `2 · batch × seq × hidden × num_layers` bytes for BF16 without recomputation. With `selective` recomputation, ~`2 · batch × seq × hidden × num_layers · 0.3` (rough factor from Korthikanti et al. 2022's tabulated per-layer components).
- **Add ~10-20%** for CUDA workspace, gradient buffers before reduce, and framework overhead.

If the sum ≤ per-accelerator HBM (H100: 80 GB; H200: 141 GB), then the model fits on one device.

**Yes → DDP.** Pure `torch.nn.parallel.DistributedDataParallel`. Recipe tuple: `(TP=1, PP=1, DP=K, SP=off, EP=1)` where K is the accelerator count. Move to chapter 4 for MFU planning.

**No → step 2.**

For most Staff-scope Programs at N ≥ ~10B, this step exits "no" — DDP is out.

### Step 2 — Does the model fit with FSDP-3 (weights and Adam state sharded across DP)?

FSDP-3 divides weight and optimizer memory by DP degree. Per-shard memory is roughly `(2 · N + 12 · N) / DP + activations + gathered_current_block`. The gathered-current-block cost is `2 · block_params` (BF16 gather for the layer being computed).

Peak per-rank memory ≈ `(14 · N / DP) + 2 · block_params + activations`. If this fits on the target accelerator with the target microbatch, FSDP-3 alone suffices.

Concrete sanity example: 70B model, DP=64, block_params ~ 2.2B (one transformer block of Llama-2-70B), activations ~ 20 GB at seq=4096. Per-rank memory ≈ `14 · 70 / 64 + 2 · 2.2 + 20` ≈ 15.3 + 4.4 + 20 = ~40 GB. Fits on an H100. FSDP-3 works.

**Yes → FSDP-3 with `auto_wrap` at the transformer-block granularity.** Recipe tuple: `(TP=1, PP=1, DP=K, SP=off, EP=1)`. Enable `full_shard` and `backward_prefetch`. Enable mixed-precision policy (BF16 params, BF16 grads, FP32 optimizer). Move to chapter 4.

**No → step 3.**

At current-generation cluster sizes, this step exits "yes" for the vast majority of 7B-70B pretraining and fine-tune programs. Programs that hit step 3 are usually N ≥ ~150B or programs at N ~ 70B on unusually small clusters (32 accelerators or fewer).

### Step 3 — Does one transformer block's weights + local activations fit on one accelerator?

If FSDP-3's gather-per-block is itself too large — either the block weights exceed HBM, or the peak-during-gather footprint blows through — you have to shard *inside* the block. That is where TP enters.

Compute: `2 · block_params + activation-during-block` vs. HBM. For a Llama-70B block (~2.2B params, ~4.4 GB BF16) plus activations, this is comfortably in HBM. For a hypothetical 400B dense model with block_params ~13B (~26 GB BF16), the gather cost alone exceeds 30% of an H100's HBM and the peak-during-forward pushes past the budget.

**Yes (block fits) but pure-FSDP still bottlenecks on communication → add PP.** Recipe: `(TP=1, PP=P, DP=K/P, SP=off, EP=1)` where P is picked to spread layers across P stages, each stage on K/P accelerators running FSDP. This is often called *"FSDP + PP"* or *"HSDP + PP"* depending on the library, and it works well when the cluster is large enough that pure-FSDP's all-gather across the full DP dimension is the bottleneck.

**No (block does not fit) → add TP.** Recipe: `(TP=T, PP=1, DP=K/T, SP=on, EP=1)` where T is picked to make the block fit. The rule of thumb: T ≤ the NVLink domain size (typically 8 for an NVIDIA HGX H100 8×GPU node, sometimes 16 or 32 in NVSwitch-enabled clusters or the Grace-Hopper 72-GPU domain of GB200 NVL72). Move to step 4 to consider PP on top.

### Step 4 — Does the recipe still not fit, or is DP degree so large that FSDP's all-gather dominates?

If you have TP inside a node and the model still overflows across nodes, or if the DP degree at (K / TP) is large enough that FSDP's gather becomes the wall-clock, add PP.

Recipe: `(TP=T, PP=P, DP=K/(T·P), SP=on, EP=1)`. This is 3D-parallel.

**T · P · DP = K.** T is constrained to the NVLink domain; PP is constrained by the interleaved-1F1B bubble math. DP is what fills out the accelerator count.

Practical guidance from published programs:

- **T = 8** on H100 HGX nodes with NVLink (Megatron-LM default at scale).
- **P = 4 to 16.** Bigger P shards more model but has a bigger bubble. Interleaved 1F1B with V=4 to V=8 chunks is standard practice; the bubble at V=4, P=8, M=32 microbatches is `(P-1)/(M·V) = 7/128 ≈ 5.5%`, which is tolerable.
- **DP** takes what's left. DP smaller than 4-8 loses the FSDP-3 memory saving; DP much larger than 512 stresses the AllReduce.

Sanity example for a hypothetical 175B model on 512 H100s: `T=8, P=8, DP=8`. Each pipeline stage holds 175B / 8 = ~22B params. Each TP rank in a stage holds 22B / 8 = ~2.75B params. With BF16 + FP32-Adam and DP=8 FSDP-3 sharding on top, per-rank optimizer state is `12 · 2.75 / 8 ≈ 4.1 GB`. Comfortably fits.

### Step 5 — Is the model MoE?

If the architecture is Mixture-of-Experts, add EP. The EP dimension is orthogonal to TP and PP; it lives inside the DP dimension conceptually — experts are placed across some subset of the data-parallel-degree accelerators, and the AllToAll routes tokens between them.

Recipe: `(TP=T, PP=P, DP=K/(T·P·EP), SP=on, EP=E)` where E is picked to spread the experts across the fewest possible high-bandwidth-connected accelerators (AllToAll is expensive; keeping EP inside a fast domain matters more than for TP).

Practical guidance:

- **E = number of experts / num_experts_per_rank**, typically 2, 4, or 8 for architectures like Mixtral 8×7B. Larger MoEs (DeepSeek-V3 at 256 experts) use E much higher.
- **EP inside the intra-node domain when possible** for the same reason TP wants NVLink: AllToAll is more expensive than AllReduce at the same volume.
- **Add expert-capacity limit and load-balance loss.** These are recipe-level parameters, not implementation-level. The Staff engineer specifies them.

### Step 6 — Long sequences?

If sequence length is > ~8K tokens and TP is present, verify sequence-parallel is on. If sequence length is > ~32K and even SP-savings do not tame activation memory, consider Ring / Ulysses / DeepSpeed context-parallelism (Jacobs et al. 2023, *"DeepSpeed Ulysses: System Optimizations for Enabling Training of Extreme Long Sequence Transformer Models"*; Liu et al. 2023 Ring Attention). These are recipe-level additions and worth their own line in the recipe tuple as `CP=C`.

For the vast majority of Staff-scope programs at seq ≤ 8K, SP + selective recomputation is enough and context-parallelism does not need to be considered.

## The 3D-parallel mesh, geometry

When the recipe lands at 3D-parallel, the mesh matters. The convention (from Megatron-LM's Narayanan et al. 2021 paper) is to nest the parallelism axes inside the cluster's physical topology:

```
Physical topology
├── Node 0
│   ├── GPU 0..7  ← TP domain (NVLink)
├── Node 1
│   ├── GPU 0..7  ← TP domain
├── Node 2 (next PP stage)
│   ├── GPU 0..7
...
├── Node P·V  (last PP stage)
    ├── GPU 0..7
[DP dimension replicates the above stack]
```

- **TP fits inside a node** (NVLink domain).
- **PP crosses nodes** (InfiniBand). The boundary-activation traffic is small.
- **DP replicates the entire (TP × PP) block** across the remaining cluster. FSDP-3 shards weights and Adam state along DP.

The mesh nesting is not a preference; it is a bandwidth-hierarchy match. Any other nesting (TP across IB, PP inside a node) leaves throughput on the floor.

## The bake-off — worked recipes

Four `(model, cluster)` points and the recipe each lands on. These are illustrative — the specific numbers your program brief presents will differ — but the *shape of reasoning* transfers.

### Point 1 — 7B fine-tune on 32 H100s

Program: continued pretraining of a 7B base for a domain, 32 H100s (four 8×GPU nodes), seq=4096, target BF16, target global batch 256 sequences.

- Step 1: 7B model + Adam. `2·7 + 12·7 + activations ≈ 14 + 84 + 5 = 103 GB`. Does not fit on 80 GB H100. DDP is out.
- Step 2: FSDP-3. Per-rank optimizer + weight state: `(14·7)/32 ≈ 3 GB`. Trivial. Add activations and gathered block; comfortable.
- Recipe: `(TP=1, PP=1, DP=32, SP=off, EP=1)`. Pure FSDP-3, BF16, no recompute (activations fit at batch 8 per rank × 4 grad-accum = global 256).

Defend against alternates. A reviewer might ask *"why not TP=2 for higher throughput?"* Answer: at 7B, block gather is cheap and TP's inside-node communication buys nothing at this scale; TP=2 would reduce activation-memory a little but at the cost of an all-reduce per block that pure-FSDP does not pay. Reviewer asks *"why not TP=8?"* Answer: TP=8 across all four nodes exceeds the NVLink domain if the nodes are not NVSwitched, and inside a node it does not help because FSDP already fits.

### Point 2 — 70B pretraining on 512 H100s (64 nodes × 8 GPUs)

Program: pretraining a new 70B base model, 512 H100s, seq=8192, BF16, target global batch 2048 sequences.

- Step 1: 70B + Adam. `14·70 + activations = 980 + activations` GB. Does not fit.
- Step 2: FSDP-3. Per-rank `(14·70)/512 ≈ 1.9 GB` optimizer/weight state, plus a gathered-block cost of `2·2.2 = 4.4 GB`, plus activations. Fits.
- But: at DP=512, the FSDP all-gather crosses the InfiniBand fabric heavily and the wall-clock hides behind the network. This is where a *mixed* recipe wins.
- Step 3-4: add TP=8 (inside-node NVLink) and PP=4 to split layers across pipeline stages. DP = 512/(8·4) = 16.
- Recipe: `(TP=8, PP=4, DP=16, SP=on, EP=1)` with interleaved 1F1B, V=4 chunks per stage. BF16 with selective recomputation.

Defend against alternates. Reviewer asks *"why not pure FSDP-3 at DP=512?"* Answer: at DP=512, per-rank optimizer state is tiny (< 2 GB), but the all-gather of a 70B model across 512 ranks per training step over IB is measured in hundreds of MB per step-transition and does not overlap efficiently. 3D-parallel keeps the TP AllReduce on NVLink and only crosses IB for the PP boundary activation. Reviewer asks *"why PP=4 not PP=8?"* Answer: PP=8 at V=4, M=32 gives an ~5% bubble (worked in the previous section); PP=4 at V=4 gives ~2.4%. PP=8 shards the model more but adds bubble; the recipe traded bubble for the memory saving that PP=4 already achieved. If P=4 turned out not to be enough during the ablation phase (measured MFU below plan), the re-plan lifts P to 8.

### Point 3 — Mixtral-shape MoE (8 experts, 47B total, ~13B active) on 128 H100s

Program: continued-pretraining of an MoE, 128 H100s (16 nodes × 8 GPUs), seq=4096, BF16, global batch 1024.

- Step 1: dense equivalent 47B+Adam does not fit. Out.
- Step 2: FSDP-3 divides 47B / 128 = per-rank ~0.4 GB weight/optimizer state. Fits comfortably.
- Step 5: MoE. Router + 8 experts. Placing experts across ranks: E=8 with each expert on a subset of ranks, or E=4 with two experts per rank.
- Recipe: `(TP=1, PP=1, DP=16, SP=off, EP=8)`, one expert per node's expert-parallel group, FSDP-3 sharding the shared attention/embedding params across DP=16.

Defend against alternates. Reviewer asks *"why not TP for the attention block?"* Answer: at 13B active params, attention block is small enough that TP's AllReduce buys nothing; the EP AllToAll is the dominant collective and adding TP inside the node also fighting for NVLink bandwidth degrades EP wall-clock. Reviewer asks *"why EP=8 not EP=2?"* Answer: EP=2 puts 4 experts per rank, doubling the per-rank memory for expert weights. EP=8 spreads them thinner and matches the intra-node structure of Mixtral cleanly.

### Point 4 — Frontier-scale program that likely exceeds this module's altitude

Program: 400B+ dense model on 4096+ H100s. Frontier-lab scale.

This is the case where the recipe still uses the primitives above but the specific numbers are dominated by the specific fabric — NVSwitch generation, IB topology, spine-and-leaf hop count — and the recipe needs to be co-designed with the ai-infra-performance track from the start. Chapter 7's hand-off contract, in this case, includes an *inbound* clause: the recipe author is entitled to a bandwidth-and-topology report from ai-infra-performance before finalising the mesh. The mod-410 principal-track skill (technical leadership across the org) is what carries the co-design conversation. Chapter 3's decision tree still applies; the numbers get denser.

## Sanity-checking the recipe against the cluster

Before signing the recipe, run these sanity checks:

- **Does `TP · PP · DP · EP` equal the target accelerator count?** Off-by-one here is a common recipe-doc error.
- **Is TP ≤ the NVLink domain size?** If TP crosses IB, the recipe is going to underperform and the reviewer will notice.
- **Is the pipeline bubble ≤ ~10%?** With interleaved 1F1B and V chosen appropriately.
- **Does the microbatch × PP-stages fit in the pipeline's activation memory budget?** Chapter 4's roofline shows the trade.
- **Does the global batch size land inside the LLM-training-batch-critical range?** McCandlish et al. 2018 (*"An Empirical Model of Large-Batch Training"*) is the classical reference; batch sizes that are too large hit diminishing returns on gradient noise even if throughput scales.

A recipe that fails any of these five checks is a recipe that will need to be re-litigated in review. Catch them at authoring time.

## The re-plan trigger

Same shape as mod-403 chapter 3's re-plan trigger. The recipe should state: *at what measured MFU disagreement with plan (from the ablation phase) does the recipe change?* Concrete triggers:

- **MFU comes in > 20% below plan.** Root-cause with ai-infra-performance. Likely re-plan.
- **A specific primitive fails a measured target** (e.g., pipeline bubble > 15% instead of planned ~5%). Adjust V, M, or P.
- **A cluster-shape change** (fewer nodes than reserved, IB fabric change). Full re-plan.

Writing the re-plan trigger in the recipe doc is what makes the recipe defensible when reality disagrees with plan. Programs that ship the recipe without a re-plan trigger have to re-open the whole conversation from scratch when measurement lands.

## What the recipe doc looks like

Section shape of the recipe decision doc (exercise-01):

1. **The five inputs.** Model shape, cluster shape, batch shape, precision plan, recompute plan. One paragraph each.
2. **The recipe tuple.** `(TP, PP, DP, SP, EP, precision, recompute, batch, seq)` with the mesh geometry.
3. **The decision-tree walk.** How each step of the tree above landed at the chosen value.
4. **The defense against two alternate recipes.** Named alternates (usually "one degree less parallelism" and "one degree more parallelism") and why they are worse.
5. **The mesh-to-topology mapping.** Which node is which pipeline stage, which GPU inside a node is which TP rank, how DP replicates.
6. **The sanity-check table.** Five checks above.
7. **The re-plan trigger.** Numeric.

Two-to-four pages. Longer than that and the reviewer will not read it.

## Summary

- The recipe is a `(TP, PP, DP, SP, EP, precision, recompute, batch, seq)` tuple picked by a five-step decision tree over the (model, cluster, dataset) triple.
- The tree exits early to DDP or pure FSDP-3 for most sub-70B programs on modest clusters; it lands at 3D-parallel for larger models or larger clusters; it adds EP for MoE and SP whenever TP is present.
- The 3D-parallel mesh is nested to match bandwidth hierarchy: **TP inside NVLink, PP across IB, DP replicates the rest**.
- Four worked recipes across scale points anchor the choice: 7B/32 GPUs (pure FSDP), 70B/512 (3D-parallel), MoE/128 (EP-heavy), frontier (co-designed with ai-infra-performance).
- The recipe doc names five inputs, the tuple, the tree walk, the defense against two alternates, the mesh-to-topology mapping, the sanity checks, and a numeric re-plan trigger.

The next chapter walks the MFU planning number that turns this recipe into a wall-clock forecast, and the roofline framework that identifies the bottleneck the recipe will sit against.
