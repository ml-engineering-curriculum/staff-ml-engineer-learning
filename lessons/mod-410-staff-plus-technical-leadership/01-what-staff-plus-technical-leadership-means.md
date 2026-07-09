# What "Staff-Plus Technical Leadership" Actually Means

## Motivation

The learner has now worked mod-401 through mod-409. They can blueprint a portfolio, run a pretraining program, defend a distributed-training recipe, negotiate a platform consumer contract, standardise cross-team eval, build a CFO-readable TCO model, hold portfolio-scope reliability, and author a portfolio-scope AI-risk register. All of that is *staff-scope technical output*. None of it, by itself, is *staff-plus technical leadership*.

Technical leadership at staff-plus altitude is what happens *around* those artifacts — the multi-quarter roadmap the artifacts fit inside, the exec-audience one-pager that translates them for a CFO or CTO, the hiring loop that decides who joins the org that has to consume them, the coaching of the L30 tech leads who will inherit the day-to-day of each artifact, the vendor call that unblocks the roadmap, and the sustained bar that keeps the archetype the learner picked in mod-401 from silently drifting into IC-only or manager-only work.

This chapter names *staff-plus technical leadership* as a scope of work so the remaining six chapters — roadmap, exec comms, hiring, coaching, vendor judgement, sustaining the bar — have a stable definition to build on. It closes the loop that [`mod-401`](../mod-401-staff-ml-role-scope/) opened, now that the learner has done the intermediate modules and knows what the artifacts feel like.

## What "leadership" means in a staff-plus IC context

Larson's *Staff Engineer* (Stripe Press, 2021) and Reilly's *The Staff Engineer's Path* (O'Reilly, 2022) both make the same point in their opening chapters: at staff-plus altitude the individual contributor's job is only partially about producing artifacts. Increasingly it is about *setting the conditions under which the org can produce the right artifacts even when the staff engineer is not personally in the room*. That is the working definition this module uses.

Concretely, staff-plus technical leadership at ML L40 is the set of activities that:

- **Point the org multiple quarters out.** The roadmap that leadership commits to, the exec-audience framing that gets it funded, the RFCs that lock in the sequencing.
- **Raise and hold the technical bar of everyone else's work.** The hiring loop that keeps the bar from silently drifting; the coaching of the L30 tech leads whose reviews and roadmaps are the main filter for that bar.
- **Absorb the calls the org cannot delegate up or down.** Vendor mix, foundation-model provider choice, cloud-provider trade-offs, build-vs-buy at cost that is real. These calls are too consequential for a Senior tech lead and too technical for the Director without support.

Everything in this module is one of those three shapes. If a piece of work does not fit one of them, it is either mod-401-through-409 work (that is fine — technical output is a large fraction of the job) or it is peer-EM work (which is not this track's job at all).

## Staff-plus leadership vs. the Solver over-index

The most common way a strong senior engineer misreads the staff bar is by over-indexing on the **Solver** archetype from mod-401 chapter 2 — treating every hard problem as an invitation to dig in personally, ship the fix, and move on to the next fire. Solver work is real staff-plus work. But Solver work by itself does not add up to a promotion packet, and more importantly it does not add up to *an ML org that keeps getting better after the staff engineer leaves for a different portfolio*.

Reilly's chapters on influence and on "big-picture thinking" push the same point. The Solver ships answers. The staff-plus *technical leader* ships answers **and** the roadmap that made the answer scheduled, **and** the coaching that means the next such problem does not need the same staff engineer, **and** the hiring bar that makes sure the org can absorb the fix.

A useful triage question for a staff engineer looking at their calendar:

> *Would this piece of work still add value if I never personally touched a single line of code or a single review this quarter?*

If the honest answer is "no" for the majority of the week, the engineer is doing L30-plus-Solver work, not staff-plus technical leadership. That is a fixable diagnosis — the rest of this module is the fix — but it has to be named.

## Staff-plus leadership vs. crossing into the peer EM path

The mirror-image misread is crossing into the peer L40 Engineering Manager path — `ai-infra-team-lead-learning`. Once the staff engineer starts holding 1:1s, running performance reviews, negotiating headcount, or making comp calls, they have left the IC ladder and are doing EM work without the EM training. Fournier's *The Manager's Path* (O'Reilly, 2017) describes the EM ladder in enough detail that the boundary is easy to see once it is named: the EM's object of the verb is *the person and the team as a delivery unit*; the Staff engineer's object of the verb remains *the technical direction, the technical bar, and the technical artifacts*, even when the leadership activity looks people-adjacent.

The boundary that keeps this straight is the nine-boundary partnership contract from [`mod-401/05-staff-and-em-partnership.md`](../mod-401-staff-ml-role-scope/05-staff-and-em-partnership.md). This module does not re-derive it. Chapter 5 uses it as the ground rule for coaching seniors — Staff owns technical coaching of the L30 tech lead; the EM owns career management of the same person. Getting that line right is *the* signature of a working staff-plus/EM pair.

Julie Zhuo's *The Making of a Manager* (2019) is worth reading once for the same reason — to feel where the EM job actually is — but this track defers people management to the EM peer track and does not re-teach it.

## What the staff-plus technical leader owns end-to-end in this module

Concretely, in the fourteen hours of this module the learner will produce or rehearse:

1. **A multi-quarter ML program roadmap.** Sequenced, with defended non-goals and resource asks, and dependencies on the peer platform teams named. Pairs with chapter 2 and exercise-01.
2. **An executive-audience one-pager and RFC portfolio.** A CTO-, CEO-, or board-readable framing of a real portfolio decision, using Amazon PR/FAQ-style working-backwards where the audience calls for it. Pairs with chapter 3 and exercise-02.
3. **A staff-plus hiring-loop calibration.** Rubric-based interviews for senior and staff ML engineers, calibration between hire / no-hire / lean-strong-hire / senior-vs-staff, and debias practices for the hiring committee. Pairs with chapter 4 and exercise-03.
4. **A coaching plan for an L30 tech lead.** How the staff engineer coaches the L30's reviews, roadmaps, and mentorship — and where the coaching stops and becomes the EM's job. Pairs with chapter 5 and exercise-04.
5. **A vendor-and-partnership judgement doc.** LLM API vendor mix, foundation-model provider strategy, cloud provider trade-offs, MLOps vendor selection — with the decision, the reversibility class, and the exit contract. Pairs with chapter 6 and exercise-05.
6. **A sustained-bar plan.** How the learner keeps the archetype they picked in mod-401 as an ongoing constraint through the next two years, without drifting into IC-only or EM-only work. Pairs with chapter 7.

Everything on that list is *institutional*. It outlives the staff engineer's continuous attention — the L30 heuristic from mod-401 chapter 1 still holds.

## Revisiting the four archetypes with a year of scope under the belt

The learner picked an archetype centre-of-gravity in [`mod-401 exercise-02`](../mod-401-staff-ml-role-scope/exercises/exercise-02-self-assessment-against-archetypes.md). That was a forward-looking bet. This module treats the archetype as an *ongoing constraint* on the leadership activities above:

- **Tech Lead centre of gravity.** The roadmap in chapter 2 and the L30-coaching in chapter 5 dominate the week. Exec comms in chapter 3 mostly means the roadmap-defence deck. Hiring in chapter 4 is one team's senior loop. Vendor judgement in chapter 6 is the calls the team's roadmap depends on.
- **Architect centre of gravity.** Exec comms in chapter 3 dominates (multi-team RFCs sit in front of executives every quarter). Roadmap in chapter 2 is *cross-team*. Hiring in chapter 4 spans several teams' loops. Vendor judgement in chapter 6 is architecture-driven.
- **Solver centre of gravity.** Exec comms in chapter 3 is where the Solver's investigation writeups land as one-pagers. The roadmap is thinner — the Solver has a shape rather than a slate. Coaching in chapter 5 is the hand-off contract at the end of each engagement. Vendor judgement in chapter 6 is unusual, ad hoc, and heavy when it happens.
- **Right Hand centre of gravity.** Exec comms in chapter 3 *is* the job. Roadmap authoring in chapter 2 is on behalf of the Director. Hiring in chapter 4 is calibration across many loops. Vendor judgement in chapter 6 is on the Director's behalf.

Every archetype uses all six activities. The centre of gravity determines the *weighting*. Chapter 7 comes back to this weighting as the capstone reflection.

## The "am I sustaining staff-plus scope?" heuristic

The mod-401 chapter 1 heuristic was *does the artifact I produced this week outlive my continuous attention?*. That heuristic was written for a learner *entering* staff scope. For a learner *sustaining* staff scope over multiple quarters, a stronger version is needed:

> **Over the last quarter, did the sum of my artifacts point the ML org more clearly two-to-four quarters out than it was pointed at the start of the quarter? If yes, I sustained staff-plus scope. If the org is only marginally clearer, or clearer on this quarter's ship but not the next four, I did mostly Senior-plus-Solver work.**

The heuristic is uncomfortable. It ignores whether the code shipped, whether the incident got resolved, whether the review got run. Those are still valuable and they still show up in the packet. But they are the price of admission at L40, not the differentiator. The differentiator is *the direction of the org over the next four quarters*, and this module produces the artifacts that move that direction.

## What Staff owns vs. what the peer L40 EM owns for this module's activities

For each of the six activities above, the staff-plus/EM boundary matters. The full nine-boundary contract lives in mod-401 chapter 5; this chapter names the four boundaries that dominate this module:

- **Roadmap (boundary 2 in mod-401 ch 5).** Staff owns the *content* — sequencing, technical bets, non-goals, resource shape. EM owns the *commitment* — capacity, delivery risk, on-call load. Both sit on the roadmap review.
- **Exec comms (boundary 6).** Staff owns the *technical framing* of exec briefings. EM owns the *delivery framing*. They stand side-by-side in front of the Director or VP.
- **Hiring (boundary 3).** Staff owns the *technical bar* — rubric, calibration, hiring-loop coaching. EM owns *headcount, closing, and the final offer*. Both sit on the loop.
- **Coaching (boundary 4).** Staff coaches L30s on *technical direction and technical judgement*. EM owns *career management* — promotions, performance reviews, retention. Neither side does the other's job even in the other's absence.

Every subsequent chapter names the peer-EM half explicitly so the boundary stays visible.

## Summary

- **Staff-plus technical leadership** is what the staff engineer does *around* the technical artifacts of mod-401 through mod-409 — the roadmap, exec comms, hiring loop, coaching of seniors, vendor judgement, and sustained bar that let the ML org keep producing the right artifacts without the staff engineer personally in the room.
- The two dominant misreads are **over-indexing on the Solver archetype** (shipping fixes without shipping the leadership around them) and **crossing into the peer EM path** (holding 1:1s, headcount, and performance reviews).
- The four archetypes from mod-401 chapter 2 come back as *ongoing weighting constraints* on this module's six activities, not as one-time career choices.
- The self-test upgrades from mod-401's *does the artifact outlive my attention* to *does the sum of the quarter's artifacts point the org more clearly two-to-four quarters out*.
- The staff-plus/EM boundary — roadmap content vs. commitment, technical vs. delivery framing, technical bar vs. headcount, technical coaching vs. career management — is the ground rule for the module.

The next chapter walks the multi-quarter ML program roadmap: sequencing, non-goals, resource asks, and dependencies on peer platform teams — the artifact leadership actually commits to.
