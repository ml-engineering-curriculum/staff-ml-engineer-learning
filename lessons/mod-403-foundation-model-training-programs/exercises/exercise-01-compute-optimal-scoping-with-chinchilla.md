# exercise-01: Compute-Optimal Scoping With Chinchilla

**Estimated effort:** 3 hours

## Objective

Author the **scaling-law bet document** for a concrete training-program brief: pick a compute-optimal (N, D, C), plot the isoflop bowl around it, decide whether to sit on the Chinchilla-optimal point or over-train for inference cost, and put a defensible confidence band on the extrapolation. The document produced here is section 1 of the program brief and the top-of-page-1 content of the executive one-pager (exercise-05).

The exercise is deliberately about a *bet* — a written commitment to a specific (N, D) with numeric justification — not about running any actual training. Chapter 6's ablation cadence is what tests the bet later; exercise-03 walks that.

## Prerequisites

- Chapter 01 — What a Training Program Is (the six-artifact framing, program vs. run, staff scope).
- Chapter 02 — Scaling Laws and the Chinchilla Bet (Kaplan → Hoffmann, 6ND, isoflop bowl, deployment-cost-dominated over-training).
- Chapter 03 skim — enough to know the FLOP number will later convert to accelerator-hours; exercise-01 does not do that conversion.

## Choose your program brief

Pick one of the following, in preference order. You will carry the same brief through exercises 02, 03, 04, and 05 — do not swap it.

1. **A real program at your current employer that you would plausibly own or co-own.** Anonymise team names in the writeup; keep the actual constraints (target downstream product, expected inference volume, hardware availability, deadline pressure).
2. **A real program at a prior employer less than 18 months ago.** Same anonymisation.
3. **A publicly-scoped program you can reverse-engineer from a paper or blog.** Good picks: LLaMA-2 7B or 13B (Touvron et al. 2023), LLaMA-3 8B (Meta 2024), Falcon 7B / 40B, MPT-7B (MosaicML). Pretend you are the program owner and re-derive the (N, D) bet from scratch — do not copy the paper's answer, arrive at it.
4. **A domain-adapted fine-tune program you propose.** Legal-doc chat, medical-note summarisation, code assistant for a specific programming language. Assume you are continuing pretraining from an existing 7B or 13B open-weights base.

Do not fabricate a fictional company with fictional constraints. The exercise's value is in the messiness of a real budget and a real deployment expectation.

## Steps

1. **State the program's compute budget in FLOPs.** Pick a defensible C from one of two entry points: (a) an accelerator-hour allocation your org would plausibly approve (e.g., "we can get 200,000 A100-hours") converted to FLOPs at a stated MFU assumption, or (b) a dollar budget (e.g., "$500k") converted through a stated per-accelerator-hour price and per-accelerator FLOPs. Show the conversion.
2. **Classify the program as training-cost-dominated or deployment-cost-dominated.** State the expected inference volume over one year: number of downstream users × requests-per-user-per-day × tokens-per-request × 365. If deployment inference-FLOPs exceed training FLOPs by more than 3×, the program is deployment-cost-dominated. Otherwise, treat as training-cost-dominated.
3. **Compute the Chinchilla-optimal point.** N* = sqrt(C / 120); D* = 20 · N*. Show the arithmetic. Verify 6 · N* · D* = C.
4. **Plot the isoflop bowl at your C.** Evaluate the Hoffmann parametric loss form — L(N, D) = E + A / N^α + B / D^β with the coefficients Hoffmann et al. published — for N in {N*/4, N*/2, N*, 2N*, 4N*} with D = C / (6N). Plot loss vs. N. A hand-drawn plot is fine; label the axes and the Chinchilla-optimal minimum.
5. **Overlay the deployment-cost curve if deployment-cost-dominated.** For each candidate N, compute one-year inference FLOPs = 2N × (expected inference tokens). Convert to dollars at your accelerator rate. Overlay on the plot as a second axis or a stacked-cost bar.
6. **Pick your (N, D).** State the chosen point. If deployment-cost-dominated, defend the over-training (or under-training) call vs. the Chinchilla-optimal point on the plot. If training-cost-dominated, defend why the Chinchilla-optimal point is right and not a Kaplan-era over-N pick.
7. **State the confidence band.** How much of the extrapolation is Hoffmann-parametric (published fit) and how much is your own small-scale grid (Chapter 6 phase 1)? Put a ±X% band on N and D and defend X. Read the Besiroglu et al. 2024 replication attempt before picking X.
8. **State the re-plan trigger.** At what phase-1 grid disagreement magnitude do you re-plan the (N, D)? Concrete, numeric.

## Deliverable

A single document, 3-5 pages, containing:

- **Section 1** — Program brief overview (one paragraph): what you would train, what downstream product it serves, hardware environment.
- **Section 2** — The compute budget C in FLOPs, with the arithmetic showing where it came from.
- **Section 3** — The training-vs-deployment-cost classification with the expected-inference-volume calculation.
- **Section 4** — The Chinchilla-optimal (N*, D*) with the arithmetic.
- **Section 5** — The isoflop plot (embedded or linked).
- **Section 6** — The chosen (N, D) with the defense against Chinchilla-optimal if you moved off it.
- **Section 7** — The confidence band with the citation for the fit source.
- **Section 8** — The re-plan trigger.
- **Section 9** — Three-sentence personal reflection: what surprised you between running the arithmetic and the (N, D) you had in your head before?

## Starter guidance

- **Do not skip the arithmetic.** The exercise's value is that you compute C, N*, D* longhand at least once. LLM-assisted calculation is fine to check your work; do not use it to skip the calculation.
- **Do not read Hoffmann et al.'s tabulated (N*, D*) and reverse-engineer.** The point of the exercise is the derivation, not the answer.
- **The deployment-cost inference-volume estimate is speculative — that is fine.** State the assumption, then rerun the plot at 0.5× and 2× the volume. That is the sensitivity analysis the review will ask for.
- **The confidence band is uncomfortable.** ±30% feels vague to write. It is the honest number. If you write ±5%, the review will notice and push back.
- **Do not let the plot be beautiful at the cost of being wrong.** A rough sketch that is arithmetically correct beats a Matplotlib figure with an off-by-2× error.

## Acceptance criteria

- The compute budget C is stated in FLOPs with the arithmetic converting from accelerator-hours or dollars.
- The training-vs-deployment classification is stated and supported by the inference-volume calculation.
- The Chinchilla-optimal (N*, D*) is computed with the arithmetic shown; verify 6ND ≈ C to two significant figures.
- The isoflop plot has at least five points; the Chinchilla-optimal minimum is labelled.
- The chosen (N, D) is defended against the Chinchilla-optimal point using the plot; if the program is deployment-cost-dominated and the chosen point is not over-trained toward smaller N, explain why not.
- The confidence band is stated with a specific ±% and cites at least one source (Hoffmann et al. 2022 or Besiroglu et al. 2024).
- The re-plan trigger is stated with a specific numeric magnitude.
- The personal reflection makes at least one specific observation about what surprised you.

## Stretch goals

- **Multi-scenario sensitivity.** Redo steps 3-6 at 0.5× C, 1× C, and 2× C. Which of the three scenarios is Pareto-defensible if leadership offers you a bigger or smaller budget than you planned for?
- **Domain-specific fit.** If your program is a fine-tune of an existing base (LLaMA / Mistral / Qwen), read the base paper's scaling-law section and adapt the Hoffmann fit to the base's reported ratios. Note where the domain-adapted fit diverges from Hoffmann and defend the divergence.
- **Compare with a published program.** Take one of the LLaMA-1/2/3 papers or the DeepSeek-V2 paper, apply the same steps to their brief, and report where your derivation lands vs. the paper's (N, D). If you land more than 2× off in either direction, investigate — it is usually because the paper made a deployment-cost or data-quality bet you did not model.
