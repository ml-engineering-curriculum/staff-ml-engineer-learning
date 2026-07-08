# Self-Assessment — Naming the Archetype You Are In and the One You Want To Grow Into

## Motivation

The previous five chapters built up the shared vocabulary — altitude, archetypes, verbs, hand-off contracts, EM partnership. This chapter turns that vocabulary inward. The learner should finish the module with an honest self-assessment that answers three questions:

1. Where does my *current* work centre of gravity fall among the four Larson archetypes?
2. Which archetype do I want to grow into over the next two to four quarters?
3. What are the two or three specific artifacts I need to author, and the two or three habits I need to change, to close the gap?

The answers matter because the rest of the track will land differently depending on them. A learner whose current work is Tech-Lead-archetype and wants to grow into Architect will get different value from mod-402 than a learner whose current work is Right-Hand-adjacent and wants to grow into Solver. This module's investment pays off when the learner uses the self-assessment to *sequence* their reading and their practice through the rest of the track.

Exercise-02 is where the self-assessment gets written. This chapter builds the framework the exercise uses.

## The three-cut self-assessment framework

A workable self-assessment does not try to score every dimension of every archetype. It makes three cuts:

- **Cut 1 — current archetype.** Where is my centre of gravity *today* on the four Larson archetypes?
- **Cut 2 — target archetype.** Where do I want the centre of gravity to be in four quarters?
- **Cut 3 — the gap plan.** Two or three artifacts and habits that close the gap.

Everything below is scaffolding for those three cuts.

## Cut 1 — current archetype

The centre-of-gravity question is answered by looking at *what the last six weeks of calendar actually contained*. Not what the promo doc says, not what the job description says — what the calendar and the artifacts say.

### The calendar audit

Pull the last six weeks of calendar. Categorise every meeting and every deep-work block by the *primary object of the verb* — using the taxonomy from chapter 3. Use these buckets:

- **Single-team execution** — meetings and deep work whose object is one system on one team. (Senior-tech-lead-shaped.)
- **Multi-team architecture / substrate** — meetings and artifacts whose object is a portfolio, a shared substrate, a cross-team RFC. (Architect-shaped.)
- **Roaming investigation** — sustained deep work on an ambiguous cross-team problem the Director escalated. (Solver-shaped.)
- **Executive-adjacent synthesis** — briefings, one-pagers, exec meetings representing the ML org, Director's second-brain work. (Right-Hand-shaped.)
- **Team-embedded roadmap / hiring / coaching** — running the two-quarter roadmap for one team, calibrating hires for one team, coaching that team's L30s. (Tech-Lead-shaped.)
- **People-management-shaped** — 1:1s, performance conversations, headcount discussions. (EM-shaped — flag if this is more than 5-10%.)

Aggregate the fractions. The bucket with the highest fraction is a strong signal of the current centre of gravity.

Two calibration warnings:

- **Filter out prep time.** Two hours of prep for a 30-minute exec briefing counts as executive-adjacent synthesis, not deep work on the underlying system. Otherwise Right-Hand work is systematically undercounted.
- **Handle the "invisible" hours.** Off-calendar work — reading RFCs, coaching over Slack, writing hand-off contracts — has to be included. A calendar-only audit systematically undercounts the Architect and Right Hand archetypes because their work is quieter.

### The artifact audit

The calendar audit is corroborated by looking at what was *produced* in the last six weeks. Every archetype has a signature artifact set:

| Archetype | Signature artifact |
|---|---|
| Tech Lead | Single-team roadmap, single-team hiring calibration, single-team architecture RFC |
| Architect | Multi-team substrate RFC, cross-team standards doc, portfolio-scope architecture blueprint |
| Solver | Cross-team investigation writeup, failure-mode budget update, fix RFC for a Director-escalated problem |
| Right Hand | Executive one-pagers, exec-briefing synthesis, cross-org capacity ask, Director-audience TCO memo |

If the calendar and the artifacts disagree, trust the artifacts. What the engineer *shipped* is a better signal than how they *spent their day*.

### The two-tag rule

Most Staff engineers spend most of their time in *one dominant archetype* and *one secondary archetype*. Very few spend all their time in one, and none healthily hold all four. The self-assessment output for Cut 1 is:

> *"My centre of gravity over the last six weeks is [dominant archetype], with a secondary presence in [secondary archetype]. Evidence: [3-5 line summary of calendar + artifact audit]."*

Two tags — dominant plus secondary — is the honest answer. One tag is usually incomplete; three or four tags usually means the audit was not honest.

## Cut 2 — target archetype

The target archetype is the shape the learner wants the work to take in four quarters. This is where the module has the most leverage — most Staff engineers who stall do so because they never named the target, so they never noticed they were still in the same shape a year later.

Two calibration checks:

- **The target should be reachable in four quarters, not eight.** "I want to be a Right Hand to the CTO in five years" is a long-term aspiration, not a target. The four-quarter target is *the archetype the engineer wants to be identifiably operating in by the end of the year*, given the current organisation.
- **The target should be plausible given the organisation.** If the ML org does not have an executive layer deep enough to need a Right Hand, targeting Right Hand at this company is targeting a role that does not exist. Chapter 2 covered which organisations hire which archetypes; use that filter.

The target archetype can be the *same* as the current archetype — that is a valid outcome, and means the learner wants to deepen rather than transform. It can be an adjacent archetype (Tech Lead → Architect is the most common transition on the ML ladder). It should rarely be the *opposite* archetype (Tech Lead → Right Hand is a large shape change and often requires a change of company or executive-pair partner to land).

The output for Cut 2 is:

> *"In four quarters I want my centre of gravity to be [target archetype], because [one sentence on why this is the next expansion of scope]. This is plausible given [one sentence on why the organisation has room for this archetype]."*

## Cut 3 — the gap plan

The gap plan is the actionable half of the assessment. It has two components: *artifacts to author* and *habits to change*.

### Artifacts to author

Pick two or three artifacts whose authorship *would only happen if the target archetype were the centre of gravity*. Each archetype has canonical artifacts (see the artifact table above); the target archetype's artifacts are the ones to pick.

Examples of the artifacts-to-author component for each transition:

| Transition | Two or three artifacts to author in the next four quarters |
|---|---|
| Tech Lead → Architect | (1) The multi-team substrate RFC for a shared area (mod-402). (2) The cross-team standards doc for one shared concern — eval standard, model-registry consumer contract, promotion-gate policy (mod-406, mod-405). (3) The portfolio-scope architecture blueprint (project-401). |
| Tech Lead → Solver | (1) A written investigation writeup for one cross-team ambiguity — pick the one the Director already wishes were owned. (2) The hand-off contract to the team that will own the fix long-term. (3) The failure-mode-budget update that results (mod-404, mod-408). |
| Architect → Right Hand | (1) The executive one-pager for one program (mod-410). (2) The TCO memo for the ML org's cluster ask (mod-407). (3) The cross-org capacity synthesis (mod-407). |
| Tech Lead → deeper Tech Lead | (1) The multi-quarter roadmap that leadership commits to (mod-410). (2) The hiring calibration artifact for the L30 loop (mod-410). (3) The L30 coaching curriculum for the team's senior engineers. |
| Right Hand → Architect | (1) An owned technical area (usually the highest-leverage substrate the Right Hand has been briefing on). (2) Recurring authorship of that area's RFCs, not just synthesis of others' RFCs. |

The rest of the track — every subsequent module and every project — is structured so that authoring these artifacts is what the exercises and projects produce. The self-assessment is what tells the learner which artifacts to prioritise.

### Habits to change

Alongside the artifacts, name two or three *habits* that would let the target archetype take root. Habits are more important than artifacts because a good habit produces the artifacts on its own.

Examples per transition:

| Transition | Two or three habits |
|---|---|
| Tech Lead → Architect | (1) Read every RFC from the adjacent modelling teams weekly. (2) Author a two-page consolidation memo once a month naming the pattern across teams. (3) Attend the peer team's design reviews without owning them. |
| Tech Lead → Solver | (1) Ask the Director once a quarter: "what is the ambiguous cross-team problem I could take?" (2) Time-box each investigation with a hand-off deadline. (3) Publish the writeup before returning to the home team. |
| Architect → Right Hand | (1) Attend the Director's exec staff meeting weekly. (2) Author the exec framing of one program per month, unprompted. (3) Read one CFO-adjacent artifact per month (capacity plan, quarterly close). |
| Tech Lead → deeper Tech Lead | (1) Stop reviewing the same PRs as the team's L30s — coach the L30s to review them instead. (2) Refresh the two-quarter roadmap every month, not once a quarter. (3) Sit on more L30 hiring loops. |

### Habits to *stop* — the un-doing half of the plan

Growing into a new archetype requires stopping some current work, not just adding new work. Every self-assessment should name at least one thing the learner will *stop* doing. Common examples:

- Stop reviewing every model-training PR — coach L30s to review them.
- Stop being on the daily stand-up of one team — attend once a week.
- Stop authoring one team's model card — coach the team's Senior tech lead to author it.
- Stop being the SME on a specific vendor tool — coach a peer specialist to be it.

The habits-to-stop list is the single most common way self-assessments fail. Learners eagerly write the new habits; they leave the calendar full of the old habits; nothing new lands.

## The output document

Exercise-02 asks the learner to produce a short document — two-to-three pages — that captures:

- Current archetype (dominant + secondary), with calendar + artifact evidence.
- Target archetype in four quarters, with reachability and organisational-plausibility checks.
- Gap plan: two or three artifacts to author, two or three habits to start, one or two habits to stop.
- Two or three module-and-project pointers from the rest of the track that the plan will draw on.
- A refresh cadence — quarterly at minimum.

The document is the learner's own; it is not submitted to a manager. It is best refreshed every quarter and re-read at every roadmap review.

## The relationship between this assessment and a promotion packet

The self-assessment is *not* a promotion packet. Two important distinctions:

- **Audience.** The self-assessment is for the learner and (optionally) their EM peer or coach. A promotion packet is for a promo committee and follows the company's specific rubric.
- **Framing.** The self-assessment is honest about where the learner currently is — including where the current work is L30-shaped. A promotion packet is a *case* for a specific level and highlights evidence for that case.

That said, a self-assessment kept honest over four quarters usually produces a strong promotion packet as a by-product. The artifacts named in Cut 3 are the exact artifacts a promo packet cites; the target archetype is the shape the packet argues for; the habits-to-stop list is what the packet does not need to defend.

The Staff-plus IC promotion patterns are treated in more depth in mod-410 (Staff-Plus Technical Leadership).

## Common failure modes when writing the self-assessment

- **Aspirational current-archetype tag.** Naming *"Architect"* as the current tag when the calendar and artifacts read Tech Lead. This makes Cut 2 (target) meaningless.
- **Target archetype the organisation cannot support.** Targeting Right Hand in an org with no exec pair, or Solver in an org with no cross-team ambiguity budget.
- **Gap plan is only additive.** Two or three new artifacts and no un-doing. The calendar stays full; no artifact lands.
- **Gap plan is not artifact-shaped.** *"I will improve my strategic thinking"* is not a plan. *"I will author the shared retrieval substrate RFC by Q3"* is.
- **No refresh cadence.** Self-assessments not refreshed drift out of usefulness in a quarter.

## Summary

- The self-assessment answers three questions: where the learner's current archetype is, which archetype they want to grow into in four quarters, and the two-or-three-artifact-plus-two-or-three-habit gap plan that closes the distance.
- Cut 1 (current) is anchored to a calendar audit and an artifact audit over the last six weeks; two tags (dominant plus secondary) is the honest output.
- Cut 2 (target) has to be reachable in four quarters and plausible in the current organisation; adjacent-archetype transitions are the most common healthy path.
- Cut 3 (gap plan) requires artifacts to author *and* habits to change — including at least one habit to stop. The habits-to-stop list is where most self-assessments fail.
- The self-assessment is refreshed once a quarter and re-read at every roadmap review. Kept honest over four quarters, it produces a strong promotion packet as a by-product.

Exercise-02 authors this self-assessment document. Together with exercise-01 (staff verb audit against postings), exercise-03 (staff/EM partnership contract), and exercise-04 (hand-off map to peer tracks), it closes mod-401 and hands off to mod-402, which starts the multi-team ML architecture work.
