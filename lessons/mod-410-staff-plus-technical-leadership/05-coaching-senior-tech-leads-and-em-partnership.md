# Coaching Senior Tech Leads and the EM Partnership

## Motivation

Staff-plus scope only works when the L30 senior tech leads under it hold their teams' technical bar without staff intervention on every decision. A staff engineer whose calendar is consumed by writing every team's RFCs, sitting in every team's design review, and pairing on every team's incident is not doing staff-plus work — they are running four L30 jobs at once and clearing the bar for none. The lever that fixes this is *coaching the L30s* — the senior tech leads on each portfolio team — so they run their team's reviews, roadmaps, and mentorship at the same technical bar the staff engineer would set themselves.

Coaching L30s is technical work. It is not people management, it is not career management, it is not performance review — those all sit on the peer L40 EM's side per the mod-401 chapter 5 nine-boundary partnership contract. But the technical coaching side is one of the highest-leverage activities the staff engineer does, and it is the activity most vulnerable to silent drift into either doing the L30's job for them or drifting into the EM's job. This chapter names what technical coaching of a senior tech lead looks like, where it stops, and how the staff engineer partners with the peer EM without crossing the boundary. Pairs with exercise-04. Grounds heavily on [`mod-401 chapter 5`](../mod-401-staff-ml-role-scope/05-staff-and-em-partnership.md), Camille Fournier's *The Manager's Path* (O'Reilly, 2017), and Lara Hogan's *Resilient Management* (2019).

## What technical coaching of an L30 actually is

Coaching a senior tech lead at technical bar is a specific, bounded activity. The staff engineer's job is to build up the L30's *judgement* on the four L30-owned artifacts, so the L30 makes better decisions without the staff engineer in the room. The four artifacts:

1. **The L30's team-scope RFCs** — architecture, deprecation, migration, incident post-actions. The staff engineer coaches on the shape of the RFC and the defence, not by rewriting the RFC themselves.
2. **The L30's team roadmap** — quarterly plan, sequencing, non-goals scoped to the team's system (not the portfolio; the portfolio roadmap is the staff engineer's, from chapter 2).
3. **The L30's team design reviews** — how the L30 runs the review, what verdicts they call, how they handle disagreement between team members.
4. **The L30's mentorship of L20 mid-level engineers on the team** — how the L30 grows the next Senior on the team, not by the staff engineer mentoring the L20 directly.

Everything else — 1:1s, performance conversations, career planning, promotion packets, retention conversations — belongs to the peer EM. The staff engineer stays out even when the L30 asks for input, because *appearing* to do EM work silently reallocates the L30's perception of who owns their career.

## The three coaching modes

Reilly's *Staff Engineer's Path* names three coaching modes and each maps onto a distinct staff-plus coaching activity:

**Mode 1 — Review, don't rewrite.** The L30 drafts the RFC; the staff engineer reviews it with the same lens they would apply to a peer staff-engineer RFC — is the diagnosis right, is the sequencing defended, are the non-goals first-class, is the political-cost model honest. The staff engineer writes comments in the doc. The L30 revises. The staff engineer *does not* rewrite the RFC even when it would be faster. The learning is in the revision loop.

**Mode 2 — Pair, then withdraw.** For a new-shape artifact the L30 has never authored — the L30's first exec one-pager, their first multi-team RFC, their first architecture-review facilitation — the staff engineer *pairs* on the first one, does most of the work in a shared session, and then explicitly withdraws for the second one. The pairing session is a demonstration of the shape; the withdrawal is what forces the L30 to internalise it.

**Mode 3 — Sponsor, not sponsor-as-shield.** For high-stakes artifacts — a program the L30 wants leadership to commit to, an RFC crossing peer platform teams, a career-visible design review — the staff engineer *sponsors* the L30 by co-signing the artifact and standing behind it in the room. Sponsorship is public. It is different from *doing the work for the L30 while their name goes on the doc* — that is fake sponsorship and both the L30 and the wider org can tell. Real sponsorship means the L30 authored the work, the staff engineer defends it publicly, and if the artifact lands well the credit belongs to the L30.

Lara Hogan's *Resilient Management* frames the sponsor / mentor / coach distinction that these three modes borrow from — sponsor = opens the door in public, mentor = gives advice in private, coach = asks questions that help the person find their own answer. All three are staff-engineer coaching activities. All three are separate from EM work.

## Where technical coaching stops and EM work starts

The boundary that keeps this straight is the mod-401 chapter 5 boundary-4 split (career and coaching):

- **Staff owns.** Technical coaching on the four L30 artifacts above — RFCs, roadmaps, design reviews, technical mentorship of the L20s.
- **EM owns.** Career management — promotions, performance reviews, 1:1s, personal growth planning, retention conversations, difficult performance conversations, compensation.
- **Overlap.** Both give signal to the other. The staff engineer gives the EM a technical-performance read on the L30 at review time; the EM tells the staff engineer where the L30 needs technical coaching that the staff engineer is best-placed to give.

Three specific situations where the boundary blurs and how to hold it:

**The L30 asks the staff engineer for career advice.** *"Should I try for promotion to staff this cycle?"* The right response is to redirect: *"That is a conversation for you and [EM name]. I can tell you where I think your technical bar sits against the L40 rubric, which I have shared with your EM. The career plan is theirs."* Answering the career question directly — even with a well-meaning "yes, I think you're ready" — silently claims the EM's boundary.

**The L30 has a performance issue.** They are not delivering. The staff engineer notices in code review and design review. The right move is to give the read to the EM privately and coach the L30 on *the technical work* (mode 1 / mode 2 above), not to have the performance conversation with the L30 themselves. The performance conversation is the EM's; the coaching on the technical shape is the staff engineer's.

**The L30 is in an interpersonal conflict with another engineer.** *"I can't work with X."* The mod-401 chapter 5 boundary-8 split (conflict resolution) applies: interpersonal conflict is the EM's; the staff engineer arbitrates *technical* disagreements only. If the conflict is technical (the two engineers disagree on an RFC), the staff engineer runs the design-review verdict. If the conflict is interpersonal (they cannot collaborate), the EM handles it and the staff engineer stays out.

## The partnership contract with the EM peer for career-ladder conversations

The peer EM is the person who runs the L30's promotion packet, the L30's performance review, and the L30's career trajectory. The staff engineer is the person who provides the *technical bar signal* the EM uses when doing those things. The partnership works when the two roles have a shared read on the L30, refresh the read routinely, and speak with one voice in front of the L30 and in front of leadership.

The concrete cadence that makes this work:

- **Weekly Staff/EM 1:1** (owned by the EM, per mod-401 chapter 5). One recurring agenda item is the L30 roster — what is each L30 working on, where are they stretching, where are they blocked, where does the staff engineer need to invest coaching.
- **Quarterly technical-bar read.** At the start of each quarter, the staff engineer writes a two-paragraph read on each L30 the EM manages — where their technical bar sits against the L30 rubric, where they are stretching toward L40, what one dimension the staff engineer plans to coach. The read goes to the EM in writing; the EM uses it as input to the L30's growth plan.
- **Real-time signal.** Anything the staff engineer sees in review or in design that materially updates the read — either a strong signal or a concerning one — goes to the EM the same week. Not saved for review time; the EM cannot manage on quarterly-only signal.

The mirror image on the EM side is that the EM shares the growth plan with the staff engineer, so the staff engineer's coaching investment aligns with the EM's development plan for the L30. The two-way exchange is what makes the partnership real.

## Coaching without doing the L30's job

The single most common failure mode when a staff engineer is coaching an L30 is *taking the work back*. The L30's RFC has problems, the exec review is tomorrow, the staff engineer takes over the doc "just for the deadline". The following week the same pattern repeats. Six months later the L30 has authored nothing at level — every consequential artifact from their team has the staff engineer's fingerprints, the L30's promotion packet has nothing to point at, and the staff engineer's calendar is full.

Three practices that prevent this:

**A hard deadline for the L30's work is not a reason to take it over.** If the L30's RFC is not ready for the exec review, the RFC misses the review. The staff engineer explains this to the peer EM in advance and to leadership in the review; leadership adjusts. Missing a review is a smaller cost than persistent staff-engineer surrogacy.

**Coaching happens in review, not in re-authorship.** The staff engineer writes comments, not paragraphs. Even when they know exactly what the paragraph should say, the comment is *"this section needs to defend X against Y — how would you frame that?"* — not the rewritten paragraph.

**Track the coaching invest against the L30's growth, not against the staff engineer's convenience.** Once a month, the staff engineer looks back at the L30 roster and asks *"which L30 has authored a level-appropriate artifact this month that they authored without me rewriting it?"* A roster where the honest answer is "none" is a coaching failure regardless of how much time the staff engineer spent on coaching.

## When coaching does not work

Some coaching engagements do not close the gap. The L30 does not internalise the shape of the artifact, the coaching cycle repeats without progression, and the staff engineer's honest read is that the L30 is not at bar for their role. This is a hard conversation and it lives on the boundary the staff engineer must be careful to hold:

- The **technical-bar read is the staff engineer's**, and it goes to the EM in writing. The staff engineer says *"I do not see [L30] at bar on [dimension]; I have coached on [X, Y] for [horizon] with limited progression; here is the specific evidence."*
- The **performance decision is the EM's**. What the EM does with the read — put the L30 on a growth plan, adjust their scope, or (in the escalated case) manage them out — is EM work. The staff engineer does not have that conversation with the L30.
- The **staff engineer keeps coaching until the EM tells them the growth plan has changed**. Withdrawing coaching silently, in advance of the EM's decision, is a form of quietly signalling the L30 is on their way out. That is not a signal the staff engineer is empowered to give.

Fournier's chapters on managing under-performing engineers are worth reading here on the EM side — even though the staff engineer does not run the conversation, they need to understand the shape the EM is operating in.

## The special case of coaching an L30 who is stronger than the staff engineer on a dimension

Common in mature ML orgs: the L30 tech lead on the pretraining team is a deeper pretraining specialist than the staff engineer will ever be. The staff engineer's coaching cannot be on the pretraining technical depth — that is not where the staff engineer's leverage is. It is on the *shape of the artifacts* — how the RFC is structured, how the exec framing lands, how the roadmap defends sequencing. The staff engineer coaches on the *cross-team, portfolio-scope, exec-audience* skills the L30 has not needed to build.

This is often uncomfortable for the newly-staff engineer, who feels imposter syndrome about coaching a deeper specialist. The framing that fixes it: the coaching is on a *different axis* than the L30's technical depth. Larson names this in *Staff Engineer* as the observation that staff-plus engineers are usually not the deepest specialist in any single technical domain within their scope; they are the person who can *coordinate across domains at the staff altitude*. That coordination skill is what they coach the L30 on.

## What Staff owns vs. what the peer L40 EM owns

Restated for this chapter:

- **Staff owns.** Technical coaching of L30s on RFCs, roadmaps, design reviews, technical mentorship; sponsorship of L30 artifacts in the room; the quarterly technical-bar read on each L30; real-time signal to the EM on notable technical performance.
- **EM owns.** 1:1s with each L30; performance reviews; promotion packets; career planning; retention conversations; performance-issue conversations; compensation; team headcount planning.
- **Both.** Weekly Staff/EM 1:1 covering the L30 roster; quarterly refresh of the growth plans; joint appearance in front of the Director / VP on any career-visible artifact.

## Common failure modes on the coaching side

- **Staff engineer as shadow EM.** Holding 1:1s with L30s, giving performance feedback directly, running growth-plan conversations. The result is a hollowed-out EM and an over-scoped staff engineer.
- **Staff engineer as re-author.** Every consequential L30 artifact ends up rewritten. The L30 never grows. Coaching becomes indistinguishable from surrogacy.
- **Coaching only the strongest L30.** The staff engineer coaches the L30 who is already close to L40 and ignores the L30s who need the coaching more. The result is a widening gap on the team.
- **Silent withdrawal.** The staff engineer decides the coaching is not working and quietly reduces investment. The L30 reads the withdrawal as the staff engineer having given up. The EM is not looped in. This should always be a conversation with the EM first.
- **Coaching that has drifted into friendship.** The staff engineer and the L30 have become close, and the coaching conversations are no longer bar-holding. The fix is naming it and refreshing the coaching cadence in the presence of the EM.

## Summary

- **Coaching L30 senior tech leads at technical bar** is one of the highest-leverage staff-plus activities. It builds up the L30's judgement on RFCs, roadmaps, design reviews, and mentorship — the four L30-owned artifacts.
- Three coaching modes — **review, don't rewrite** / **pair, then withdraw** / **sponsor, not sponsor-as-shield** — cover the range. All three are distinct from doing the work for the L30.
- The **Staff/EM boundary** (mod-401 chapter 5 boundary-4) is the ground rule: Staff owns technical coaching; EM owns career management, performance, and compensation. The staff engineer redirects career questions and performance conversations to the EM.
- The partnership rhythm is a **weekly Staff/EM 1:1**, a **quarterly technical-bar read on each L30**, and **real-time signal** on anything material.
- The most common failure is **taking the L30's work back** under deadline pressure. The fix is to miss the deadline rather than surrogate.
- When coaching does not work, the staff engineer gives the honest technical-bar read to the EM in writing; the performance decision is the EM's; the staff engineer keeps coaching until the growth plan changes.
- When the L30 is deeper than the staff engineer on a technical axis, coaching happens on the *cross-team / portfolio / exec-audience* axis where the staff engineer's leverage is.

The next chapter walks vendor and partnership judgement calls at staff scope — LLM API vendor mix, foundation-model provider strategy, cloud-provider trade-offs, MLOps-vendor selection.
