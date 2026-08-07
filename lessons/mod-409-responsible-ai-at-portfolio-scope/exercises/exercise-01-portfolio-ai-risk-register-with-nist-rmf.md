# exercise-01: Portfolio AI Risk Register with NIST AI RMF

**Estimated effort:** 3 hours

## Objective

Author the **portfolio-scope AI risk register** for a real three-to-five-system ML portfolio, using chapter 2's column set — the four NIST AI RMF 1.0 core functions (Govern, Map, Measure, Manage) plus the NIST AI 600-1 Gen-AI Profile overlay on generative rows. The deliverable is a single register document — one row per production system, a cover paragraph naming which columns apply where, and a standing cadence — that a governance analyst could review cold without a per-team walk-through. After this exercise the learner should hold the first of the module's five artifacts and the row shape every downstream exercise composes with (exercise-02's classification table locks to the register at the row level; exercise-03's model card names its register row; exercise-04's gate blocks on register freshness).

This is the anchor artifact of the module. Chapter 1 was explicit that a responsible-AI posture that only survives because one engineer remembers to check things is not a posture at all; the register is what turns that dependence into a document. Pick the portfolio you can carry through all four exercises — consistency across the four exercise artifacts is worth more than novelty in any one.

## Prerequisites

- Chapter 01 — the "am I doing portfolio RAI work?" heuristic and the five-artifact framing. You should be able to name, in one sentence per role, what Staff owns vs. what the governance analyst owns vs. what the security partner owns.
- Chapter 02 — the register's column set, the four NIST AI RMF functions as column groups, the Gen-AI Profile overlay, the healthy-row worked example, and the three cadence-drift failure modes. Read in full.
- NIST AI 100-1 (*AI Risk Management Framework 1.0*, January 2023) — at minimum the Core section that names Govern / Map / Measure / Manage. The RMF Playbook page is a useful expansion but not required before this exercise.
- NIST AI 600-1 (*AI RMF Generative AI Profile*, July 2024) — the risk-category enumeration for any gen-AI rows in your portfolio. Name the categories from the published document rather than paraphrasing.
- Recommended: skim the mod-402 portfolio blueprint's per-system paragraph shape (that shape flows straight into the register's Map column) and the mod-408 incident-severity vocabulary (the Manage column reuses it).
- If you carried a portfolio through mod-402: reuse it. Chapter 2 was explicit that consistency with the mod-402 blueprint's system inventory is what makes the register composable.

## Choose the portfolio

Pick **one** portfolio of three-to-five production ML systems. Priority order:

1. If you completed mod-402, use the same portfolio. The blueprint's system inventory is the natural input to the register; the per-system paragraph flows into the Map column with minimal rework.
2. Otherwise, pick a portfolio from a *real* org you can describe — current employer, past employer, or a set of well-documented public systems from engineering blogs. Named systems produce defensible Manage entries; hypothetical systems collapse into decoration.
3. Only if neither of the above applies: stipulate a five-system portfolio at the top of the doc — one online ranking model, one classical fraud classifier, one LLM-backed chat product, one embedding-based retrieval system with a generative post-processing layer, one batch-scored propensity model — and note which of the five are gen-AI (Gen-AI Profile columns filled) and which are classical (Gen-AI Profile columns marked N/A).

Do not try to standardise across two portfolios. One portfolio in three hours produces the register; two produces two shallow drafts and neither can carry to exercise-04.

## Steps

1. **Draft the cover paragraph.** One paragraph at the top of the register naming the three-to-five systems, the register version and date, who owns the register (Staff ML engineer — you), which columns apply to which rows (specifically: which systems are gen-AI and therefore have the Gen-AI Profile overlay populated), the cadence at which rows are refreshed, and the location of the governance analyst's quarterly signoff. Chapter 2's *"one-row-per-system"* discipline is stated here so a reader does not misread the granularity.
2. **Draft the Govern column group per row.** For each system, populate:
   - **Applicable policy.** The org's responsible-AI policy that binds this system, and the policy's tier or category if the policy has one. If the org's policy is missing or under-defined, mark `policy: not-yet-defined — escalation to analyst pending` rather than inventing one. Chapter 1 was explicit that authoring policy is a scope error.
   - **Accountable owner.** By role, not by named person (`fraud team lead`, not `Priya`). A role-not-person entry survives staff turnover; a named-person entry rots.
   - **Escalation path.** The chain from row-owner up to the head of AI governance, in the vocabulary of the org.
   - **Review cadence.** Quarterly by default, monthly for any row that the exercise-02 engineering read will land at high-risk.
3. **Draft the Map column group per row.** For each system:
   - **Intended purpose.** One or two sentences on what the system does.
   - **Populations affected.** Who is on the receiving end of the system's decisions — internal users, external users, third-party users of a B2B customer. If the population is geographically bounded, name it (this feeds exercise-02's extraterritorial-reach argument).
   - **Upstream data sources.** Datasets and feature pipelines the system consumes. Cross-reference the datasheets from exercise-03 for the row's system.
   - **Downstream consumers.** Systems or product surfaces that consume the model's output. If the model's output feeds another portfolio system, name it (this is where the mod-402 blueprint's shared-component map compresses in).
   - **Deployment environment.** Online / near-real-time / batch, the serving substrate, the region.
4. **Draft the Measure column group per row.** For each system:
   - **Evaluations that have run.** Accuracy on which population slices, fairness against which protected attributes, robustness against which perturbation classes, drift on which signals. Cite the eval-artifact path or dashboard URL — *do not copy numbers into the register*. Chapter 2 was explicit that copied numbers drift silently.
   - **Most recent result and date.** One line per eval class with the last-run date and one-line result. If the last-run date is more than a quarter old, mark `stale — refresh scheduled` and put the refresh on the calendar.
   - **Next scheduled eval.** Date and named owner.
   - If the system has not been fairness-evaluated because the population attributes are not collected, say so explicitly — `fairness eval: not applicable — decisions are on transactions not on people` is a legitimate entry; a blank cell is not.
5. **Draft the Manage column group per row.** For each system:
   - **Known risks being mitigated** with the mitigation named — human-review queue, output filter, rate limit, model-choice conservatism.
   - **Residual risks accepted** with the accepting body and date — "fraud committee accepted false-decline residual 2026-04-10".
   - **Production monitoring** — the mod-408 SLI(s) the system is watched with, the dashboards, the alerting.
   - **Incident linkage** — recent Sev-1 / Sev-2 incidents affecting this system with links to the mod-408 incident register or the postmortem doc.
6. **Populate the Gen-AI Profile overlay for gen-AI rows.** For each gen-AI system, add a column group with entries drawn from the NIST AI 600-1 risk-category enumeration. At minimum cover confabulation / hallucination, harmful-content, data-provenance, prompt-injection and jailbreak, and human-AI configuration risks; use the canonical NIST AI 600-1 category names rather than paraphrases. For classical-ML systems, populate the group with a single line — `N/A — classical model, Gen-AI Profile overlay does not apply` — and leave the columns visibly present so a reviewer does not misread absence as omission.
7. **Draft the standing calendar.** A separate half-page in the register:
   - The quarterly all-rows review meeting (day of quarter, chair, attendees).
   - The monthly refresh for any high-risk rows.
   - The row-freshness thresholds the exercise-04 gate will read.
   - The rule that a row-owner change triggers a same-week row refresh, and the escalation when a row is stale.
8. **Draft the ownership split note.** A quarter-page block. Chapter 2's Staff-owns vs. analyst-owns split, stated for *this* portfolio. Which columns you (Staff ML engineer) fill in yourself; which the analyst reviews and signs; which the analyst owns entirely. This block is what the analyst reads first when they open the register.
9. **Score the register against a "governance-analyst cold read" test.** After drafting, put the doc down for at least an hour, then re-read as if you were the analyst who has never seen it. For each row ask: can I tell what the system does, who is on the hook, what the eval posture is, what the mitigations are, and where the evidence lives, without asking a follow-up question? Mark the rows that fail the test and revise. This is a rehearsal for the analyst's first quarterly review.

## Deliverable

A single register document, 3-6 pages depending on portfolio size, containing:

- **Section 0 — Cover paragraph.** Portfolio inventory, version, owner, which columns apply where, cadence, signoff location (step 1).
- **Section 1 — The register table.** One row per system, four column groups (Govern, Map, Measure, Manage) plus the Gen-AI Profile overlay group (steps 2-6). A row is a paragraph plus a small sub-table, not a novella — chapter 2's worked example is the length reference.
- **Section 2 — Standing calendar.** Quarterly all-rows review, monthly high-risk refresh, freshness thresholds, row-owner rotation rule (step 7).
- **Section 3 — Ownership split note.** Chapter 2's Staff-owns vs. analyst-owns split specialised to this portfolio (step 8).
- **Appendix A — Cold-read audit.** The result of step 9 — which rows passed and which rows you revised. Keep it in the doc; the audit is evidence of discipline.

## Starter guidance

- **The row granularity is the most common misfire.** Chapter 2 was explicit — one row per *production system*, not per model version, not per risk, not per incident. If you have forty rows on five systems, you have drifted into a defect tracker; walk back to one row per system and lift the per-version detail into the Measure entry (or into the model card in exercise-03).
- **Prefer live pointers over copied numbers.** A Measure entry that says `precision-at-review-threshold on the fraud-eval dashboard (link)` survives the next re-train; an entry that says `precision = 0.83` goes stale the next Monday. Chapter 2's cadence discipline depends on this.
- **The Gen-AI Profile column group is *visibly present* on classical rows, not silently absent.** A classical fraud model's row has the Gen-AI Profile columns rendered with `N/A — classical model` in each. A reviewer who cannot tell whether the absence is deliberate or an omission is a reviewer who will file a defect against your register.
- **Roles, not people.** Every accountable-owner and escalation-path entry is a role name. The register survives a re-org if the roles are named; it does not if the people are.
- **The extraterritorial-reach question does not live here.** That is exercise-02's classification-table job. If you find yourself writing "system is EU-in-scope because ..." in the register, move it to the classification-table row and cross-reference.
- **The `needs-research` marker is a legitimate entry.** If you genuinely do not know a row's fairness-eval status because the eval was run by a team that no longer exists, mark `needs-research — chase down eval artifact by 2026-Q4` with a named owner. Chapter 2's discipline is honesty about gaps, not fabrication of completeness.
- **Do not exceed the length reference.** Chapter 2's worked-example row is a paragraph plus a small sub-table. A row that is a full page has probably absorbed content that belongs in the model card (exercise-03) or the eval report; extract it back.
- **The cadence section is where most registers rot.** A register with no next-review dates is one that dies the day after it is written. If you find yourself writing `next review: TBD`, that is the row the exercise-04 gate will block on first — pick a date.

## Acceptance criteria

- Exactly one portfolio of 3-5 named production systems is registered; each row has a named row-owner *by role*.
- The cover paragraph names the register version, the owner, the cadence, the signoff location, and which rows carry the Gen-AI Profile overlay.
- Every row has all four NIST AI RMF column groups (Govern, Map, Measure, Manage) populated with the sub-entries specified in steps 2-5.
- Every gen-AI row has the Gen-AI Profile overlay column group populated with entries in the vocabulary of NIST AI 600-1's published risk categories.
- Every classical-ML row has the Gen-AI Profile column group visibly present with `N/A — classical model` (or equivalent) in every cell, so absence is not read as omission.
- Every Measure entry cites a live eval-artifact link or dashboard URL rather than a copied number; entries whose last-run date is more than a quarter old are marked `stale — refresh scheduled` with a named owner.
- The standing calendar names a quarterly all-rows review with a chair and attendee list, a monthly refresh for any high-risk rows, and the row-freshness thresholds exercise-04's gate reads.
- The ownership split note names, for this portfolio specifically, which columns Staff owns, which the analyst reviews, and which the analyst owns.
- Every escalation-path entry names a role, not a person.
- The cold-read audit in Appendix A is present and names at least one row that was revised as a result of the audit.
- No entry copies a fairness or accuracy number that is available on a live dashboard.

## Stretch goals

- **Populate a second gen-AI row on a foundation-model consumer surface.** If the base portfolio has one LLM chat feature, add a second gen-AI system that consumes a foundation model differently — for instance an embedding-based retrieval with a generative summariser. Chapter 2's Gen-AI Profile overlay lands differently on different consumption patterns; exercising two rows sharpens the risk-category vocabulary.
- **Cross-link a shared feature explicitly.** Pick one feature or dataset used by two or more systems in the portfolio and cross-reference the Map columns on both rows to the same underlying source. Chapter 4's cross-linking discipline previews this; landing it here is the register-side rehearsal.
- **Draft the analyst-review agenda for one quarterly meeting.** A one-page mock agenda for the quarterly review of the register you just wrote — which rows will get the most attention, which have residual-risk decisions the analyst must sign, which are on a monthly cadence and get discussed separately. Rehearsing the meeting is what shakes out the rows that are not yet analyst-ready.
- **Simulate a row-owner rotation.** Pick one row, imagine the owning team lead has left, and write the row refresh the incoming lead would produce in their first week. This is a rehearsal for the mod-408 on-call-handoff-adjacent trigger and the discipline that keeps rows from going stale on turnover.
- **Take the draft to a peer for adversarial pre-review.** A Staff-plus peer or (ideally) a governance-analyst-adjacent reviewer. Ask them to name the row they would push back on first and the column entry they trust least. Absorb the pushback. This is a rehearsal for the analyst's first quarterly signoff.
- **Draft the exec one-liner.** One sentence at the top of the register: what the register asserts, what it does not, when the next full refresh lands. "The register enumerates the responsible-AI posture of the org's five production ML systems against NIST AI RMF 1.0 and the Gen-AI Profile; the analyst signs quarterly; the next refresh lands 2026-Q4." That sentence is what a director reads before opening the table.
