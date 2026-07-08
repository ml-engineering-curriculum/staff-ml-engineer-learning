# Running the Architecture Review: Block, Nudge, Escalate

## Motivation

Chapter 5 hands you an RFC. This chapter runs the meeting that decides its fate. The **architecture review** — one to two hours, six-to-ten people, one RFC on the table — is where portfolio decisions are actually made in most healthy ML orgs. A staff-plus IC who cannot run this meeting well produces good RFCs that never ship; the analysis is fine, the decision does not land.

Two roles the staff-plus IC will occupy across a two-year period:

- **Author-side** — you wrote the RFC and are defending it. This is the more common first-time-Staff experience.
- **Reviewer-side** — you did not write the RFC; you are one of the peer staff reviewers or the platform lead in the room. This shows up more often in year two.

The failure modes differ per role and this chapter walks both. The output of the review is one of four states — **block**, **nudge**, **escalate**, or **accept** — and knowing when to use each is the differentiating skill.

## Prior art worth reading before running one

The mechanics of engineering review meetings have been written up thoroughly. Two references sit closest to the multi-team ML case:

- **Chapter on "Design Documents" and "Code Review" in *Software Engineering at Google* (Winters, Manshreck, Wright; O'Reilly, 2020).** Names the properties a review culture needs — quick turnaround, framed as a shared problem, escalation paths — that transfer to architecture-review meetings directly.
- **The Kubernetes API-review process.** [The Kubernetes API review process document](https://github.com/kubernetes/community/blob/master/sig-architecture/api-review-process.md) is the closest public template for a *cross-team* API-shape review. Not because ML RFCs need Kubernetes's ceremony, but because it distinguishes what blocks a change from what merely comments on it — exactly the block-vs-nudge distinction below.

The classic amazon-style "silent-reading + written-comments-first" meeting protocol (Bezos's six-page-narrative memo) is also worth reading once — every serious portfolio decision benefits from the reviewers having *read* the RFC before opinions are expressed aloud.

## The four review outputs

At the end of an architecture-review meeting the RFC is in exactly one of four states.

### Accept

The reviewers agree the RFC is defensible and can be implemented as written. Timeline, ownership, migration plan all stand. The status field in the RFC moves to `accepted`; the author owns the follow-up implementation ADRs and the rollout announcement.

Almost no first-round RFC hits **accept** on a portfolio-scope decision. Do not aim for it; aim for *accept-with-nudges* on round one and *accept* on round two.

### Nudge

The reviewers accept the substance of the RFC but flag revisions that do not block acceptance — clarifications, cosmetic changes, minor tightening of a migration schedule, additional evidence for one claim. The RFC moves to `accepted with revisions`; the author revises within a week and re-opens for final acknowledgement.

Nudges are the healthiest common output. They mean the RFC's shape is right and the review meeting was productive without derailing.

### Block

The reviewers name a specific concern that must be resolved before the RFC can be accepted. A block is not a personal veto; it is a claim that the RFC is not yet defensible as written. Common legitimate reasons to block:

- **Ownership not named or not committed.** No specific team lead has agreed to own the merged substrate on a first-class basis.
- **Political cost missing or under-defended.** The migration binds a team whose lead has not agreed to the roadmap slip.
- **Rollback plan is theoretical.** No specific metric, threshold, or human is named for firing the rollback.
- **The evidence for the failure mode is thin.** Reviewers cannot see the drift measurement, the incident number, or the exec ask.

A block sends the RFC back to draft. The author revises and re-opens the review. Blocks are not moral failures; they are how the process works.

### Escalate

The reviewers agree the substance is right but the decision is above the room. This is the rarest and most senior output. Common legitimate reasons to escalate:

- **Cross-org headcount trade.** The RFC requires the platform team to allocate an engineer that only their VP can commit.
- **Cross-org roadmap trade.** The RFC asks the consumer teams to slip commitments to their own director. Only the shared upstream director can rank.
- **Regulatory or governance dependency.** The RFC touches a substrate the peer governance track (`ai-governance-analyst`) or peer security track (`ai-infra-security-learning`) must approve.
- **The reviewers are irreconcilably split.** Two peer staff engineers disagree on the verdict; the disagreement is technical, not political, and needs a director-level tiebreak.

The RFC moves to `pending escalation`. The author writes a **one-page escalation brief** (see below) and takes it to the appropriate senior. The review reconvenes only after the escalation lands.

## What blocks vs. what nudges

The single hardest calibration for a first-time-Staff reviewer is distinguishing a block from a nudge. The rule of thumb:

> **A block is a claim that the RFC as written would produce a bad outcome. A nudge is a claim that the RFC as written could produce a better outcome. If the reviewer's concern is about goodness, not badness, it is a nudge.**

Applied concretely:

- *"The rollback threshold is `SLO regression greater than 5%` and I would prefer `greater than 3%`."* — nudge. Either threshold is defensible.
- *"The rollback threshold is not stated and there is no named human who fires it."* — block. Absent, the RFC is not defensible.
- *"The Search team's migration schedule is aggressive; four weeks feels light."* — nudge if the schedule is defensible with acknowledged risk; block if the Search team's lead has not seen or agreed to the schedule.
- *"I would prefer a different feature-store vendor as the merge target."* — should not even be a nudge in this review; vendor discussion belongs to mod-405.

Newly-Staff reviewers over-block on preference. Newly-Staff authors over-negotiate on legitimate blocks. Both mistakes cost credibility. Naming the block-vs-nudge line explicitly at the top of the meeting — *"we are here to decide whether the RFC is defensible, not whether it is optimal"* — resolves 80% of the ambiguity.

## The escalation brief

When the review escalates, the RFC alone is not the right document to hand up. Executives read decisions, not designs. The **escalation brief** is one page:

- **Header:** the one-sentence decision being escalated. *"The RFC proposes merging the ranking and home-feed feature namespaces; the merge requires the platform team to allocate one engineer next quarter that only the platform VP can commit."*
- **What was decided in the review:** the substance of the RFC that the reviewers agreed on.
- **What was not decided:** the specific unresolved question.
- **The two-to-three options for the escalator to pick between:** each as one bullet, with the RFC-author's recommendation named.
- **The cost of no decision:** what happens if the escalation sits for a quarter. This is the section that motivates the executive to spend fifteen minutes on it.

The RFC is linked from the brief for context. Do not send the twelve-page RFC in place of the brief; executives read what fits on one page.

## Running the meeting — author side

The author has three jobs in the meeting: represent the RFC, absorb the feedback, and get to a decision.

**Before the meeting:**

- Circulate the RFC at least two working days before. Reviewers who see the doc for the first time in the meeting cannot give useful feedback.
- Pre-brief the reviewers you expect the most-friction from. The 1:1 you had with the platform lead the day before is where the deepest technical feedback usually lands. The meeting is where the *decision* happens; it is not where the deep review happens.
- Have your own list of two-to-three concerns you know are weak. Volunteer them at the meeting. Reviewers respect authors who name their own weaknesses; they distrust authors who hide them.

**During the meeting — the two-minute frame the author opens with:**

1. *"The decision on the table is [verdict] on [substrate cell]. Timeline is [T]. Consumer teams are [A, B, C]."*
2. *"The two concerns I want us to focus on are [X] and [Y]."*
3. *"I have already spoken with [team leads]; their concerns are captured in section 7 of the RFC."*

Anything longer than two minutes at the top burns reviewer attention.

**During the meeting — what to say when a block lands:**

- *"Yes, that is a legitimate block. Let me confirm — the specific revision that would resolve it is [Z], correct?"*
- Do not argue against a legitimate block in the meeting. Absorb it, name the specific revision that resolves it, and move on. The argument, if any, happens in the follow-up 1:1.

**During the meeting — what to say when a nudge lands:**

- *"Noted; I will revise to [Z] before the RFC hits accepted."*
- Or, if you disagree with the nudge: *"I hear the concern; I would prefer to keep [Z] because [reason]. Can we defer this to the follow-up ADR?"*

**After the meeting — what the author owes:**

- A written summary of the decision within 24 hours. The summary is short: what was accepted, what was nudged, what was blocked, what was escalated. Copy the reviewers and the consumer team leads.
- If blocked: a revision plan and a re-review date.
- If escalated: the escalation brief, delivered within 48 hours.

## Running the meeting — reviewer side

The reviewer has one job: represent the org's interest, not the reviewer's aesthetic. Three practical rules:

**Read the RFC before the meeting.** Reading it in the meeting is disrespectful and produces bad reviews. If you have not read it, decline the review or delay it a day.

**Frame every comment as either block, nudge, or "I need more information before I can decide".** Free-form commentary that does not resolve to one of these three burns meeting time without moving the decision. Newly-Staff reviewers benefit from writing the frame word — *block, nudge, question* — at the front of every comment they make. It sounds mechanical; it is the discipline that keeps the meeting on track.

**Know when you are out of scope.** The reviewer role is to defend portfolio-scope correctness, not to redesign the substrate. If the RFC proposes MLflow as the merge target and you prefer W&B, that is a vendor conversation for mod-405; it is not a block for this RFC. Chapter 5's "what the RFC is not" list is your reviewer-side reference.

**Look for the missing sections, not the imperfect ones.** The most common review failure — for both authors and reviewers — is to spend the meeting on the section that is well-written and skip the section that is missing. Section 7 (political cost) is the section reliably missed by first-time authors and reliably skipped by first-time reviewers. Read it explicitly. If it is thin, the RFC is not yet defensible.

## Composition of the review body

A portfolio-scope RFC review typically involves six-to-ten people:

- **RFC author** (staff-plus IC) — one.
- **Peer staff engineers** on adjacent portfolios — two to three. Their job is to check the technical case and the scope.
- **ML platform lead** — one. Their job is to check the implementation and the consumer-contract shape.
- **Consumer team leads** — one per consumer team, so two to four total. Their job is to defend their team's roadmap and to confirm the migration schedule.
- **Peer EM** — one, per mod-401 chapter 5. Their job is to check the political-cost section and to raise concerns about roadmap or team stability.
- **Occasional but valuable — the peer governance / security engineer** if the RFC touches a regulated substrate or a data-residency boundary.

Do not invite fewer than six; the review under-samples the affected surface. Do not invite more than ten; the meeting becomes a status update. If more than ten stakeholders need to see the decision, the review meeting decides and the decision is announced separately.

## What to do when the review body disagrees

Two disagreement patterns worth naming:

**The technical disagreement.** Two peer staff engineers disagree on the merge target or the migration order. Resolve inside the meeting where possible: ask each engineer to state the strongest case for the other's proposal, then decide together which of the two the RFC will adopt. If the disagreement cannot be resolved, that is an escalate output — take it to the shared upstream director. Do not attempt to resolve a technical disagreement between peer staff engineers by outvoting one of them in the meeting; it burns their capital and yours.

**The political disagreement.** A consumer team lead does not want to be bound by the RFC. The disagreement is not about technical merit; it is about roadmap or ownership. This is where chapter 4's political-cost mitigations were supposed to have landed. If the mitigation was insufficient, the RFC is not yet defensible — block, revise section 7, offer a different mitigation, re-open the review. Do not attempt to push past a team-lead block by force; the follow-through will punish you for a year.

## When to defer the decision

The **defer** verdict from chapter 4 is the review's fifth outcome — no acceptance, no block, no escalation, but explicit *not now*. Use defer when:

- The pain is real but a platform-team roadmap slot (mod-405) is expected within one to two quarters that will make the decision meaningfully cheaper.
- The consumer teams are legitimately over-committed and the review is adjacent to a scheduled promotion or planning cycle.
- The evidence for the failure mode is currently borderline and one more quarter of measurement will settle it.

Defer with a **trigger** and a **revisit date**. A deferred RFC that does not name its trigger becomes a permanently deferred RFC, which is the same as a rejected RFC without the courtesy of saying so.

## The review as a career signal

A quiet property of the architecture review that is worth naming: it is where the org sees who you are.

Consumer team leads notice which staff-plus IC absorbs feedback gracefully and which one over-negotiates every nudge. Peer staff engineers notice which reviewer distinguishes block from nudge and which one over-blocks on preference. The peer EM notices whether the political-cost section was written by an engineer who understands their partner's job. The director watches whether the room reached a decision.

None of these are the primary output of the review — the primary output is the RFC's status. But the secondary signal is a durable career artifact. A staff-plus IC who runs three portfolio reviews well accumulates *review credibility*, which is what makes subsequent RFCs easier to land. A staff-plus IC who runs three reviews badly finds each subsequent RFC harder. The review is a signal-emitting exercise as much as it is a decision-making one.

## Summary

- The architecture-review meeting's output is one of four states: **accept**, **nudge**, **block**, **escalate** — with **defer** as a legitimate fifth on portfolio decisions.
- **Nudge vs. block:** a block claims the RFC would produce a bad outcome; a nudge claims it could produce a better outcome. Name the difference explicitly at the top of the meeting.
- **Escalate** when the decision is genuinely above the room — cross-org headcount, cross-org roadmap trade, governance dependency, irreconcilable peer-staff disagreement. Ship a one-page escalation brief, not the twelve-page RFC.
- **Author-side discipline:** circulate 48 hours ahead, pre-brief the highest-friction reviewer, open with a two-minute frame, absorb blocks without arguing, write a 24-hour summary.
- **Reviewer-side discipline:** read the RFC before the meeting; frame every comment as block / nudge / question; stay in scope; read section 7 explicitly.
- **Composition** is six-to-ten people: author, peer staff, platform lead, consumer team leads, peer EM, optionally peer governance / security.
- **Disagreements:** resolve technical disagreements inside the meeting where possible, escalate where not; do not force past a consumer-team-lead political block, revise the political-cost mitigation instead.
- The review is a **career signal** as well as a decision-making one — how you run it accumulates credibility that makes subsequent portfolio decisions easier or harder to land.

This closes the module's chapter arc. Exercise 01-04 walk the artifact chain from blueprint to RFC. Project-401 ships the full portfolio blueprint, RFC, TCO model, and review recording as one capstone deliverable.
