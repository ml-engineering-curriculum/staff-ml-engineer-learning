# exercise-04: Portfolio Postmortem Template

**Estimated effort:** 3 hours

## Objective

Author the **org-standard ML postmortem template** the portfolio will adopt, using chapter 5's ten-section framing with the load-bearing portfolio-lessons section and the seven ML-specific action-item categories. The deliverable is a template document (the blank template every future postmortem is written against) plus a two-page companion doc that names the review-meeting agenda and the portfolio-lesson mechanism, plus one fully-worked example postmortem populated from a real or simulated incident. After this exercise the learner should hold the fourth of the four artifacts this module produces and have closed the module's reliability loop — SLIs are named (exercise-01), severities are assigned (exercise-02), incidents are commanded (exercise-03), and lessons carry across teams (this exercise).

This exercise is the module's terminal artifact. Its portfolio-lesson mechanism is what makes the reliability discipline sticky across the three-to-five teams in the portfolio; without it the SLI catalogue, severity matrix, and incident-command practices all still work per-team but do not carry forward. Chapter 5 was explicit that the Staff ML engineer's contribution here is not the postmortem itself — it is the *mechanism* by which one team's lesson prevents the same class of failure on another team's system.

## Prerequisites

- Exercise-01's SLI catalogue for the same portfolio. The `catalogue change` action-item category writes back into that document.
- Exercise-02's severity matrix. The `severity change` action-item category writes back into that document.
- Exercise-03's tabletop drill retrospective. The drill's follow-up action items are the raw material for the fully-worked example postmortem in this exercise.
- Chapter 05 — the ten template sections, the portfolio-lessons section mechanism, the seven ML-specific action-item categories, the two ML-specific blame pitfalls (blaming the model, blaming the data), and the two-week review-meeting agenda. Read in full.
- Allspaw's *Blameless PostMortems and a Just Culture* (Etsy Code as Craft, 2012) and SRE Book chapter 15 (Postmortem Culture). These are the load-bearing prerequisite reading the chapter cites; do not skip them.
- Recommended: skim [Learning from Incidents in Software](https://www.learningfromincidents.io/) for the human-factors extension to blameless framing, and one company-blog postmortem writeup for the operational tone (Netflix Tech Blog and Stripe engineering blog both have public postmortems worth reading).

## Choose the input

Reuse the exercise-01/02/03 portfolio. Priority order for the fully-worked example:

1. **A real ML incident on the portfolio in the last quarter.** Anonymise names and dollar figures if needed. Real incidents produce defensible portfolio-lesson sections; simulated incidents do not surface the load-bearing carry-over failures.
2. **The exercise-03 tabletop drill.** The drill is a simulated incident with a real timeline and real observed pathologies; treat it as the input, populate every section including the honest "what went well / what went wrong" from the drill, and let the portfolio-lesson section carry the drill's coaching notes into portfolio-scope action items.
3. **Only as a last resort:** a fully-hypothetical incident drawn from the scenario candidates in exercise-03 step 1. Note the hypothetical status at the top of the example document.

Do not skip the fully-worked example. The blank template is 40% of the value; the worked example is what teams read to understand how each section is populated in their own postmortems.

## Steps

1. **State the scope of the template.** One paragraph at the top. Which incidents does the template apply to (Sev-0 and Sev-1 automatically; Sev-2 at the discretion of the affected team; Sev-3 rolls up into the reliability-review meeting without a per-incident postmortem). Chapter 5 was explicit that a template that requires a full postmortem for every Sev-3 loses adoption; keep the scope honest.
2. **Draft the blank template.** All ten chapter-5 sections. For each section, write:
   - **Section title.** In the order chapter 5 uses.
   - **One-sentence purpose.** What this section is for. This is the prompt the person writing the postmortem reads first.
   - **The prompt questions.** Two-to-five questions per section, in the vocabulary chapter 5 uses. For section 4 (Timeline), the prompt is "the live timeline from the incident, cleaned but not sanitised, with honest hypotheses at each point". For section 9 (portfolio-scope action items), the prompts are the three properties from chapter 5 — portfolio-scope owner, list of receiving systems, review date.
   - **Length guidance.** Read-in-30-seconds sections are one-to-four sentences; deep sections (5, 8, 9) are half a page each; the timeline is as long as the incident was.
3. **Draft the seven action-item category taxonomy.** For each of the seven chapter-5 categories (`catalogue change`, `severity change`, `runbook change`, `feature-pipeline contract change`, `retraining contract change`, `monitoring substrate change`, `training / staffing change`), write:
   - The category name and its one-sentence definition.
   - The default owner (Staff ML engineer, L30 tech lead, feature owner, platform team, peer EM).
   - An example action item in the vocabulary of your portfolio.
   - Which exercise-01 / 02 / 03 artifact the action item writes back into.
   This taxonomy is the enforcement mechanism — every action item in every future postmortem must fit a category; if it doesn't, either the category set is incomplete or the item is not actionable.
4. **Draft the two-blame-pitfall guardrail.** Half a page. Chapter 5's two ML-specific blame pitfalls — blaming the model, blaming the data — with two-to-three example reframes each drawn from the portfolio. This section sits inside the template as a call-out box the review-meeting facilitator reads through during the two-week review; teams that skip the guardrail regress to artifact-blame within two quarters.
5. **Draft the review-meeting agenda.** One page. Chapter 5's four-block agenda:
   - 15 minutes — affected team walks the timeline.
   - 15 minutes — portfolio-lessons section, every named receiving team confirms or disputes the action item on their system.
   - 10 minutes — catalogue changes and severity changes (section 10 of the template).
   - 15 minutes — blame-pitfall audit; the group re-reads the doc for artifact-blame or data-blame language and rewrites it.
   - 5 minutes — schedule the action-item review date.
   Name the attendee list (affected team, portfolio-lesson receiving teams, peer EM, platform-team lead, Staff ML engineer as facilitator) and the cadence (two weeks post-close is chapter 5's default).
6. **Draft the review-of-review mechanism.** Quarter of a page. Chapter 5's third property of the portfolio-lessons section — every action item has a review date, and on that date the Staff ML engineer verifies the action item landed on each named receiving system. Name who owns the review calendar and where the outcomes are logged. Without this section the portfolio-lesson mechanism is a wishlist.
7. **Draft the exception-and-scope section.** Quarter of a page. When the template does *not* apply — a research-only incident with no production surface; an incident closed within 15 minutes with no user-visible impact and no drift-catalogue breach; an incident where a peer platform SRE team's postmortem already covers the affected surface. Chapter 5 was explicit that a template with no exemption path collapses under bureaucratic friction; document the exemptions.
8. **Populate the fully-worked example.** All ten sections filled for one real (or drill-derived) incident. Every section written to the length-guidance from step 2. Every action item mapped to one of the seven categories from step 3. Every portfolio-lesson action item names its receiving systems and its review date. The example is read as much as the template; it defines the tone every future postmortem is written in.
9. **Draft the two-blame-pitfall audit pass on the example.** After populating the example, re-read it looking for artifact-blame ("the model made a wrong prediction") and data-blame ("the upstream data was wrong") language. Rewrite each instance into the systems-and-defences reframe from chapter 5. Note the changes made — this is a rehearsal for the review-meeting audit in step 5.
10. **Author the one-page adoption checklist.** A one-page appendix a team lead can walk through in ten minutes when they are about to write their first postmortem against the template. Ten-to-fifteen checkboxes covering: section 8 vs section 9 discrimination (is this a team-scope or portfolio-scope action item?), action-item category assignment, portfolio-lesson receiving-systems named, review date set, blame-pitfall audit completed.

## Deliverable

Three artifacts in one document (or three sibling documents in a folder):

**Artifact 1 — Blank template** (2-3 pages):

- **Scope preamble.** One paragraph on which incidents the template applies to (step 1).
- **Ten sections.** Titles, purposes, prompt questions, length guidance for each of chapter 5's ten sections (step 2).
- **The seven-category action-item taxonomy.** Named and defined inline in section 8 and section 9 (step 3).
- **The two-blame-pitfall call-out.** Half-page guardrail box (step 4).
- **Exception-and-scope section.** Quarter-page (step 7).

**Artifact 2 — Companion doc** (1-2 pages):

- **Review-meeting agenda.** One page, four-block agenda per chapter 5 (step 5).
- **Review-of-review mechanism.** Quarter page (step 6).
- **Adoption checklist.** One page, 10-15 checkboxes (step 10).

**Artifact 3 — Fully-worked example** (2-4 pages):

- All ten sections populated from a real, drill-derived, or (last resort) hypothetical incident (step 8).
- Every action item categorised.
- Every portfolio-lesson action item with receiving-systems list and review date.
- Blame-pitfall audit pass documented at the end (step 9).

## Starter guidance

- **The portfolio-lessons section (section 9) is the load-bearing bit.** Chapter 5 was emphatic — this is the section most orgs skip, and skipping it is why the same class of failure repeats on another team's system six months later. If your template gives section 9 the same weight as section 6 (what went well), you have under-weighted it. Section 9 is where the Staff ML engineer's contribution lives.
- **Every portfolio-lesson action item has three properties or it is not portfolio-scope.** Chapter 5 was explicit: portfolio-scope owner (not a team owner), an explicit list of receiving systems, a review date. If any one is missing, the action item stays in section 8 (team-scope), not section 9. The template's prompt questions in step 2 must enforce this.
- **The seven-category taxonomy is not decorative.** Chapter 5 was explicit that a postmortem whose action items don't fit clean categories is one whose lessons the org cannot aggregate. If the fully-worked example has an action item that doesn't fit, either add the category (rare — the seven were chosen deliberately) or the item is not actionable and should be rewritten.
- **Blameless framing is not a section — it is a property of the whole document.** Chapter 5 was explicit: the blame-pitfall call-out is a guardrail the review-meeting facilitator reads, not a section teams fill. The template's tone throughout must be systems-and-defences; if section 7 (what went wrong) reads as "person X did wrong thing", the whole template regresses.
- **The two-blame-pitfall pass on the example is a rehearsal.** Step 9 is what teaches the tone. When you re-read the example looking for "the model" and "the data" as blame subjects, you will find them; that is the point. Rewrite them into "the offline eval did not catch X; the canary criterion did not catch it; the drift SLI did not catch it" style. The rewrite is chapter 5's defence-in-depth principle in action.
- **The review-meeting cadence is two weeks post-close.** Chapter 5 was explicit that the meeting produces 70% of the value. If your companion doc names a one-week or four-week cadence, defend the choice; the two-week window is the SRE-Workbook default and is calibrated to when the affected team is still fresh but no longer in fire-fighting mode.
- **The fully-worked example is what teams read.** The blank template is what the review body approves; the example is what team leads read the first time they write a postmortem. Populate the example carefully — the tone of the example sets the tone of every postmortem that follows.
- **Do not exceed the section-length guidance.** Chapter 5's read-in-30-seconds discipline for the summary and impact sections is what makes the exec digest work. A five-paragraph summary is a summary nobody reads; keep to the guidance.

## Acceptance criteria

- Artifact 1 (blank template) is present and covers all ten chapter-5 sections with title, purpose, prompt questions, and length guidance per section.
- The seven-category action-item taxonomy is present with name, definition, default owner, and portfolio-specific example per category.
- The two-blame-pitfall call-out is present in the template with two-to-three reframes per pitfall drawn from the portfolio.
- The exception-and-scope section names at least two legitimate exemptions.
- Artifact 2 (companion doc) is present with the four-block review-meeting agenda, the review-of-review mechanism, and the 10-15-checkbox adoption checklist.
- The review-meeting cadence is two weeks post-close (or explicitly justified other) and names the attendee list.
- Artifact 3 (fully-worked example) is present with all ten sections populated.
- Every action item in the example is mapped to one of the seven categories.
- Every portfolio-lesson action item (section 9) in the example has a portfolio-scope owner, an explicit list of receiving systems, and a review date.
- The blame-pitfall audit pass on the example is documented at the end of Artifact 3, with at least one before/after reframe.
- The template nowhere blames an individual by name.
- The template nowhere treats section 8 (team-scope) and section 9 (portfolio-scope) as interchangeable; the discrimination is enforced by the prompt questions.

## Stretch goals

- **Populate a second worked example from a different failure class.** If the first example is a shared-feature-pipeline breakage, populate a second from a silent-drift or retraining-loop stall incident. Chapter 5's two-blame-pitfall pattern varies by failure class; a template that only handles one class robustly does not carry across the portfolio.
- **Draft the review-of-review dashboard.** A one-page mock dashboard the Staff ML engineer opens quarterly showing every portfolio-lesson action item, its owner, its receiving systems, and its landing status per system. Chapter 5's third property of section 9 depends on this dashboard existing; without it the review dates rot.
- **Simulate the first review meeting.** Write a one-page mock transcript of the two-week review meeting for the worked example. Include the moment a receiving team disputes an action item ("we already have a canary check on that feature; this action item is redundant for us") and how the facilitator resolves it. The facilitator move here is chapter 5's most-often-missed practice.
- **Draft the exec digest snippet.** A one-sentence-per-incident weekly digest for the exec chat, drawn from every open postmortem's section 1 (summary). Chapter 3's exec-notification threshold reads against this; the digest is what makes execs stop asking "is anything on fire?" one-off and start reading the summary.
- **Cross-reference to exercise-01 and exercise-02.** In an appendix to the companion doc, name which action-item categories most commonly land back on the exercise-01 catalogue vs the exercise-02 severity matrix vs the chapter-6 retraining contract. Chapter 5's iteration argument reads against this — the catalogue and matrix grow through the postmortem stream; if no postmortem in the last two quarters has changed the catalogue, either the catalogue is complete (unlikely) or the mechanism is not working.
- **Take the draft to a peer for adversarial pre-review.** A Staff-plus peer or the peer EM. Ask them to read the worked example looking for artifact-blame and data-blame language; absorb the pushback and rewrite. This is a rehearsal for the two-week review meeting's blame-pitfall audit block.
- **Draft the exec one-liner.** In one sentence at the top of the companion doc: what the template requires, which mechanism it adds beyond a per-team postmortem, when it takes effect. "The template adds the portfolio-lessons section and the seven-category action-item taxonomy; effective for every Sev-0 and Sev-1 from Q3, so no ML failure class recurs on a second team's system without a defensible reason." That sentence is what the director reads; the rest of the doc justifies it.
