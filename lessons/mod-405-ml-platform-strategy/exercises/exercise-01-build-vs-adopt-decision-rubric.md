# exercise-01: Build-vs-Adopt Decision Rubric

**Estimated effort:** 3 hours

## Objective

Pick **one** of the five ML-platform components (feature store, model registry, eval platform, training platform, inference gateway), score it against chapter 3's seven-criterion weighted rubric, and author the two-to-four-page **build-vs-adopt decision doc** that carries the verdict. After this exercise the learner should hold a defensible verdict (`build` / `adopt-managed` / `adopt-OSS` / `defer`), the scoring table that produced it, the runner-up, and the re-visit trigger that acknowledges the answer will change.

This is the first of the four artifacts that assemble into mod-405's contribution to project-401. Every downstream exercise in this module — the consumer contract (exercise-02), the gap RFC (exercise-03), the contribute-back RFC (exercise-04) — reads against the verdict this exercise commits to. Choose the component you can carry for the whole module.

## Prerequisites

- Chapter 01 — the consumer/producer framing and the "am I doing platform-strategy work?" heuristic.
- Chapter 02 — the five-component vocabulary; you cannot score a component you cannot describe.
- Chapter 03 — the seven-criterion weighted rubric, the two worked examples, and the four common failure modes. Read in full.
- Recommended: one page of Fowler's [*Utility vs. Strategic Dichotomy*](https://martinfowler.com/bliki/UtilityVsStrategicDichotomy.html) to prime criterion 2, and one page of the [Feast concepts](https://docs.feast.dev/getting-started/concepts) or [MLflow Model Registry](https://mlflow.org/docs/latest/model-registry.html) docs for the component you pick.
- If you carried a portfolio through mod-402: the blueprint's painful-cells list. The right component to score here is often the one sitting under a mod-402 cell your blueprint already flagged.

## Choose the component

Pick **one** component. Priority order:

1. If you completed mod-402, pick the component that sits under the highest-pain painful cell in that blueprint. Consistency across exercises is worth more than novelty.
2. Otherwise, pick a component from a *real* org you can describe (current employer, past employer, a well-documented public write-up). Real inputs produce defensible scoring; hypothetical inputs collapse into the fictional-org failure mode chapter 3 warns against.
3. Only if neither of the above applies: pick from the chapter-3 worked examples' setup (small SaaS org for feature store, large consumer-app org for inference gateway) and stipulate the setup at the top of the doc.

Do not try to score two components in this exercise. Two scores in three hours produces one thin doc; one score in three hours produces the artifact.

## Steps

1. **State the org context in one paragraph.** Named: team count, engineer count, current implementation status of the chosen component, one or two most recent pain moments. Chapter 3's worked examples are the shape — three-to-five sentences, not a page. This paragraph is what every criterion below reads against; if it is vague, the scoring will be too.
2. **Name the four verdicts under consideration.** `build` / `adopt-managed` / `adopt-OSS` / `defer`. For each, one sentence naming the specific option you would pick if that verdict won — "adopt-managed = Tecton", not "adopt-managed = some managed feature store." If a verdict is not a real option for this component in this org (e.g., no OSS exists), say so and explain in one sentence.
3. **Walk the seven criteria.** For each criterion 1-7:
    - Re-read the chapter 3 definition; write the criterion name.
    - Write the honest score (1-5) for build vs. adopt for **this** component in **this** org. 1 = strongly favours adopt/defer; 5 = strongly favours build.
    - Write the weight you assign this criterion for this component. Chapter 3's suggested per-component weights are your starting point; if you deviate, justify in one sentence.
    - Write one-to-three sentences of reasoning. This is the sentence the director will push back on — write it as if under adversarial review.
4. **Build the scoring table.** Use the chapter 3 template. Columns: criterion / score / weight / weighted / reasoning. Sum the weighted column to a `build_score`. Compute the ceiling for your weights (`5 × sum-of-weights`).
5. **State the verdict.** One sentence. Include: the winning verdict, the specific option (Tecton / Feast / MLflow-adopt-managed via Databricks / bespoke build / etc.), and the `build_score` as a fraction of the ceiling. If your gut disagrees with the score, chapter 3 named the resolution: either your weights are wrong (argue them in step 3) or your gut was wrong (accept). Do not report a verdict your gut and the score both reject.
6. **State the runner-up and the score gap.** One paragraph. If the runner-up is within 10% of the winner, the verdict is close and the RFC will be reviewed on the criteria that closed the gap; call those criteria out. If the runner-up is more than 30% behind, name why it lost decisively.
7. **Address the two rejected verdicts.** One-to-two sentences each on why they were rejected. Do not skip a verdict because "obviously not"; the chapter 3 failure mode of *scoring for a hypothetical org* is caught here.
8. **State the re-visit trigger.** Chapter 3 was emphatic: a rubric without a re-visit trigger produces a fossil. Named condition: "revisit if the org grows past 25 ML teams", "revisit if Tecton pricing exceeds $500k/yr", "revisit if KServe's control-plane matures to cover canary primitives." A specific, observable, dated condition — not "revisit in a year."
9. **State the consumer-contract implications.** Two-to-four bullets summarising what chapter 4's contract will need to name for this verdict — the SLOs, the migration path, the adoption commitment, the exit cost. This is not the contract itself (exercise-02 authors that); it is the pointer forward that keeps the two exercises coherent.

## Deliverable

A single document, 2-4 pages, containing:

- **Section 1 — Component and current state.** One paragraph (from step 1).
- **Section 2 — Verdicts under consideration.** Four one-sentence descriptions of the specific option under each verdict (step 2).
- **Section 3 — Rubric scoring.** The seven-criterion table with score, weight, weighted, and reasoning per row; the totals and the ceiling (steps 3-4). Weights are named per criterion with a justification when you deviate from chapter 3's suggested weights.
- **Section 4 — Verdict and runner-up.** One-sentence verdict, one-paragraph runner-up analysis with the score gap (steps 5-6).
- **Section 5 — Rejected verdicts.** One-to-two sentences per rejected verdict (step 7).
- **Section 6 — Re-visit trigger.** One-to-two sentences with a specific, observable condition (step 8).
- **Section 7 — Consumer-contract implications.** Two-to-four bullets carried forward to exercise-02 (step 9).
- **Section 8 — Reflection.** Two-to-three sentences: which criterion surprised you? Which criterion was hardest to score honestly?

## Starter guidance

- **Score the real org, not the org you wish you had.** Chapter 3's first common failure mode. If your team has three engineers today, criterion 1 (team-size / readiness) scores 1 for build, not 3-because-we're-hiring. If the hiring changes the score, put it in the re-visit trigger.
- **The weights are reviewable.** Chapter 3's per-component weights are a suggested starting point; you may deviate. But you must justify each deviation in one sentence, in the row's reasoning column. "Bumped criterion 4 (vendor lock-in) from 2 to 4 because our regulatory posture makes any switching cost material" is defensible. Silent re-weighting is not.
- **Do not report the number as the answer.** Chapter 3 was emphatic: the score is *the check on your gut*, not the decision. If gut and score disagree, argue it out in the RFC — that argument is the value the doc delivers.
- **TCO is where drafts get sloppy.** Chapter 3's TCO discipline is the four-row table (engineering / operations / license / opportunity cost) with the 1.5-2× multiplier on build estimates and 3-5× on year-2/3 operations. A TCO row that says "TBD" is a red flag; either write the number or explicitly note the confidence.
- **Two pages is fine.** Do not pad to four. Chapter 3's decision doc has a specific structure and no bonus for length. A tight two-page doc that lands the verdict, runner-up, re-visit trigger, and contract implications is better than a four-page doc with a padded motivation section.
- **Cite one prior-art reference per criterion where the scoring is contested.** Chapter 3's worked examples cite Feast, MLflow, KServe, and the specific vendors under each verdict. Your doc should do the same — a scoring cell that cites zero real substrate options is scoring against imagination.

## Acceptance criteria

- Exactly **one** of the five components is scored; the org context is a named org (or an explicitly stipulated hypothetical) with team count and current implementation state.
- All **seven** criteria are scored with a numeric score (1-5), a weight, and a one-to-three-sentence reasoning column.
- Deviations from chapter 3's suggested weights are justified in the reasoning column.
- The scoring table sums to a `build_score` and the ceiling is computed; the verdict is named as a fraction of ceiling.
- The verdict names the **specific** implementation option (Tecton / Feast / MLflow / KServe / bespoke build / etc.), not the verdict category alone.
- The runner-up is named with the score gap.
- Both rejected verdicts are addressed in one-to-two sentences each; no verdict is skipped as "obviously not."
- The re-visit trigger is a specific, observable, dated condition — not "revisit in a year."
- The consumer-contract implications are named as two-to-four bullets pointing forward to exercise-02.
- The reflection paragraph identifies at least one honest surprise from the scoring.

## Stretch goals

- **Score a second component quickly.** Not a full decision doc — a two-column scoring table only, no reasoning column. Watch how differently the same seven criteria weight when the component changes. This is the discovery that "the platform-strategy conversation" is really five conversations.
- **Shadow-score the runner-up as if it had won.** Two-page write-up of the contract implications, the migration path, and the re-visit trigger if the second-place verdict had actually won. Reviewers will occasionally ask "why not X"; having the shadow doc means you can answer with a paragraph, not a hand-wave.
- **Wardley-map the component.** Draw a rough Wardley map ([Wardley Maps intro](https://medium.com/wardleymaps)) of the component's dependency chain — the visible user need at the top, the substrate components underneath — with each substrate placed on the utility / product / custom axis. Use it to sanity-check criterion 2 (differentiation). A Wardley map that places the component in "utility" while your criterion-2 score says 5-for-build is a discrepancy worth explaining.
- **Take the draft to a peer for a pre-review.** A staff-plus peer, a platform-team lead, or a senior tech-lead on a consumer team. Ask them to name the one criterion score they would push back on. Absorb the feedback as an addendum. This is a rehearsal for the RFC-review conversation that carries the gap RFC and contribute-back RFC in exercises 03-04.
- **Draft the exec one-liner.** In one sentence at the top of the doc: verdict, business impact, next commitment. "We are adopting Tecton for the feature store this quarter; the ranking team's freshness SLA is the load-bearing driver; the consumer contract is targeted for signature by end of Q2." That sentence is what the director will read; the rest of the doc justifies it.
