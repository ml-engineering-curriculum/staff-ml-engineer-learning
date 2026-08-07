# exercise-02: MFU and Bottleneck Analysis

**Estimated effort:** 3 hours

## Objective

Turn exercise-01's recipe into a **planned MFU with an uncertainty band** and a **stated bottleneck**, decompose the step wall-clock into named contributions, sketch the roofline, name the instrumentation plan the training-pipeline engineer will build to validate the number, convert MFU to a wall-clock forecast for the main run, and set a numeric re-plan trigger. This is section 2 of the mod-404 five-artifact stack and the input the mod-403 cluster-hour budget reads to reconcile against its own risk multiplier.

The exercise is deliberately about a *mechanistic* MFU plan — not a plausibility guess pulled from a published paper on different hardware. The MFU number has to be derivable from the recipe's step-time decomposition, the hardware roofline, and the published band chapter 4 cites. If you cannot derive it, the plan is not defensible.

## Prerequisites

- Chapter 04 — MFU, throughput, and bottleneck analysis (the roofline, MFU vs. HFU, published bands, step-time decomposition, four candidate bottlenecks, instrumentation, wall-clock forecast).
- Exercise-01 delivered: recipe tuple, mesh, defense against alternates, sanity checks.
- Chapter 03's peer-mod-403 6ND compute estimator, chapter 3 (of mod-403) for the cluster-hour context the wall-clock feeds.

## Steps

1. **Restate the recipe.** One-paragraph pointer to exercise-01. Recipe tuple, mesh-to-topology, target global batch and sequence length.
2. **Compute the hardware roofline.** For the accelerator model in your program brief:
   - Peak BF16 FLOPs per accelerator per second (H100: ~989 TFLOPs; H200: ~989 TFLOPs; B200: ~2250 TFLOPs BF16 — cite the vendor spec).
   - HBM bandwidth per accelerator (H100: 3.35 TB/s HBM3; H200: 4.8 TB/s HBM3e).
   - The roofline ridge point: `peak_FLOPs / bandwidth` — the arithmetic intensity above which operations are compute-bound.
   - A one-plot sketch (hand-drawn is fine): FLOPs vs. arithmetic-intensity with the two ceilings and the ridge point marked.
3. **Pick the MFU planning band.** From chapter 4's published bands: DDP 20-30%, FSDP 35-45%, 3D-parallel 45-55% (BF16 H100), 3D-parallel FP8 60-65% (frontier). Cite the specific reference (LLaMA-2, LLaMA-3, PaLM, MegaScale, etc.) whose measurement matches your recipe class. State whether your program lands at the low end or high end of the band and why (first-time-on-hardware ramp cost vs. mature-recipe fine-tune).
4. **Decompose the step wall-clock.** Table with rows for: forward GEMMs, attention, forward exposed-communication, backward GEMMs, backward exposed-communication, optimizer step, pipeline bubble / idle. For each row, state the estimated percentage of step wall-clock and a one-line source (recipe primitive, hardware peak, published-recipe reference, chapter 4 formula). The percentages should sum to ~100%.
5. **Name the expected bottleneck.** One of: compute-bound, HBM-bandwidth-bound, interconnect-bound, IO-bound. Argue mechanistically why — cite the step-time row from (4) that dominates. State the signature diagnostics (from chapter 4's per-bottleneck signature list) that the training-pipeline engineer will look for to confirm.
6. **Compute the planned MFU as a number with a band.** Pick the point estimate (e.g., 40%) with a ±5-10% band. Show the reasoning that puts you at the point (e.g., "3D-parallel on H100 in the LLaMA-3 band, discount 5% for first-time-on-recipe ramp").
7. **Name the instrumentation targets.** Chapter 4 lists five categories (step-time telemetry, MFU/HFU dashboards, NCCL/comm telemetry, HBM utilisation, dataloader latency). For each, state a numeric target the training-pipeline engineer's dashboard should alert against.
8. **Compute the wall-clock forecast.** Use `wall-clock = C / (peak · MFU · 3600 · K)` from chapter 4. Show the arithmetic at your point MFU. Compute the 1-sigma band: wall-clock at MFU-lower-band and MFU-upper-band.
9. **State the re-plan trigger.** Numeric. E.g., "measured MFU below 30% at ablation-phase end → root-cause with ai-infra-performance and re-plan"; "step-time-decomposition disagrees with plan by > 10% on any row → adjust the recipe."

## Deliverable

A single document, 2-3 pages plus one roofline sketch, containing:

- **Section 1** — Recipe restated (one-paragraph pointer to exercise-01).
- **Section 2** — Hardware roofline: peak FLOPs, HBM bandwidth, ridge point, sketch.
- **Section 3** — Planned MFU with band, cited reference for the band, argument for where in the band you land.
- **Section 4** — Step-time decomposition table.
- **Section 5** — Expected bottleneck with mechanistic argument and signature diagnostics.
- **Section 6** — Instrumentation plan: five categories with numeric targets.
- **Section 7** — Wall-clock forecast at point MFU with 1-sigma band.
- **Section 8** — Re-plan trigger (numeric).
- **Section 9** — Three-sentence personal reflection: what surprised you about the step-time decomposition compared to the "raw compute" intuition you had before?

## Starter guidance

- **Do not quote MFU without citing a source.** *"We expect 45% MFU"* with no reference is a plausibility guess. *"We expect 45% MFU because LLaMA-3 reported ~42% MFU on H100 in a similar recipe class and our step-time decomposition (below) supports the estimate"* is a plan.
- **The step-time decomposition is where the plan lives.** If you cannot fill in a plausible row-by-row breakdown, your MFU number is not defensible. Chapter 4 gives typical ranges — use them as sanity checks, not as answers.
- **Attention often surprises.** For long sequences (> ~4K), attention FLOPs can dominate; FlashAttention-2/3 brings the attention row down but does not remove it. State which FlashAttention version the recipe assumes.
- **The bottleneck is often "interconnect" for anything above 128 accelerators.** Recipes that plan "compute-bound" at 512+ accelerators without justification are usually wrong.
- **Wall-clock arithmetic is where MFU meets deadlines.** Do the arithmetic at both bounds of your MFU band. If the upper bound is a week and the lower bound is a month, the program has a scheduling problem the plan should surface.
- **Do not hand off the instrumentation implementation.** State the targets; the training-pipeline engineer builds the dashboards. If your instrumentation section reads like a Grafana query definition, you are doing implementation work.

## Acceptance criteria

- Hardware roofline is stated with vendor-cited peak FLOPs and HBM bandwidth for the specific accelerator model in the recipe.
- MFU planning number is a point estimate with a ±5-10% band and cites at least one published-recipe reference (chapter 4's list is the source).
- Step-time decomposition table has all seven rows populated with percentages that sum to ~100% and a one-line source per row.
- Expected bottleneck is named as one of the four candidates and argued mechanistically from the step-time table, not by analogy.
- Instrumentation plan names all five categories from chapter 4 and gives each a specific numeric target.
- Wall-clock forecast shows the arithmetic and reports both the point estimate and the 1-sigma band.
- The re-plan trigger is stated with a specific numeric threshold and a specific action.
- The wall-clock forecast reconciles with the cluster-hour budget from mod-403 chapter 3 (or the discrepancy is stated and the re-plan trigger names how it gets closed).

## Stretch goals

- **HFU projection.** Estimate HFU alongside MFU using the chapter 4 HFU / MFU ratio bands (1.1×-1.4× depending on recompute policy). State which the training-pipeline engineer's GPU-counter dashboard will natively measure.
- **Precision comparison.** Re-do the MFU plan under BF16 and under FP8 (Transformer Engine). Which bottleneck row changes? What is the wall-clock delta if FP8 achieves its projected uplift?
- **Sensitivity to seq length.** Redo the step-time decomposition at 0.5× and 2× the target sequence length. Which row grows fastest? Does the recipe's SP-or-not decision hold at 2× seq?
- **Compare with a public step-time decomposition.** LLaMA-3 (Meta 2024), MegaScale (ByteDance 2024), and DeepSeek-V3 (2024) all publish step-time or MFU decompositions. Overlay your decomposition against one of theirs; note the differences and defend the deltas.
- **Roofline plot with the recipe's operations placed.** Sketch the roofline (log-log FLOPs vs. arithmetic-intensity) and place the four operation categories — forward GEMM, attention, LayerNorm, optimizer — on the plot. Which are compute-bound; which are HBM-bound?
