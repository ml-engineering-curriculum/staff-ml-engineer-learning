# exercise-01: Portfolio Demand Forecast

**Estimated effort:** 3 hours

## Objective

Author the **portfolio demand forecast** for a real ML org across all its training and inference workloads, using chapter 2's three-bucket framing (baseline / program-driven / speculative) and three-tier confidence discipline (P50 / P75 / P90). The deliverable is a workbook (spreadsheet or equivalent table set) plus a 2-4 page companion doc that a finance partner could read and reconcile against the company plan of record. After this exercise the learner should hold the first of the four artifacts this module produces — the number every downstream chapter reads from.

This is the input to exercises 02, 03, and 04. Exercise-02's capacity portfolio is bought against this forecast; exercise-03's utilisation review reads its baseline; exercise-04's TCO model rolls it up. Pick the portfolio you can carry through all four exercises.

## Prerequisites

- Chapter 01 — the "am I doing portfolio TCO work?" heuristic and the portfolio-vs-program-vs-workload boundary. You should be able to answer "why is this a Staff-scope artifact, not a program budget?" in one sentence.
- Chapter 02 — the three demand buckets, the three-tier confidence discipline, and the section shape of the companion doc. Read in full.
- Mod-403 chapter 3 — the single-program cluster-hour budget shape. This exercise *cites* program budgets in the mod-403 shape; if you have not read chapter 3, you will not recognise what to cite.
- Recommended: skim the [FinOps Foundation Framework](https://www.finops.org/framework/) *Inform* section on unit economics and the AWS / GCP / Azure pricing-calculator docs (see `../resources.md`) to ground the dollar-column arithmetic in real vendor rates. Reserved / committed-use discount ranges specifically — you will need them for the "on-demand equivalent" column.
- If you completed mod-402: use the same portfolio. Consistency across modules matters more than novelty; the portfolio blueprint's system list is a natural input to the forecast's workload class list.

## Choose the portfolio

Pick **one** portfolio you will carry through exercises 01-04. Priority order:

1. If you completed mod-402 and mod-406 with a specific portfolio, reuse it. The blueprint's workload inventory is the natural input to the forecast's baseline; consistency across artifacts is the goal at module scope.
2. Otherwise, pick a *real* ML org's portfolio you can describe from first-hand knowledge or from public engineering-blog material — 3-5 named production models plus 1-2 named training programs the org has run or plans to run. Named workloads produce defensible bucket assignments; hypothetical workloads collapse into wish-list territory.
3. Only as a last resort: stipulate a mid-sized ML org at the top of the doc — one online ranker, one recommender, one batch classifier, one online LLM chat product, one batch-embedding pipeline, plus one named pretraining program in flight and one named speculative agent product on the roadmap. Anonymise dollar figures if you use real internal numbers.

Do not try to forecast for two portfolios in this exercise. One portfolio in three hours produces a defensible artifact; two produces two shallow drafts and neither can carry through to exercise-04.

## Steps

1. **State the portfolio in one paragraph.** Named: 3-5 production inference workloads, 1-2 training programs (approved or in flight), 1-2 speculative items on the roadmap. For each, name the owning team and one line on the current capacity mode (managed API? self-hosted? reserved? on-demand?). Chapter 2's opening was emphatic that a wish-list forecast collapses without a named portfolio.
2. **Establish the baseline from prior-year actuals.** Start from the FinOps team's chargeback / showback report (or, if the org has none, from the cloud bill's per-tag breakdown, or, if neither exists, an explicit stipulation with a `<!-- needs-research -->` marker). For each production workload in the portfolio, quote the trailing-12-month spend in dollars and the trailing-12-month accelerator-hours. Chapter 2 was explicit that baseline is *retrospective* — do not re-derive from first principles.
3. **Adjust baseline for organic growth and decommissioning.** For each workload, name (a) the expected organic-growth rate over the horizon (traffic growth on chat, natural growth on batch-embedding volume), and (b) any planned decommissioning (an old ranking version sunset, a batch pipeline consolidated onto a shared substrate). Publish the adjusted baseline as a defensible number with an explicit ~10% uncertainty band.
4. **Enumerate program-driven demand.** For every named training program in flight or on the approved roadmap, quote:
   - The program's own cluster-hour total from its mod-403-shape brief.
   - The program's confidence tier (P90: approved, staffed, brief in flight; P50: on the roadmap, not started; P25: floated, no owner — and belongs in speculative).
   - The hardware generation the program targets and any transition-cost line item (dual-running, validation, MFU regression during ramp).
   Do not re-derive; do cite. If a program's brief disagrees with your assumed MFU, name the delta in the reconciliation section but do not silently correct.
5. **Enumerate program-driven demand for new inference products.** For every named new inference product launching in the horizon, quote the product-management team's public traffic estimate. If they refuse to quote a number, publish three tiers (low / mid / high) and let the P50 be the mid. Include any hardware-generation-transition line item.
6. **Enumerate speculative demand.** For every speculative item — named-but-unfunded roadmap program, reactive-capacity buffer, frontier-following investigation — write an explicit "if X then Y" clause naming the trigger and the compute footprint. Include the 5-10% reactive-capacity buffer as a named line, not as a slush multiplier.
7. **Compose the P50 / P75 / P90 totals per bucket.** For each of baseline / program-driven / speculative, compute the total under each confidence tier. Chapter 2's tier definitions:
   - **P50** — baseline as adjusted plus P90 program-driven plus reactive-capacity buffer only.
   - **P75** — baseline plus 15-20% growth plus P50-and-above program-driven plus one named speculative item.
   - **P90** — everything on the roadmap with a plausible owner plus growth-case buffer.
   Publish as a 3×3 table in the executive summary.
8. **Compose the one-year quarterly detail.** Quarter-by-quarter, workload-class by workload-class (training / batch-inference / online-inference). Use the two-column dollar / accelerator-hours format from mod-403 chapter 3. This is the artifact leadership commits against.
9. **Compose the three-year envelope.** Annual totals only, three scenarios (base / growth / headwind). No quarterly detail. This is the input the exercise-02 capacity portfolio uses to price reserved-vs-committed-vs-on-demand commit lengths.
10. **Draft the sensitivity table.** For the P50 total, quote what it changes to if:
    - Inference traffic grows 30% instead of the baseline organic-growth assumption.
    - A named hardware-generation transition (H100 → H200 or equivalent) slips two quarters.
    - A named speculative item does not launch.
    - A program's MFU misses by 10 points.
    Publish as a five-row delta table.
11. **Draft the reconciliation notes.** Where the forecast agrees and disagrees with the FinOps team's forward-looking model (if one exists), and where it agrees and disagrees with each cited program's own budget. The disagreement is the interesting part — that is where leadership needs to see your judgement calls.
12. **Author the executive summary.** Three sentences and one 3×3 table (buckets × tiers, in dollars). This is the artifact the CFO will read; the rest of the doc justifies it.

## Deliverable

A workbook (spreadsheet or table set) plus a companion document, 2-4 pages, containing:

- **Executive summary** — Three sentences and the 3×3 P50/P75/P90 × baseline/program/speculative dollar table (step 12).
- **Section 1 — Portfolio context.** Named portfolio in one paragraph (step 1).
- **Section 2 — Baseline breakdown.** Prior-year actuals, per-workload adjustment for growth and decommissioning, adjusted baseline with ~10% uncertainty band (steps 2-3).
- **Section 3 — Program-driven breakdown.** One row per program, per-program cluster-hour total and confidence tier, hardware-generation transition costs (steps 4-5).
- **Section 4 — Speculative breakdown.** One row per speculative item with "if X then Y" clause, reactive-capacity buffer with justification (step 6).
- **Section 5 — Totals.** P50/P75/P90 per bucket and totals; one-year quarterly detail table; three-year envelope with three scenarios (steps 7-9).
- **Section 6 — Sensitivity table.** Five-row delta table (step 10).
- **Section 7 — Reconciliation notes.** Agreement and disagreement with FinOps forward-looking model and with cited program briefs (step 11).

Companion workbook: one sheet per section 2/3/4 with per-workload arithmetic; one sheet with the section 5 rollup; one sheet with the sensitivity calculation. Cell references, not hardcoded numbers.

## Starter guidance

- **Baseline is retrospective; do not re-derive.** Chapter 2 was explicit. If you find yourself building baseline from throughput-per-GPU × requests-per-second × 24 × 365, you have drifted into program-budget territory. The FinOps chargeback is the source; your job is to sanity-check and adjust, not re-forecast from first principles.
- **Cite program briefs; do not overrule them.** If you think a program brief's MFU is optimistic, note the delta in section 7 and use the program's own number in section 3. Silently correcting a program's number in the portfolio forecast is the fastest way to lose the program owner's cooperation on the next quarter's refresh.
- **The three tiers are the deliverable, not one number.** The pressure to "just give me one number" will be intense. The right response is *"P50 to plan against, P75 to hold in reserve, P90 as the ceiling — your program is in P75."* If your executive summary shows a single number, you have caved on the discipline.
- **Explicit "if X then Y" clauses on speculative.** Do not roll speculative demand into program-driven "for simplicity" or hide it in headroom. The finance partner will value the honesty and the reactive-capacity buffer discipline; a forecast that pretends unplanned demand does not happen is a forecast that burns credibility in month five.
- **The hardware-generation transition budget is the item most-often forgotten.** If your portfolio includes any workload that will migrate across an NVIDIA generation boundary in the horizon (H100 → H200 → B200), budget the dual-running / validation / MFU-regression cost as a named line item under program-driven. Chapter 3 will re-use this line item in the reserved-commit analysis.
- **Two-column dollar / accelerator-hour format.** Chapter 2 was explicit that reusing the mod-403 chapter 3 format is what makes program-forecast reconciliation possible. Do not invent a new format.
- **2-4 pages plus the workbook is fine.** Do not pad to six. A tight forecast that names buckets, tiers, sensitivities, and reconciliations is more useful than a long one with padded motivation. Chapter 2's failure modes both come from over-length: "add every team's ask and multiply by 1.5" is easy to hide in a long document.

## Acceptance criteria

- Exactly one portfolio is forecast; the portfolio contains 3-5 named production inference workloads and 1-2 named training programs with owning teams.
- Section 2 quotes prior-year actuals per workload (from FinOps chargeback or explicit stipulation) plus the organic-growth-and-decommissioning adjustment with a stated uncertainty band.
- Section 3 quotes each program's own cluster-hour total from its brief and assigns a confidence tier (P25 / P50 / P75 / P90); no program's number is silently overridden.
- Section 4 lists at least two speculative items, each with an explicit "if X then Y" clause, plus the reactive-capacity buffer as a named line with a percent-of-baseline justification.
- Section 5 publishes P50 / P75 / P90 totals per bucket in a 3×3 table plus a one-year quarterly detail table (in the two-column dollar / accelerator-hour format) plus a three-year envelope table with three scenarios (base / growth / headwind).
- Section 6 sensitivity table has at least four rows covering inference-traffic shift, hardware-generation slip, speculative-item non-launch, and MFU miss.
- Section 7 reconciles to at least one cited program brief and to the FinOps forward-looking model (or names the absence of one and adds a `<!-- needs-research -->` marker).
- Executive summary is three sentences and one 3×3 dollar table; nowhere in the summary is a single point-estimate quoted without a tier.
- The workbook backs every section-5 number with visible arithmetic (cell references, not hardcoded totals).
- The forecast nowhere re-derives baseline from first-principles throughput math and nowhere overrules a cited program's MFU assumption.

## Stretch goals

- **Score the portfolio's forecast risk.** For each workload, mark green / yellow / red based on how confident you are in its 12-month spend estimate. The resulting grid is the "where the sensitivity table is likely to bite" summary. Feeds directly into exercise-04's sensitivity section.
- **Simulate the finance-partner review meeting.** Write a one-page mock agenda for the meeting where you present the forecast to the finance partner. Include the two questions you most fear and how you plan to answer them. Rehearse the "why P50 and not P70?" question; that is the one that most often collapses first-time authors.
- **Compare against a published external benchmark.** Pick one publicly reported ML compute spend (from a hyperscaler AI-infrastructure blog post or a public-company earnings-call ML disclosure — see `../resources.md`) and quote how your portfolio's per-GPU-hour and per-request unit economics compare. External anchors are what turn the forecast from an internal document into a defensible one.
- **Draft the quarterly-refresh workflow.** One page. Who runs the refresh, what changes on each refresh (only the last quarter's actuals and any changed program tier?), what the refresh does not touch (the three-year envelope; the section 6 sensitivity structure). Chapter 2 named this as a quarterly artifact; without a documented refresh workflow, the artifact goes stale.
- **Take the draft to a peer for adversarial pre-review.** A Staff-plus peer or the finance partner if available. Ask them to name the assumption they would push back on and the number they would ask to be re-cut. Absorb the pushback. This is a rehearsal for the real review.
- **Draft the exec one-liner.** In one sentence at the top of the doc: what the forecast says, what changed year-over-year, what the top risk is. "The ML org's fiscal-year P50 spend is $18M (up 22% year-over-year); the top risk is the H100 → H200 transition slipping into Q4 and adding $2M of dual-running." That sentence is what the director reads before opening the workbook.
