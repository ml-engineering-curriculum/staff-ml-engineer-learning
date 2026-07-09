# LLM-as-Judge Policy at Org Scope

## Motivation

Two years ago most ML orgs did not need this chapter. LLM-as-judge — using a strong language model to score another model's outputs, either pairwise against a baseline or on a scalar rubric — was a research curiosity. Today it is the default eval instrument for chat, agentic, and generative-content systems in most orgs the Staff engineer will operate inside, and the "default" arrived faster than the discipline for using it responsibly.

The Zheng et al. 2023 NeurIPS paper *Judging LLM-as-a-Judge with MT-Bench and Chatbot Arena* is the canonical reference; it named the technique and identified the specific bias classes — **position bias, verbosity bias, self-enhancement bias** — that reappear across every judge-model deployment. The Chatbot Arena / LMSYS platform demonstrated that pairwise human preference at scale produces a defensible ordering; a strong LLM judge tracks that ordering usefully but imperfectly. Every ML org's Staff engineer inherits that literature and the practical question: **for which of our teams' evals is a judge score allowed to gate a decision, and for which is it merely a diagnostic?**

This chapter names the org policy. It pairs with [`exercises/exercise-02-llm-as-judge-org-policy.md`](exercises/exercise-02-llm-as-judge-org-policy.md).

## What "LLM-as-judge" means here

For this chapter, LLM-as-judge covers three concrete deployment shapes:

- **Pairwise preference** — the judge model is shown two candidate outputs (baseline vs. candidate) and asked which is better on a specific rubric. Win-rate is the aggregate.
- **Scalar rubric** — the judge is asked to score a single output on a defined scale (helpfulness, correctness, safety) and the aggregate is the mean or a threshold pass-rate.
- **Reference-based grading** — the judge is asked whether the candidate output matches a reference answer, either exactly or on a semantic-equivalence rubric.

Retrieval-based eval scoring (e.g. rubric prompts that condition on a document set), specialised classifiers (toxicity, factuality) that happen to be LLM-based, and self-critique loops within a model all fall under the same policy umbrella if their output gates a decision the org signs off on. The policy is decision-scoped, not architecture-scoped.

## The three-tier allow-list

The policy names three tiers of use, and every team's proposed use of a judge score must be classified into one of them by the review body.

### Tier A — allowed as a primary metric

Judge scores may serve as the primary metric that gates a launch when *all* of the following hold:

1. **The task has been calibrated against human raters** at least once in the last quarter for the specific (model class, judge model, rubric) combination. Calibration procedure is defined in the "Calibration cadence" section below.
2. **Bias-tracking is in place** for the specific known-bias classes (chapter section below) with dashboards visible to the review body.
3. **The judge model is not a close relative of the model under test.** Self-enhancement bias — a judge scoring outputs from its own family higher — is well-documented; the policy forbids evaluating GPT-family outputs with a GPT-family judge as a primary metric, and equivalent constraints on other families. The specialist track (ai-eval-engineer) owns the "close relative" definition; the policy defers to their guidance.
4. **A human-eval anchor set exists** and is refreshed at the calibration cadence; the judge metric is reported alongside the human-eval anchor number, not in place of it.

Tier A is deliberately narrow. Most orgs' most-common LLM-as-judge deployment does not qualify for Tier A — it qualifies for Tier B.

### Tier B — allowed as a diagnostic

Judge scores may serve as a diagnostic — a signal the review gate looks at alongside a primary metric that is not itself a judge score — when the calibration and bias-tracking bars in Tier A are met but any of the following also hold:

- The (task, judge, rubric) combination has not been human-calibrated recently.
- The task is subject to a regulatory or safety constraint where a human-in-the-loop is required for the primary metric.
- The judge model may be a close relative of the model under test.
- The eval is for a system that is user-facing at a scale where a false-positive judge score would ship a real product harm.

Tier B is the modal case. Most agentic and chat systems' evals live here — the judge is a fast, cheap, high-throughput signal that catches regressions and surfaces failure modes; a human eval or a task-specific metric is what actually gates the launch.

### Tier C — forbidden

Judge scores may not be used as either primary or diagnostic when:

- **Safety-critical systems** with legal, medical, financial, or physical-harm outputs. The primary metric is human-graded against a rubric authored by the responsible-AI function (mod-409). A judge score in this tier is not merely un-trusted; it is not allowed as a diagnostic either, because a diagnostic score can drift into the review body's justification narrative.
- **Rank-order launch decisions where the delta is within known judge noise.** If two candidates score within the calibrated judge noise floor of each other, the judge cannot pick. The review body defers the decision to human eval, to a different metric, or to "no-launch, iterate".
- **Any eval where the judge model is the model under test.** No self-judging; not even as a diagnostic. The self-enhancement literature is unambiguous on this.

The policy document lists the specific systems in the portfolio that fall under Tier C by name.

## The known bias classes and how they are tracked

The policy tracks at minimum the following bias classes, drawing on the LLM-as-a-Judge literature (Zheng et al. 2023 and its follow-ons):

- **Position bias.** The judge scores whichever candidate appears first (or second) systematically higher, independent of content. Tracked by evaluating the same candidate pair in both orderings and reporting the swap-flip rate.
- **Verbosity bias.** The judge scores longer outputs higher independent of quality. Tracked by measuring judge-score correlation with output length on a set where length is uncorrelated with human-rated quality.
- **Self-enhancement bias.** The judge scores outputs from its own family higher. Tracked by evaluating cross-family baselines and reporting the family-vs-non-family score gap.
- **Rubric-drift bias.** The judge's implicit rubric drifts from the written rubric on ambiguous cases. Tracked by sampling judge rationales and having a human rater score whether the rationale aligns with the written rubric.

Each bias class has a **dashboard number** the review body looks at when accepting a Tier-A or Tier-B eval. If the dashboard shows the bias has crept above the org threshold since last calibration, the eval reverts to Tier B (or the review gate blocks the launch, in the Tier-A case).

## Calibration cadence against human raters

The policy sets the calibration cadence at (typical numbers, tuned per org):

- **Every quarter** for Tier-A judge deployments — a fresh sample of judge scores compared against a fresh sample of human-rater scores on the same items, with the correlation and the disagreement patterns reported to the review body.
- **Every two quarters** for Tier-B judge deployments — the same procedure, lighter budget.
- **On any judge-model version change** — even inside a quarter, a version bump on the judge (e.g. moving from a `-2024-06` to a `-2024-09` snapshot of the same vendor's model) requires recalibration before the judge continues to be used at Tier A.

Calibration samples are drawn from the team's held-out or golden-regression sets (chapter 2). Human raters follow an org-standard annotation protocol owned by the ai-eval-engineer specialist track. This module does not author the protocol; it requires that one exists.

## What per-team flexibility is preserved

The policy is explicit about what teams still choose:

- The **judge model itself**. Teams pick which vendor / open-weights model / self-hosted model they use as judge, subject to the Tier-A "close relative" constraint.
- The **rubric prompt**. Teams author their own rubric, subject to review by the ai-eval-engineer specialist track. The policy requires the rubric be documented and version-controlled; it does not dictate its contents.
- The **judge-eval compute budget**. Teams choose how many judge queries their eval consumes.

What the policy does *not* leave to teams: the tier classification of each use (that is the review body's), the bias-tracking cadence, and the calibration cadence.

## The judge-model-vendor-lock-in question

A Staff engineer authoring this policy will notice a second-order concern: an org whose primary evals depend on a single vendor's judge model has quietly created a critical-path dependency. If the vendor deprecates the model, changes its pricing, or degrades its behaviour, every dependent eval breaks at once.

The policy handles this by requiring, for any Tier-A judge deployment:

- **A fallback judge** — a second judge model (typically from a different family) that the eval can fall back to, calibrated against the primary judge at the calibration cadence.
- **A local-checkpoint clause** for any judge whose vendor may change behaviour without notice — either a self-hosted open-weights alternative, or a version-pinning contract with the vendor that guarantees stability for the duration of the fiscal quarter.

The vendor-lock-in question interacts with mod-405 (platform strategy) and mod-407 (TCO); mod-406 owns the eval-policy side, mod-405 the platform-strategy side, mod-407 the cost side. Chapter 5 walks the interaction with the experimentation platform contract.

## The artifact this chapter produces

A single **LLM-as-judge org policy document**, two-to-three pages, containing:

1. The three tiers and the criteria for each.
2. The list of current judge deployments per team, classified into a tier.
3. The bias classes tracked and the dashboards.
4. The calibration cadence and the last-calibration timestamps.
5. The vendor-lock-in fallback for each Tier-A deployment.
6. The exception path (as with chapter 2's standard).

This is exercise-02's deliverable and pairs with mod-409's responsible-AI portfolio risk register where a judge-scored eval intersects a Tier-C system.

## Summary

- LLM-as-judge is the default eval instrument for generative systems in most portfolios today; the risk is that it arrived faster than the discipline for using it.
- The org policy names **three tiers** — primary (allowed with calibration and bias-tracking), diagnostic (the modal case), forbidden (safety-critical, self-judging, sub-noise-floor rank decisions).
- Four bias classes are tracked with dashboards the review body watches — **position, verbosity, self-enhancement, rubric-drift**. Calibration against human raters runs quarterly for Tier A, two-quarterly for Tier B, and on every judge-model version change.
- Per-team flexibility is preserved on judge choice, rubric, and eval compute budget; tier classification and calibration cadence are policy-level, not team-level.
- Vendor-lock-in on a judge model is a real risk; Tier-A deployments require a calibrated fallback judge.

The next chapter walks the shadow / canary / progressive-rollout contract every team's deploy adheres to once the eval gate has accepted.
