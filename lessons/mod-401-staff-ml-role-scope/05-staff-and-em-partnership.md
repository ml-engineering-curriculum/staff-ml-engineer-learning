# The Staff-Plus IC and the Peer L40 Engineering Manager

## Motivation

The Staff ML Engineer and the peer L40 Engineering Manager sit on adjacent rungs of the same ladder. They are not competing paths — they are two halves of the *same L40 job* the ML org needs done. When both roles are staffed and their partnership is written down, the ML org can hold multi-team scope with one Staff engineer and one EM per team. When the partnership is not written down, one of three failure modes appears: the Staff engineer drifts into people-management by default, the EM drifts into technical leadership by default, or both drift into scoping arguments and neither ships their L40 mandate.

This chapter defines the partnership contract explicitly. It is written for the Staff engineer's side — the EM peer track ([`ai-infra-team-lead-learning`](https://github.com/ai-infra-curriculum/ai-infra-team-lead-learning)) writes the mirror image. Both sides should recognise the same document from either side.

## The two halves of the L40 mandate

At L40, the ML org has two mandates that must both be held. On any single team or program, they split like this:

| Half | Owner | Object of the verb | Cadence |
|---|---|---|---|
| **Technical direction and the technical bar** | Staff ML engineer (IC path) | RFCs, architecture, roadmap content, hiring calibration, code-review bar, cross-team contract | Two-to-four-quarter horizon; artifact-driven cadence |
| **People, delivery, and the org-motion** | Engineering Manager (people path) | 1:1s, performance reviews, hiring headcount, quarterly commitment, retention, project execution | Quarterly commit cadence; person-driven cadence |

Both halves have to land. A Staff engineer who owns the technical direction but does not partner with the EM ends up with roadmaps no one commits to. An EM who owns delivery without a Staff partner ends up with well-executed teams that ship the wrong architecture. Neither role is a *complete* L40 job on its own; the partnership is the L40 job.

Two industry references cross-check this framing:

- Camille Fournier, *The Manager's Path* (O'Reilly, 2017) — the canonical EM ladder text; describes the L40 EM's mandate in enough detail that the Staff side of the partnership is derivable by inversion.
- Charity Majors, *[The Engineer/Manager Pendulum](https://charity.wtf/2017/05/11/the-engineer-manager-pendulum/)* (2017) — the standard essay on how individual careers oscillate between the two paths; important context for why the two paths must remain *equal* rather than one becoming a promotion of the other.

## The partnership contract — nine boundaries to write down

The Staff/EM partnership works by *writing down each of nine specific boundaries* between the two roles. The nine boundaries are the ones that reliably cause conflict when they are unwritten. On a healthy L40 pair, the boundaries are agreed in a one-page document that both sides refresh once a quarter and share with their common Director.

### Boundary 1 — Technical direction

- **Staff owns.** The technical direction — architecture, RFCs, technical roadmap content, technical trade-offs, technical bar in code review.
- **EM owns.** The commitment to that direction — does the team commit to the roadmap; is the delivery plan realistic; is there capacity to ship what the Staff engineer proposed.
- **Failure mode if unwritten.** The EM starts making technical calls in the Staff engineer's absence; or the Staff engineer commits the team to a roadmap the EM has not vetted for delivery capacity.

### Boundary 2 — Roadmap authorship and roadmap commitment

- **Staff owns.** The *content* of the roadmap — which programs, in what sequence, at what cost, defending which technical bets.
- **EM owns.** The *commitment* to that roadmap in the quarterly org cycle — capacity, dependencies, on-call impact, delivery risk.
- **Failure mode if unwritten.** Roadmap slides that look great in review but that no one has committed to shipping.

### Boundary 3 — Hiring

- **Staff owns.** The technical bar. Sits on the hiring loop, calibrates senior engineers, coaches the interviewers, sets the technical rubric, delivers the technical hire/no-hire signal.
- **EM owns.** Headcount, budget, pipeline, closing, comp negotiation, offer decisions, org-fit signal.
- **Failure mode if unwritten.** The EM makes hiring decisions the Staff engineer thinks are below-bar, or the Staff engineer blocks hires the EM needs to make quarterly headcount.
- **Note.** Both sides sit on the hiring loop; both give signal. The final offer is the EM's; the technical rubric is the Staff engineer's.

### Boundary 4 — Career and coaching

- **Staff owns.** Coaching senior tech leads (L30) on *technical direction and technical judgement* — how to author RFCs, how to run design reviews, how to lead incidents, how to think about scope.
- **EM owns.** Career management — promotions, performance reviews, 1:1s, personal growth planning, retention conversations, difficult performance conversations.
- **Overlap.** The two roles routinely input into each other's coaching. The Staff engineer gives technical-performance signal to the EM at review time; the EM tells the Staff engineer where an L30 needs technical coaching that the Staff engineer is best-placed to give.
- **Failure mode if unwritten.** The Staff engineer takes on 1:1s and becomes a shadow EM; or the EM makes technical-performance calls with no Staff input.

### Boundary 5 — Delivery and on-call

- **Staff owns.** The design of the on-call rotation, the incident-response structure, the SLI/SLO catalogue (mod-408).
- **EM owns.** Enforcing the on-call rotation, escalating burnout, negotiating capacity for on-call load, ensuring the on-call cost lands somewhere on the roadmap.
- **Failure mode if unwritten.** On-call load creeps up because no one holds the capacity side; or on-call rotations get watered down because no one holds the technical bar.

### Boundary 6 — External communication (up the chain)

- **Staff owns.** The technical framing of exec briefings — one-pagers, RFCs, roadmap-defence deck, TCO narrative.
- **EM owns.** The delivery framing of exec briefings — quarterly commits, hiring plan, org health, team-morale signal.
- **Both.** Standing side-by-side in front of the Director or VP with the two halves of one L40 story.
- **Failure mode if unwritten.** Two contradictory stories reach the exec; or one side speaks and the other's mandate becomes invisible.

### Boundary 7 — Program vs. team-execution rhythm

- **Staff owns.** The *program* cadence — quarterly roadmap review, portfolio-level architecture review, monthly cross-team RFC review, program-scope postmortem.
- **EM owns.** The *team-execution* cadence — daily stand-up, sprint planning, retro, 1:1s, quarterly commit.
- **Failure mode if unwritten.** The Staff engineer gets pulled into the daily execution rhythm; or the EM has no visibility into the program-level artifacts the Staff engineer is producing.

### Boundary 8 — Conflict resolution between team members

- **Staff owns.** Technical disagreements between engineers — arbitrates the RFC, calls the design-review verdict, resolves an architecture split.
- **EM owns.** Interpersonal conflict and performance issues — handles two engineers who cannot work together, addresses a performance problem, has the hard conversation.
- **Failure mode if unwritten.** The Staff engineer inherits the interpersonal conflict because they were "there when it happened"; or the EM makes a technical call to resolve a personality dispute and both engineers feel unheard.

### Boundary 9 — Scope changes and prioritisation

- **Staff owns.** Whether the change is *technically necessary* — does the org need to add / drop / re-scope this program on technical grounds.
- **EM owns.** Whether the change is *deliverable* — can the team absorb it in the current quarter; what has to drop to make room.
- **Both.** Joint decision on the scope change, with the Director as tiebreaker.
- **Failure mode if unwritten.** Scope changes announced without EM buy-in blow up delivery; scope changes filtered through the EM without Staff input miss technical constraints.

## The one-page partnership contract

The nine boundaries above compress to a one-page document that the L40 pair signs and shares with the Director. The template:

```
Staff/EM partnership contract — [team name]
Effective quarter: [YYYY-Qn]
Staff ML: [name]
EM: [name]
Common lead: [Director / VP]

Boundary                 | Staff owns                    | EM owns
-------------------------|-------------------------------|------------------------------
Technical direction      | ...                           | ...
Roadmap                  | ... (content)                 | ... (commit)
Hiring                   | ... (bar)                     | ... (headcount)
Career & coaching        | ... (technical)               | ... (career)
Delivery & on-call       | ... (design)                  | ... (enforcement)
Exec communication       | ... (technical framing)       | ... (delivery framing)
Program vs. exec rhythm  | ... (program cadence)         | ... (team cadence)
Conflict                 | ... (technical disagreement)  | ... (interpersonal)
Scope changes            | ... (technically necessary)   | ... (deliverable)

Escalations
- Unresolved technical/delivery conflict → common lead ([Director / VP])
- Technical bar disagreement → common lead
- Hiring-loop calibration disagreement → common lead

Non-goals (neither side)
- ...
- ...

Cadences
- Weekly 30-min Staff/EM 1:1 (owned by EM)
- Monthly 60-min roadmap sync with the Director
- Quarterly refresh of this document
```

Exercise-03 puts the learner in the seat of authoring this one-pager for a specific team.

## Special cases that break the default contract

Two special cases show up often enough to name.

### Case 1 — The Staff engineer has no EM peer yet

Common in early-stage companies or when a team just formed. The Staff engineer *does not* absorb the EM mandate — that is the single most common way a Staff career quietly ends. Instead:

- Ask the Director to name a peer EM as soon as possible.
- In the interim, ask a peer L40 EM from an adjacent team to be the *pro-tem* partner for the boundaries that must be held now (hiring loop, on-call rotation, scope commit).
- The Staff engineer does *not* take 1:1s, headcount ownership, performance reviews, or comp decisions on themselves. Those wait for the EM.
- Author the partnership contract *in advance* — it becomes the hiring spec for the EM role, exactly like the peer-track hand-off from chapter 4.

### Case 2 — The EM peer is new and building the technical bar

Common when a strong technical L30 has just been promoted into the EM path. The Staff engineer supports the transition by:

- Explicitly authoring the technical-bar half of the contract in more detail than usual.
- Doing the technical framing of exec comms for the first two quarters, then handing back to the joint version.
- Coaching the new EM on when to bring the Staff engineer in rather than making a technical call solo.
- Not doing the EM's job for them. Coaching is not surrogacy.

## What this contract is *not*

- **Not a wall.** The Staff engineer does not stop caring about people, and the EM does not stop caring about technical direction. The contract names who *owns* each boundary, not who *cares* about it.
- **Not a fixed document.** Refresh once a quarter; renegotiate when the team's scope changes.
- **Not a substitute for the peer-track hand-off contracts from chapter 4.** The Staff/EM contract binds the two L40 roles inside one team; the peer-track hand-offs bind the Staff role to other teams entirely.
- **Not a career-path document.** The Staff engineer and the EM are not on paths of different seniority — they are on paths of different discipline at the same seniority. Do not confuse the partnership contract with a promotion track.

## Common failure modes

- **The "combined Staff/EM" role.** A single L40 person is asked to hold both mandates for one team. This is a hybrid role that will not clear either bar over the long run; treat it as a temporary staffing gap, not a stable structure.
- **The Staff engineer as shadow EM.** The Staff engineer holds 1:1s "informally," gives performance signal directly to engineers rather than through the EM, and starts owning delivery commits. The result is an EM with less authority and a Staff engineer who no longer has time for technical direction.
- **The EM as shadow Staff engineer.** The EM authors RFCs, calls architecture, sits on technical calls without the Staff engineer, and starts owning the technical bar. The result is a Staff engineer who has been quietly dismantled; the peer L40 EM track has its own reasons to avoid this pattern, symmetrically.
- **Silent boundary drift.** Neither side raises the boundary; over two quarters the ownership silently swaps on some boundaries. The countermeasure is the quarterly refresh — re-read the contract, name the drifts, and either adjust the contract or push the drift back.

## Summary

- The Staff ML engineer (IC path) and the peer L40 EM are two halves of one L40 mandate. Technical direction and technical bar are on the Staff side; people, delivery, and the org-motion are on the EM side.
- The partnership works by writing down nine boundaries in a one-page contract: technical direction, roadmap, hiring, career and coaching, delivery and on-call, exec comms, program vs. team rhythm, conflict resolution, and scope changes.
- The contract is not a wall — both sides *care* about all nine boundaries; only one *owns* each.
- When the EM peer is missing, do not absorb the EM mandate; ask for the peer, use a pro-tem EM in the interim, and write the contract as the hiring spec.
- The most common failure modes are the "combined Staff/EM" role, the Staff engineer as shadow EM, the EM as shadow Staff, and silent boundary drift.

Exercise-03 asks the learner to author this contract for a concrete team. The next chapter closes the module by turning the archetypes, verbs, and contracts inward — writing the self-assessment that names the archetype the learner is closest to today and the archetype they want to grow into.
