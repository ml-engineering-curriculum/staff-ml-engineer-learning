# exercise-03: Utilisation Review and Migration Doc

**Estimated effort:** 3 hours

## Objective

Author the **quarterly cost / throughput / utilisation review diagnostic** for the exercise-01 portfolio and — from the diagnostic — write **one full migration doc** for the highest-value red-flag candidate the review surfaces. The diagnostic is a per-workload table that surfaces migration candidates; the migration doc defends a specific move against alternatives with an implementation plan and a rollback. Together they are the artifacts that turn the mod-407 chapter 4 review body from a meeting into a decision-making forum.

This is section 3 of the mod-407 four-artifact stack. The diagnostic reads exercise-01 (baseline and program-driven workloads), exercise-02 (which capacity bucket each workload sits in), and hands its migration-doc dollar deltas forward to exercise-04's amortisation and marginal-cost sections. The migration doc is the concrete artifact the reader carries forward as the exemplar of a move; every organisation the reader joins will need this shape re-authored quarterly.

## Prerequisites

- Exercise-01 delivered — the workload list, per-workload spend, and forecast tiers are inputs.
- Exercise-02 delivered — the capacity-bucket assignment per workload is an input.
- Chapter 04 — the diagnostic table shape (owning team / capacity mode / spend / throughput / utilisation / flag), the four migration types (managed-API ↔ self-hosted, GPU-generation, cross-cloud, consolidation), the migration doc's nine-section shape, the review cadence and escalation gates. Read in full.
- Chapter 03 — enough to know each capacity bucket's cost characteristics; the migration doc's alternatives section will cite these.
- Mod-402 chapter 5 — the multi-team RFC template the migration doc adapts. If you completed mod-402 exercise-03, reuse the section shape as a skeleton.
- Recommended: skim the [PagedAttention / vLLM paper](https://arxiv.org/abs/2309.06180) if any candidate is a managed-API ↔ self-hosted LLM migration, and one hyperscaler engineering-blog throughput post that grounds the MLPerf-reference numbers your diagnostic reads against (see `../resources.md`).
- Have real numbers ready: at minimum, per-workload spend from FinOps (or a defensible stipulation), measured or stated throughput per workload, and the MLPerf-derived reference throughput for the workload class. If you cannot source any of these three, the diagnostic will not stand up in review.

## Steps

### Part A — The diagnostic

1. **Compose the diagnostic table.** One row per workload in the exercise-01 portfolio (both production inference and any training-program-in-flight). Columns:
    - **Owning team.** Named person or team.
    - **Capacity mode.** Reserved / committed / on-demand / spot / self-hosted / hosted-API (from exercise-02).
    - **Current quarter spend.** Dollars, from FinOps chargeback or explicit stipulation.
    - **Requests per second (inference) or accelerator-hours (training).**
    - **Throughput vs. reference.** Measured throughput / MLPerf-derived reference throughput for inference (or program-brief planned MFU for training). Publish as a ratio; state the reference cited.
    - **Utilisation.** Peak / off-peak GPU utilisation for inference; pod utilisation for training. State how measured (cloud metric, nvidia-smi, orchestrator).
    - **Flag.** `green` (within 20% of reference), `yellow` (20-50% below), `red` (>50% below, or hosted-API line item >$100K/quarter).
2. **Assign a verdict per yellow / red row.** For each candidate row, apply the chapter-4 four-verdict vocabulary: **ship migration doc** / **defer** / **explain away** / **escalate**. For each verdict:
    - `ship migration doc` — name the migration type (of the four), the proposed target, an assigned owner, and the target review date.
    - `defer` — name the specific trigger that would re-open the candidate in a future review.
    - `explain away` — name the specific reason the numbers look bad but should not be acted on (e.g., "workload was migrated to a new base model 6 weeks ago; re-tuning is in flight and expected to close the gap by end of next quarter").
    - `escalate` — name the escalation gate (chapter 4 lists three: >$500K/quarter red-flagged for two consecutive quarters; migration doc blocked cross-team for >1 cycle; FinOps allocation gap makes diagnostic unreliable) and the party escalated to.
3. **Name the review body composition.** Named roles per chapter 4: Staff engineer, finance partner, one representative from the peer platform team, one from FinOps, plus the L30 owner of each yellow-or-red-flagged workload for that meeting only. State the cadence (quarterly is the default) and the meeting duration (~2 hours).
4. **List the escalation gates in effect.** Three numeric gates from chapter 4, each with the specific trigger and the specific escalation target. Do not invent new gates in this exercise; if a gap you see is not covered, note it in Section 8 stretch.

### Part B — The migration doc

5. **Pick the candidate.** From the diagnostic, choose the single highest-value red-flag candidate that is *actually actionable* — i.e., where the migration has a plausible target, a plausible owner, and a plausible six-month payback. If two candidates tie in dollar impact, prefer the one whose migration type you have less first-hand experience with (rehearsal value beats redundancy).
6. **Draft Section 1 — Executive summary.** One paragraph and one table. Current cost, projected cost, gross savings, engineering cost of migration, payback period. This is the one section the peer-team EM will read; if it does not defend the move on dollar terms, the rest will not be read.
7. **Draft Section 2 — Current-state description.** The workload, its owning team, its current capacity mode, its current cost / throughput / utilisation numbers (with references to the diagnostic row), the SLO it currently serves, the reason nothing has been done sooner. One page.
8. **Draft Section 3 — Proposed migration.** The target capacity mode / hardware / cloud / implementation. Cite chapter 4's migration type (of the four). Sketch the migration path from current to target — pre-work, cutover, validation, full-traffic switch. Half a page.
9. **Draft Section 4 — Alternatives considered.** At least three alternatives, one of which is *"do nothing."* For each, name the dollar cost, the SLO impact, and the reason it was rejected in favour of the primary proposal. The alternatives are what the reviewer will check first for missing options; a section that omits the obvious counter-argument (e.g., "keep on the current cloud but shift to reserved") will get bounced back.
10. **Draft Section 5 — Cost model.** Dollar comparison over one year and three years including one-time migration costs and the ongoing operational delta. Sensitivity to the two most-uncertain assumptions (e.g., "if the target model quality slips 2% on eval, we run both fleets for an extra quarter — add $X"). Cite exercise-02's discount rates and commit terms.
11. **Draft Section 6 — SLO / quality impact.** What regressions are expected. What quality re-tuning is required. Named acceptance criteria for the migrated workload — the numbers the review body will use in the post-migration review to decide whether the migration succeeded.
12. **Draft Section 7 — Implementation plan.** Who does what in what order. Which peer platform / peer specialist track is involved. Named rollback plan. Named kill switches (traffic shifter, model-registry alias flip, DNS-level fallback, whichever fits the migration type).
13. **Draft Section 8 — Political cost.** Who loses control of what. Who has to change process. Who is on-call for the migrated workload post-migration. Same discipline as mod-402 chapter 5. If you cannot name a real political cost, either the migration is a rare consensus case or Section 8 has not done its work — usually the latter.
14. **Draft Section 9 — Success metrics.** The numbers the next review will read to decide whether the migration succeeded. Include a "revert" criterion — the specific threshold at which the org would roll back the migration. Without a named revert criterion the migration cannot be honestly evaluated post-launch.
15. **State the review-timeline commitment.** Named target-completion quarter, named 30-day / 90-day check-in gates against Section 9's success metrics, named next-review date at which the diagnostic re-scores the migrated workload.

## Deliverable

Two artifacts, packaged in a single document set:

**Artifact 1 — Diagnostic (2 pages).**

- **Section 1 — Diagnostic table.** One row per portfolio workload with all seven columns (step 1).
- **Section 2 — Per-candidate verdicts.** One verdict per yellow / red row with the chapter-4 four-verdict vocabulary and the required per-verdict fields (step 2).
- **Section 3 — Review body.** Named roles, cadence, duration (step 3).
- **Section 4 — Escalation gates.** Three numeric gates with triggers and targets (step 4).

**Artifact 2 — Migration doc (3 pages).**

- **Section 1 — Executive summary.** One paragraph + one table (step 6).
- **Section 2 — Current state.** (step 7)
- **Section 3 — Proposed migration.** (step 8)
- **Section 4 — Alternatives considered.** At least three, including do-nothing (step 9).
- **Section 5 — Cost model.** One-year and three-year with sensitivity to top two assumptions (step 10).
- **Section 6 — SLO / quality impact.** (step 11)
- **Section 7 — Implementation plan.** With rollback and kill switches (step 12).
- **Section 8 — Political cost.** (step 13)
- **Section 9 — Success metrics.** Including named revert criterion (step 14).
- **Section 10 — Review timeline.** (step 15)

## Starter guidance

- **The diagnostic surfaces candidates; it does not decide migrations.** Chapter 4 was explicit — the review meeting is where verdicts are assigned; the migration doc is where the decision is defended. If your diagnostic includes a "recommended migration path" column, delete it; verdicts and per-verdict fields do that work.
- **`explain away` is a legitimate verdict but should be rare.** Chapter 4 was clear — every quarter's `explain away` should be checked at next quarter's review. If the same row is `explain away` two quarters in a row, escalate.
- **MLPerf is the reference; measured is the observation.** Column 5 of the diagnostic is the ratio. If your organisation cannot measure throughput on a specific workload, either the peer performance track is not instrumenting inference (name that gap in Section 4 escalations) or the review cannot honestly flag the row.
- **The migration doc's Section 4 is where reviewers spend their time.** A three-alternative section is the minimum. A migration that only makes sense against a straw-man alternative will be sent back. Do-nothing is one of the three by chapter-4 discipline — the "keep paying what we pay" number matters.
- **Section 5's payback period is the number the CFO reads.** If your payback is greater than 18 months, the migration should probably wait for a hardware or platform change that shortens it. Below 6 months, the migration is a fast yes; between 6 and 18 the political cost section is the differentiator.
- **Section 8 is not a formality.** Chapter 4 named it as the second-most-often-skipped section and the most-often-blocking. If you cannot name someone who loses control of something as a result of the migration, either the migration is trivial (in which case it does not need a doc) or you have not asked the affected team what they think.
- **Section 9's revert criterion is the section that separates a defensible move from a hopeful one.** Every migration should have a clear "at this measured value we revert" line. Without it, the org will keep a broken migration in place because nobody wants to admit the move was wrong.
- **Do not size your migration doc to the wrong workload.** Chapter 4 was explicit that migrating a $20K/month workload is a scope error — the migration doc will consume more engineering than the workload spends. Pick a candidate whose current-annual-cost is at least 10× the migration engineering estimate.

## Acceptance criteria

- Diagnostic table has one row per exercise-01 workload with all seven columns (owning team / capacity mode / spend / rps or hours / throughput vs. reference / utilisation / flag).
- Every yellow / red row has a verdict (`ship migration doc` / `defer` / `explain away` / `escalate`) with all required per-verdict fields per chapter 4.
- Review body has named roles per chapter 4 (Staff, finance partner, peer platform rep, FinOps rep, per-meeting L30 owners), a named quarterly cadence, and a stated ~2-hour meeting duration.
- Three numeric escalation gates are named with specific triggers and specific escalation targets (per chapter 4's three named gates).
- Migration doc's Section 1 executive summary includes current cost, projected cost, savings, engineering cost, and payback period.
- Migration doc's Section 3 cites exactly one of the four chapter-4 migration types.
- Migration doc's Section 4 alternatives include at least three, one of which is explicitly do-nothing, each with dollar cost / SLO impact / rejection reason.
- Migration doc's Section 5 has one-year and three-year cost comparison and sensitivity to the top two uncertain assumptions.
- Migration doc's Section 6 has named quality acceptance criteria that a post-migration review can read.
- Migration doc's Section 7 has a rollback plan and named kill switches.
- Migration doc's Section 8 names a specific team or role that loses control of something as a result of the migration and how the loss is compensated (paged access, review-body seat, migration-time engineering budget).
- Migration doc's Section 9 has a specific named revert criterion (a measured value at which the org would roll back the migration).
- Migration doc's Section 10 names 30-day and 90-day check-in gates.
- The picked candidate's current annual cost is at least 10× the estimated migration engineering cost.

## Stretch goals

- **Author a second migration doc against a different migration type.** The four types (managed-API ↔ self-hosted, GPU-generation, cross-cloud, consolidation) each have a different decision framework. Authoring two docs from the same diagnostic catches which framework you are strongest and weakest on.
- **Draft the review-meeting agenda.** One page. The 2-hour meeting broken down: 15 minutes on last-quarter's verdicts and their status, 45 minutes on this-quarter's yellow / red rows (chapter 4's target), 30 minutes on the migration doc's decision, 30 minutes on escalation gates and FinOps-substrate gaps. Rehearses the meeting the diagnostic exists to run.
- **Simulate a rejected migration doc.** Deliberately weaken Section 4 (drop the strongest alternative), Section 8 (omit the political cost), or Section 9 (drop the revert criterion). Write the review body's rejection note. This is a rehearsal for the peer-review dynamic; deliberately failing catches which sections are load-bearing.
- **Take the diagnostic to the FinOps team for pre-review.** For each row's spend and utilisation number, ask whether the numbers are correct as sourced. Correct the diagnostic. This is a rehearsal for the real quarterly meeting and often surfaces a FinOps-substrate gap that is worth naming as its own escalation.
- **Compare against a published migration writeup.** Pick one publicly reported cloud or hardware migration (from an engineering-blog post — see `../resources.md`) and compare its structure to your migration doc's. Where does yours diverge? Often the divergence is in Section 8 (political cost); public writeups are polite about it, real docs are not.
- **Draft the exec one-liner for the migration doc.** In one sentence at the top of Section 1: what moves, what saves, what breaks. "Migrating the chat-product inference from managed-API to self-hosted vLLM on the org's existing H100 reserved capacity saves $1.2M in year one at the cost of a two-engineer-quarter build and a 30ms P95 latency regression the product team has approved." That sentence is what the director reads before opening the doc.
