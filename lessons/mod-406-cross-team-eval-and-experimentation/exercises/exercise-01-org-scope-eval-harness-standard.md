# exercise-01: Org-Scope Offline Eval Harness Standard

**Estimated effort:** 3 hours

## Objective

Author the **org-scope offline eval harness standard** for a real three-to-five-system ML portfolio, using chapter 2's five-bucket framing. The deliverable is a three-to-five-page document plus a one-page adoption checklist that every team in the portfolio would sign — the primary metric stays team-owned; the buckets around it become non-negotiable, the metadata schema becomes machinery, and the review gate becomes a body with a vocabulary. After this exercise the learner should hold the first of the four eval-program contracts and the pre-flight artifact that chapter 4's rollout contract reads on entry.

This is the first of the four artifacts mod-406 produces. Exercises 02, 03, and 04 read against it — the LLM-as-judge tier classifications from exercise-02 fold into this document's bucket 4; the review body from exercise-03 runs this document's gate; the platform contract from exercise-04 supplies the guardrail-metric machinery this document requires. Choose the portfolio you can carry through all four exercises.

## Prerequisites

- Chapter 01 — the "am I doing eval-program work?" heuristic and the drift-into-single-team-eval failure mode.
- Chapter 02 — the five required eval components (primary, slices, held-out / regression, adversarial / guardrail, drift / freshness), the metadata schema, and the accept / block / defer / nudge gate vocabulary. Read in full.
- Chapter 04 — enough to understand that this document is the pre-flight requirement the rollout contract reads. You do not need chapter 4's stage detail here; you need to know the eval-gate verdict is a launch gate.
- Recommended: skim the Kohavi/Tang/Xu chapter on OEC and guardrail metrics (see `../resources.md`), one page of the [Breck et al. ML Test Score](https://research.google/pubs/the-ml-test-score-a-rubric-for-ml-production-readiness-and-technical-debt-reduction/), and the [MLflow Model Registry stage/alias vocabulary](https://mlflow.org/docs/latest/model-registry.html) that the metadata schema will reference.
- If you carried a portfolio through mod-402: the blueprint's eval-metric-conflict cell and the specific per-team metrics named in it. That cell is the raw material for this document.

## Choose the portfolio

Pick **one** portfolio of three-to-five ML systems. Priority order:

1. If you completed mod-402, use the same portfolio. Chapter 1 was explicit that this module is the systemic fix for that blueprint's eval-metric-conflict cell; consistency across exercises is worth more than novelty.
2. Otherwise, pick a portfolio from a *real* org you can describe — current employer, past employer, or a set of well-documented public systems (e.g., a public company's product ML surface described in engineering-blog posts). Named systems produce defensible bucket assignments; hypothetical systems collapse into the chapter-1 drift failure modes.
3. Only if neither of the above applies: stipulate a five-system portfolio at the top of the doc — one ranker, one recommender, one classical classifier, one generative chat / agent, one batch-scored system — and note which of the chapter-2 buckets each system is currently missing.

Do not try to standardise across two portfolios in this exercise. One portfolio in three hours produces the artifact; two produces two shallow drafts.

## Steps

1. **State the portfolio in one paragraph.** Named: three-to-five systems, each with owning team, primary metric, current eval cadence, and one line on the most recent eval-related incident or near-miss. Chapter 2's opening was emphatic that a wiki-page standard is decorative; a standard grounded in a named portfolio is not.
2. **Draft Section 1 — Scope of application.** List every system in the portfolio the standard applies to. Name any explicit exemption (a research-only system, a system in sunset, a system on a peer platform team's roadmap). No implicit exemptions — a system not named is either in scope or out of scope, not "we'll figure it out."
3. **Draft Section 2 — Required eval components.** For each of the five chapter-2 buckets, write:
    - The bucket name and its purpose in one sentence.
    - What every team must produce for this bucket (one to three bullets — what set is used, what statistic is computed, how it is reported).
    - The concrete per-system fill for at least three of the portfolio's systems. Named metrics, named sets, named thresholds where they exist.
    - What the review gate looks for when reading the bucket.
    Chapter 2's five-bucket walk is the shape; do not invent a sixth bucket in this pass. New buckets are a chapter-2 review-body decision, not a per-exercise decision.
4. **Draft Section 3 — Metadata schema.** Enumerate the fields every eval run must persist. Chapter 2's minimum list is the floor — model version and registry lineage, eval-run identifier and timestamp, per-bucket metric name / set version / result / CI, baseline and delta with CI, author or on-call. Add fields specific to your portfolio (e.g., judge-model version for chapter-3 Tier-A judge scores, cost-per-request for cost-guardrail buckets). Name the persistence store — the model registry, a metric store, or a logging backend — and how it is indexed.
5. **Draft Section 4 — Review-gate procedure.** Name the review body (composition, quorum), the cadence (weekly / bi-weekly / on-demand), the submission format (a filled metadata record + a short pre-registered rollout plan per chapter 4), and the four verdicts — `accept`, `block`, `defer`, `nudge`. For each verdict write one to two sentences on what it means operationally and what the team must do next. This is where exercise-03's review-body charter attaches; keep this section thin enough that exercise-03 can carry the composition and cadence detail.
6. **Draft Section 5 — Exception-and-override rule.** Name the legitimate deviation path — who submits an exception, who signs off (Staff engineer + peer EM per the mod-401 partnership contract is the default), what the exception log looks like, how long an exception is valid before it expires or is re-reviewed. Chapter 2 was explicit that a standard with no exception path becomes lore teams route around.
7. **Draft Section 6 — Review-and-refresh cadence.** Name the cadence at which the standard itself is re-opened (quarterly is the chapter-6 default). Name the evidence the refresh reads — bucket-by-bucket compliance rates, exception counts, new failure modes surfaced upward per the chapter-6 upward flow, incidents where the standard would have caught the miss.
8. **Draft Section 7 — Per-team flexibility preserved.** Explicitly enumerate what the standard does *not* dictate: primary metric choice, eval-set construction methodology, eval compute budget, judge-model choice (the tier is dictated by exercise-02, but the specific vendor is not). Chapter 2 was emphatic that this section is the difference between adoption and resentment.
9. **Author the one-page adoption checklist.** A one-page appendix a team lead can walk through in ten minutes to confirm their eval is standard-compliant. Twelve to twenty checkboxes, each pointing back to a section number. This is the artifact team leads will actually use; the main document is the review-body reference.
10. **Draft the mod-402 Section-9 roll-up paragraph.** Two paragraphs, no more, for the portfolio RFC's rollout-and-rollback section — how this standard fixes the eval-metric-conflict cell of the blueprint. Chapter 1 named this as the integrating fifth artifact; it is what the mod-402 architecture-review body reads when it evaluates the eval-conflict cell.

## Deliverable

A single document, 3-5 pages, containing:

- **Section 0 — Portfolio context.** One paragraph naming the three-to-five systems, teams, primary metrics, and the most recent eval-related incident (step 1).
- **Section 1 — Scope of application.** Enumerated in-scope systems and explicit exemptions (step 2).
- **Section 2 — Required eval components.** The five buckets with per-bucket requirements, portfolio fills for at least three systems, and the review-gate check per bucket (step 3).
- **Section 3 — Metadata schema.** Field list, persistence store, indexing scheme (step 4).
- **Section 4 — Review-gate procedure.** Body, cadence, submission format, four verdicts with operational meanings (step 5).
- **Section 5 — Exception-and-override rule.** Submission, sign-off, log, expiry (step 6).
- **Section 6 — Review-and-refresh cadence.** Cadence and evidence read (step 7).
- **Section 7 — Per-team flexibility preserved.** Explicit non-dictated items (step 8).
- **Appendix A — Adoption checklist.** One page, 12-20 checkboxes, each pointing to a section number (step 9).
- **Appendix B — mod-402 Section-9 roll-up.** Two paragraphs for the portfolio RFC (step 10).

## Starter guidance

- **The primary metric stays team-owned.** Chapter 2 was emphatic. If Section 2, Bucket 1 tries to force ranking and recommendation onto the same metric, you have drifted into single-team eval work. Buckets 2-5 are where the org-wide contract does its work.
- **The metadata schema is where enforcement lives.** Chapter 2 named this as the audit trail. If the schema requires only "the numbers" and not the set versions, baselines, and lineage, half the review-gate disputes will be "we changed the set" arguments that never resolve. Include set version and baseline as mandatory fields.
- **The gate vocabulary is deliberately reused.** `accept` / `block` / `defer` / `nudge` is the mod-402 chapter-6 architecture-review vocabulary. Do not invent new verdicts — teams have already learned the four, and consistency across gates is the reason the vocabulary works.
- **The three concrete portfolio fills matter.** Section 2's per-system bucket fills are what turns the standard from a template into a contract. A section that says "each team names their slices" is a template; a section that says "the ranker's slices are per-language for top-10 languages and top-decile-by-revenue" is a contract. Do at least three systems in full; the remaining systems can be one-line pointers.
- **The exception path is what saves the standard.** Chapter 2 named the failure mode where the standard grows into lore teams route around. If your exception path requires more than one signature or takes more than a week to close, teams will not use it — they will silently ship without the missing bucket. Keep the exception path light-touch and logged.
- **Do not put the LLM-as-judge policy inline.** Bucket 4 (adversarial / guardrail) intersects the judge policy from exercise-02, but the tier classifications belong in that exercise. Reference exercise-02's document; do not duplicate it. The two documents compose; they do not merge.
- **Two-to-four pages plus one-page appendix is fine.** Do not pad to six. A tight standard that lands the buckets, the schema, the gate, and the exception path is more useful than a long one with a padded motivation.

## Acceptance criteria

- Exactly one portfolio is standardised across; the portfolio contains 3-5 named systems with named owning teams and named primary metrics.
- All five chapter-2 buckets appear in Section 2 with per-bucket requirements, at least three concrete per-system fills, and a review-gate check per bucket.
- Section 3's metadata schema names model-version-and-lineage, eval-run identifier and timestamp, per-bucket metric name / set version / result / CI, baseline and delta with CI, and author or on-call — plus any portfolio-specific field required by chapter-3 judge deployments or cost/latency guardrails.
- Section 4 names a specific review body, a specific cadence, and the four verdicts (`accept` / `block` / `defer` / `nudge`) with an operational meaning per verdict.
- Section 5 names the exception path with sign-off (Staff + peer EM by default), a log format, and an expiry.
- Section 6 names a quarterly (or explicitly justified other) refresh cadence and the evidence read.
- Section 7 explicitly names at least four items the standard does not dictate.
- The one-page adoption checklist has 12-20 checkboxes, each pointing to a numbered section.
- The two-paragraph mod-402 Section-9 roll-up is present.
- The document nowhere dictates the primary metric, the specific eval-set construction methodology, or the judge-model vendor.

## Stretch goals

- **Score the portfolio against the current draft.** For each system, mark each bucket as green (satisfied today), yellow (partially satisfied), or red (missing). The resulting 5×5 grid is the migration cost the standard imposes; if every cell is red for every system, the standard is a fantasy the org cannot land in one quarter. Chapter 5 of mod-402 named the migration-cost model; apply it here.
- **Draft the deviation-log template.** A one-page template teams fill when they file an exception under Section 5. Fields: which bucket, why the deviation, what the team is doing instead, expiry date, sign-off. This is the artifact the review body reads at the quarterly refresh in Section 6.
- **Simulate the first review-gate meeting.** Write a one-page mock agenda: three candidate deploys under review, one clean accept, one nudge, one defer or block. Include the verdict rationale each time. This rehearsal catches whether the four-verdict vocabulary actually maps cleanly onto real decisions.
- **Cross-reference to chapter 4's rollout contract.** In an appendix, name the specific fields from the metadata schema (Section 3) that the pre-flight requirements of chapter 4 will consume. Consistency is the deliverable at module scope; if the rollout contract needs a field the schema does not require, one of the two documents is wrong.
- **Take the draft to a peer for adversarial pre-review.** A Staff-plus peer or a Senior tech-lead on one of the portfolio's teams. Ask them to name the bucket they would push back on and the exception they would want on day one. Absorb the pushback; adjust the standard or the exception path. This is a rehearsal for the review-body meeting where the standard is proposed.
- **Draft the exec one-liner.** In one sentence at the top of the doc: what the standard requires, which portfolio failure mode it fixes, when it takes effect. "The standard requires all five eval buckets and the metadata schema on every deploy from Q3; it closes the eval-metric-conflict cell of the portfolio blueprint." That sentence is what the director reads; the rest of the doc justifies it.
