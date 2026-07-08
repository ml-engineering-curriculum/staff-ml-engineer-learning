# exercise-02: Archetype Mapping and Self-Placement

**Estimated effort:** 3 hours

## Objective

Produce an honest, artifact-grounded self-assessment naming: (1) the Larson archetype the learner is closest to *today*, (2) the archetype the learner wants to grow into over the next four quarters, and (3) the two-or-three-artifact-plus-two-or-three-habit gap plan that closes the distance. The self-assessment is the personal-plan artifact that the rest of the track loads its practice against.

## Prerequisites

- Chapter 06 — Self-Assessment — Naming the Archetype You Are In and the One You Want To Grow Into. The three-cut framework and the failure modes come from this chapter.
- Chapter 02 — The Four Staff-Plus Archetypes (for the archetype definitions and the signature artifacts per archetype).
- Chapter 01 — What "Staff-Plus" Altitude Actually Means (for the L30 vs L40 vs L50 altitude reference).
- Exercise-01 — the verb-audit skill lets the learner honestly classify their own *last-six-weeks* verbs, which is what Cut 1 is anchored on.

## Materials to collect

- **Six weeks of calendar history.** Pull it from your work calendar. If your calendar is empty because the work is off-calendar, allow an extra 20 minutes to reconstruct it from Slack, email, and Git history.
- **Everything you *shipped* over the same six weeks.** Merged PRs, published RFCs, roadmap docs, hiring calibrations, exec briefings, investigations. If nothing was shipped, that itself is a data point.
- **The list of modules and projects in [`../../CURRICULUM.md`](../../CURRICULUM.md)** — the gap plan will point to a subset of them.

## Steps

### Cut 1 — Current archetype

1. **Calendar audit.** Bucket every meeting and every deep-work block over the last six weeks into one of the six categories from chapter 6: single-team execution, multi-team architecture / substrate, roaming investigation, executive-adjacent synthesis, team-embedded roadmap / hiring / coaching, people-management-shaped. Include off-calendar work.
2. **Artifact audit.** List every artifact shipped and label each with its signature archetype (chapter 2 has the table).
3. **Reconcile.** If the calendar and the artifacts disagree, follow the chapter-6 rule: trust the artifacts. Note where they disagreed and why.
4. **State the two tags.** Write the dominant archetype and the secondary archetype, plus 3-5 lines of evidence.

### Cut 2 — Target archetype

5. **Name the target archetype.** State the archetype you want to be identifiably operating in by four quarters from today.
6. **Reachability check.** Is this a same-shape-deepen, an adjacent-archetype transition, or an opposite-archetype transition? Chapter 6 flags adjacent as the most common healthy path. If it is an opposite transition, name explicitly what makes it plausible (change of team, change of executive pair, change of company).
7. **Organisational plausibility check.** Does your organisation actually hire and support this archetype? Chapter 2's "Which archetype does your organisation actually hire?" section is the reference. If the archetype does not exist in your current org, either name the change that would create it or acknowledge the target implies a role change.

### Cut 3 — Gap plan

8. **Two or three artifacts to author.** Pick from chapter 6's transition tables *or* propose your own. Each artifact must be specific enough to declare done — "the shared retrieval substrate RFC by Q3", not "an architecture doc sometime this year". Point each artifact to the module (mod-402 through mod-410) or project (project-401 through project-403) that produces it.
9. **Two or three habits to start.** These are recurring behaviours, not artifacts. "Attend the Director's exec staff meeting weekly" is a habit; "author an exec one-pager" is an artifact.
10. **One or two habits to stop.** Chapter 6 flags this as the single most common way self-assessments fail. Name specific work you will hand off or drop — chapter 4's hand-off-contract vocabulary is the mechanism.
11. **Set a refresh cadence.** Quarterly at minimum.

## Deliverable

A single markdown document, 2-3 pages, structured:

- **Section 1 — Cut 1: Current archetype.** Two-tag statement plus evidence from the calendar and artifact audit.
- **Section 2 — Cut 2: Target archetype.** Statement plus reachability and organisational plausibility checks.
- **Section 3 — Cut 3: Gap plan.** Two-or-three artifacts to author, two-or-three habits to start, one-or-two habits to stop, each with a target quarter.
- **Section 4 — Module and project sequencing.** Which two-or-three of the remaining modules and projects in this track will you sequence first because of Cuts 1 through 3?
- **Section 5 — Refresh cadence.** Date of first refresh; who (if anyone) will read the refreshed version.

The audience is *you*. You may optionally share it with your EM peer, your Director, or a coach, but the primary audience is the learner.

## Starter guidance

- **Do the calendar audit first, before you *think* about which archetype you're in.** Chapter 6 flags the "aspirational current-archetype tag" failure mode — naming Architect when the calendar says Tech Lead. The audit forces honesty.
- **Two tags, not one.** Almost no Staff engineer sits purely inside one archetype. Two tags — dominant plus secondary — is the honest output.
- **The target should be reachable in four quarters, not eight.** Chapter 6 makes this specific: what shape can you be identifiably operating in by year-end, in your current org, with the L40 EM you actually have.
- **Artifacts, not adjectives.** "I want to be more strategic" is not a gap plan. "I want to have authored the shared eval standard by Q4" is.
- **The habits-to-stop list is not optional.** If your calendar stays full of the same work, no new artifact will land. Force yourself to write down at least one thing you will hand off — chapter 4 taught you how to write the hand-off contract.
- **This document is not a promotion packet.** Chapter 6 explains the distinction. Do not try to make yourself look promotable; try to be honest about where the work is today.
- **Sequence the rest of the track from the gap plan.** The final section of the deliverable is where the self-assessment pays off — it tells you which modules and projects to prioritise. If Cut 2 is Tech Lead → Architect, mod-402 and project-401 sequence first. If Cut 2 is Architect → Right Hand, mod-410 and project-403 sequence first.

## Acceptance criteria

- Cut 1 is anchored to a real six-week calendar audit and a real artifact audit; both are summarised in the document as evidence.
- Cut 1 states two archetype tags (dominant + secondary) with a 3-5 line evidence block.
- Cut 2 states a specific target archetype, an explicit reachability classification (deepen / adjacent / opposite), and a specific organisational-plausibility check.
- Cut 3 names two or three concrete artifacts (each with a target quarter and a mapped module or project), two or three habits to start, and at least one habit to stop.
- The document names which two or three of the remaining modules/projects this learner will sequence next.
- A refresh cadence is set (date + optional audience).

## Stretch goals

- **Extend the calendar audit to twelve weeks** rather than six. Compare the archetype-mix trajectory over quarter one vs. quarter two. Is the current archetype consistent, or has it been drifting?
- **Author the mirror-image self-assessment for your peer L40 EM.** Where is *their* centre of gravity across their L40 mandate (from chapter 5's nine boundaries)? Where would they say the partnership contract needs to shift? Compare notes. This is the single fastest way to strengthen the boundary between the two roles.
- **Build a two-quarter check-in template.** Given your gap plan, what would evidence of progress look like on the day of the two-quarter check-in? Preview the check-in *before* the two quarters happen — chapter 6 flags "no refresh cadence" as a failure mode; a pre-declared check-in template kills it.
- **Ask three peers (a peer Staff engineer, an EM, a Director) to independently name the archetype they think you are currently in.** Compare with your own tag from Cut 1. Where the peer tags diverge from yours, note whether it is a signal about how you are perceived or about your own honesty gap.
