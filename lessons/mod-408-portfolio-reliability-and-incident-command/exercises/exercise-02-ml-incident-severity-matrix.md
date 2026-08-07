# exercise-02: ML Incident Severity Matrix

**Estimated effort:** 3 hours

## Objective

Author the **ML incident severity matrix** for the portfolio you catalogued in exercise-01, layered on top of the org's existing SRE severity matrix and adding the silent-failure column classical SRE has no vocabulary for. The deliverable is a one-page table plus a two-to-three-page companion doc — the artifact the on-call opens at 3 AM to answer "does this page me, ticket me, or wait for me to see it at standup?" and the artifact chapter 4's incident-command decision tree reads to pick command shape. After this exercise the learner should hold the second of the four artifacts this module produces.

This is the input to exercise-03. The tabletop drill in exercise-03 uses breaches from this matrix as the incident triggers, and the role assignments in the drill read the exec-notification threshold this matrix defines. Reuse the portfolio from exercise-01; the matrix rows are the SLIs from that catalogue.

## Prerequisites

- Exercise-01's SLI catalogue for the same portfolio. The matrix rows are catalogue SLIs; without the catalogue this exercise has no domain.
- Chapter 03 — the two shapes of ML incident (user-visible, silent-failure), the four severity tiers (Sev-0 portfolio outage / Sev-1 single-system / Sev-2 ticketed / Sev-3 rolling), the page-vs-ticket-vs-log line, the exec-notification threshold, and the silent-failure column. Read in full.
- Site Reliability Workbook chapter 5 (Alerting on SLOs) and chapter 8 (Managing Incidents). The 4-hour Sev-0 tempo and the alert-fatigue argument in chapter 3 are from these.
- The org's existing SRE severity matrix, if one exists. This exercise *layers on top of* it; if you overwrite the peer platform SRE team's matrix, you have picked the wrong deliverable.
- Recommended: skim [PagerDuty's incident response documentation](https://response.pagerduty.com/) for the severity-tier vocabulary and role assignments; the terminology overlap with chapter 3 is deliberate.

## Choose the input

Reuse the exercise-01 portfolio. Priority order:

1. Use the exercise-01 catalogue verbatim. The matrix rows are catalogue SLIs; do not re-scope the portfolio in this exercise.
2. If you skipped exercise-01, complete it first. This exercise reads its per-system commitments and yellow / red thresholds; drafting the matrix without the catalogue produces a decorative table.

## Steps

1. **Extract the matrix row list from the catalogue.** For each SLI in exercise-01's per-system commitments, one row in the matrix. If the catalogue has 15 SLIs across the portfolio, the matrix has 15 rows; if that is too long for a one-page table, collapse to one row per family with system-column overrides (chapter 3's practical-shape section names both patterns).
2. **Confirm the tier vocabulary with the org.** Sev-0 through Sev-3 or P0 through P3 — use whichever the platform SRE team uses. Do not invent a new naming scheme; the on-call must not context-switch between two vocabularies in the middle of an incident. Name the vocabulary in a one-sentence preamble at the top of the doc.
3. **Draft Section 1 — Two shapes preamble.** Half a page. Name Shape A (user-visible) and Shape B (silent-failure) with one concrete example from the portfolio each. Chapter 3 was explicit that the shapes need different columns, different command structures, and different postmortem categories; that argument sits at the top of the doc so a reader who lands cold understands why the matrix has the silent-failure column.
4. **Draft the matrix table.** One row per SLI (or SLI family with system overrides). Columns per chapter 3's practical-shape section:
   - **SLI + threshold.** Which SLI, which breach threshold triggers the row (usually the yellow or red threshold from the exercise-01 catalogue).
   - **Shape.** A (user-visible) or B (silent).
   - **Severity.** Sev-0 through Sev-3.
   - **Route.** Page / ticket / log.
   - **Owner.** Feature owner / model owner / platform SRE.
   - **Exec-notify?** Yes always / yes if user-visible / no.
   - **Runbook.** Link to the runbook the on-call reads first (link may be a placeholder pointing at a runbook that does not exist yet — flag those under Section 4 below).
   Aim for one page. If the table is longer than one page, either collapse per-system rows into per-family rows with overrides or prune the exercise-01 catalogue.
5. **Populate the silent-failure rows carefully.** The single largest content contribution the matrix makes over the peer SRE matrix is the Sev-1 and Sev-2 mappings for silent-failure signals. Chapter 3 was explicit: if a silent-failure signal defaults to Sev-3 informational, it disappears. For every calibration, drift, retraining-SLA, and judge-agreement row, defend the severity assignment in one sentence in Section 3 below.
6. **Draft Section 2 — Page-vs-ticket-vs-log rules.** Half a page. Restate chapter 3's three heuristics — pages require actionable immediate response, silent-failure signals rarely page in the moment, multi-consumer feature breakages page the feature owner and notify the model owners — and give one concrete example from the portfolio per heuristic.
7. **Draft Section 3 — Silent-failure severity rationale.** Half a page. For each silent-failure row in the matrix, one sentence defending why the severity is what it is. This section is what defends the matrix during the review-body meeting; without it the platform SRE will push every silent-failure row down to Sev-3 and the matrix collapses.
8. **Draft Section 4 — Exec-notification rules.** Quarter of a page. The three chapter-3 rules — Sev-0 always within 30 minutes, Sev-1 if user-visible or expected to become so, duration escalates the rule at 4 hours. Name who the exec chat is (CTO chat, VP Eng chat, or an incident-specific channel) and who is the default Comms Lead per mod-401 chapter 5.
9. **Draft Section 5 — Rotation model.** Quarter of a page. Name the merged-vs-parallel decision from chapter 4 — when the ML on-call and the platform SRE on-call merge into a single Incident Commander and when they run parallel. Chapter 3's closing paragraph names this seam; document your portfolio's choice.
10. **Draft Section 6 — Runbook gap list.** A one-page appendix listing the runbooks the matrix references that do not exist yet. Each gap has an owner (model owner or feature owner) and a due date. Chapter 5's postmortem `runbook change` action-item category will read this list; the matrix is not credible until the runbooks it points to exist.
11. **Score the matrix against the alert-fatigue budget.** Estimate — from prior three months of SLI data if you have it, from expected breach rates if you do not — how many pages the matrix would have produced in the last quarter across the portfolio. Chapter 3's load-bearing warning was that alert fatigue is the primary failure mode of a maturing on-call rotation. If your matrix would have paged the on-call more than 3-5 times per week across the whole portfolio, walk back the page-triggering rows to ticket-triggering.

## Deliverable

A single document, 2-3 pages plus a one-page matrix table, containing:

- **Preamble.** One sentence naming the severity vocabulary in use (Sev-0..3 or P0..3) and the parent SRE matrix this layers on top of (step 2).
- **Section 1 — Two shapes.** Half-page framing of Shape A vs Shape B with portfolio-specific examples (step 3).
- **The matrix table.** One page, one row per SLI (or SLI family), with the seven columns from step 4.
- **Section 2 — Page / ticket / log rules.** Half a page, three heuristics with portfolio examples (step 6).
- **Section 3 — Silent-failure severity rationale.** Half a page defending every silent-failure row's severity (step 7).
- **Section 4 — Exec-notification rules.** Quarter page, the three chapter-3 rules named for the org (step 8).
- **Section 5 — Rotation model.** Quarter page, merged-vs-parallel decision for the portfolio (step 9).
- **Appendix A — Runbook gap list.** One page listing the runbook links the matrix references that do not yet exist (step 10).
- **Appendix B — Alert-fatigue estimate.** Two paragraphs quoting the estimated page rate the matrix would produce and the rationale for the page-vs-ticket line (step 11).

## Starter guidance

- **The matrix is read at 3 AM.** Chapter 3 was explicit: the on-call opens it at 3 AM to answer one question ("do I get out of bed?"). A three-page table with paragraphs of commentary is not a matrix. Get to a one-page table with all seven columns, and put the commentary in Sections 1-5.
- **Every silent-failure row must have a severity above Sev-3.** If your matrix has calibration decay, drift SLI, retraining SLA, and judge-agreement all sitting at Sev-3 with a ticket route, you have imported the classical SRE matrix unchanged and the silent-failure column has done nothing. Chapter 3 was unambiguous: at least the two-consecutive-window and hard-red breaches promote to Sev-2 or Sev-1.
- **Two-consecutive-window rule prevents flapping.** Chapter 3's alert-fatigue argument reads directly here. A single window of any SLI below SLO is Sev-3; two consecutive windows is Sev-2; two consecutive windows plus a downstream business-metric shift is Sev-1. Encode this in the row's threshold column.
- **The exec-notification threshold is the load-bearing exec-facing decision.** Chapter 3 was explicit that early is better than late — the failure mode is the exec learning about an ML incident from Twitter before the ML org tells them. Once. Err on the side of notifying; the row's default is "yes if user-visible", not "no".
- **The runbook gap list is the trust surface for the matrix.** A matrix that points at 15 runbooks of which 8 do not exist is a matrix the on-call does not trust. Chapter 5's `runbook change` action-item category reads this list; publish it, do not hide the gaps.
- **The alert-fatigue estimate defends the page-vs-ticket line.** Chapter 3's load-bearing warning was that alert fatigue is what breaks a maturing on-call rotation. If the estimated page rate is more than 3-5 per week across the whole portfolio, the matrix is training the on-call to ignore the pager; walk back a row.
- **Two-to-three pages plus the one-page table is fine.** Do not pad. The single most useful thing you can do for the on-call is make the table shorter and the routes clearer; the commentary sections are for the review-body meeting, not for the on-call.

## Acceptance criteria

- The matrix table is present, one page, with one row per SLI (or SLI family with system overrides) from exercise-01's catalogue, with all seven columns per step 4.
- Every row has a shape assigned (A or B) and a severity assigned (Sev-0 through Sev-3 or the org's equivalent).
- At least one silent-failure row is assigned Sev-1 and at least two are assigned Sev-2; no silent-failure row is silently defaulted to Sev-3 informational.
- The page-vs-ticket-vs-log route column is populated for every row, and Section 2 defends the page-triggering rows with the "actionable immediate response" heuristic.
- The exec-notification column is populated for every row per chapter 3's three rules, and Section 4 names the exec chat and the default Comms Lead.
- Section 5 names the merged-vs-parallel rotation model for the portfolio (or explicitly declares only one rotation exists and defends the choice).
- Appendix A runbook gap list is present with owner and due date per gap.
- Appendix B alert-fatigue estimate is present with a numeric estimated page rate and the rationale for the page-vs-ticket line.
- The matrix nowhere invents a new severity vocabulary; it uses the org's existing Sev-0..3 or P0..3 tiers.
- The matrix nowhere defaults a silent-failure signal to Sev-3 informational without a Sev-2 promotion rule (two-consecutive-window rule or equivalent).

## Stretch goals

- **Walk the matrix against the last three months of real incidents.** For every ML-touching incident in the portfolio in the last quarter, quote (a) what the on-call actually did, (b) what the new matrix would say to do. Divergences are the reason to publish the matrix; agreements are the reason to trust it. If you cannot get three months of incident data, use one and mark the sample size in Appendix B.
- **Draft the parallel-rotation escalation runbook.** A one-page runbook for the moment described in chapter 4 where the platform SRE IC realises the incident is ML-side and hands off to the Staff ML engineer IC (or the reverse). Chapter 3's rotation-model section names this seam; the runbook is what makes it survive contact with reality.
- **Simulate a review with the platform SRE team.** Write a one-page mock agenda for the meeting where you present the matrix to the peer platform SRE lead. Include the two rows you most fear pushback on (usually a silent-failure row promoted to Sev-1) and how you plan to defend them. Rehearse the "we only page on user-visible signals" argument; that is the one that most often collapses first-time authors.
- **Draft the customer-facing severity mapping.** If the org has a public status page or customer-visible SLA, name which of your Sev-0 rows result in a customer-facing communication and what that communication says. Chapter 3 was explicit that Sev-0 is a public-facing status update if the failure is user-visible; without this mapping, the Comms Lead has to invent the language during the incident.
- **Take the draft to a peer for adversarial pre-review.** A Staff-plus peer or a Senior tech-lead on-call in the portfolio. Ask them to name the row they would refuse to page on and the row they would insist on paging on. Absorb the pushback. This is a rehearsal for the review-body meeting where the matrix is proposed.
- **Draft the exec one-liner.** In one sentence at the top of the doc: what the matrix does, what the alert-fatigue budget is, when it takes effect. "The matrix layers eight ML-specific SLI rows onto the existing SRE severity matrix, with an alert-fatigue budget of four pages per week across the ML portfolio; effective Q3." That sentence is what the director reads; the rest of the doc justifies it.
