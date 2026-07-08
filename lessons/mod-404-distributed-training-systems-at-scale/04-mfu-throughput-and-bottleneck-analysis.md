# MFU, Throughput, and Bottleneck Analysis

## Motivation

Chapter 3 picks a recipe. This chapter converts the recipe into a wall-clock forecast the program can commit to. The load-bearing artifact is the **planned MFU with an uncertainty band** and a stated *bottleneck* — the physical resource the recipe expects to sit against. Without the bottleneck, the MFU planning number is a wish; with it, a measured deviation from plan (during the ablation phase, per chapter 3's re-plan trigger) becomes a diagnostic that tells the training-pipeline engineer where to tune.

The mistake to avoid: quoting an MFU number without a mechanistic argument for why *that* number. *"We expect 45% MFU because LLaMA-3 hit 45%"* is not a defensible plan. The defensible version is *"We expect 40% MFU ± 5% because our recipe's 3D-parallel bake is TP=8 inside NVLink and PP=4 across NDR IB, and our recipe's dominant cost per step is the PP boundary activation, which at our batch shape and hidden size sits at ~15% of the step wall-clock — so the ceiling is around 85% of peak GEMM, and after selective recomputation overhead and Adam-step arithmetic we settle in the 40% MFU band."* The second version tells you what to measure and what to do if the measurement disagrees.

## The roofline model, one page

The roofline model (Williams et al. 2009, *"Roofline: An Insightful Visual Performance Model for Multicore Architectures"*) frames every computation as a point in an *arithmetic-intensity vs. attainable-FLOPs* plane. For a given operation with arithmetic intensity `I` (FLOPs per byte moved from memory), the attainable FLOPs are:

> **Attainable = min(peak_FLOPs, I · peak_bandwidth)**

Two regimes:

- **I ≥ peak_FLOPs / peak_bandwidth (the "ridge point")** — the operation is *compute-bound*. Attainable FLOPs = peak_FLOPs. The ceiling is the hardware's arithmetic throughput.
- **I < ridge point** — the operation is *memory-bound*. Attainable FLOPs = I · peak_bandwidth. The ceiling is the hardware's memory bandwidth.

For an H100 (BF16): peak_FLOPs ≈ 989 TFLOPs/s, HBM3 bandwidth ≈ 3.35 TB/s. Ridge point ≈ 989 · 10¹² / 3.35 · 10¹² ≈ **295 FLOPs per byte**. A modern GEMM at reasonable sizes (batch × 4K × 4K matmul) has arithmetic intensity in the low thousands and is comfortably compute-bound. A small attention operation on short sequences can drop below the ridge point.

The roofline in a training recipe is not a single point — it is a *ceiling* the whole step must fit under. The step's overall MFU is roughly the arithmetic-mean-weighted-by-time of each operation's utilisation of its own ceiling.

## MFU and HFU, defined

Two related but distinct utilisation metrics. Both are used in published pretraining recipes and both belong in a recipe doc, quoted precisely.

**Model FLOPs Utilisation (MFU).** Fraction of accelerator peak FLOPs converted into *useful model FLOPs*. Useful model FLOPs are counted using the 6ND estimator from mod-403 chapter 2: per step, `6 · N_active_params · batch × seq` BF16 FLOPs (for a dense model on next-token prediction).

> **MFU = (useful model FLOPs per step) / (peak FLOPs per accelerator per second × step wall-clock × num_accelerators)**

**Hardware FLOPs Utilisation (HFU).** Fraction of peak converted into *any* FLOPs — including the extra forward passes done for activation recomputation, gradient-accumulation arithmetic, optimizer-step FLOPs, and layer-fusion overhead.

> **HFU = (all FLOPs actually issued to the tensor cores per step) / (peak FLOPs per accelerator per second × step wall-clock × num_accelerators)**

HFU is typically 1.1×-1.4× MFU. The ratio depends on the recompute policy: aggressive recomputation nearly doubles the forward compute and pushes HFU/MFU toward 1.5×; selective recomputation (Korthikanti et al. 2022) keeps HFU/MFU near 1.15×.

Chapter 3's recipe should quote MFU as the *planning* number (because mod-403's 6ND compute budget is defined in the same units) and HFU as a *diagnostic* number (because the training-pipeline engineer's instrumentation measures HFU natively — GPU counters see all FLOPs, not just useful ones).

## MFU bands by recipe

Published pretraining programs cluster MFU in narrow bands per recipe class. Chapter 3 quoted these once; chapter 4 restates them with the mechanistic reason so you can pick a *plan* rather than a copy.

- **DDP on modest models.** 20-30% MFU. Ceiling: usually AllReduce overlap. A modest DDP model on a small cluster is often compute-bound at the GEMM level but network-limited at the step level.
- **FSDP / ZeRO-3, well-tuned.** 35-45% MFU. Ceiling: FSDP's all-gather + reduce-scatter overlap. Modern FSDP2 with prefetching, transformer-block sharding, and BF16-native optimizer regularly hits the top of this band. LLaMA-2 reported ~40% MFU on A100 at 70B (Touvron et al. 2023).
- **3D-parallel (TP × PP × DP), well-tuned.** 45-55% MFU on H100-class hardware. PaLM reported 46% MFU on TPUv4 (Chowdhery et al. 2022, *"PaLM: Scaling Language Modeling with Pathways"*). MegaScale reported ~55% MFU on 12k GPUs (Jiang et al. 2024, ByteDance, *"MegaScale: Scaling Large Language Model Training to More Than 10,000 GPUs"*). Ceiling: TP AllReduce inside NVLink + PP bubble + DP all-gather overlap.
- **3D-parallel with FP8 (Transformer Engine).** ~60-65% MFU on H100 has been reported by well-tuned frontier programs; anything above that is publication material and should not be assumed by a program that has not measured it.

For your program's plan, pick the band that matches your recipe and *state the number with a ±5-10% band*. A point estimate without a band is what gets a recipe doc pushed back in review.

Then state *why* your program lands where it does inside the band. First-time-on-a-hardware-generation programs, novel architectures, or unusually small clusters land at the low end; mature-recipe fine-tunes with a well-instrumented training-pipeline engineer land at the high end.

## The step-time decomposition

The mechanical way to plan MFU is to decompose the step into named contributions and account for each.

Step wall-clock = **(a) forward GEMMs + (b) attention + (c) forward exposed-communication + (d) backward GEMMs + (e) backward exposed-communication + (f) optimizer step + (g) idle / pipeline bubble**.

For a typical recipe:

- **(a) and (d) forward and backward GEMMs.** The 6ND FLOPs. Runs at 60-80% of peak GEMM throughput at typical LLM sizes (short of peak because kernels have prologue/epilogue overhead and short-N matmuls happen at the boundaries).
- **(b) attention.** FLOPs scale as `2 · batch × num_heads × seq² × head_dim` for standard attention; FlashAttention-2/3 (Dao 2023, 2024) brings this to nearly memory-bandwidth-bound. Fraction of step: ~5-15% depending on seq.
- **(c) and (e) exposed communication.** The AllReduce/AllGather/AllToAll that could not be overlapped with compute. Well-tuned programs push this below 10% of step; poor overlap can push it above 40%.
- **(f) optimizer step.** Adam-family: element-wise memory-bound. Fraction of step: ~2-5%.
- **(g) idle / pipeline bubble.** For 3D-parallel with interleaved 1F1B, ~2-10%.

Sum of (a) + (b) + (d) as fraction of step wall-clock → this is roughly HFU. Sum of (a) + (b) + (d) counting only *useful* GEMMs (no recomputation) → roughly MFU. The wedge between them is (c) + (e) + (f) + (g) + recomputation-forward-passes.

A recipe doc that presents this decomposition as its MFU plan is legible; a recipe doc that just quotes "45%" is not.

## Identifying the bottleneck — four candidates

A recipe expects to sit against one of four bottlenecks. Naming the expected bottleneck in the plan turns "measured MFU disagreement" from a mystery into a diagnostic direction.

### Compute-bound

**Signature.** MFU is at or near the recipe band's high end. HFU is only slightly above MFU. Nsight profiles show tensor cores at high utilisation; kernel launches are dense; there are no long gaps in the timeline.

**When this is the ceiling.** DDP or well-tuned FSDP at modest cluster sizes, or 3D-parallel where every collective has been overlapped and the pipeline bubble is small.

**What to do if measurement lands here.** Nothing — this is the good outcome. If MFU is *still below plan* while compute-bound, the ceiling itself is the problem: precision (upgrade to FP8), kernel selection (FlashAttention version), or numerical shape (batch × hidden causing suboptimal GEMM tiling).

### HBM-bandwidth-bound (memory-bound)

**Signature.** MFU is well below the recipe band. HFU is close to MFU (not much wasted compute). Nsight shows tensor cores idle a significant fraction of the time; the timeline shows long HBM-to-SM transfers.

**When this is the ceiling.** Attention with short sequences (below FlashAttention's arithmetic-intensity ridge). Optimizer step. LayerNorm and dropout. Small residual operations.

**What to do.** FlashAttention-3 on H100 (higher arithmetic intensity via better warpgroup-async scheduling). Fuse LayerNorm and dropout with adjacent GEMMs. Reduce optimizer state precision (BF16 Adam variants — sometimes at a small quality cost).

### Interconnect-bandwidth-bound (network-bound)

**Signature.** MFU is well below the recipe band. HFU is close to MFU. Nsight timeline shows tensor cores idle while NCCL kernels dominate the wall-clock. Communication kernels do not overlap with compute.

**When this is the ceiling.** TP AllReduce not fitting inside NVLink (TP crossed IB by mistake). FSDP all-gather too large for the DP degree × per-step time. PP boundary activation at odd shapes (batch × seq × hidden not aligning with NIC MTU).

**What to do.** Verify TP degree ≤ NVLink domain. Verify FSDP `forward_prefetch` and `backward_prefetch` are on and configured for the model shape. Reduce TP degree if it was over-provisioned. Increase batch to amortize collective cost against more compute.

### IO-bound (dataloader-bound)

**Signature.** MFU low, HFU low, tensor cores idle at the *start* of each step waiting for the batch. Nsight shows PCIe or NVMe activity at the step boundary; the first kernel of each step is preceded by a stall.

**When this is the ceiling.** Programs with expensive tokenisation-per-step. Programs streaming from cold object-store shards without prefetch. Programs with small dataloader worker counts.

**What to do.** Pre-tokenise the dataset (spend that compute up-front, once, not per step). Increase dataloader worker count. Use `packed_sequence` if the recipe supports it. Move the dataset to a hot local NVMe cache. Prefetch two or three batches deep.

## The instrumentation plan

A recipe doc does not implement instrumentation — that is the training-pipeline engineer's job — but the recipe does *plan* what instrumentation the engineer will need to validate the MFU number and diagnose deviation. Chapter 7's hand-off contract makes this concrete; this section names the instrumentation categories.

- **Step-time telemetry.** Total step wall-clock, forward, backward, optimizer, per-kernel time percentiles. Every training run should log these to a time-series backend. LLaMA-2 and BLOOM training logs are examples of what this looks like in publication form.
- **MFU / HFU dashboards.** Rolling MFU and HFU per training step. Alert on deviation from the recipe band.
- **NCCL / communication telemetry.** Per-collective wall-clock and per-rank latency distributions. NVIDIA `NCCL_DEBUG=INFO` and DCGM metrics are the standard sources.
- **HBM utilisation and bandwidth.** DCGM's `NVML_DEV_MEM_UTIL` and per-kernel HBM traffic from Nsight Systems / Nsight Compute.
- **Dataloader latency percentiles.** Per-worker fetch latency, queue depth, batch-ready-before-forward margin.

The recipe should state the *targets* for the top-level instrumentation and defer the specific dashboard build to the training-pipeline engineer. A target of "MFU sustained > 40% at rolling 30-minute window; alert on deviation below 30%" is a Staff-scope commitment. Building the dashboard that measures it is not.

## From MFU plan to wall-clock forecast

The wall-clock forecast is the number leadership actually reads. Given the recipe's planned MFU:

> **wall-clock hours (main run) = C / (peak_FLOPs_per_accelerator × MFU × 3600 × num_accelerators)**

For the mod-403 chapter 3 worked example — 15B model, D=1.1T tokens, C = 6 · 15B · 1.1T ≈ 10²³ FLOPs, 512 H100s at 40% MFU:

- Useful FLOPs per accelerator per second: 0.4 · 989 · 10¹² ≈ 3.96 · 10¹⁴ FLOPs/s.
- Main-run accelerator-hours: 10²³ / (3.96 · 10¹⁴ · 3600) ≈ 70,400 H100-hours.
- Wall-clock on 512 accelerators: 70,400 / 512 ≈ 137 hours ≈ 5.7 days.

If the MFU plan is off by 20% (35% instead of 40%), the wall-clock lengthens to ~6.5 days. If the plan is off by half (20% MFU), wall-clock roughly doubles. This is why the *band*, not the point, is what the plan commits.

## The MFU-plus-bottleneck-analysis doc

Section shape of the MFU and bottleneck analysis (exercise-02):

1. **The recipe restated.** One-line pointer to the recipe-decision doc.
2. **The planned MFU with band.** Numeric, with the mechanistic reason.
3. **The step-time decomposition.** Table of forward-GEMM / attention / exposed-comm / backward-GEMM / optimizer / bubble percentages, each with a one-line source (recipe primitive, hardware peak, published-recipe reference).
4. **The expected bottleneck.** One of compute-bound, HBM-bound, network-bound, IO-bound — chosen mechanistically, with signature diagnostics stated.
5. **The instrumentation plan.** Named telemetry categories and MFU / step-time / NCCL / HBM / dataloader targets.
6. **The wall-clock forecast.** Main-run days at the planned MFU with 1σ band.
7. **The re-plan trigger.** Numeric — at what measured MFU deviation the recipe re-opens.

Two-to-three pages plus one roofline sketch. Anything longer and reviewers will skim.

## The one thing junior program-owners forget

The roofline ceiling changes when the hardware changes but *the FLOP-per-parameter-per-token constant does not*. That is why the mod-403 6ND estimator and the chapter-4 MFU planning fit together into one artifact: the useful-FLOPs number is a property of the model and dataset, not the cluster. The recipe author who quotes MFU without pinning it to the 6ND-based useful-FLOPs count leaves a hole a reviewer will always find.

## Summary

- **MFU and HFU are the load-bearing utilisation numbers.** MFU counts useful (6ND) FLOPs; HFU counts all FLOPs. Recipe docs quote MFU with a mechanistic band and use HFU as diagnostic.
- Published MFU bands: DDP 20-30%, FSDP 35-45%, 3D-parallel 45-55% (H100 BF16), 3D-parallel FP8 60-65% (best-in-class). Cite the reference and defend where in the band you land.
- The **step-time decomposition** — GEMM / attention / exposed-comm / optimizer / bubble — turns "MFU = 40%" into a legible plan.
- Four candidate bottlenecks — **compute-bound, HBM-bandwidth-bound, interconnect-bound, IO-bound** — each has a signature diagnostic and a next-move.
- The recipe commits to an **instrumentation plan** but the training-pipeline engineer builds it. Chapter 7 makes the hand-off contract explicit.
- The wall-clock forecast is the top-line number leadership reads: `C / (peak · MFU · 3600 · K)`.
- The re-plan trigger is numeric and *in the doc*; without it, measurement disagreement re-opens the whole conversation.

The next chapter enumerates the failure modes the program will spend cluster-hours mitigating during the main run and sets the detection-and-response cadence for each.
