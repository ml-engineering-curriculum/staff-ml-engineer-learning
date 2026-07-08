# exercise-03: Ablation Sequencing Plan

**Estimated effort:** 3 hours

## Objective

Author the **ablation and eval plan** for the program brief from exercises 01 and 02: the phased cadence of proxy runs, small-scale ablations, full-scale ablations, and main run; the eval waterfall across the five tiers; the checkpoint policy; and the pre-agreed stop / re-plan triggers. The document produced here is section 5 of the program brief and the "timeline and gates" section of the executive one-pager.

The exercise is about the *cadence and the gates*, not about running actual training. The written artifact is what the review committee has to sign off on before the compute budget is released.

## Prerequisites

- Exercises 01 and 02 completed. The (N, D) target, the mixture, and the pipeline design set what the ablations are testing.
- Chapter 06 — Ablation Cadence and the Eval Waterfall (four phases, µP transfer, five-tier waterfall, decontamination, checkpoint policy, stop-conditions).
- Chapter 03 skim on the risk-adjusted budget breakdown — the ablation compute must fit inside the cluster-hour budget you defended.

## Steps

1. **Restate the compute budget breakdown from exercise-01, chapter 3.** Confirm the ablation buckets — small-scale ablation grid (10-25%), full-scale ablations (20-50%), restarts (10-25%), post-training-and-eval (5-15%), headroom (10-20%) — with your program's numeric split.
2. **Design phase 1 (proxy runs / isoflop grid).** Pick the grid: 3-5 N values (e.g., 40M / 100M / 300M / 800M) at 3-5 C points each. Compute the total proxy-run compute; verify it fits inside the small-scale ablation bucket. State the deliverables: the fitted L(N, D) parametric form, the µP-transferred learning-rate table.
3. **Design phase 2 (small-scale ablation matrix).** List the 10-25 cells you would run, each with the variable it isolates (data mixture, dedup aggressiveness, architecture, optimizer, tokenizer), the fixed config, the small-scale N and D, and the eval-tier the cell is decided on. For each cell, state a specific effect-size threshold below which the cell is a null result.
4. **Design phase 3 (full-scale ablations).** List 1-3 full-scale ablations you would run before main-run commit, each at the target N and a fraction of the target D. State what each ablation is confirming (usually the phase-2 winner at scale). Or, if you decide to skip phase 3, defend the skip.
5. **Design phase 4 (main run).** State the warmup / main / cool-down step-count breakdown. State the checkpoint cadence: every-N-step rolling for restart, and permanent waypoints at 10% / 25% / 50% / 75% / 100% and every 2% of cool-down. Compute the storage cost.
6. **Design the eval waterfall.** Fill in each of the five tiers: loss (every step), held-out perplexity (which held-out sets, how often), downstream benchmarks (which benchmarks — pick a specific set including at least MMLU / HellaSwag / HumanEval / GSM8K and one domain benchmark if applicable), safety and red-team (categories, cadence), human/preference eval (when it runs, sample size).
7. **Set the decontamination policy.** Which benchmarks are decontaminated against at pipeline ingest? How is decontamination verified? Where does the audit trail live?
8. **Set the stop and re-plan triggers.** Write the four gates: phase-1 exit, phase-2 exit, phase-3 exit, mid-main-run halt. Each with a specific numeric threshold and the decision the program committee has to make when the threshold is crossed.
9. **Name the owners.** Who runs the eval waterfall between checkpoints? Who calls the mid-run halt if the gate is crossed? Who signs off on advancing from phase to phase?

## Deliverable

A single document, 4-6 pages containing:

- **Section 1** — Restated compute-budget breakdown.
- **Section 2** — Phase 1 (proxy) design: grid, compute, deliverables.
- **Section 3** — Phase 2 (small-scale ablation) matrix: 10-25 cells with variable, config, compute, and decision-tier.
- **Section 4** — Phase 3 (full-scale ablation) plan or defended skip.
- **Section 5** — Phase 4 (main run) shape: warmup/main/cool-down step budget and checkpoint policy.
- **Section 6** — Eval waterfall specification, five tiers with cadences and eval sets.
- **Section 7** — Decontamination policy and audit trail.
- **Section 8** — Stop and re-plan triggers, per phase, numeric.
- **Section 9** — Owners table (who runs which piece).
- **Section 10** — Three-sentence reflection on the gate you found hardest to write and why.

## Starter guidance

- **Do the compute arithmetic per phase.** The ablation phase is where cost creeps in. If your phase-2 matrix adds up to 50% of main-run compute, you have over-scoped. Cut cells.
- **The mid-run halt trigger is the hard one.** Program-owners routinely defer writing it and end up unable to halt runs that should be halted. Sit with the discomfort and write a specific number.
- **Do not invent benchmarks.** Pick from the standard suites (MMLU, MMLU-Pro, HellaSwag, Winogrande, ARC-Easy, ARC-Challenge, HumanEval, MBPP, GSM8K, MATH, TriviaQA, TruthfulQA, BBQ, RealToxicityPrompts) and, for a domain program, add one or two published domain benchmarks. Cite each.
- **µP transfer belongs in phase 1.** If your plan skips it, you are gambling on hyperparameter transfer without the tool that makes it work. Read Yang et al. 2022 before defending the skip.
- **Decontamination is often skipped in first drafts.** The review will catch this. Write it in from the start.

## Acceptance criteria

- The compute breakdown per phase adds up to no more than the risk-adjusted budget from exercise-01 (main run + ablation + restart + post-training + headroom).
- Phase 1 has a grid of 3-5 N values at 3-5 C points each; total compute fits the small-scale bucket.
- Phase 2 matrix has 10-25 cells, each with variable / config / small-scale (N, D) / decision-tier / effect-size threshold.
- Phase 3 either has 1-3 planned ablations or a defended skip with rationale.
- Phase 4 has explicit warmup / main / cool-down fractions summing to 100% of main-run steps.
- Checkpoint policy is specific enough to compute storage cost.
- Eval waterfall covers all five tiers with named benchmarks and citation for each. At least one safety and one preference benchmark named.
- Decontamination policy is stated with the list of benchmarks decontaminated against and the audit-trail location.
- Every phase has a specific numeric stop or re-plan trigger. The mid-run halt trigger is not `TBD`.
- Owners table has a specific role (or a role placeholder like "the pretraining lead") named for each responsibility.

## Stretch goals

- **The µP transfer sanity check.** Read Yang et al. 2022 in enough depth to describe, in one paragraph, what µP predicts about the learning-rate range at your phase-1 grid vs. what you would expect from a naive linear-rate scaling. Include the paragraph in the plan.
- **Cross-check against a published program's ablation list.** Read the LLaMA-3 or the DeepSeek-V2 report and list the ablations they published. Which of your phase-2 cells appear in their list? Which appear on their list but not yours? For each mismatch, defend the difference.
- **Design an interference test for the eval waterfall.** For your downstream benchmark set, propose a synthetic contamination test: inject a known set of benchmark-adjacent tokens into the training corpus and measure the resulting eval-score inflation. This is a chapter-6 tier-3 diagnostic that most programs skip.
- **Post-training eval-waterfall extension.** Extend the tier-3 and tier-4 benchmarks to include the instruction-following benchmarks (MT-Bench, AlpacaEval 2.0, IFEval) that only make sense for post-trained models. Which of these run during phase 4 vs. only in exercise-04's post-training plan?
