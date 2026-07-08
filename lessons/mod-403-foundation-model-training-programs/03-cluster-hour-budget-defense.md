# From FLOPs to Cluster-Hours: Defending the Budget

## Motivation

The scaling-law bet in chapter 2 hands the program a target C in FLOPs. Leadership does not approve FLOPs; leadership approves **accelerator-hours** and **dollars**. Converting between the two is the second-most-arithmetic-heavy artifact of the program — and the one that is most often padded (by nervous engineers) or under-priced (by ambitious engineers), both of which cost the program credibility. This chapter walks the conversion honestly, adds the risk buffer the program actually needs for ablations and restarts, and shows how to defend the total against the three alternate calls the review will raise.

The staff-plus deliverable at the end of this chapter is a **cluster-hour budget document** — three or four pages — that answers, in order: *how many accelerator-hours does the target C need at plausible utilisation? how many additional hours does the ablation phase and the restart buffer require? what does the total cost in dollars? why is that better than the alternatives?* Chapter 8's exec one-pager reads three numbers from this document: the main-run FLOPs, the risk-adjusted accelerator-hours, and the total cost.

## The arithmetic — FLOPs to accelerator-hours

The conversion has four factors:

**1. Accelerator peak FLOPs.** The vendor-published peak FLOPs of the accelerator at the numerical precision the program actually trains at.

- **H100** at BF16 tensor-core throughput: ~989 TFLOPs/s peak dense (~1.98 PFLOPs/s with sparsity, which pretraining does not use).
- **A100** at BF16 tensor-core throughput: ~312 TFLOPs/s peak dense.
- **H200** at BF16: same math throughput as H100 (~989 TFLOPs/s); the difference is memory bandwidth and capacity.
- **TPU v5p** at BF16: ~459 TFLOPs/s peak dense per chip.
- **B100 / B200** family: vendor-published numbers; check current NVIDIA whitepapers before writing the doc.

Use the correct precision. Pretraining programs typically train in BF16 activations with FP32 optimizer state (mixed-precision AMP), and the tensor-core BF16 number is the right ceiling. Programs using FP8 (Transformer Engine, DeepSpeed FP8) get a ~2× throughput uplift but pay a small quality tax that the program has to price separately.

**2. Model FLOPs Utilisation (MFU).** The fraction of peak FLOPs the training loop actually converts into useful model FLOPs. This is where the honest number lives.

The MFU numbers from published pretraining programs cluster in a narrow range:

- **Naive DDP on small models**: 20-30%.
- **FSDP / ZeRO-3 with careful communication overlap**: 35-45%.
- **3D parallelism (tensor + pipeline + data), well-tuned**: 45-60% at scale (PaLM reported 46% MFU on TPUv4; LLaMA-2 reported ~40% on A100; the MegaScale paper from ByteDance reported ~55% at 12k GPUs).
- **State-of-the-art programs** running FP8 with fused kernels sometimes reach **60-65% MFU** on H100s at scale; anything above that is publication-material and should not be assumed by a program that has not demonstrated it.

Chapter 3's honest planning number is usually **35-45% MFU**. Programs new to a hardware generation often start at 20-30% and climb to 40-50% over the ablation phase as the training-pipeline engineers tune. Chapter 6's ablation phase is when the number gets measured; the program doc should quote **a planned MFU with an uncertainty band**, not a point estimate.

**3. Hardware FLOPs Utilisation (HFU).** MFU counts only useful model FLOPs — the forward and backward passes on the model itself. HFU includes activation recomputation, gradient accumulation, optimiser step arithmetic, and other legitimate but not-directly-model FLOPs. HFU is typically 1.1-1.4× MFU. The MFU vs HFU distinction matters when comparing published numbers — a 45% MFU program and a 55% HFU program may be doing the same amount of useful work. Chapter 3 recommends the program doc quote MFU for scaling-law arithmetic (because that is what the 6ND FLOP estimate maps to) and quote HFU when cross-referencing training-pipeline-engineer instrumentation.

**4. Cluster wall-clock hours.** Once MFU is fixed:

> **accelerator-hours = C / (peak FLOPs per accelerator per second · MFU · 3600)**

And:

> **wall-clock hours = accelerator-hours / (number of accelerators)**

The wall-clock number is what leadership actually cares about — *"when does the main run finish?"* — and it is what constrains the ablation-cadence design in chapter 6.

## A worked example

Take the deployment-cost-dominated program from chapter 2: N = 15B parameters, D = 1.1T tokens, C = 6ND ≈ 10^23 FLOPs. Suppose the org has a 512-H100 pod earmarked for this program, and the program has a planned MFU of 40%.

- **Peak FLOPs per H100 per second at BF16**: 989 × 10^12 FLOPs/s.
- **Useful FLOPs per H100 per second at 40% MFU**: 0.4 · 989 × 10^12 ≈ 3.96 × 10^14 FLOPs/s.
- **Accelerator-hours required for the main run**: 10^23 / (3.96 × 10^14 · 3600) ≈ 10^23 / (1.42 × 10^18) ≈ 70,400 H100-hours.
- **Wall-clock on 512 H100s**: 70,400 / 512 ≈ 137 hours ≈ 5.7 days.

That is the main-run number. It is not the budget. The budget adds ablations, restarts, and headroom.

## Risk-adjusting the budget

The most common mistake a first-time program owner makes is to submit the raw main-run number and treat everything else as "we'll figure it out." The main run is 20-40% of the actual program consumption for a healthy program.

The additional buckets:

**1. Small-scale ablation grid.** Chapter 6 walks the ablation phase in detail; at this stage, budget for **10-25% of the main-run compute** spent on small-model runs across the isoflop grid (N/4, N/2, N with D correspondingly scaled). These are cheap individually but numerous.

**2. Full-scale ablations before commit.** Before the main run starts, most programs run **two to five full-scale, shorter-duration ablations** at the target N to decide between candidate architectures, mixtures, or optimizer settings. Each is often 10-25% of the main-run compute. Budget **20-50% of main-run compute** for this bucket in aggregate.

**3. Restart and failure budget.** The main run at 512+ accelerators for days-to-weeks will experience hardware failures. Meta's OPT-175B logbook, the BLOOM 176B logs, and the LLaMA papers all document tens to hundreds of restarts and re-shuffling events across a main run. Budget **10-25% additional compute for restarts and re-computation from the last checkpoint**. Programs with high-quality health-check tooling and warm-standby node counts get to the low end of that range; programs with none pay the high end.

**4. Post-training compute.** Chapter 7 walks post-training. As a first-order estimate, SFT + preference optimisation typically consumes **1-5% of the main-run compute** — very small relative to pretraining. But it is not zero. And the program's evaluation harness (chapter 6) runs continuously, sampling checkpoints, running benchmarks. Budget **5-10% of main-run compute** for the combined post-training and continuous-eval bucket.

**5. Headroom for the unknown.** Programs that budget zero headroom are the programs that ship six weeks late. Budget **10-20% additional compute** as an explicit line-item headroom, and be prepared to defend not spending it if the program comes in under.

Adding up the middle-of-the-band multipliers: main-run compute × (1 + 0.17 ablation-grid + 0.35 full-scale-ablations + 0.17 restarts + 0.075 post-training-plus-eval + 0.15 headroom) ≈ main-run × **1.9**. Round to **2× main-run compute** as the total program-level cluster-hour budget.

The 2× multiplier is not universal. A **large-scale fine-tune program** on a well-understood base with a mature training pipeline can come in at 1.2-1.5×. A **new pretraining program from scratch, first time on a hardware generation, mixture is novel** can come in at 2.5-3×. The point is to write down the multiplier and its component breakdown so the review conversation is about the components rather than the total.

## Continuing the worked example

Main run: 70,400 H100-hours. Total program at 2× multiplier: **~140,000 H100-hours**. On the 512-H100 pod, that is ~275 wall-clock hours of pod time — call it ~11.5 pod-days. In practice, the ablation and full-scale-ablation compute is distributed over one to two months of calendar time (chapter 6's cadence) and the main run is one contiguous block; the total program calendar is often two to three months, not two weeks.

At an H100 rented at ~$3/hour (public cloud on-demand mid-2024), 140,000 hours ≈ **$420,000**. At a reserved-capacity or self-hosted price of ~$1.50/hour effective, ≈ **$210,000**. Chapter 3's budget doc quotes both a public-cloud-equivalent number (for CFO cross-checking) and the actual internal cost (for the real spend line).

## Defending the budget

The review meeting will always raise three alternate program calls. The program brief should pre-empt them, in order:

### Alternate 1 — "Why not train a smaller model on the same data budget?"

Reviewer intuition: smaller model is cheaper, and for many downstream tasks a well-trained 7B is competitive.

**The defense.** State the deployment-cost analysis from chapter 2. If the program is deployment-cost-dominated, you *already* over-trained toward a smaller model. If the program is training-cost-dominated and the reviewer is asking why not go from 15B to 7B: quote the isoflop bowl — the loss at 7B / same-C is worse than at 15B by a specific amount, and that specific amount fails the specific launch gate the program is committing to. If it does not fail the launch gate, the reviewer is right and you should go smaller.

### Alternate 2 — "Why not train a larger model on the same data budget?"

Reviewer intuition: larger models look better on benchmarks; the org has the capacity.

**The defense.** Two prongs. First, **the isoflop bowl at fixed C punishes going bigger and D-lighter** just as it punishes going smaller and D-heavier; the Kaplan-era mistake was exactly this. Show the isoflop plot with the 30B / same-C point marked and its loss delta versus the picked N. Second, if the reviewer is proposing bigger N *and* bigger C (a larger *program*), that is a separate program-level conversation about *whether the marginal cluster-hours are the org's best use of the compute*. Answering that is chapter 8's exec one-pager, not chapter 3's budget doc. Punt to chapter 8 without deflecting the budget conversation.

### Alternate 3 — "Why not buy the base model from a frontier lab and skip training?"

Reviewer intuition: OpenAI, Anthropic, Google, and Meta ship strong base and API-hosted models; use of a frontier API costs orders of magnitude less than a training program.

**The defense.** Almost always the strongest of the three alternate calls, and the one Staff engineers most often fumble because it feels adversarial. It is not. Answer it head-on, in the program brief:

- **Cost comparison.** The training program's amortised cost per inference token vs. the frontier API's per-token cost, over the expected serving volume. If the frontier API wins by an order of magnitude and the program cannot argue quality parity, the program is not the right call.
- **Licensing and IP.** Does the frontier API's ToS permit the intended use? Is training-data provenance a regulatory requirement (EU AI Act high-risk, financial-services or medical-device deployment) that only in-house training can meet?
- **Latency and dependency risk.** Can the deployment tolerate a third-party API in the critical path? Sovereign / on-prem requirements?
- **Domain quality parity.** Are there specific benchmarks where the domain-adapted internal model materially beats the frontier API, and is that delta worth the training cost? This is where the chapter 6 evaluation results feed back in.

The program brief that pretends this alternate does not exist gets sent back for a rewrite. The program brief that answers the four sub-questions above credibly gets funded.

### The bonus alternate — "Why not defer this quarter and buy the wait?"

Occasionally leadership will ask *"can we run this program six months later on cheaper hardware / more mature software / more data?"* The right answer is quantitative: **what does deferring cost in delayed value, and what does it save in program cost?** Chapters 7 and 8 revisit the framing. Deferral is almost never the right call for a program with an established downstream use case and almost always the right call for a program whose downstream use case is speculative.

## What the budget doc looks like

The section shape of chapter 3's budget doc:

1. **Compute target** — the C from chapter 2's scaling-law bet, restated.
2. **Accelerator and precision** — the specific accelerator, the precision, the peak FLOPs number.
3. **MFU assumption** — the planned MFU with an uncertainty band, sourced by cross-reference to the training-pipeline engineer's benchmarking. If the number is not yet measured, say so and give the plan for measuring it.
4. **Main-run accelerator-hours and wall-clock** — the arithmetic, shown.
5. **Risk-adjustment table** — the six buckets (ablation grid, full-scale ablations, restarts, post-training, continuous eval, headroom) with a percent-of-main-run and a total.
6. **Total accelerator-hours** — the sum, and the equivalent dollar cost at both public-cloud rate and internal rate.
7. **Defense against alternates** — smaller model, larger model, buy-instead-of-train, defer-this-quarter. One paragraph each.
8. **Sensitivity analysis** — what the total changes to if MFU comes in at the low end vs. the high end of the planning band, and what the pre-agreed re-plan trigger is.

Item 8 is the item most often skipped and most often the reason the budget doc gets pushed back. Leadership will not fund a program without an "if it costs 30% more than budgeted, we do X" clause. Write the clause.

## Coordinating with the peer platform tracks

Chapter 3's budget doc gets circulated for review to more than the ML org. The peer platform tracks — specifically [`ai-infra-performance-learning`](https://github.com/ai-infra-curriculum/ai-infra-performance-learning) (MFU depth), [`ai-infra-ml-platform-learning`](https://github.com/ai-infra-curriculum/ai-infra-ml-platform-learning) (training cluster ownership), and mod-407 (the org-scope capacity portfolio) — all have skin in it.

The staff-plus discipline is to **cite their numbers, not invent them**. If the training-pipeline engineer's MFU benchmarking shows 42% at similar model shape, quote 42% ±5%. If mod-407's capacity portfolio shows 512 H100s available in reserved capacity from date X to date Y, plan the main run inside that window. If the capacity conflict is genuine — two programs both want the same 512-H100 pod — that is a portfolio-level trade-off, chapter 8's exec one-pager, and often the mod-410 tech-leadership skill.

## Summary

- Convert the FLOP target C to accelerator-hours via **peak accelerator FLOPs × MFU × 3600 seconds per hour**. Use the honest MFU number — usually 35-45% for a well-tuned FSDP-or-3D-parallel program.
- Risk-adjust the main-run number for the six buckets: **ablation grid (10-25%), full-scale ablations (20-50%), restarts (10-25%), post-training (1-5%), continuous eval (5-10%), headroom (10-20%)**. Middle-of-the-band multiplier is ≈ 2× main-run.
- Publish both the accelerator-hour total and the dollar total (public-cloud rate and internal rate). Publish the sensitivity analysis.
- Defend against three alternates: **smaller model, bigger model, buy-instead-of-train** — plus the bonus alternate of **deferral**. Pre-empt in the brief; do not wait for the review to raise them.
- Cite the peer platform tracks' numbers rather than inventing them. The budget is a portfolio artifact, not a solo document.

The next chapter walks the data side of the program: mixture design, source classes, and the quality-vs-tokens trade-off that determines whether the D from chapter 2 is achievable at all.
