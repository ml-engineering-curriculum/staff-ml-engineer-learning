# exercise-03: Staff / EM Partnership Contract

**Estimated effort:** 2 hours

## Objective

Author a working one-page Staff / EM partnership contract for a concrete team — real or realistic — that names, for each of the nine boundaries from chapter 5, which side of the L40 partnership owns which verbs. The contract must be defensible from *both* sides — a peer L40 EM reading it should recognise their own mandate on the EM side of the ledger, not just accept an IC-favourable framing.

## Prerequisites

- Chapter 05 — The Staff-Plus IC and the Peer L40 Engineering Manager. The nine boundaries, the one-page template, the special cases, and the failure modes come from this chapter.
- Chapter 01 — for the L40 altitude reference and the "the same seniority, different discipline" framing.
- Chapter 03 — the verb classification vocabulary is reused when writing each boundary's rows.

## Scenario

Pick **one** of the following two scenarios for the exercise. Both are within-track and grounded in the module's coverage; both produce a comparable deliverable.

### Scenario A — your current team

Author the partnership contract for the L40 pair on *your current team* (or the most recent one you were on). Both sides — Staff ML and peer EM — should be real people whose mandates you know. This scenario is the highest-leverage version of the exercise because the contract is immediately actionable: you can share the draft with your peer EM and ratify a real quarterly refresh.

### Scenario B — a realistic composite team

Author the contract for a *composite* team, sketched below. Use this scenario if scenario A is not viable (early-career learner, no current L40 pair, or organisation-sensitive material).

- **Team.** Marketplace ranking team inside a mid-sized consumer marketplace ML org. 8 ML engineers: 1 L40 Staff ML (you), 1 L40 EM (peer), 3 L30 Seniors, 3 L20 mid-levels.
- **Roadmap.** Two-quarter horizon: (a) migrate the ranker from a gradient-boosted tree to a neural retrieval + ranker; (b) adopt the org's shared model-registry paved-road (owned by the ML platform team); (c) hire two L30 Seniors to fill capacity gaps.
- **Cross-team dependencies.** Peer ranker teams on adjacent surfaces (feed, notifications). Peer ML platform team owning the model registry. Peer training-pipeline specialist team supporting the neural training run.
- **Common lead.** Director of ML.

Both scenarios use the same nine-boundary template.

## Steps

1. **Restate the mandate for both roles in your own words.** Two paragraphs — one for the Staff side, one for the EM side — that name the L40 mandate for this specific team. This is where you calibrate the boundary decisions in the following steps.
2. **Fill in the nine boundaries.** For each of the nine boundaries from chapter 5 — technical direction, roadmap, hiring, career and coaching, delivery and on-call, exec comms, program vs. team rhythm, conflict resolution, scope changes — fill in a row of the template with the Staff-owns and the EM-owns columns. Use the verb vocabulary from chapter 3. Every row must have specific verbs and specific objects; "collaborates on X" is not a boundary decision.
3. **Author the escalation section.** For each boundary, name the escalation path when it is contested. In most cases this is "common lead" (the Director); some are "peer Staff/EM in adjacent team" if the ambiguity is cross-team.
4. **Author the non-goals section.** Name at least three things *neither* side will hold — usually because they belong to a peer specialist track (chapter 4), a peer platform track (chapter 4), or the peer L40 EM on an *adjacent* team.
5. **Set the cadences.** Weekly Staff/EM 1:1, monthly roadmap sync, quarterly contract refresh. If there are additional cadences the team relies on, add them.
6. **Adversarial re-read from the EM side.** Sit in the EM's chair and read your draft. Would they recognise the mandate? Is there any boundary where you have taken the more-fun side and left the EM with the residual? Any boundary where the "Staff owns" language sounds like a veto over the EM's authority? Rewrite until both sides read as balanced.
7. **Stress-test with the failure modes.** Chapter 5 lists four common failure modes (combined Staff/EM, Staff-as-shadow-EM, EM-as-shadow-Staff, silent boundary drift). For each, note in one sentence whether your draft protects against it.

## Deliverable

A single markdown document, 2-3 pages:

- **Section 1** — the two-paragraph restatement of both mandates for this team.
- **Section 2** — the nine-boundary table (Staff / EM columns), filled in with specific verbs and objects.
- **Section 3** — escalation paths.
- **Section 4** — non-goals (at least three).
- **Section 5** — cadence list.
- **Section 6** — a short "failure-mode protection" checklist re-reading the draft against the four failure modes from chapter 5.
- **Section 7** — one paragraph on what you would do differently in the two special cases from chapter 5 (no EM peer yet; EM peer new to the technical bar), for the same team.

## Starter guidance

- **Verbs and objects. Not adjectives.** Do not write "Staff owns technical direction" as a row; write "Staff owns the ranker architecture RFC, the design-review verdict, and the code-review bar." Chapter 5's table is the vocabulary source.
- **Both sides get to hold *good* work.** A common self-favouring bias is to hand every "strategic" verb to the Staff side and every "operational" verb to the EM side. The EM's operational work is where the team's delivery actually lands; it is L40 work, not lesser work. If the EM side of your draft reads as chore-heavy, rewrite.
- **Non-goals are the section learners most often skimp on.** They are also the section that saves the most conflict later. Three non-goals minimum. Chapter 4's peer-track hand-offs (specialist, platform, governance) usually provide them.
- **Adversarial re-read is non-optional.** Step 6 is where the exercise clicks. A contract that reads well from your own seat but not from the EM's is not yet a partnership contract; it is a wish list.
- **Do not name individuals in the contract itself.** Chapter 4 says the same about peer-track hand-offs. Individuals rotate; roles do not.
- **If you picked scenario A, share the draft with your peer EM.** The contract only becomes real when both sides sign it. Getting the EM's mark-up back is *the* stretch goal for scenario A learners.

## Acceptance criteria

- Both L40 mandates for this team are restated in the learner's own words.
- All nine boundaries are filled in with specific verbs and specific objects — no adjective-only rows.
- Escalation paths are named for every contested boundary.
- At least three non-goals are named, each pointing at the track that owns the deferred work.
- Cadences are set with owner and duration.
- The draft is adversarially re-read from the EM seat and rewritten until it is defensible from both sides.
- The four failure modes from chapter 5 are addressed explicitly.
- The two special cases from chapter 5 are addressed with a paragraph each on the adjustments the learner would make.

## Stretch goals

- **Get the EM's real markup.** If you used scenario A, share the draft with the actual EM and record the changes. If the EM disagreed with a boundary, why? What did you learn about your own scope assumptions?
- **Author the mirror-image document from the EM's seat.** Take the same team and write the contract as if you were the L40 EM. Compare the two — where do the drafts converge and where do they diverge? Divergences are usually where the real boundary work is.
- **Extend to the extended-L40 pod.** Real ML teams often have one Staff engineer paired with two EMs (e.g., the modelling EM and the ML-platform-consumer EM), or two Staff engineers paired with one EM. Redraft the contract for one of these extended-pod shapes. Which boundaries need to split three ways? Which do not?
- **Draft the "L40 pair pre-mortem".** Six months into a hypothetical bad version of this partnership, what would have gone wrong? Which boundary would silently have drifted? What would you have wished the contract had said differently? This is a mod-408-flavoured practice on multi-team partnership failure modes.
- **Cross-reference the outcome with mod-410.** Mod-410 covers staff-plus hiring calibration, exec comms, and roadmap authorship in more depth. Which of the boundaries in your contract feed inputs into mod-410 topics? Which of mod-410's topics will require you to revisit this contract in a few quarters?
