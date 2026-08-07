# exercise-04: Cluster Reserve and Checkpoint Strategy

**Estimated effort:** 3 hours

## Objective

Author the **cluster-reserve-and-checkpoint strategy** for the recipe from exercises 01-03: set the reserved / burst / spot capacity split with per-posture rationale, size the warm-standby pool via an MTBF-and-3-sigma calculation, derive the checkpoint interval with the Young/Daly optimum, name the restart-latency budget with a component decomposition, and state the storage-substrate acceptance criteria the peer platform track will design against. Roll all of this into a revised cluster-hour total that reconciles with mod-403 chapter 3's budget.

This is section 4 of the mod-404 five-artifact stack. The deliverable is a two-to-four-page strategy document plus the arithmetic that supports each committed number.

## Prerequisites

- Chapter 06 — Cluster reserve and checkpoint strategy (three capacity postures, warm-standby sizing, Daly derivation, restart-latency components, storage acceptance criteria, cluster-hour rollup).
- Exercise-01, exercise-02, exercise-03 delivered: recipe tuple, wall-clock forecast, failure-mode budget with per-mode event rates.
- Mod-403 chapter 3 for the cluster-hour budget the revised total reconciles with.

## Steps

1. **State the capacity split.** As percentages of total planned accelerator-hours: reserved / burst / spot. Chapter 6 recommends 85-90% reserved / 10-15% burst / 0-5% spot as the typical band. Adjust for your program's shape (a 2-day fine-tune has different economics from a 2-month pretrain) and cite the rationale. Include the on-demand-vs-reserved price ratio your organization actually pays.
2. **Size the warm-standby pool.** Two calculations:
   - **MegaScale-style heuristic.** State ~5% of the main-run pool as a floor; adjust up if your MTBF assumption is worse than published fleet numbers.
   - **N-sigma calculation.** Given per-node MTBF `M` and main-run duration `T` on `K` accelerators, expected failure count is `T · K / M`. Pick standby `S` to give a 3-sigma buffer (i.e., `S ≈ mean + 3√mean` under a Poisson approximation). Show the arithmetic. State the MTBF citation.
   State the committed standby as both an absolute number and a percentage of the main-run pool.
3. **Derive the checkpoint interval.** Apply the Daly formula `τ* = √(2 · T_c · M)`:
   - Estimate `T_c` (single-checkpoint wall-clock cost including chapter 5 mode 4 verification). Cite: prior-program measurement, platform-team storage numbers, or an explicit plausibility estimate.
   - Use `M` from the failure-mode budget (exercise-03) — the aggregate MTBF for modes that trigger a full-cluster restart (typically modes 1, 3, 5, some 4).
   - Compute τ*. Round to a convenient interval (30 min / 1 h / 2 h). State the retention policy — chapter 5 mode 4 recommends at least 3 recent checkpoints kept.
4. **State the restart-latency budget.** Chapter 6's component decomposition:
   - Detection latency (per-mode; take the max).
   - Job teardown (scheduler-dependent).
   - Job reboot (image-pull, pod-schedule).
   - Framework initialisation (NCCL cold-init, mesh construction).
   - Checkpoint load (object-store read throughput bound).
   - Warmup steps (framework-dependent).
   Give a target for each and a total. State the worst-case (~2× target) that the exercise-03 wall-clock arithmetic used.
5. **State the storage substrate acceptance criteria.** Aggregate write bandwidth (`checkpoint_size / T_c`), aggregate read bandwidth (bound by restart-latency step 5), durability (11 nines), retention (per your policy), atomicity (manifest-write-last or equivalent). Cite the specific storage backend and the peer platform track that will implement it.
6. **Compose the revised cluster-hour rollup.** Chapter 6's formula: `total = main_run + ablation + eval + main_run × failure_fraction + main_run × standby_fraction`. Use the failure_fraction from exercise-03. Reconcile with mod-403's committed cluster-hour total; state any delta and how it gets closed.
7. **Iterate exercise-03 if needed.** If the restart-latency budget from this exercise materially differs from the 20-45-minute placeholder exercise-03 used, revise exercise-03's per-mode wall-clock costs and re-sum the failure fraction.
8. **State the re-plan trigger.** Numeric per number: measured checkpoint IO > 1.5× the `T_c` assumption → renegotiate interval or storage; measured restart-latency > 1.5× target → decompose and escalate to component owner; measured failure rate > 1.5× MTBF assumption → grow standby.

## Deliverable

A single document, 2-4 pages, containing:

- **Section 1** — Capacity split (reserved / burst / spot) with per-posture rationale.
- **Section 2** — Warm-standby pool sizing (MegaScale heuristic + N-sigma calculation).
- **Section 3** — Checkpoint interval (Daly derivation + retention policy).
- **Section 4** — Restart-latency budget (component decomposition + target + worst-case).
- **Section 5** — Storage substrate acceptance criteria.
- **Section 6** — Revised cluster-hour rollup, reconciled with mod-403.
- **Section 7** — Re-plan triggers (numeric per number).
- **Section 8** — Three-sentence personal reflection: which of the three levers (reserve size, checkpoint interval, restart-latency) surprised you as the dominant contributor to the failure-fraction total?

## Starter guidance

- **The three numbers are one optimisation, not three.** A tight checkpoint interval without warm standby is wasted; warm standby without a tight interval is over-reserved. Iterate — do not treat the sections sequentially.
- **Do not budget from vibes.** Every committed number in this document should have arithmetic behind it. Reviewers will ask "why 30 minutes and not 60?" — the Daly derivation is the defensible answer.
- **Spot is almost never the right choice for the main run.** State this explicitly if you are considering it. The exception is a very short single-node fine-tune with checkpointing every few minutes; that is a small minority of Staff-scope programs.
- **The on-demand price ratio matters more than you expect.** A program that plans 90% reserved / 10% on-demand can end up spending 40% of its accelerator budget on the 10% if the ratio is 4×. Show the arithmetic in cash terms if your program brief includes a dollar budget.
- **Cite the platform team.** Storage bandwidth, network topology, and scheduler-teardown latency are peer platform-track numbers you consume, not numbers you derive. Note which platform team owns each and what the current SLO is.
- **The revised rollup is the artifact mod-403 reads.** If it exceeds the mod-403 committed budget, the delta is a hand-back to the program brief, not something to bury. Chapter 1's five-artifact framing anticipates this.

## Acceptance criteria

- Capacity split adds to 100% and is stated as percentages with a per-posture rationale.
- Warm-standby size is stated as absolute number and percentage; the N-sigma calculation is shown with the MTBF citation.
- Checkpoint interval is derived via the Daly formula with the `T_c` and `M` inputs stated; the rounded committed interval is explicit; retention policy is stated.
- Restart-latency budget includes all six component estimates; total is stated; worst-case is stated.
- Storage acceptance criteria include write bandwidth, read bandwidth, durability, retention, atomicity; each is a specific number.
- Revised cluster-hour rollup is arithmetic-visible and reconciled with mod-403's committed total; any delta is called out with the hand-back direction.
- Re-plan triggers are stated per committed number with a specific threshold and action.
- Exercise-03's failure-mode wall-clock arithmetic is updated (or explicitly cross-verified) with the restart-latency number this exercise commits.
- Reflection paragraph names which lever surprised you.

## Stretch goals

- **Two-scenario reserve.** Redo the capacity split for the same program at 0.5× and 2× the main-run duration. Where does the reserved-vs-burst split shift? A 2-day fine-tune is very different from a 2-month pretrain even at the same accelerator count.
- **Async-checkpoint sensitivity.** Chapter 6 notes that async checkpointing (`torch.dcp.async_save`, DeepSpeed's async pipeline) shifts IO off the critical path. Redo the Daly derivation assuming `T_c → 0` on the critical path. Which failure-mode assumption gets weaker and which gets stronger? Discuss the atomicity implication.
- **Storage-backend bake-off.** For your `T_c` requirement, quote the write bandwidth needed and enumerate three candidate object-store or filesystem backends (GCS, S3 Express One Zone, Weka, VAST, in-house Ceph). Which meets the bandwidth cheapest? Which meets the durability requirement without additional replication?
- **Elastic training vs. checkpoint-restart.** Compare the wall-clock cost of straggler-eviction under (a) restart-from-checkpoint at 20-minute latency and (b) elastic node substitution at 1-minute latency. At what per-week event rate does elastic become the higher-value investment? Chapter 6 opens the question; you close it with arithmetic.
- **Compare against a published program.** Read the LLaMA-3 paper's operational-engineering section or the MegaScale paper. Report their standby percentage, checkpoint interval, and restart-latency. Where does your recipe diverge, and defend the divergence with a specific mod-404 chapter constraint.
