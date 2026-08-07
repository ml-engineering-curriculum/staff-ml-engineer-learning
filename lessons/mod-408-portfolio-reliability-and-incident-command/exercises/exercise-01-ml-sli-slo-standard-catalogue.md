# exercise-01: ML SLI/SLO Standard Catalogue

**Estimated effort:** 3 hours

## Objective

Author the **portfolio-scope ML SLI/SLO catalogue** for a real three-to-five-system ML portfolio, using chapter 2's five-family framing (prediction-quality drift, retraining SLA, feature freshness, calibration / quality proxy, judge-model agreement). The deliverable is a two-to-four-page catalogue document plus a per-system commitment sheet — the artifact every team's model registry entry will reference and the artifact chapter 3's severity matrix, chapter 4's incident-command decision tree, and chapter 5's postmortem action-item categories all read against. After this exercise the learner should hold the anchor artifact of the module — the number every downstream chapter reads from.

This is the first of the four artifacts mod-408 produces. Exercises 02, 03, and 04 read against it — exercise-02's severity matrix maps SLI breaches to severity tiers; exercise-03's tabletop drill uses catalogue breaches as the incident triggers; exercise-04's postmortem template's `catalogue change` action-item category writes back into this document. Pick the portfolio you can carry through all four exercises.

## Prerequisites

- Chapter 01 — the four ML failure classes (bad predictions, silent drift, feature-pipeline breakage, retraining-loop stall) and the "am I doing portfolio-reliability work?" heuristic. You should be able to answer "why is this an SLI that belongs at portfolio scope, not at single-team scope or platform-team scope?" in one sentence per family.
- Chapter 02 — the five SLI families, the SLI/SLO/SLA vocabulary specialised to ML, the error-budget policy shape, and the three SLO-setting failure modes (aspirational, unmeasurable, single-team-shaped). Read in full.
- SRE Book chapter 4 (Service Level Objectives) and Site Reliability Workbook chapter 2 (Implementing SLOs) plus chapter 5 (Alerting on SLOs). These are the load-bearing prerequisite reading the chapter cites; do not skip them.
- Recommended: skim the [Breck et al. ML Test Score](https://research.google/pubs/pub46555/) paper (Sections 3-5 on monitoring), the [Feast feature freshness documentation](https://docs.feast.dev/), and one drift-detection library's SLI vocabulary — [Alibi Detect](https://docs.seldon.io/projects/alibi-detect/en/stable/) or [Evidently AI](https://docs.evidentlyai.com/) — so your catalogue's vocabulary lines up with the tooling teams already use.
- If you carried a portfolio through mod-402 and mod-406: reuse it. The blueprint's system inventory is the natural input to this catalogue's per-system commitments; consistency across artifacts is worth more than novelty.

## Choose the portfolio

Pick **one** portfolio of three-to-five ML systems. Priority order:

1. If you completed mod-402, use the same portfolio. Chapter 1 was explicit that this module builds on that blueprint; consistency across exercises is worth more than novelty.
2. Otherwise, pick a portfolio from a *real* org you can describe — current employer, past employer, or a set of well-documented public systems from engineering blogs. Named systems produce defensible per-family commitments; hypothetical systems collapse into the chapter-2 aspirational-SLO failure mode.
3. Only if neither of the above applies: stipulate a five-system portfolio at the top of the doc — one online ranker, one recommender, one classical fraud classifier, one online LLM chat product, one batch-scored propensity model — and note which of the five families each system is exposed to and which it is legitimately exempt from.

Do not try to standardise across two portfolios. One portfolio in three hours produces the artifact; two produces two shallow drafts and neither can carry through to exercise-04.

## Steps

1. **State the portfolio in one paragraph.** Named: three-to-five systems, each with owning team, primary business metric, current freshness cadence (online / near-real-time / batch), and one line on the most recent ML-specific incident or near-miss on that system. Chapter 2's opening was emphatic that a wiki-page catalogue is decorative; a catalogue grounded in a named portfolio is not.
2. **Enumerate the family × system exposure grid.** A 5×N table (five families × N systems). For each cell, mark whether the system is *exposed* to the family (needs an SLI), *not exposed* (legitimately exempt), or *needs discussion*. A rules-based fraud rule does not "retrain" — mark exempt with a one-line rationale. A batch-scored propensity model has weak feature-freshness exposure — mark exposed with a longer SLO window. Chapter 2 was explicit that the exemptions must be surfaced, not silent.
3. **Draft Section 1 — Family definitions.** For each of the five families, write:
   - The family name and the failure class(es) from chapter 1 it owns, in one sentence.
   - The canonical SLI definition — the concrete measurement, in chapter 2's vocabulary. Prefer library or vendor terminology (PSI, KL, KS, ECE, Brier, NDCG@k, MRR, judge-agreement rate) over invented terms so teams recognise it.
   - The SLO formulation shape — is this bounded above (drift), bounded below (calibration), or a fresh-by target (retraining, feature freshness).
   - The error-budget policy shape — what happens when the budget is spent (yellow → freeze experiments; red → rollback candidate; two-consecutive-window rule for calibration).
   Do not invent a sixth family in this pass. New families are a review-body decision, not a per-exercise decision.
4. **Draft Section 2 — Per-system commitments.** For each system in the portfolio, one page or less containing:
   - The chosen SLI for each exposed family, with the vendor / library that computes it.
   - The SLO band for each SLI (numeric, defensible, hittable 95%+ of the time — chapter 2's aspirational-SLO failure mode is what this defends against).
   - The error-budget policy per SLI, especially the yellow / red thresholds that chapter 3's severity matrix will read.
   - The alert routing per SLI (page / ticket / log — anticipating exercise-02).
   - The exemptions from step 2 with rationale.
   Chapter 2 was explicit: the SLO belongs to the (feature, consumer) pair, not to the feature alone; if a shared feature has three consumers with different tolerances, each consumer's row states its own SLO on the same underlying SLI.
5. **Draft Section 3 — Shared-feature freshness map.** A separate half-page focused on shared features (features consumed by two or more models in the portfolio). For each shared feature, name the feature, the pipeline owner, the consumers, and the *tightest* consumer's SLO — because that is the SLO the feature owner is on the hook to hit. Chapter 2 named this as the load-bearing shift from single-team SRE.
6. **Draft Section 4 — Review cadence and change control.** Name who owns the catalogue, who approves changes, and the cadence at which it is re-visited (twice a year is the chapter-2 default). Name the evidence a refresh reads — SLI compliance rates over the window, exemption requests, postmortem action items in the `catalogue change` category from exercise-04.
7. **Draft Section 5 — What the catalogue does not standardise.** Explicit: primary business metric, model-family choice, offline-eval methodology, canary duration (that is chapter 6 / mod-406), retraining cadence for a compliant system. Chapter 2's per-team flexibility discipline is what makes the catalogue adoptable; naming the boundaries loudly is what makes teams believe it.
8. **Score the current portfolio against the catalogue.** A 5×N grid — for each (family, system) cell, mark green (has an SLI compliant with the catalogue today), yellow (has an SLI but does not meet the catalogue's shape), red (missing entirely), or exempt. This grid is the migration cost the catalogue imposes; the yellow and red cells feed exercise-04's action-item categories.
9. **Author the one-page adoption checklist.** A one-page appendix a team lead can walk through in ten minutes to confirm their system is catalogue-compliant. Twelve to twenty checkboxes, each pointing back to a section number and a family. This is the artifact team leads will actually use; the main document is the review-body reference.

## Deliverable

A single document, 2-4 pages, containing:

- **Section 0 — Portfolio context.** One paragraph naming the three-to-five systems and one line on the most recent ML-specific incident per system (step 1).
- **Section 1 — Family definitions.** Five families with canonical SLI, SLO formulation shape, and error-budget policy shape (step 3).
- **Section 2 — Per-system commitments.** One page or less per system with SLI choice, SLO band, error-budget policy, alert routing, and exemptions (step 4).
- **Section 3 — Shared-feature freshness map.** Half-page table of shared features, owners, consumers, tightest-consumer SLO (step 5).
- **Section 4 — Review cadence and change control.** Owner, approval path, refresh cadence, evidence read at refresh (step 6).
- **Section 5 — What the catalogue does not standardise.** Explicit non-dictated items (step 7).
- **Appendix A — Compliance grid.** 5×N green / yellow / red / exempt grid (step 8).
- **Appendix B — Adoption checklist.** One page, 12-20 checkboxes, each pointing to a section and family (step 9).

## Starter guidance

- **The SLO number is where most catalogues fail.** Chapter 2's three failure modes — aspirational, unmeasurable, single-team-shaped — are what to defend against in step 4. If your rolling-window SLO breaches every second week in the current architecture, you have written an aspirational SLO; walk it back to a number the team can hit 95%+ of the time and file the tighter number as a *goal* separately.
- **Prefer one SLI per family per system, not two.** Chapter 2 was explicit that PSI-and-KS on the same score distribution is two SLIs measuring the same thing; the alert routing gets ambiguous and the on-call trusts neither. Pick one, hold it, defend the choice in one sentence.
- **The shared-feature freshness section is the highest-value part of the catalogue.** A single well-scoped shared-feature-freshness contract prevents the shape of incident chapter 4 opens with — three teams paged for three different symptoms of one shared-pipeline breakage. If Section 3 is empty because "we don't have shared features yet", either the portfolio is one team's or the shared features are hidden inside the platform team; either way, name it.
- **Every exemption has a rationale, not a blank field.** Chapter 6's exemption pattern applies here — `exemption: retraining SLA — rules-based fraud rule, no training loop` is a legitimate entry; a blank cell is not. Exempt cells are re-visited at the Section 4 refresh; blank cells rot.
- **The compliance grid is where the migration cost is real.** If your grid is 80% red on day one, the catalogue is not landable in one quarter — pick the family with the highest incident-history weight and land it first (feature freshness is usually the right first landing). Do not publish the catalogue as day-one-mandatory if the grid says the org cannot comply.
- **Do not put the severity matrix inline.** The catalogue names the SLI, SLO, and error-budget shape. The severity matrix in exercise-02 reads *off* the catalogue's yellow / red thresholds and maps them to Sev-0 through Sev-3. Keep the two documents composable, not merged.
- **Two-to-four pages plus appendices is fine.** Do not pad. A tight catalogue that lands the five families, the per-system commitments, and the shared-feature contract is more useful than a long one with padded motivation. If the catalogue exceeds four pages, either you have too many SLIs per family or you have collapsed too much per-system nuance into the family definitions.

## Acceptance criteria

- Exactly one portfolio is standardised across; the portfolio contains 3-5 named systems with named owning teams.
- The family × system exposure grid (step 2) is present with an exemption rationale in every non-exposed cell.
- Section 1 covers all five chapter-2 families, each with canonical SLI, SLO formulation shape, and error-budget policy shape.
- Section 2 supplies per-system commitments — SLI choice, SLO band, error-budget policy (with yellow and red thresholds), alert routing — for every in-scope system.
- Section 3 names at least one shared feature with its owner, consumers, and tightest-consumer SLO (or explicitly declares the portfolio has none and defends the claim).
- Section 4 names a specific catalogue owner, a specific approval path, and a semi-annual (or explicitly justified other) refresh cadence with the evidence read.
- Section 5 explicitly names at least three items the catalogue does not standardise.
- Appendix A compliance grid is present, green / yellow / red / exempt, one cell per (family, system).
- Appendix B adoption checklist has 12-20 checkboxes, each pointing to a section number and family.
- No SLI in the catalogue is aspirational (breaches every second window in the current architecture), unmeasurable (has no serving-substrate computation path), or single-team-shaped without a per-consumer-row treatment in Section 2.

## Stretch goals

- **Draft the exemption-request template.** A one-page template a team fills when they want a new exemption under step 2. Fields: which family, why the exemption, what compensating control exists, expiry date, sign-off (Staff engineer + peer EM per mod-401 chapter 5). This is the artifact the Section 4 refresh reads.
- **Simulate the first review-body meeting.** Write a one-page mock agenda for the meeting where the catalogue is proposed. Include the two family-definition changes you most fear pushback on and how you plan to answer. Rehearsing the "why is calibration SLI two consecutive windows and not one?" question is the one that most often collapses first-time authors.
- **Compare against a published external catalogue.** Pick one publicly documented ML monitoring stack (Uber Michelangelo, Netflix Metaflow, Meta's FBLearner, or a well-documented vendor blog post — see `../resources.md`) and quote which of your five families they surface and which they do not. External anchors are what turn the catalogue from an internal document into a defensible one.
- **Draft the platform-team ask.** In an appendix, name the specific substrate work the platform / MLOps team must ship for the catalogue to be enforceable — a drift-detection service, a feature-freshness metric emitter, a calibration backfill pipeline, a judge-model runner. Chapter 2 was explicit that the SLI must be computable on the serving substrate the platform team ships; if the catalogue requires a substrate that does not exist, the ask must be named so mod-405's platform-team conversation can carry it.
- **Take the draft to a peer for adversarial pre-review.** A Staff-plus peer or a Senior tech-lead on one of the portfolio's teams. Ask them to name the family they would push back on and the numeric SLO band they would ask you to walk back. Absorb the pushback. This is a rehearsal for the review-body meeting where the catalogue is proposed.
- **Draft the exec one-liner.** In one sentence at the top of the doc: what the catalogue requires, which portfolio failure mode it fixes, when it takes effect. "The catalogue requires prediction-drift, retraining-SLA, feature-freshness, and calibration SLIs on every production model by Q4; it fixes the silent-drift class of incident that classical SRE monitoring misses entirely." That sentence is what the director reads; the rest of the doc justifies it.
