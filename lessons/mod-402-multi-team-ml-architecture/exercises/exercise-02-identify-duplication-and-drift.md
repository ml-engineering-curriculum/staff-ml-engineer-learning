# exercise-02: Identify Duplication and Drift

**Estimated effort:** 3 hours

## Objective

Take the `duplicated` and `forked` cells from exercise-01's blueprint matrix and turn each of them into a **precise diagnostic** against the four portfolio-scope failure modes from chapter 3. After this exercise the learner should have, per painful cell, a named failure mode, evidence for the failure (or evidence that it is *not* a failure yet), and enough context to author a merge / split / share decision in exercise-03.

The exercise sharpens two skills that are hard to grow without deliberate practice: **distinguishing the four failure modes** (which look similar on first read but require different fixes) and **grounding a claim in evidence** rather than in staff-plus-IC gut feel.

## Prerequisites

- Exercise 01 complete, with the painful-cells table populated.
- Chapter 03 — Portfolio-Scope Failure Modes read in full. The four modes, their mechanisms, their diagnostic questions, and their anti-patterns.
- Skim of Sculley et al., "Hidden Technical Debt in Machine Learning Systems" (NIPS 2015). Read the abstract and sections 2 (CACE) and 4 (Data Dependencies). Twenty minutes.

## Choose the cells

Pick **three-to-five painful cells** from exercise-01's table. The distribution should be, if the blueprint supports it:

- At least one cell each from **two different failure modes** — so the diagnostic skill has to distinguish them.
- At least one cell where you *suspect* a failure but do not yet have evidence. This is where the exercise is most valuable.
- At most one cell per system pair, so the exercise doesn't collapse into a single-relationship deep-dive.

If exercise-01's painful-cells table has fewer than three entries, the portfolio is probably too clean — either you soft-pedalled the interviews (revisit them) or your portfolio genuinely is well-consolidated (rare, but real). If the latter, use a public reference architecture for this exercise instead.

## Steps

For each of the three-to-five cells:

1. **Name the failure mode.** Assign the cell to one of: `pipeline-jungle`, `duplicated-feature-drift`, `model-registry-fragmentation`, `eval-metric-conflict`, or `silent-contract-erosion`. If the cell fits none of the five, either the cell is not a failure or your labelling is off — argue for one of the two.
2. **Run the three diagnostic questions from chapter 3 for that failure mode.** Write down the answers. Cite the team member who provided each answer or the reference doc that supplied it.
3. **Collect the evidence.** For each answer, note the concrete evidence: a drift measurement, an incident number, a spreadsheet, an exec ask, a slack thread. If no concrete evidence exists, mark the cell as `suspected but not evidenced`. This is a legitimate outcome — chapter 4's *defer* verdict is often the right response.
4. **Check for the anti-pattern.** Every failure mode in chapter 3 has an anti-pattern that looks like it but is not. Explicitly rule out the anti-pattern in one-to-two sentences. This step is what separates a defensible diagnosis from staff-plus IC hand-wave.
5. **Rate the pain.** On a three-point scale (`low`, `medium`, `high`) score:
    - **Frequency** — how often the failure produces an observable symptom.
    - **Blast radius** — how many teams are affected each time.
    - **Recovery cost** — engineering hours (order of magnitude) to fix a single instance.
6. **Note the composition.** If the cell composes with a cell you already diagnosed — chapter 3's pipeline-jungle → feature-drift or registry-fragmentation → eval-conflict patterns — write it down. The composition determines the ordering of exercise-03's decisions.

## Deliverable

A single document, 3-5 pages, containing:

- **Section 1** — Recap of the painful-cells subset chosen (why these three-to-five).
- **Section 2** — Per-cell diagnostic (one sub-section per cell) with:
    - Failure mode label.
    - Answers to the three diagnostic questions from chapter 3.
    - Evidence per answer, with citations.
    - Anti-pattern check.
    - Pain scoring (frequency / blast radius / recovery cost).
- **Section 3** — Composition observations. Which of the diagnosed cells compose with each other. Which cell will need to be decided first.
- **Section 4** — Three-sentence reflection. Which failure mode was hardest to distinguish from its anti-pattern? Where did you have to go collect evidence and could not get it?

## Starter guidance

- **Do the anti-pattern check honestly.** Learners under time pressure skip step 4 and rediscover it in exercise-03 when the decision doesn't quite make sense. Two sentences ruling out the anti-pattern take three minutes and save an hour.
- **Do not conflate "features have different names" with "features have different semantics".** The chapter 3 anti-pattern for duplicated-feature drift is same-name-two-semantics; the failure is same-semantics-two-implementations. Look at the underlying compute, not the label.
- **When the evidence is thin, say so.** A cell marked `suspected but not evidenced` is a legitimate output. It becomes a **defer** verdict in exercise-03 with a data-collection commitment. Chapter 4 explicitly authorises this.
- **Watch for the fifth failure mode.** Silent contract erosion (chapter 3's bonus) is under-reported because it is boring. If you can find any substrate whose "shared contract" wiki page is more than six months stale, it is on the list.
- **Do not diagnose your own team's cells more harshly than others'.** Learners are often over-critical of their prior team's work and under-critical of adjacent teams' work. Use the same three diagnostic questions on every cell.

## Acceptance criteria

- Three-to-five painful cells chosen and justified.
- Every cell has a failure-mode label; if `none of the above`, the cell is defended as either non-failure or non-classifiable.
- Every cell has answers to the three chapter-3 diagnostic questions, with concrete evidence cited or explicitly marked as `unevidenced`.
- Every cell has an explicit anti-pattern check — one-to-two sentences ruling out the anti-pattern for that failure mode.
- Every cell has a three-axis pain score (frequency / blast radius / recovery cost).
- Composition table lists at least one pair of cells that compose, or explicitly notes that no composition was found (which is unusual — verify).
- The reflection identifies one specific spot the evidence was thin, which is the input to a follow-up data-collection commitment in exercise-03 or -04.

## Stretch goals

- **Measure the drift.** For one duplicated-feature cell, actually diff the two definitions against the same one-week slice of data. Report the absolute-and-relative disagreement on a single well-known feature (e.g. `user_7d_purchase_count`). This is the evidence chapter 5 wants for section 3 of the RFC.
- **Reconstruct the substrate's contract.** For one shared-behind-a-contract cell, find the wiki page that documents the contract, then diff the contract against the actual behaviour reported by one consumer team. If the diff is longer than a paragraph, chapter 3's silent-contract-erosion failure applies; add the cell to the diagnostic set.
- **Author a one-slide diagnostic summary** — a single slide the staff engineer could bring to a peer-staff or peer-EM 1:1 that summarises the top-three painful cells and their failure modes. This is a common preliminary artifact before an RFC opens; practice it.
- **Score the same portfolio against the four failure modes over time.** If you have access to older docs (six-to-twelve months back), score the same cells at that earlier time and note which cells are getting worse, which are stable, which are improving. Trend direction is a meaningful input to the *defer* verdict.
