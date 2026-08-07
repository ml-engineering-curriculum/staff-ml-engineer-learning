# exercise-04: CFO-Readable TCO Model

**Estimated effort:** 3 hours

## Objective

Author the **CFO-readable TCO model** for the exercise-01 portfolio, rolling up exercises 01-03 into a workbook, a 5-8 page companion doc, and a one-page executive summary. The model handles the four chapter-6 arithmetic patterns correctly — training amortisation over model-family lifetime, inference marginal cost, per-request cost target twelve months out, sensitivity analysis across the four dominant axes — and closes with the one-pager that leadership actually reads. This is the artifact the mod-407 module produces; every prior exercise exists to feed it.

This is the final artifact in the mod-407 four-artifact stack and the roll-up that makes the module capstone-shaped. If you have not delivered exercises 01-03, do not skip forward — this exercise cites their outputs directly and will not stand up on its own numbers. If exercises 01-03 disagreed with each other on the workload list, the capacity mix, or the diagnostic row-by-row, resolve the disagreement before authoring this document; the TCO model is where inconsistencies become visible to leadership.

## Prerequisites

- Exercise-01, exercise-02, and exercise-03 delivered. Their outputs are the inputs.
- Chapter 06 — the four arithmetic patterns, the workbook / companion / one-pager section shape, the model-size and hardware-generation sensitivity discussion, and the Staff-vs-finance-vs-leadership boundary. Read in full.
- Chapter 05 — enough to know the operating cadence the model plugs into (weekly / monthly / quarterly / annual) and the unit-economics vocabulary the model uses.
- Chapter 01 — the three CFO questions that the model must answer.
- Mod-403 chapter 3 — the training-program cluster-hour budget that amortises into Section 5.
- Recommended: read one *Cloud FinOps* chapter on unit economics (Storment & Fuller, ch. 14-15 — see `../resources.md`) plus one vendor TCO writeup (the AWS Aurora price-performance whitepaper is a good template for how vendors defend TCO to enterprise buyers) to see how a TCO doc reads when the audience is a finance decision-maker rather than an engineer.
- Optional: mod-410 chapter (or equivalent staff-plus leadership-comms material) if you have access — the one-pager is a first-class staff-plus artifact and the mod-410 lens on exec framing helps.

## Steps

1. **Restate the three CFO questions.** In one paragraph at the top of the companion doc, quote chapter 1's three questions verbatim. The rest of the document exists to answer them.
2. **Build the assumptions register.** One page. Every material assumption in the model, one per line, with the source (program brief, MLPerf reference, prior-year actuals, vendor pricing sheet, exercise-01/02/03) and the confidence tier (P50 / P75 / P90). Chapter 6 was explicit that sensitivity depends on this register being honest. Do this section first — it disciplines every downstream number.
3. **Compose the baseline breakdown.** One page. Steady-state inference, retraining, continuous eval, feature-store and pipeline compute. Reads exercise-01 Section 2 (baseline). Publish as a table with a per-line source citation and a per-line confidence tier.
4. **Compose the program-driven breakdown.** One or two pages. One row per training program and per new inference product. Cites program briefs — do not re-derive MFU or throughput numbers. Reads exercise-01 Section 3.
5. **Compose the amortisation section (chapter 6 pattern 1).** One page. Each named training program's total cost (main run + ablation + operational overhead + shared-infra fraction) amortised over the model-family lifetime (usually 12-36 months). Cite the amortisation policy (three-year vs. five-year straight-line — this is finance-partner territory; if you have not talked to finance, name the assumed policy explicitly and mark it for reconciliation).
    - Handle model deprecation explicitly: if a base model is decommissioned early, name the un-amortised training cost that hits that quarter's line and the deprecation trigger.
    - Do not double-count: pick training-cost attribution to the program-year OR to inference-marginal-cost, not both. State which choice you made.
6. **Compose the marginal-cost table (chapter 6 pattern 2).** One page. For every production inference workload, quote:
    - Current measured marginal cost per 1000 requests (or per 1000 tokens for LLM workloads).
    - The derivation — GPU-seconds per request × (dollars per GPU-hour / 3600), plus small allocations for storage and networking.
    - The 12-month year-over-year trend (from prior TCO if available, or from FinOps trailing-12-month math).
    - The hosted-API alternative price for direct comparison (for LLM workloads, quote the closest managed-API tier's public price).
    - For LLM workloads: distinguish per-input-token and per-output-token cost (output dominates because of KV cache).
7. **Compose the per-request cost target section (chapter 6 pattern 3).** For each material workload, quote the current marginal cost, the twelve-months-out target, and the top two engineering moves that close the gap. The engineering moves tie back to exercise-03's migration doc. If a workload's target requires no engineering moves ("the current path takes us there"), name that explicitly — that is the most defensible target of all.
8. **Compose the capacity-portfolio dollar breakdown.** One page. Reads exercise-02's Section 4 percentage mix and multiplies by the exercise-01 total. Publish as a bucket-by-bucket dollar table with the effective discount rate applied and one line of exit-cost commentary per multi-year commit (from exercise-02 Section 5).
9. **Compose the sensitivity table (chapter 6 pattern 4).** One page. Four scenarios:
    - **Demand shift.** Inference traffic +30% / -30% vs. forecast.
    - **Hardware generation.** Next-generation hardware ships two quarters early / two quarters late; performance uplift at the low / high end of vendor claims.
    - **Hosted-API pricing.** Rates drop 30% / rise 30%. Relevant if managed-API is a material line item.
    - **MFU / throughput miss.** Every training program's MFU misses by 10 points; every inference workload's throughput misses by 20%. Aggregate cost impact.
    Each scenario quoted as a delta to the P50 total in dollars and as a percent. For each scenario, name the **pre-agreed re-plan trigger** (chapter 6 was explicit that this is the item most often missing).
10. **Author the model-size and hardware-generation shift paragraph.** Chapter 6 called these the two sensitivities that dominate the three-year envelope. Write one paragraph per sensitivity for the companion doc, quoting a "model shape stays constant" case and a "model shape shifts to next generation" case, and naming which is more likely and why.
11. **Author the cross-references section.** Half a page. Pointer to the FinOps chargeback report the numbers come from, pointer to each cited program brief, pointer to the peer platform team's paved-road roadmap the model assumes.
12. **Author the executive summary (the one-pager).** Chapter 6's structure exactly:
    - **The number.** Fiscal-year total in dollars, year-over-year delta as a percent. One sentence.
    - **The workload-class breakdown.** Table with three rows (training, inference self-hosted, inference hosted-API) with percent-of-total and year-over-year-delta columns.
    - **The top three levers.** Each: the lever, the dollar impact if pulled, the trade-off. Tied to exercise-03's migration doc where possible.
    - **The top three risks.** Each: the risk, the sensitivity in dollars, the pre-agreed re-plan trigger. From Section 9.
    - **The ask.** One sentence naming what leadership is being asked to approve — fiscal-year budget, reserved-capacity commit, headcount, or explicit no-ask ("this is an informational update").
    Every claim on the one-pager must trace to a numbered section of the workbook.
13. **Reconcile the totals.** Cross-check: does the sum of Section 3 (baseline) + Section 4 (program-driven) + a reasonable draw on speculative reach the exercise-01 P50 total? Does the sum of Section 8's bucket-by-bucket dollar table match Section 3 + Section 4? If not, one of the numbers is wrong. Chapter 6 was explicit — the assumptions register (Section 2) is where reconciliation happens.
14. **Draft the annual-refresh workflow.** One paragraph in an appendix. Who re-runs the model, what changes on refresh (all four arithmetic patterns re-computed against the new forecast), what stays (the sensitivity structure, the amortisation policy), when leadership reviews the refresh (the annual leadership review from chapter 5).

## Deliverable

Three artifacts, packaged together:

**Artifact 1 — Workbook (spreadsheet or equivalent table set).** One sheet per companion-doc section that has arithmetic (Sections 2-9). Every published number in the companion doc traces to a workbook cell.

**Artifact 2 — Companion document, 5-8 pages.**

- **Section 1 — Three CFO questions.** One paragraph (step 1).
- **Section 2 — Assumptions register.** One page (step 2).
- **Section 3 — Baseline breakdown.** One page (step 3).
- **Section 4 — Program-driven breakdown.** 1-2 pages (step 4).
- **Section 5 — Amortisation.** One page (step 5).
- **Section 6 — Marginal cost table.** One page (step 6).
- **Section 7 — Per-request cost target.** Half to one page (step 7).
- **Section 8 — Capacity-portfolio dollar breakdown.** One page (step 8).
- **Section 9 — Sensitivity and re-plan triggers.** One page (step 9).
- **Section 10 — Model-size and hardware-generation shifts.** One paragraph each (step 10).
- **Section 11 — Cross-references.** Half a page (step 11).
- **Appendix A — Annual-refresh workflow.** One paragraph (step 14).
- **Appendix B — Totals reconciliation notes.** Any reconciliation deltas from step 13.

**Artifact 3 — One-page executive summary.** Chapter 6's five-block structure exactly: the number, the workload-class breakdown, the top three levers, the top three risks, the ask (step 12).

## Starter guidance

- **The one-pager is the deliverable.** Chapter 6 was blunt — leadership reads the one-pager; the workbook exists to defend it. If you spend all three hours on the workbook and forty seconds on the one-pager, you have inverted the priority. Draft the one-pager third, revise it fifth, sixth, and seventh; the workbook exists to make its claims defensible.
- **Do not surface MFU arithmetic on the one-pager.** Chapter 6 was explicit — training amortisation shows as a per-program line item, not as MFU × cluster-hours × cost-per-hour. The CFO does not read MFU numbers; they read amortised program cost divided by model-family lifetime.
- **Marginal cost is not average cost.** Chapter 6 was explicit. If your Section 6 marginal-cost numbers include reserved-commit spread over requests, that is average cost, not marginal cost. Marginal is *"the cost of the next request assuming the fleet is provisioned at current traffic."* Get the definition right or the CFO's derivative reasoning ("what does this feature cost?") will be wrong.
- **Sensitivity without a re-plan trigger is a hallucination.** Chapter 6 flagged the re-plan trigger as the most-often-missing element. A sensitivity that says "if traffic grows 30% the total grows $X" and then says nothing about what happens next is not decision-making material — it is decorative arithmetic. Every scenario in Section 9 needs a specific numeric trigger and a specific action.
- **The assumptions register is the section that separates a defensible model from a wish list.** Chapter 6 was clear. If your Section 2 is a paragraph rather than a table with source-and-confidence columns, sensitivity will be un-anchored and the reviewer will not trust the totals. Do Section 2 first; every downstream number cites a Section 2 row.
- **Do not re-derive exercise-01 through -03.** Cite them. Chapter 6 was explicit — the TCO model is a *roll-up*, not a re-derivation. If your Section 3 baseline arithmetic is longer than one paragraph, you are re-deriving exercise-01 and losing the compression that makes the TCO model readable to a CFO.
- **The three-row workload-class breakdown on the one-pager is deliberate.** Training / inference-self-hosted / inference-hosted-API is chapter 6's cut. Do not split inference into "online" and "batch" on the one-pager; do that in the workbook. The one-pager's job is to make the training-vs-inference-vs-hosted-API question visible in three seconds.
- **The ask matters more than you think.** Chapter 6 named the "ask" line as the sentence leadership scans first. "Approve the $18M fiscal-year budget with the reserved-capacity commit as described" is a real ask. "This is an informational update pending Q2 refresh" is also a real ask. "TBD" is not; if the ask is not known, you are not ready to present.
- **5-8 pages plus workbook plus one-pager is the target.** Any more and leadership will not read it; any less and the finance partner will not trust it. If the companion runs long, compress amortisation and marginal-cost sections; those are the most compressible.

## Acceptance criteria

- Three artifacts delivered: workbook, 5-8 page companion doc, and a single-page executive summary.
- Companion doc has all sections 1-11 plus at least appendix A; each section within the page-budget guidance.
- Assumptions register (Section 2) has every material number tagged with a source (specifically: exercise-01 / exercise-02 / exercise-03 section reference, program brief, MLPerf submission, vendor pricing sheet, prior-year actuals) and a confidence tier.
- Amortisation section (Section 5) states the amortisation policy explicitly (three-year vs. five-year straight-line or other), names the model-family lifetime assumption per program, and states the deprecation trigger for un-amortised residual.
- Marginal-cost table (Section 6) shows per-1000-request (or per-1000-token) cost with the GPU-seconds-per-request × dollar-per-GPU-hour derivation visible; for LLM workloads, per-input-token and per-output-token are distinguished.
- Per-request cost target section (Section 7) has a 12-months-out target per material workload, and the two engineering moves that close each gap trace to exercise-03's migration doc (or explicitly name a "no engineering move required" case).
- Capacity-portfolio dollar breakdown (Section 8) reconciles by construction with the exercise-02 percentage mix and the exercise-01 total.
- Sensitivity section (Section 9) has all four scenarios (demand, hardware, hosted-API, MFU/throughput), each with a dollar delta and a percent-of-total, each with a specific numeric pre-agreed re-plan trigger.
- Sections 3 + 4 (+ speculative draw) reconcile with the exercise-01 P50 total; any delta is explained in Appendix B.
- Executive summary follows chapter 6's five-block structure exactly (number, workload-class breakdown, top three levers, top three risks, ask); every claim traces to a numbered workbook section.
- The one-pager's "ask" line is specific — a named budget, a named commit, a named headcount, or an explicit "informational update, no ask this cycle."
- The one-pager nowhere surfaces MFU numbers, cluster-hour arithmetic, or per-GPU throughput.
- The workbook backs every published number with a visible cell reference; nowhere are the totals hardcoded independent of the arithmetic sheets.

## Stretch goals

- **Run the model against the P75 and P90 tiers.** Chapter 6's default authoring is at P50; the P75 and P90 versions show what happens if the forecast slides up. Publish a mini-table comparing the three totals side-by-side; the delta is the "how much upside insurance is in this budget" number leadership often wants to see.
- **Draft the CFO Q&A one-pager.** In an appendix, the five questions the finance partner tells you the CFO will ask, and the two-line answer to each. Rehearsal for the real review; often surfaces holes in the assumptions register.
- **Simulate the leadership-review meeting.** One-page mock agenda. 15 minutes on the one-pager, 30 minutes on the sensitivity section (where the debate happens), 15 minutes on the ask. Rehearses the meeting the one-pager exists to run.
- **Compare against a vendor-authored TCO template.** Take an AWS or GCP customer-TCO template (the AWS TCO calculator or a vendor-published Aurora / EKS TCO whitepaper — see `../resources.md`) and note where your model's structure diverges. Vendor templates over-emphasise headline savings and under-emphasise risk; your model should be the opposite. The comparison is a check that your one-pager reads as a defensible planning artifact rather than a marketing pitch.
- **Draft the mod-410 workshop entry.** One-page description of what a mod-410 workshop on the one-pager would look like — what makes a Staff engineer's one-pager land vs. get sent back, what the two most-common failure modes are (too much MFU, no specific ask), how the one-pager fits the exec-communication pattern. Feeds forward into the mod-410 chapter on staff-plus communication.
- **Take the one-pager to the finance partner (or a Staff-plus peer) for pre-review.** Ask them to name the sentence they would push back on and the number they would want re-cut. Absorb the pushback. This is the single highest-signal way to catch a one-pager that reads as engineer-writing rather than as leadership-reading.
- **Compose the project-401 hand-off.** If you are on the capstone track, write the two-paragraph pointer that lands this TCO model into the project-401 blueprint alongside the mod-402 portfolio blueprint. Chapter 6 named that composition explicitly; the pointer is what makes the capstone coherent.
