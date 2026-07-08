# mod-401-staff-ml-role-scope: The Staff ML Engineer Role — Scope, Archetypes, and Multi-Team Contracts

**Estimated effort:** 10 hours

Introductory module for the Staff Machine Learning Engineer track. Names the staff-plus IC altitude, the four Larson archetypes as they land in ML, the staff-defining verbs versus Senior and EM verbs, the hand-off contracts to peer specialist and peer platform tracks, and the partnership contract with the peer L40 Engineering Manager. Closes with a personal self-assessment that sequences the rest of the track.

## Learning objectives

- Articulate what a Staff ML Engineer owns end-to-end vs. what a Senior tech-lead (L30) owns and what a Principal (L50) owns — using the staff-plus IC archetypes (Tech Lead, Architect, Solver, Right Hand) from Larson's *Staff Engineer* book as the vocabulary.
- Read a job posting and identify the staff-defining verbs — *architect across teams*, *set multi-quarter direction*, *own the paved road contract*, *calibrate senior engineers*, *sponsor the RFC*, *partner with executives* — and distinguish them from Senior verbs and from EM verbs.
- Author the hand-off contracts to peer specialist and peer platform tracks — where staff scope ends and another track begins — and to the peer level-40 EM partner.
- Write a self-assessment that names the staff archetype the learner is closest to today and the archetype they want to grow into.

## Lecture chapters

1. [`01-what-staff-plus-altitude-means.md`](01-what-staff-plus-altitude-means.md) — What "staff-plus" altitude actually means. The three altitude shifts (object-of-the-verb widening, time-horizon lengthening, influence-surface widening), the L40 mandate, and the defer-down / defer-up / defer-sideways rules.
2. [`02-four-staff-archetypes-in-ml.md`](02-four-staff-archetypes-in-ml.md) — The four Larson archetypes (Tech Lead, Architect, Solver, Right Hand) translated to ML, each with signature artifacts and the archetype-specific failure mode.
3. [`03-reading-job-postings-for-staff-verbs.md`](03-reading-job-postings-for-staff-verbs.md) — Reading job postings by extracting verb-object pairs and classifying them Senior / Staff / EM. Anti-patterns to recognise (Senior-plus-plus, Staff/EM hybrid, unspoken Right Hand, unspoken Solver).
4. [`04-hand-off-contracts-to-peer-tracks.md`](04-hand-off-contracts-to-peer-tracks.md) — The six-section hand-off-contract shape for peer specialist, peer platform, and peer governance tracks. Three worked examples plus the "peer track not staffed yet" case.
5. [`05-staff-and-em-partnership.md`](05-staff-and-em-partnership.md) — The partnership contract with the peer L40 Engineering Manager. Nine boundaries, the one-page template, and the two special cases (no EM peer yet, new EM peer).
6. [`06-self-assessment-and-growth-map.md`](06-self-assessment-and-growth-map.md) — Three-cut self-assessment framework (current archetype from calendar + artifact audit, target archetype, gap plan) plus the artifacts-and-habits mechanics.

## Exercises

- [`exercises/exercise-01-staff-verb-audit-against-postings.md`](exercises/exercise-01-staff-verb-audit-against-postings.md) — Staff verb audit against five real, in-window postings. 2 hours.
- [`exercises/exercise-02-archetype-mapping-and-self-placement.md`](exercises/exercise-02-archetype-mapping-and-self-placement.md) — Archetype mapping and self-placement using the three-cut framework. 3 hours.
- [`exercises/exercise-03-staff-vs-em-partnership-contract.md`](exercises/exercise-03-staff-vs-em-partnership-contract.md) — Author the Staff / EM partnership contract for a concrete team. 2 hours.
- [`exercises/exercise-04-hand-off-map-to-peer-tracks.md`](exercises/exercise-04-hand-off-map-to-peer-tracks.md) — Author three hand-off contracts (peer specialist, peer platform, peer governance) for a Staff ML program. 2 hours.

## Structure

```
mod-401-staff-ml-role-scope/
├── 01-…md … 06-…md    lecture chapters
├── exercises/          per-exercise prompts (solutions live in the paired solutions repo)
├── labs/               long-form hands-on labs (planned)
├── quizzes/            knowledge checks (planned)
├── resources.md        curated external references
└── README.md           this file
```

## Suggested sequencing

Read all six chapters in order. Then take exercise-01 (verb audit) before exercise-02 (self-placement) — the audit skill anchors an honest current-archetype tag. Take exercise-03 (Staff/EM contract) and exercise-04 (peer-track hand-offs) in either order; the pair produces the full four-side scope map that mod-402 (Multi-Team ML Architecture) builds on.

## What comes next

- **[`mod-402-multi-team-ml-architecture`](../mod-402-multi-team-ml-architecture/)** — the multi-team architecture blueprint work; the target artifact for learners whose Cut 2 (from exercise-02) is Architect.
- **[`mod-410-staff-plus-technical-leadership`](../mod-410-staff-plus-technical-leadership/)** — the multi-quarter roadmap, exec comms, hiring calibration, and senior-tech-lead coaching work; the target artifact for learners deepening as Tech-Lead-archetype or Right-Hand-archetype.
- **[`project-403-staff-plus-tech-leadership-simulation`](../../projects/project-403-staff-plus-tech-leadership-simulation/)** — the capstone that consumes the artifacts produced across mod-401, mod-410, and other track modules.

## Related tracks

- [`senior-ml-engineer-learning`](https://github.com/ml-engineering-curriculum/senior-ml-engineer-learning) (L30) — the prerequisite single-team tech-lead track. See [`PREREQUISITES.md`](../../PREREQUISITES.md).
- [`ai-infra-team-lead-learning`](https://github.com/ai-infra-curriculum/ai-infra-team-lead-learning) (L40 EM) — the peer people-leadership track; chapter 5's partnership is with this role.
- The peer specialist and peer platform tracks — enumerated in [`../../PREREQUISITES.md`](../../PREREQUISITES.md); chapter 4's hand-off contracts are to these tracks.
