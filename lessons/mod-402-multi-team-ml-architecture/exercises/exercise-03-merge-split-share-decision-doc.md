# exercise-03: Merge, Split, Share — Decision Doc

**Estimated effort:** 4 hours

## Objective

Take **one painful cell** from exercise-02 and author the decision doc that resolves it — the analysis that will become the core of the multi-system RFC in exercise-04. After this exercise the learner should hold a defensible verdict (merge / split / share-behind-a-contract / defer), a migration-cost estimate with error bars, and a political-cost analysis per consumer team. This is the first exercise in the module where the learner is authoring a *staff-plus-scope decision*, not a diagnosis.

The decision doc is a longer artifact than a single-team ADR because it has to defend both the technical case and the political case across teams. Budget the time; the political-cost section reliably takes learners longer than they expect.

## Prerequisites

- Exercise 02 complete, with a diagnostic set of three-to-five cells.
- Chapter 04 — Merge / Split / Share Decisions read in full. The five-question framework, the migration-cost model, the political-cost model, the two worked examples, the frequency map, and the defer verdict.
- Optional but recommended: skim of Kruchten's *4+1 view model* essay and one Google-style design-doc example, to prime the "what does a good decision doc look like" instinct.

## Choose the cell

Pick **one** cell from exercise-02, choosing on the following priority order:

1. A cell whose composition analysis in exercise-02 identified it as the **leftmost cell** in a compose-chain — chapter 3's rule of thumb is to fix the leftmost first. If the composition puts a data-plane cell before a feature-layer cell, decide the data-plane cell.
2. A cell with **high pain scoring** on at least two of the three axes (frequency / blast radius / recovery cost).
3. A cell where the evidence collected in exercise-02 was strong. Do not build a decision on the `suspected but not evidenced` cell — that path leads to a *defer* verdict, which is a legitimate outcome but is not what this exercise is drilling.

Learners may be tempted to pick the biggest cell (the one where a merge would be the most impressive). Resist. The right cell is the highest-leverage one that this quarter's cost model can honestly support.

## Steps

1. **Walk the five-question decision framework** from chapter 4 explicitly. For each question:
    - Write the question.
    - Write the honest answer for the chosen cell.
    - Note which chapter-4 branch that answer takes you to.
    - Record any assumption that was load-bearing to the answer.
   Do not skip a question because "the answer is obviously yes". Force yourself to write the answer down; assumptions surface here that were invisible when the question was implicit.
2. **State the verdict** — one of `merge` / `split` / `share-behind-a-contract` / `defer`. One sentence. This is what section 1 of the exercise-04 RFC will say.
3. **Build the migration-cost model** per chapter 4. Four contributions rolled up:
    - Per-team migration effort — bottom-up estimate in engineering weeks, times the 1.5 underestimate factor.
    - Backfill / historical-data effort.
    - Serving-side migration.
    - Operational transition during the parallel-run period.
   Report per-team totals with error bars ("6-10 weeks, most-likely 8") and identify the two-to-three assumptions that dominate the error.
4. **Build the political-cost model** per chapter 4. Per consumer team, name which of the three components dominates:
    - Team ownership loss.
    - Team roadmap disruption.
    - Precedent risk.
   Write one paragraph per consumer team. Propose one specific mitigation per team from the chapter-4 list (ownership swap, roadmap-slot deal, migration-help commitment) or, if none fits, from your own creativity — justify the choice.
5. **Present the alternatives you did not choose.** For each of the other three verdicts, one paragraph on why it was not the answer. Present each in its strongest form; do not strawman. If your best steel-man of an alternative makes you doubt your verdict, revisit step 2 — the exercise is working.
6. **Name the decommissioning trigger.** If the verdict is *merge*, name the decommissioning date for every superseded implementation. If the verdict is *split*, name the sunset trigger for revisiting the split. If the verdict is *share-behind-a-contract*, name the review cadence and the follow-up-merge trigger. If the verdict is *defer*, name the specific trigger and the specific person who watches for it. A verdict without one of these is decorative.
7. **Draft the recovery-of-investment sentence.** In one sentence: what does the org get, per quarter, that it did not have before this decision landed? If you cannot write that sentence, the ROI is not obvious and the RFC will get blocked on it.

## Deliverable

A single document, 4-6 pages, containing:

- **Section 1** — The cell (one paragraph — same substrate cell as exercise-02).
- **Section 2** — Five-question walk-through, each question with the honest answer and the assumption note.
- **Section 3** — The verdict (one sentence) and the rationale (one paragraph).
- **Section 4** — Migration-cost model with per-team totals, error bars, and dominant assumptions.
- **Section 5** — Political-cost model with per-team analysis and per-team mitigation proposal.
- **Section 6** — Alternatives-considered, one paragraph per non-chosen verdict.
- **Section 7** — The decommissioning date / sunset trigger / review cadence / defer trigger — whichever applies.
- **Section 8** — Recovery-of-investment sentence.
- **Section 9** — Two-to-three sentences of personal reflection: which of the five questions surprised you? Where was the political-cost analysis hardest?

## Starter guidance

- **Do not merge because it feels virtuous.** Merges are the highest-cost verdict; the chapter-4 frequency map predicts merge is the right answer *sometimes*, share-behind-a-contract is the right answer *most often*, and defer is legitimate. If your first draft is a merge, sanity-check that questions 3 and 4 were answered honestly — an owner named and a migration cost accepted, not hand-waved.
- **The political-cost section is the section that fails RFCs.** Chapter 6 reiterates this; it is worth restating. If you cannot write one paragraph per team on which of ownership loss / roadmap disruption / precedent risk dominates, you cannot yet propose the decision. Talk to the team leads before finishing the doc.
- **Report cost as ranges.** *"8 weeks per team"* fails a review; *"6-10 weeks per team, most-likely 8, dominated by the backfill-effort estimate"* passes. The range communicates that you know what you don't know.
- **The 1.5 underestimate factor is not optional.** Bottom-up estimates from consumer team leads are systematically optimistic. This is not a knock on the team leads; it is a documented pattern. Do not skip the multiplier.
- **The recovery-of-investment sentence is not a slogan.** *"We reduce feature drift"* is a slogan; *"We eliminate the observed 0.6% drift between Search and Home-feed's `user_7d_purchase_count` per week, and free two engineers on the Home-feed team from monthly triage of duplicate feature bugs"* is the sentence. Reviewers care about the concreteness.

## Acceptance criteria

- One painful cell chosen from exercise-02, justified per the priority list.
- The five-question framework walked with an explicit answer to each question, with assumptions noted.
- Verdict named (one of merge / split / share-behind-a-contract / defer) with a one-paragraph rationale.
- Migration-cost model rolls up the four contributions per team, reports ranges, identifies the dominant-assumption pair.
- Political-cost model addresses every consumer team, names the dominant component, proposes a specific mitigation.
- All three non-chosen verdicts are presented in their strongest form in the alternatives section.
- The decommissioning date / sunset trigger / review cadence / defer trigger is stated concretely — a specific date, a specific measurable condition, or a specific human.
- The recovery-of-investment sentence is concrete and testable, not a slogan.
- Personal reflection identifies at least one honest surprise from the five-question walk.

## Stretch goals

- **Take the decision doc to a peer for a pre-review.** A staff-plus peer, a senior tech-lead on one of the consumer teams, or a former colleague. Ask them to name the one section they would block on. Absorb the feedback and note it as an addendum. This is a rehearsal for the RFC's pre-write conversation in exercise-04 chapter-5 cadence.
- **Model the political cost with a light matrix.** For each consumer team × each of the three cost components, score `high / medium / low` and sum. This is not a rigorous method (political cost does not add), but it forces you to notice teams where multiple components co-fire — which is where the RFC will burn the most relationship capital.
- **Author the second-best verdict as a shadow decision doc.** In two pages, write what the decision would look like if the verdict had been your second choice. Compare the two side by side. Reviewers will occasionally ask "why not X"; having the shadow doc means you can answer with a paragraph, not a hand-wave.
- **Enumerate the dependent RFCs.** Which follow-up RFCs will this decision trigger over the next two-to-four quarters? A merge in the feature layer often spawns downstream RFCs on the model registry and the eval contract; naming them ahead of time is exactly what a staff-plus-IC roadmap looks like.
