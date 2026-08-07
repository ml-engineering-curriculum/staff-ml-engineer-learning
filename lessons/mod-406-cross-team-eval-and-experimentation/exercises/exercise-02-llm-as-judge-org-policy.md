# exercise-02: LLM-as-Judge Org Policy

**Estimated effort:** 3 hours

## Objective

Author the **LLM-as-judge org policy** for the same portfolio you standardised in exercise-01, using chapter 3's three-tier allow-list. The deliverable is a two-to-three-page policy document that classifies every current judge deployment into Tier A / Tier B / Tier C, names the bias classes tracked with the dashboards that read them, sets the calibration cadence against human raters, and names the vendor-lock-in fallback for every Tier-A deployment. After this exercise the learner should hold the second of the four eval-program contracts and the tier vocabulary the review body will use when a team proposes a judge score as an experiment's primary metric.

The policy composes with exercise-01: Bucket 4 (adversarial / guardrail) of the eval-harness standard references the tier classifications from this document. Do not duplicate content between the two; reference across.

## Prerequisites

- Exercise-01 complete — the standardised portfolio, and specifically the judge deployments each team currently runs (named model classes, named judge models, named rubric shapes).
- Chapter 03 — the three-tier allow-list, the four tracked bias classes (position, verbosity, self-enhancement, rubric-drift), the calibration cadence against human raters, and the vendor-lock-in fallback requirement. Read in full.
- Chapter 06 — the delegation contract to the ai-eval-engineer specialist track, particularly the "Staff sets the policy; the specialist implements it" split. This exercise authors the policy; the specialist authors the rubric prompt, the calibration harness, the bias-measurement instrumentation.
- Recommended: read Zheng et al. 2023 [*Judging LLM-as-a-Judge with MT-Bench and Chatbot Arena*](https://arxiv.org/abs/2306.05685) end-to-end (it is short), and skim [Chatbot Arena's methodology paper](https://arxiv.org/abs/2403.04132) for how large-scale human preference collection reads as an eval instrument. See `../resources.md`.
- If the portfolio has a mod-409 responsible-AI risk register or its skeleton: pull the list of systems flagged as safety-critical. Those systems land in Tier C by default and need explicit naming in this policy.

## Enumerate the portfolio's judge deployments

Before authoring, list every judge-model use in the portfolio you can identify. For each, name:

- The **evaluating team** and the **model under test** (model class, task, e.g., "chat team, response-generation LLM").
- The **judge model** in use today (vendor, specific model version, e.g., `claude-opus-4-6`, `gpt-4o-2024-11-20`, or self-hosted `qwen-2.5-72b`).
- The **rubric shape** — pairwise preference, scalar rubric, reference-based grading, or a hybrid.
- The **current use** — primary launch-gating metric, diagnostic-only, or informal / ad-hoc.
- The **last time the (model class, judge, rubric) triple was calibrated against human raters**, if known.

If the portfolio does not run any LLM-as-judge deployment today (rare for a modern portfolio), stipulate at the top of the doc: name the two-to-three most-likely near-term judge deployments the portfolio will need in the next two quarters, and author the policy against those. A policy authored against zero deployments is theatre.

## Steps

1. **Draft Section 0 — Portfolio judge inventory.** The table above, one row per (evaluating team, model under test, judge, rubric) combination. This grounds the policy in a real object; a policy authored without an inventory drifts toward abstract classes.
2. **Draft Section 1 — Scope and definitions.** One paragraph on what "LLM-as-judge" covers under this policy — pairwise preference, scalar rubric, reference-based grading, plus retrieval-based rubric scoring and specialist LLM classifiers whose output gates a decision. Chapter 3's decision-scoped framing is the shape; do not narrow to architecture. One sentence naming what the policy does *not* cover (e.g., the internal reasoning traces of a chain-of-thought model, ad-hoc developer prompting).
3. **Draft Section 2 — The three tiers.** For each of Tier A, B, C, write the definition from chapter 3 in your own words plus the entry criteria. Every criterion is a checkbox — a judge deployment either satisfies it or does not. Ambiguity in the criterion is what gives the review body a way to argue rather than decide.
4. **Draft Section 3 — Tier classifications for the portfolio inventory.** For each row in the Section-0 inventory, name the current tier and the target tier if different. Where a deployment is at Tier B today but should be at Tier A, name the specific gap (e.g., "needs calibration against human raters this quarter"). Where a deployment is at Tier B today and should stay at Tier B, name why. Where a deployment is Tier C, name the compensating primary metric (human eval, task-specific classical metric, no launch decision from this signal).
5. **Draft Section 4 — Bias classes tracked and dashboards.** Enumerate the four chapter-3 bias classes (position, verbosity, self-enhancement, rubric-drift). For each, write:
    - How the bias is measured (one-to-two sentences).
    - The dashboard field the review body reads.
    - The threshold at which the review body demotes a Tier-A deployment to Tier B or halts a launch.
    Note that the *implementation* of the bias-measurement instrumentation is ai-eval-engineer specialist work per chapter 6; this section names the numbers the specialist produces, not the code that produces them.
6. **Draft Section 5 — Calibration cadence against human raters.** Named cadence per tier — quarterly for Tier A, two-quarterly for Tier B, immediate on judge-model version change (chapter 3's defaults; deviate if the portfolio's context requires it, and justify). Named calibration procedure — sample size, source of the human-rated anchor, statistic reported (correlation, disagreement patterns, per-slice agreement). Named accountable owner per team.
7. **Draft Section 6 — Vendor-lock-in fallback.** For every Tier-A deployment in Section 3, name the fallback judge — a second judge model, typically from a different family, calibrated against the primary at the calibration cadence. Name the local-checkpoint clause where applicable (self-hosted alternative, vendor version-pinning contract). Chapter 3 named this as a critical-path resilience concern; the exercise-04 platform contract will not save you if the judge itself disappears.
8. **Draft Section 7 — Per-team flexibility preserved.** Explicit list of what teams retain — judge model choice (subject to close-relative constraint), rubric prompt content (subject to ai-eval-engineer review), judge-eval compute budget. Explicit list of what teams do *not* retain — tier classification, bias-tracking cadence, calibration cadence, close-relative rule.
9. **Draft Section 8 — Exception path.** Same shape as exercise-01's Section 5. Who submits, who signs off, what the deviation log looks like, expiry. A judge deployment operating outside policy (e.g., Tier-A use without current calibration) is a legitimate exception during a migration window; the exception path documents it rather than pretending it does not exist.
10. **Draft the mod-402 Section-9 fold-in.** One paragraph for the portfolio RFC's rollout-and-rollback section — how this policy fixes the judge-related sub-case of the eval-metric-conflict cell. Chapter 3's fold-in is thinner than exercise-01's; a single paragraph is enough.

## Deliverable

A single document, 2-3 pages, containing:

- **Section 0 — Portfolio judge inventory.** Table of judge deployments (step 1).
- **Section 1 — Scope and definitions.** One paragraph plus a one-sentence non-scope (step 2).
- **Section 2 — The three tiers.** Definitions and entry criteria (step 3).
- **Section 3 — Tier classifications.** Per-row tier assignments with current-vs-target and compensating-metric notes (step 4).
- **Section 4 — Bias classes tracked and dashboards.** Four classes with measurement, dashboard, and demotion threshold (step 5).
- **Section 5 — Calibration cadence.** Cadence per tier, procedure, accountable owner (step 6).
- **Section 6 — Vendor-lock-in fallback.** Per-Tier-A-deployment fallback and local-checkpoint clause (step 7).
- **Section 7 — Per-team flexibility preserved / not preserved.** Two lists (step 8).
- **Section 8 — Exception path.** Submission, sign-off, log, expiry (step 9).
- **Appendix — mod-402 Section-9 fold-in.** One paragraph for the portfolio RFC (step 10).

## Starter guidance

- **Tier A is deliberately narrow.** Chapter 3 was emphatic: most orgs' most-common judge deployments do not qualify. If your Section 3 classifies more than half the inventory as Tier A, either the calibration and bias-tracking are more mature than a modern org typically has, or you are being too generous. Re-read the four Tier-A criteria and check every deployment against every criterion.
- **Tier C is not "we don't use judges here."** Chapter 3 named Tier C as *forbidden* — a positive statement about a safety-critical or self-judging or sub-noise-floor case. If a system in the portfolio has no judge deployment today but is safety-critical, it belongs in Tier C to name the constraint: *no judge score may be used as a launch-gating signal on this system.* The tier is a rule, not a description.
- **The bias thresholds need numbers.** Chapter 3 named the bias classes; the exercise puts numbers on them. A dashboard field with no demotion threshold ("we watch this") gives the review body nothing to act on. A defensible starter for position bias is a swap-flip rate above 15%; verbosity-bias thresholds depend on the length distribution; self-enhancement thresholds are typically the family-vs-non-family score gap in percentage-point terms. Chapter 3's specialist track (ai-eval-engineer) sets the numbers; if you do not know them, mark `NEGOTIATE-WITH-SPECIALIST` and name the specialist.
- **The close-relative rule needs a definition.** Chapter 3 said the ai-eval-engineer specialist owns the definition and the policy defers. For this exercise, name the definition you will negotiate for (e.g., "same model family and same vendor within 12 months of release" or "any model sharing the pre-training recipe with the model under test"). Even a rough definition is better than an undefined term the review body cannot act on.
- **The fallback judge is not a hypothetical.** A Tier-A deployment whose vendor deprecates the judge tomorrow needs to keep evaluating. Name the specific fallback — not "some other model" — and name whether it has ever been calibrated against the primary. A fallback that has never been calibrated is theatre.
- **Two pages is fine.** Do not pad to four. Chapter 3's policy is a short document; long policies read as legal defense rather than as a working contract. A tight two-page policy with a real inventory is more useful than a five-page policy with abstract classes.

## Acceptance criteria

- Section 0 has a real inventory of judge deployments in the portfolio (or a stipulated near-term inventory with justification).
- Section 2 defines all three tiers with checkbox-style entry criteria — every criterion is either satisfied or not, no ambiguity.
- Every row in the Section-0 inventory has a Section-3 tier assignment. Every Tier-A row cites which of the four Tier-A criteria are satisfied. Every Tier-C row names the compensating primary metric.
- Section 4 covers all four chapter-3 bias classes (position, verbosity, self-enhancement, rubric-drift) with measurement, dashboard, and demotion threshold.
- Section 5 names a specific calibration cadence per tier, a specific procedure, and a named accountable owner. Deviations from chapter 3's defaults are justified.
- Section 6 names a fallback judge for every Tier-A deployment. Fallbacks that have never been calibrated against the primary are marked as such.
- Section 7 lists at least three items teams retain and at least three items teams do not retain.
- Section 8 has an exception path with a named signer, a log format, and an expiry.
- The policy nowhere dictates the specific judge-model vendor, the rubric prompt content, or the eval compute budget.
- Any bias threshold or close-relative definition the Staff engineer does not yet know is explicitly marked `NEGOTIATE-WITH-SPECIALIST`, not silently guessed.

## Stretch goals

- **Draft the calibration-report template.** One page. Fields the specialist fills at each quarterly calibration for a Tier-A deployment: sample size, human-rater set, per-slice correlation, disagreement patterns, bias-class deltas since last calibration, verdict (keep at Tier A / demote to Tier B / block until re-calibration). This is the artifact the specialist actually produces; the policy names *that* it exists.
- **Tier the portfolio's *proposed* judge deployments for the next two quarters.** Under chapter 5's review body vocabulary, a proposed Tier-A deployment is a launch-blocking commitment. Naming the future inventory forces the resourcing conversation (does the calibration budget cover it?) before the deployment is live.
- **Draft the vendor-deprecation runbook.** One page. If the primary Tier-A vendor deprecates their judge model on 60-day notice, what is the migration path? Who executes the fallback swap? How does the review body confirm the fallback's calibration is fresh? This is the disaster rehearsal chapter 3 gestured at but did not walk.
- **Cross-reference to mod-409.** In an appendix, name the systems in the portfolio flagged as safety-critical by the responsible-AI risk register (or the equivalent). Those systems' Tier-C classification in Section 3 should trace to a specific mod-409 flag; if the flag does not exist yet, the exercise has surfaced a gap in the responsible-AI substrate that mod-409's authoring will need to close.
- **Take the draft to the ai-eval-engineer specialist for pre-review.** If you can, hand the policy to the peer specialist track's lead. Ask them: is the tier vocabulary usable? Are the bias thresholds within reach at current instrumentation? Is the calibration cadence realistic against the annotation budget? Absorb the pushback; adjust or escalate the resourcing gap. This is a rehearsal for the delegation-contract conversation from chapter 6.
- **Draft the exec one-liner.** In one sentence at the top of the doc: what the policy allows and forbids, which class of launch decisions it protects, when it takes effect. "The policy allows LLM-as-judge as a primary metric only under quarterly calibration and bias tracking; it forbids judge scores as launch gates on safety-critical systems; it takes effect this quarter." That sentence is what the director reads.
