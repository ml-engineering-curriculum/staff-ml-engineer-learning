# The Org-Scope Offline Eval Harness Standard

## Motivation

The most common failure of a newly-authored cross-team eval program is the one where every team's eval keeps whatever shape it already had, the "standard" is a one-page wiki page that says "everyone must eval before deploying", and the review body six months later still cannot decide whether team A's launch is comparable to team B's. The wiki page was decorative.

The offline eval harness standard is what turns the mod-402 chapter 3 eval-metric-conflict diagnosis into something enforceable. It does *not* try to make every team's primary metric identical — chapter 1 said why that is impossible — but it *does* say what every team's eval run must produce alongside that primary metric so a shared review body can compare risk, catch regressions the primary metric hides, and prevent a launch that ships obviously-bad behaviour with a defensible-looking headline number.

This chapter walks the standard: the required slices, the held-out and adversarial guardrails, the metadata every eval run must attach, and the review-gate the eval must pass before a model is allowed to deploy. It pairs with [`exercises/exercise-01-org-scope-eval-harness-standard.md`](exercises/exercise-01-org-scope-eval-harness-standard.md).

## What "the standard" is as a document

The standard is a single document (typically three-to-five pages plus a checklist) that every ML team in the portfolio agrees to. Its structure:

1. **Scope of application.** Which ML systems in the portfolio the standard applies to, and any explicit exemptions.
2. **Required eval components.** The five buckets in the section below.
3. **The metadata schema.** What every eval run must record and where it must be persisted.
4. **The review-gate procedure.** Who runs the gate, on what cadence, with what accept / block / defer verdicts.
5. **The exception-and-override rule.** When a team may legitimately deviate, who signs off, and how the deviation is logged.
6. **The review-and-refresh cadence.** How often the standard itself is re-opened, by whom, against what evidence.

Section 5 is what keeps the standard from becoming decorative. A standard with no legitimate deviation path becomes lore that teams route around; a standard with a light-touch, logged deviation path stays a living contract.

## The five required eval components

Every team's offline eval run — the one that gates the pre-deploy review — must produce results in each of the five buckets below. The team picks the *specific* metric that best fits their business object; the standard picks the *bucket* that must be filled.

### 1. Primary metric on the team's business object

The team's own choice — NDCG@10 for search, recall@50 for a recommender, macro-F1 for a document classifier, exact-match for a code model, human-rated preference win-rate for a chat model. The standard does not dictate the primary; it only requires:

- The metric is defined in the eval-metadata schema (section below) with its exact computation, so a second team's engineer could reproduce it from the metadata alone.
- The metric is bootstrap-resampled or otherwise reported with a confidence interval, not as a point estimate. Point-estimate metrics with no CI are treated by the review gate as decorative.
- The comparison baseline is named — either the current production model version, or the last-known green offline number, or an explicitly-named held-out anchor.

### 2. Slice metrics on protected and business-critical subpopulations

At least three slices per primary metric, drawn from a portfolio-wide list. Common shapes:

- **Protected-attribute slices** — e.g. per language, per market, per user-cohort. Sourced from the mod-409 responsible-AI risk register when it exists.
- **Volume-critical slices** — top-decile users or accounts by revenue or engagement, whose regression would materially harm the business regardless of aggregate movement.
- **Long-tail slices** — the lowest-traffic decile of the business object, where the primary metric's aggregate can hide a collapse.

The slice list is portfolio-wide because it is where per-team drift produces cross-team surprise. If Team A ships a change that improves aggregate NDCG but tanks the Japanese-market slice, and Team B's next launch inherits the same shared upstream change and tanks their Japanese slice too, the shared review body should have caught it at Team A's gate. The slice list is the mechanism.

### 3. Held-out / regression sets that never move

A frozen held-out set the team may not augment or edit without the review body's approval. Two properties:

- **Time-frozen.** The set was drawn at a specific date, from a specific data snapshot, and is versioned. Model evaluations across quarters compare against the same set.
- **Contamination-audited.** The set is checked, at the eval-metadata level, for overlap with training data. A contaminated held-out set is worse than useless — it produces high scores that certify nothing.

At least one **golden regression set** — a small hand-curated set of specific inputs the team never wants to regress on, expanded whenever a real incident produces a new "we never want to see this again" case. The regression set is the memory of past mistakes.

### 4. Adversarial and guardrail sets

At least two of the following, drawn from the org-wide guardrail catalogue:

- **Adversarial inputs** — prompts, queries, or transactions constructed to trigger known failure modes (jailbreaks, prompt-injection payloads, adversarial-example inputs for vision or classification models). The catalogue is maintained centrally, with team-specific additions.
- **Toxicity / safety guardrails** — for any model producing user-facing text, a scored pass through a shared safety-evaluation set. Threshold set by the mod-409 responsible-AI policy, not by the individual team.
- **Regulatory / policy guardrails** — for models subject to a regulatory constraint (fair-lending, EU-AI-Act-Article-X, sector-specific), the set that demonstrates the specific compliance property.
- **Cost / latency guardrails** — for models where a quality gain against the primary metric can be trivially bought with more inference compute, the standard requires the eval run report cost-per-request and p95 latency alongside quality. Prevents "we shipped a +2% NDCG at 5× serving cost" from looking like an unambiguous win.

### 5. Drift and freshness checks against training data

Two checks every eval run must include:

- **Feature-distribution drift** vs. the model's training window — a PSI / KL / Wasserstein-style score per input feature, with the review gate flagging any score above the org-standard threshold for review-body attention. Draws on the mod-402 feature-layer contract.
- **Label-freshness check** — for supervised models, the age of the newest label in the training set and a red flag if the newest label is older than the standard's freshness bar for the model class. Prevents "we retrained on ancient data because the label pipeline broke and no one noticed".

For LLM-shaped systems that do not have a labelled training set in the classical sense, the drift check is replaced by a distributional check on the eval-set input distribution vs. production traffic. Chapter 3 walks how this interacts with LLM-as-judge policy.

## The metadata schema

Every eval run must persist a metadata record with at minimum:

- Model version and its lineage back to the registry (the mod-402 model-registry contract).
- Eval run identifier, timestamp, and eval-harness version.
- For each metric bucket above: the specific metric name, the exact set version, the result, and the CI.
- The baseline the metric is compared against, and the delta and its CI.
- The eval-run author or on-call.

A metadata schema that requires only *the numbers* and not the *set versions and baselines* is not enough. Half the reason review-gates fail is that the numbers are correct and the sets or baselines have silently changed. The schema is where the audit trail lives.

## The review-gate before deploy

The gate is a lightweight review — typically async in a channel or a weekly cadence for the review body — that emits one of four verdicts:

- **Accept.** The eval satisfies the standard; the deploy proceeds under the chapter-4 rollout contract.
- **Block.** The eval violates a required component or the review body has substantive concerns; the deploy is halted and the team returns with a revised eval.
- **Defer.** The eval is incomplete but the missing component is a known-in-progress work item; the deploy is halted, expected fix date logged.
- **Nudge.** The eval satisfies the required components but the review body flags a recommended improvement (add a slice, add an adversarial set, tighten a threshold); the deploy proceeds and the nudge lands as a follow-up.

The `accept / block / defer / nudge` shape is the same as the mod-402 chapter 6 architecture-review shape, deliberately. Teams have already learned the vocabulary; the eval gate reuses it.

## Where per-team flexibility is preserved

Two-thirds of the standard's chance of adoption depends on how carefully it names what teams *do not* have to do:

- **The primary metric stays the team's choice.** No attempt to force ranking and recommendation onto the same metric.
- **The eval-set construction stays the team's choice.** How they draw their held-out, how they source their annotations, how they iterate on the golden regression set — team territory. The standard specifies *that* the sets exist and their properties; not *how* they are built.
- **The compute budget for eval stays the team's choice.** Some teams' evals are cheap; some are expensive. The standard does not mandate a budget.
- **New guardrails need review-body approval to be added to the required list.** Preventing the standard from becoming a wish-list that grows without discipline.

The "here is what you retain" section of the standard document is what makes a team lead recommend adoption instead of resenting it. Chapter 5 of mod-402 called the migration-cost model out; the same discipline applies here.

## The artifact this chapter produces

A single **org-scope offline eval harness standard document**, three-to-five pages plus a one-page adoption checklist. This is exercise-01's deliverable. Its structure was named in the "What 'the standard' is as a document" section above. When rolled up into the mod-402 portfolio RFC's Section 9 (rollout and rollback), it becomes the two-paragraph summary of "how the eval-metric-conflict cell is fixed at the portfolio level".

## Summary

- The standard specifies **buckets, not primary metrics** — every team's eval must produce results in five buckets (primary, slices, held-out, adversarial-and-guardrail, drift-and-freshness). The bucket contents stay team-owned.
- The **metadata schema** and the **review gate** are what make the standard enforceable rather than decorative.
- The gate emits **accept / block / defer / nudge**, reusing the mod-402 architecture-review vocabulary.
- Per-team flexibility on primary metric, eval-set construction, and eval compute budget is *preserved on purpose*; the exception path is what prevents the standard from becoming lore teams route around.

The next chapter walks the LLM-as-judge org policy — where judge-model scores are allowed as a primary metric, where only as a diagnostic, and how bias in the judge is tracked and calibrated.
