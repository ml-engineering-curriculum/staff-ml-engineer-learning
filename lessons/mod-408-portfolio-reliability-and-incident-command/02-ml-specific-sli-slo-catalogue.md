# The ML-Specific SLI/SLO Catalogue

## Motivation

Chapter 1 named the four ML failure classes and said the Staff engineer's contribution is the *standard* every team's model ships against. This chapter is that standard. It walks the SLI catalogue you will require of every system in the portfolio, defends why each SLI belongs there, and shows how to derive an SLO band from it that leadership will accept and teams can actually hit.

Two motivations for making this a catalogue rather than a per-team choice. First, when three ML teams each pick their own drift metric on their own cadence, there is no way to compare or aggregate; the recommender's PSI and fraud's KS statistic are technically fine and jointly useless. Second, when a shared feature pipeline breaks, it takes down all three consumers at once; you need the *same* SLI on all three so the pattern is visible in one dashboard, not three. Catalogue standardisation is where the portfolio staff engineer earns the coordination cost.

This chapter pairs with **exercise-01 — ML SLI/SLO Standard Catalogue**. Do the catalogue authoring in the exercise; use this chapter as the reference and the argument-defence for the catalogue you produce.

## Vocabulary check: SLI, SLO, SLA

Straight from Google's *Site Reliability Engineering* and specialised to ML:

- **SLI (Service-Level Indicator).** A measurement — the fraction of predictions passing a calibration check on a rolling window, the age of the currently-serving model, the freshness of a required feature.
- **SLO (Service-Level Objective).** A target for the SLI — "≥ 99% of predictions pass calibration on a rolling 24h window", "serving model age ≤ 72h", "≥ 99.5% of feature reads on `user_7d_purchase_count` are fresher than 15 minutes".
- **SLA (Service-Level Agreement).** A contractual promise to an *external* consumer with a consequence if missed — usually a customer refund or a regulator-facing filing. Most ML portfolios have zero *external* SLAs; do not confuse the internal SLO with an SLA in RFCs.
- **Error budget.** `1 - SLO`. If the SLO is 99% and the SLI drops to 98.5% on the rolling window, the model has consumed 1.5 / 1.0 = 150% of its budget for the window. The error-budget policy — what happens when the budget is spent — is the load-bearing bit; the SLO number alone is decorative.

Chapter 5 of the *Site Reliability Workbook* is the reference for the error-budget policy pattern this catalogue reads against.

## The five SLI families for the portfolio

The catalogue has five families. Each family maps back to a failure class from chapter 1 and to a family of tooling the platform team ships. Each system in the portfolio commits to at least one SLI in each family it is exposed to (a batch-scored model may not have a "prediction-quality drift" SLI in the online sense; a rules-based fraud rule may not have a "retraining SLA"; call those out explicitly in the exercise-01 deliverable).

### Family 1 — Prediction-quality drift

**Owns failure classes:** bad predictions, silent drift.

**The SLI.** A measurement of how far the currently-serving model's *prediction distribution* has moved from the training or reference distribution. Common concrete SLIs: **Population Stability Index (PSI)** on the score distribution, **KL divergence** between serving and training score histograms, **Kolmogorov-Smirnov** statistic on the score CDF. Pick one per system and hold it; do not accept two SLIs measuring nominally the same thing.

**The SLO.** Formulated as a bound — "PSI ≤ 0.1 on the daily rolling window" is a plausible starting point; 0.1 is the widely-cited "small drift" threshold in the credit-scoring literature (Karakoulas et al., "Population Stability Index and its usefulness", is the frequently-cited but hard-to-primary-source reference; the 0.1 / 0.25 thresholds show up in most vendor documentation and predate that reference).

<!-- needs-research: locate the primary academic citation for the PSI 0.1 / 0.25 thresholds; the number circulates without a clean primary. -->

**The error-budget policy.** When PSI ≥ 0.1, the model enters "yellow" — no new experiments, retraining accelerated. When PSI ≥ 0.25, "red" — the model is a candidate for rollback to a prior checkpoint and the launch is frozen. Chapter 3's severity matrix reads directly off these thresholds.

### Family 2 — Retraining SLA

**Owns failure classes:** retraining-loop stall.

**The SLI.** **The age of the currently-serving model** — measured as wall-clock time since the training-data snapshot the serving model was fit on. Also acceptable: **time since the last successful retraining pipeline run that produced a promotable candidate** (weaker but easier to instrument).

**The SLO.** A per-system freshness target: "serving model age ≤ 72 hours" for a fraud model; "≤ 7 days" for a ranking model; "≤ 30 days" for a slower-moving recommender. The right band is the SLI conversation with the team, not a portfolio-uniform number; the portfolio *standard* is that the SLI must exist and an SLO must be committed.

**The error-budget policy.** When the SLI exceeds the SLO for more than one window, the retraining pipeline is on-call incident. When it exceeds by 2×, an exec is notified. The distinction is essential — a two-day-late retrain is a ticket; a two-week-late retrain is a portfolio-wide story.

### Family 3 — Feature freshness

**Owns failure classes:** feature-pipeline breakage.

**The SLI.** The **staleness distribution of feature reads at serving time** — the p50 and p99 age of each required feature when read from the online feature store. The Feast documentation and Tecton feature-monitoring documentation define this SLI in essentially the same way; use their vocabulary rather than inventing new terms.

**The SLO.** A per-feature target: p99 age ≤ 15 minutes for near-real-time features; p99 age ≤ 24 hours for batch features. The SLO belongs to the *feature*, not to the *model*, because a shared feature has multiple downstream consumers; this is where the portfolio-scope discipline earns its coordination.

**The error-budget policy.** Feature-freshness SLO breaches page the *feature owner*, not the model owner. This is the load-bearing shift from single-team SRE. In a portfolio with three consumers of a shared user-embedding feature, if the embedding pipeline breaks, three models degrade — but only one team gets paged (the feature owner). Everyone else gets a courtesy notification and shifts to their model-side degradation-mode logic.

### Family 4 — Calibration and quality proxy

**Owns failure classes:** bad predictions, silent drift.

**The SLI.** For probabilistic models — **expected calibration error (ECE)** or **Brier score** on a rolling window with delayed ground-truth labels. For rank/order models — **NDCG@k** or **MRR** on the rolling backfill. For LLM outputs — **judge-model agreement rate** with a held-out golden set (see mod-406 for the judge-model policy).

**The SLO.** A degradation-bound: "ECE ≤ 1.5× the offline ECE at last training" or "NDCG@10 ≥ 90% of the offline number for two consecutive rolling windows". Absolute-number SLOs on calibration are a trap — the SLO you want is *how much has it decayed from the last known-good state*, not the absolute number.

**The error-budget policy.** Two consecutive windows below the degradation bound is a portfolio Sev-2 (chapter 3). A single window is not — labels are noisy and single-window flapping loses trust in the alert.

### Family 5 — Judge-model agreement (LLM-specific)

**Owns failure classes:** bad predictions, silent drift.

**The SLI.** For LLM-serving systems where classical offline metrics don't apply — **agreement rate between the serving model's outputs and a judge-model's evaluation on a fixed golden set**, rolling over a window.

**The SLO.** "Judge agreement rate ≥ 95% on the golden set, rolling weekly". Golden set is versioned; when it changes, the SLO resets and a two-window burn-in applies. Mod-406 chapter 3 (LLM-as-judge policy) is the deeper reference.

**The error-budget policy.** Judge-model agreement is *diagnostic*, not primary, per the mod-406 policy — a single window below SLO is a rolling ticket, not a page. A prolonged degradation is a Sev-3 that either promotes to Sev-2 with a labelled evaluation or downgrades if the judge itself is at fault.

**Bias-in-the-judge caveat.** The judge model is itself a model with its own drift. The portfolio SLI must include a **judge-agreement audit** against a small human-labelled slice each quarter; if the judge disagrees with humans, the whole family's SLO is not credible until the audit is re-established. This is the load-bearing reason judge agreement is only ever a *diagnostic* SLI.

## What the catalogue is as a portfolio artifact

The **portfolio SLI/SLO catalogue** produced by exercise-01 is a single document — typically a page of catalogue tables plus 3-6 pages of per-system commitments — that:

1. **Names the five SLI families** with their canonical definitions, in the vocabulary above.
2. **Requires each system in the portfolio** to expose at least one SLI per family it is exposed to, or explicitly exempt with a rationale.
3. **Names the SLO band** per system per SLI and the error-budget policy per SLI.
4. **Names the alert routing** — which SLI breaches page, which ticket, which merely dashboard.
5. **Names the review cadence** — when the catalogue is re-visited, who edits, who approves. Typically twice a year.

The catalogue is the input to chapter 3's severity matrix (which SLI breaches map to which severity), chapter 4's incident-command decision tree (which SLI breach class triggers which command shape), and chapter 5's postmortem action-item categories (which action items map to catalogue changes vs. per-team fixes).

## Setting the SLO — the three failure modes to avoid

Setting the SLO number is where most portfolio catalogues fail. Three failure modes to watch:

**1. The aspirational SLO.** A team sets ECE ≤ 1.05× last-training as their calibration SLO because it sounds tight. In practice their rolling window frequently breaches; the alert gets muted; the SLO is decorative. **The correct SLO is one the team can defensibly hit 95%+ of the time in the current architecture, with headroom for the improvements they know they are shipping.** The Site Reliability Workbook chapter on SLO engineering is unambiguous on this — an SLO that always breaches is not an SLO. If the team needs a tighter SLO for regulatory reasons, that is a *goal*, not an SLO — track it separately.

**2. The unmeasurable SLO.** "Predictions are correct" is not an SLO. There is no SLI you can compute in the moment; ground-truth labels arrive on a delay you cannot control. The catalogue must require that every SLI is computable *on the serving substrate the platform team ships* — no offline-only, no research-time-only.

**3. The single-team-shaped SLO.** Fraud sets a feature-freshness SLO on `user_embedding_v3` at p99 ≤ 15 minutes. Fraud's model degrades fast if the embedding is stale. Recommender consumes the same feature and needs p99 ≤ 24 hours. If the catalogue makes 15 minutes the *portfolio* SLO, recommender pays the platform-team cost of a 15-minute pipeline they don't need; if it makes 24 hours the portfolio SLO, fraud loses their alert. **The SLO belongs to the (feature, consumer) pair, not to the feature alone.** Chapter 3's severity matrix handles the same nuance.

## Summary

- The **SLI/SLO catalogue** is the portfolio-standard the Staff ML engineer authors; every system in the portfolio commits its SLIs into it.
- The catalogue has **five families**: prediction-quality drift, retraining SLA, feature freshness, calibration/quality proxy, and (for LLM systems) judge-model agreement. Each maps back to one of the four ML failure classes from chapter 1.
- **SLI vs. SLO vs. SLA vocabulary is from the Site Reliability Engineering book**; specialised here to ML. Error-budget policy is where the SLO becomes load-bearing.
- **Failure modes when setting SLOs**: aspirational (always breaches), unmeasurable (no serving-time SLI), single-team-shaped (ignores multi-consumer reality). The catalogue must catch all three.
- The catalogue feeds chapter 3 (severity), chapter 4 (incident command), chapter 5 (postmortem action items), and chapter 6 (retraining-as-deploy contract). It is the anchor artifact of the module.

The next chapter turns SLI breaches into severity — the page-vs-ticket-vs-notify rules, the exec-notification thresholds, and the special treatment silent failures need in a matrix originally shaped for user-visible incidents.
