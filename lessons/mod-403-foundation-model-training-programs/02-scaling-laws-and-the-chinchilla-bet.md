# Scaling Laws and the Chinchilla Bet

## Motivation

The staff-plus decision at the top of a training program is a single arithmetic bet: **for the compute the org is willing to spend, what is the right point on the (parameters, tokens) trade-off, and how confident is the extrapolation?** Every subsequent decision in the program — cluster-hour budget, data volume required, ablation cadence, main-run wall-clock — flows from that bet. Getting it wrong by a factor of two costs the program a factor of two in wall-clock and, more importantly, produces a model that under-performs the same compute budget spent at the compute-optimal point. Leadership does not distinguish "under-performed because we picked the wrong point" from "under-performed because the technology is not there yet"; the program owner does not get a rematch.

This chapter walks the modern shape of the bet — from Kaplan et al. (2020) *"Scaling Laws for Neural Language Models"* through Hoffmann et al. (2022) *"Training Compute-Optimal Large Language Models"* (Chinchilla) and the follow-on literature that partially retracts the Chinchilla-optimal call for inference-heavy deployments. It ends with the concrete written artifact — the **scaling-law bet document** — that goes into the program's executive one-pager (chapter 8).

## The three variables

The scaling-law literature parameterises a language-model training run with three variables:

- **N** — the number of trainable parameters in the model (in billions, typically).
- **D** — the number of training tokens the model sees (in billions, typically; a "token pass" is one occurrence in the training corpus, not one epoch of unique tokens).
- **C** — the total training compute in FLOPs.

The three are related by a good-enough closed form. For a decoder-only transformer trained on next-token prediction, ignoring embeddings and the final unembedding:

> **C ≈ 6 · N · D**

The 6× coefficient comes from the arithmetic of the forward and backward pass: roughly 2ND FLOPs for the forward pass and 4ND FLOPs for the backward pass, per token. Kaplan et al. (2020) is the canonical write-up of this counting; it is a *good enough* estimate at production scale — it under-counts attention FLOPs for long context and mildly over-counts for MoE architectures — but the coefficient is stable enough that it is the number every serious program uses in its budget arithmetic. Chapter 3 defends the coefficient against the more precise per-architecture counting and shows when to switch.

So the program's arithmetic bet reduces to *"choose N and D such that 6ND matches the compute budget C and the resulting model minimises loss at that C."*

## Kaplan et al. (2020) — the first scaling law

Kaplan et al. (2020) fit a power law to the loss as a function of compute, parameters, and tokens across a sweep of models on the OpenAI corpus. The headline finding was that loss falls as a power law in each of N, D, and C, with roughly independent exponents, and that within the range they studied **larger models are more sample-efficient than smaller models**. The practical implication was: given a compute budget, scale N aggressively and keep D modest.

That interpretation drove the shape of GPT-3 (175B parameters, ~300B training tokens, roughly 1.7 tokens per parameter — very D-light by later standards). It is the reason a generation of large models were dramatically under-trained on data by the standards we now apply. Kaplan et al. themselves were careful about the extrapolation range; the load-bearing part of the mistake was that later programs read the paper as "always scale N, D will catch up."

Read the paper. Read it once, then read the Hoffmann et al. paper, then read Kaplan et al. again and notice the range disclaimers. The exercise is what most staff engineers who quote scaling laws in their program docs have not done.

## Hoffmann et al. (2022) — Chinchilla

Hoffmann et al. (2022) re-fit the scaling laws with three methods (isoflop curves, IsoLoss curves, and a parametric loss function) on a much larger sweep of models with a more careful compute-optimal search. The headline finding overturned the Kaplan et al. practical implication:

> **For a fixed compute budget C, the compute-optimal (N*, D*) pair scales N and D roughly in step: N* and D* each grow as C^0.5.**

The concrete heuristic that fell out of the fit:

> **D* ≈ 20 · N*** (i.e., roughly twenty training tokens per parameter is compute-optimal at the scale of a few-hundred-million to a few-tens-of-billions of parameters).

The 70B-parameter Chinchilla model, trained on ~1.4T tokens (20× its parameter count), out-performed the 280B-parameter Gopher trained on 300B tokens on essentially every benchmark, at roughly the same total compute. The lesson was direct: at that compute scale, Gopher had been dramatically over-parameterised and under-trained on data; a smaller model trained on more data won.

The 20:1 heuristic is not law; it is a **fit at the scale range Hoffmann et al. studied**. Two important caveats up front:

- The exponents Hoffmann et al. fit have been re-fit by later researchers (notably Besiroglu et al. 2024, "Chinchilla Scaling: A replication attempt") who argue the *N*:D* ratio* is closer to 20:1 with wider confidence intervals than the paper's phrasing implies. The bottom line: **use 20:1 as the point estimate, but treat the true ratio as 10:1 to 40:1 with roughly log-uniform uncertainty**, and the program's ablation runs (chapter 6) are what tighten it for your architecture.
- The Chinchilla-optimal point minimises **loss at end of training**. It does not minimise loss-plus-inference-cost integrated over a model's deployment lifetime. That distinction is the reason modern inference-heavy programs deliberately train *past* the Chinchilla-optimal point. The next section walks that call.

## When Chinchilla-optimal is the wrong call — over-training for inference

Chinchilla-optimal is the right call if the program is **training-cost-dominated**. It is often the wrong call for programs whose model will serve orders of magnitude more inference tokens than training tokens.

Consider a model deployed for a year, serving 100T inference tokens (a plausible number for a well-adopted chat model). At the Chinchilla-optimal point for a 70B model — ~1.4T training tokens — training compute is roughly 6 · 70B · 1.4T ≈ 6 × 10^23 FLOPs. Inference compute at ~2N FLOPs per token is 2 · 70B · 100T ≈ 1.4 × 10^25 FLOPs — over an order of magnitude more than training. Halving the parameter count to 35B halves inference cost for the same throughput; the training loss is worse than 70B-at-Chinchilla-optimal but the total (training + deployment) FLOPs and dollar cost can still be lower.

That is why the LLaMA-1 paper (Touvron et al. 2023) trained the 7B model on 1T tokens (roughly 140:1, far beyond Chinchilla-optimal) — the deliberate choice was to spend more training compute per parameter so the resulting model is smaller and cheaper to serve. LLaMA-2 continued the pattern (2T tokens on 7B and 13B). LLaMA-3 pushed further (15T tokens on 8B and 70B — roughly 1900:1 and 210:1 respectively). By the LLaMA-3 report, the position had shifted to *"training-optimal is nearly irrelevant; serving-optimal is what determines the ratio."*

**The staff-plus decision.** A program-owner Staff engineer needs a single-sentence answer to *"is this program training-cost-dominated, or deployment-cost-dominated?"* and, if the latter, *"how far past Chinchilla-optimal should we go?"* The answer parametrises the whole rest of the program.

- **Training-cost-dominated.** Model will serve a bounded amount of inference; deployment lifetime is short; primary use is research or a single downstream fine-tune. Chinchilla-optimal, roughly. Pick N and D such that D ≈ 20N and 6ND = C.
- **Deployment-cost-dominated.** Model will serve a lot of inference over a long lifetime. Over-train: pick a smaller N, spend the same or more C, and take D:N ratios of 50:1 to 200:1. The training-loss delta at fixed C between Chinchilla-optimal and 100:1-over-trained is small (a few percent of the loss headroom); the inference-cost delta is a factor of two or more.

The isoflop curves are your friend here: at fixed C, loss as a function of N traces a shallow bowl. The Chinchilla-optimal point is the bottom of the bowl; **moving 2× along the bowl trades a small loss increase for a large inference-cost decrease**. Program docs should include a plot of the isoflop bowl with the chosen (N, D) marked and the loss-delta-vs-inference-delta trade shown explicitly. That is the plot the CFO and the VP of infra actually read.

## Isoflop curves and the point estimate

The mechanical procedure to pick (N, D) for a Chinchilla-adjacent bet:

1. **Fix C.** The compute budget is the exogenous input; leadership approves it (chapter 3). For a large-scale fine-tune program, C is often expressed as accelerator-hours; convert to FLOPs via the accelerator's peak FLOPs × sustained MFU × hours.
2. **Compute the Chinchilla-optimal point.** N* = sqrt(C / 120), D* = 20 · N*. The 120 comes from 6ND = C and D = 20N, giving N = sqrt(C/120). Verify: 6 · sqrt(C/120) · 20 · sqrt(C/120) = 120 · C/120 = C. Good.
3. **Consider the isoflop bowl around the point.** At fixed C, the loss L(N) has a shallow minimum near N*. Evaluate the loss at N = N*/4, N*/2, N*, 2N*, 4N* using the parametric form from Hoffmann et al. — or your own fit from small proxy runs (chapter 6) — and plot.
4. **Overlay the deployment-cost curve.** For each candidate N, estimate one year of inference cost as 2 · N · (expected inference tokens). Convert to dollars.
5. **Pick the (N, D) that minimises the sum of training cost and deployment cost**, adjusted for the program's confidence in the inference-token forecast.

Two concrete worked examples:

**Example 1 — a training-cost-dominated program with C = 10^23 FLOPs.** N* = sqrt(10^23 / 120) ≈ sqrt(8.3 × 10^20) ≈ 29B parameters. D* = 20 · 29B ≈ 580B tokens. That is the Chinchilla-optimal point.

**Example 2 — a deployment-cost-dominated program with the same C = 10^23 FLOPs, expected 100T inference tokens over one year.** Compute the isoflop bowl. Loss at N = 15B (D = 1.1T, so D:N ≈ 73:1) is roughly 1-2% worse than at N = 29B. Inference cost at N = 15B is roughly half of the cost at N = 29B (2 · 15B vs 2 · 29B FLOPs per token, times 100T tokens). If the loss delta is not the difference between passing and failing a launch gate — usually the case, since post-training recovers most of it — the 15B model wins on TCO. The program picks 15B / 1.1T.

The point of writing this out longhand is that the exec one-pager (chapter 8) reduces to *"we picked N = 15B and D = 1.1T because at our expected inference volume, the alternative (N = 29B and D = 580B) costs $X more per year in inference for a Y% loss improvement that our launch gates do not depend on."* The isoflop bowl plot goes on the same page.

## When to trust the extrapolation

Every scaling-law extrapolation is an interpolation-plus-extrapolation from small runs to the target scale. The three failure modes of the extrapolation, in order of frequency:

**1. Fitting outside the fit range.** Kaplan et al. fit up to a compute scale that in retrospect Chinchilla-optimal-shaped anyway; the "always scale N" reading was extrapolation past the range. The mitigation is to do the isoflop grid yourself in the small-scale ablation phase (chapter 6). At least three N values across at least three C values gives you enough grid to check your fit against the Hoffmann parametric form; disagreements at small scale extrapolate to disagreements at large scale.

**2. Fitting on the wrong data distribution.** The scaling laws depend on the training data distribution. A Chinchilla-optimal ratio fit on English web crawl is not necessarily the Chinchilla-optimal ratio for a code-heavy or math-heavy mixture (the Muennighoff et al. 2023 "Scaling Data-Constrained Language Models" work is the canonical source for how repeated data changes the fit). If your program's mixture is materially different from what the published fits used, budget for a mixture-specific isoflop grid.

**3. Fitting under a different post-training regime.** Chinchilla-optimality is measured at end-of-pretraining loss. If the program's downstream success depends heavily on how the model responds to instruction tuning and RLHF, the "compute-optimal" pretraining point may not be the *post-training-optimal* pretraining point. Empirically, more-tokens-per-parameter models are usually better after post-training too, but the exact ratio is not directly Hoffmann-derived. Chapter 7 revisits this.

The written extrapolation should carry an **explicit uncertainty band** — not "N = 15B, D = 1.1T" but "N = 15B (±30%), D = 1.1T (±30%), confidence anchored by the small-scale isoflop grid in the ablation phase." Leadership can price uncertainty; leadership cannot price a false point estimate.

## The scaling-law bet document

The written artifact this chapter produces — call it the **scaling-law bet document** — is a two-to-four page section of the program brief that answers, in order:

1. **What is the compute budget C, in FLOPs?** Numeric, sourced from chapter 3.
2. **Is this program training-cost-dominated or deployment-cost-dominated?** One paragraph; if deployment-dominated, give the expected inference volume.
3. **What is the Chinchilla-optimal (N*, D*) at this C?** Compute it. Show the arithmetic.
4. **What point on the isoflop bowl are we picking, and why?** N and D actually chosen; sensitivity to the inference-volume forecast.
5. **What is the confidence band on the extrapolation?** How much of the fit is Hoffmann parametric and how much is our own small-scale grid.
6. **What is the plan if the ablation-phase isoflop grid disagrees with the published fits?** The decision gate: at what disagreement magnitude does the program re-plan?

That last question — the re-plan trigger — is what turns the bet from a projection into a program. Chapter 6's ablation cadence and eval waterfall are what feed into this question month by month.

## Common failure modes of a scaling-law bet

Three failure modes that show up in newly-Staff program docs and that the review meeting will catch:

- **Bet stated with no isoflop plot.** The bet is a single (N, D) pair with no visualisation of the surrounding bowl. Leadership cannot see the trade-off and defaults to *"why not 2× smaller / larger"*. Add the plot.
- **Bet stated as Chinchilla-optimal with no inference-volume analysis.** The program is deployment-cost-dominated and picked training-optimal by default. The review will nudge to over-train. The mod-402 review discipline applies — the block-vs-nudge distinction is the same.
- **Bet stated with confidence beyond the small-scale data.** The extrapolation from 100M-parameter proxy runs to 70B-parameter main run spans three orders of magnitude and the doc claims 5% precision. The reviewers who have run large programs will not believe the precision claim and will nudge the program to widen the band. Widen it before the review — the 30% band is the honest number.

## Summary

- Every training program reduces to a single arithmetic bet over three variables: **N** (parameters), **D** (training tokens), **C** (total FLOPs), with **C ≈ 6ND**.
- Kaplan et al. (2020) framed the scaling laws; Hoffmann et al. (2022, Chinchilla) refit them and produced the **~20 tokens per parameter compute-optimal** heuristic that redirected the whole field.
- Chinchilla-optimal minimises training-time loss at fixed C. It is the right call for **training-cost-dominated** programs and the wrong call for **deployment-cost-dominated** programs; the latter over-train to a **50:1 — 200:1** D:N ratio to save serving cost (LLaMA-1/2/3 shape).
- Pick (N, D) by walking the **isoflop bowl** and overlaying the deployment-cost curve. Publish the plot. Include a **±30% confidence band** on N and D.
- The scaling-law bet is the top of the program brief. Every subsequent chapter reads it: chapter 3 (cluster-hour budget), chapter 4 (data volume implied by D), chapter 6 (small-scale isoflop grid to tighten the extrapolation).

The next chapter converts the FLOP target C into a defensible cluster-hour budget — the number leadership actually approves — via Model FLOPs Utilisation, risk-adjustment for ablations and restarts, and a side-by-side defense against the three alternate program calls (smaller model, bigger model, buy-instead-of-train).
