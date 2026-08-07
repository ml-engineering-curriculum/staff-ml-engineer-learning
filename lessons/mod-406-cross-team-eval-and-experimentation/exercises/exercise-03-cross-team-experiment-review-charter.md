# exercise-03: Cross-Team Experiment Review Charter

**Estimated effort:** 3 hours

## Objective

Author the **cross-team experiment review body charter** for the portfolio you standardised in exercise-01 and set judge policy for in exercise-02, using chapter 5's composition-cadence-arbitration-escalation framing. The deliverable is a one-to-two-page charter that names who sits on the review body, how often they meet, what they arbitrate (new-experiment approvals, interference disputes, trust breaches, standard-of-care exceptions), and how escalation flows when arbitration fails. After this exercise the learner should hold the third of the four eval-program contracts — the operational body that runs the eval-gate from exercise-01, enforces the judge tiers from exercise-02, and reads the platform capabilities from exercise-04.

The charter is a *living* document — it lists real names or roles, has a real first-meeting date, and produces a real weekly cadence the first time someone puts it on a calendar. A charter authored in the abstract has never survived contact with an actual interference dispute.

## Prerequisites

- Exercises 01 and 02 complete — the standard the body enforces, and the judge tiers the body reads.
- Chapter 05 — the composition (five-to-seven people, rotating Staff, platform lead, DS/analytics lead, responsible-AI ex-officio, peer EM), the cadence (weekly synchronous plus async), the four routine arbitration cases, and the escalation staircase. Read in full.
- Chapter 04 — enough to know that the review body arbitrates when the pre-registered rollout plan meets an actual halt event. The rollout contract's canary and progressive stages are the body's operational surface.
- Chapter 06 — the delegation contract; the review body is the weekly touchpoint where Staff and specialists meet operationally.
- Recommended: skim the Kohavi/Tang/Xu chapters on the ExP institutional model, and one LinkedIn / Airbnb / Booking engineering-blog post on how their experimentation review discipline works in practice (see `../resources.md`).
- The mod-401 chapter 5 partnership contract with the peer EM. The peer EM is a charter member per that contract; if you have not internalised the partnership shape, do that first.

## Steps

1. **Draft Section 0 — Portfolio anchor.** One paragraph: the portfolio, the number of active experiments in a typical week (rough estimate is fine — 3, 10, 30), and the two-to-three most recent interference or trust-breach incidents (or near-misses) that motivate needing this body. The charter without a motivating incident reads as bureaucracy; the charter with one reads as a fix.
2. **Draft Section 1 — Charter purpose.** Two-to-three sentences. What the body exists to do, in the vocabulary of chapter 5's four arbitration cases. What it does *not* exist to do (product-strategy re-litigation, single-team eval authoring, platform-team roadmap decisions).
3. **Draft Section 2 — Composition.** For each of the five seat types in chapter 5:
    - Seat name (e.g., "rotating Staff ML engineer").
    - Number of seats (chapter 5: two rotating Staff, one platform lead, one DS/analytics lead, one responsible-AI ex-officio, one peer EM).
    - Named human, or named role if the human is TBD.
    - Voting status on ML judgement calls vs. process / platform-capability calls (per chapter 5).
    - Rotation cadence for the rotating seats (quarterly is chapter 5's default).
    Total should be five-to-seven seats. If your portfolio requires an additional seat (e.g., a legal or compliance representative for a heavily regulated portfolio), name it explicitly and justify.
4. **Draft Section 3 — Cadence.** Named recurring meeting slot (day, time, duration — 30-45 minutes weekly per chapter 5), named async channel for urgent halts (Slack channel, PagerDuty rotation, etc.), named quorum rule (typically ⅔ of voting seats for arbitration; simple majority for approvals). Name the standing agenda: (a) new-experiment approvals from the past week, (b) in-flight interference disputes, (c) trust-breach investigations, (d) standard-of-care exceptions, (e) forward look at experiments proposed for the next two weeks.
5. **Draft Section 4 — What the body arbitrates.** For each of the four chapter-5 routine cases, write:
    - The case name.
    - The trigger — how the case arrives at the body (a submission channel, an SRM alarm, a halt event, an exception request).
    - The inputs the body reads — pre-experiment plan, guardrail-metric dashboards, SRM diagnostics, the exercise-01 eval-gate verdict, the exercise-02 judge-tier classification.
    - The verdicts the body can emit — `accept` / `nudge` / `block` / `defer` for approvals; `halt` / `ramp-down` / `defer-to-windowed-rerun` / `no-action` for interference disputes; `invalidate` / `build-back` / `no-action` for trust breaches; `granted (time-bounded)` / `denied` for exceptions.
    - The verdict-and-audit-trail format — where the decision is recorded, how it is communicated to the affected teams.
6. **Draft Section 5 — Escalation.** The three-tier escalation staircase from chapter 5:
    - Tier 1 — to the two teams' L40 EMs and the peer platform lead per the mod-401 partnership contract. Name the specific humans / roles and the trigger (the review body cannot reach quorum, or two seats hold a good-faith disagreement).
    - Tier 2 — to the director / VP of the product area. Name the specific human / role and the trigger (Tier 1 does not resolve within 5 business days, or the dispute touches multiple product areas).
    - Tier 3 — to the mod-410 leadership-comms channel with an exec-audience one-pager. Name the trigger (Tier 2 does not resolve, or the dispute has surfaced a policy-level gap the org needs to close).
    Escalations should be rare (chapter 5: five-to-ten per year in a five-team portfolio). Name the target rate in the charter and the review cadence that watches it.
7. **Draft Section 6 — Term and refresh.** Effective date (`YYYY-MM-DD`), first meeting date, quarterly review cadence for the charter itself (chapter 6's default). Name the evidence the quarterly review reads — number of experiments arbitrated, interference-dispute count, escalation count, exception-grant count, average time-to-verdict.
8. **Draft Section 7 — Charter-scope non-goals.** Explicit list of what the body does not do — product-strategy re-litigation, per-team eval implementation review, platform-team internal roadmap arbitration, individual specialist career review. Chapter 1's altitude-drift failure modes attach here; naming them keeps the body from becoming a general-purpose ML forum.
9. **Author the standing-agenda template.** One page. The recurring meeting's agenda structure, with time boxes (5-10 minutes per case type), the required pre-reads (last week's decisions, this week's approval queue), and the post-meeting artifacts (verdict log, action items assigned). This is the artifact the body's rotating chair uses; the charter names *that* it exists.
10. **Draft the mod-402 Section-9 roll-up sentence.** One-to-two sentences for the portfolio RFC — "the cross-team experiment review body meets weekly with five-to-seven rotating members and arbitrates the four routine cases." Chapter 5's charter is thinner in the roll-up than exercises 01, 02, and 04; it is the operational body, not the contract.

## Deliverable

A single document, 1-2 pages, containing:

- **Section 0 — Portfolio anchor.** One paragraph (step 1).
- **Section 1 — Charter purpose.** Two-to-three sentences (step 2).
- **Section 2 — Composition.** Seat-by-seat table with human/role, voting status, rotation (step 3).
- **Section 3 — Cadence.** Meeting slot, async channel, quorum, standing agenda (step 4).
- **Section 4 — What the body arbitrates.** Four cases with trigger, inputs, verdicts, audit trail (step 5).
- **Section 5 — Escalation.** Three-tier staircase with humans/roles and triggers (step 6).
- **Section 6 — Term and refresh.** Effective date, first meeting date, quarterly review evidence (step 7).
- **Section 7 — Non-goals.** Explicit list (step 8).
- **Appendix A — Standing-agenda template.** One page (step 9).
- **Appendix B — mod-402 Section-9 roll-up.** One-to-two sentences (step 10).

## Starter guidance

- **The charter is a signed document, not a wiki page.** Chapter 5's review body derives its authority from the signatures of the seat holders (or their EMs) and the director who chartered it. Include a signature block at the bottom — even if you fill it with `<name, TBD>` — because the exercise of asking "who signs this?" surfaces whether the body has the authority it claims.
- **Rotation is not decorative.** Chapter 5 was explicit that the two rotating Staff seats rotate quarterly to prevent one team's fiefdom. If your charter says "the rotating seats rotate as needed", the rotation will not happen. Name the specific rotation date (the first Monday of each quarter is a defensible default) and the specific mechanism (the chair proposes the next rotation two weeks out; the seat holders' EMs confirm).
- **Verdicts must map onto action.** Chapter 5's four verdict shapes are what turn the body from a discussion group into an arbitration mechanism. Every verdict must have a specific downstream action — `accept` unblocks the deploy, `block` returns the team to iteration, `defer` waits for a named artifact, `nudge` proceeds with a follow-up. If a verdict's downstream action is unclear, teams will treat the body as advisory.
- **The async channel is not a substitute for the meeting.** Chapter 5 named the async channel as the *urgent-halt* mechanism, not the primary decision surface. If the weekly meeting turns into "we already decided in Slack", the body's arbitration is happening in a channel with no quorum discipline. Reserve the async channel for halt / no-halt calls with tight time constraints (an in-flight experiment throwing SRM alarms at 2am); route everything else to the weekly meeting.
- **Escalation is a rare event, not a routine step.** Chapter 5's target of five-to-ten escalations per year is a health metric. If the body escalates weekly, either the composition is wrong (missing a needed seat) or the org's decision authority is not vested where it should be. Name the target rate and the review that watches it; if the rate blows the target, the review body itself is a problem to be solved, not a machine to be operated.
- **The responsible-AI ex-officio seat matters even when it is quiet.** Chapter 5 named this as an ex-officio seat that attends when an experiment intersects a Tier-C system from exercise-02 or the mod-409 risk register. In quiet weeks the seat is skipped; in weeks where a safety-critical experiment is on the agenda, it is voting. Name the trigger that activates the seat, not just its existence.
- **One page is enough.** The charter is deliberately short. If it grows past two pages, either an operational detail belongs in the standing-agenda appendix or a rule belongs in exercise-01's standard. Keep the charter tight; keep the operational instrumentation in its own artifact.

## Acceptance criteria

- Section 0 anchors the charter in a real portfolio and names at least one motivating incident or near-miss.
- Section 2 names five-to-seven seats — at minimum two rotating Staff, one platform lead, one DS/analytics lead, one responsible-AI ex-officio, one peer EM — with a voting status per seat and a rotation cadence for the rotating seats.
- Section 3 names a specific weekly meeting slot, a specific async channel, a specific quorum rule, and a standing agenda that covers all four arbitration cases.
- Section 4 has all four chapter-5 arbitration cases (new-experiment approval, interference disputes, trust-breach investigations, standard-of-care exceptions) with trigger, inputs, verdicts, and audit trail per case.
- Section 5 names the three-tier escalation staircase with specific humans / roles and specific triggers at each tier, plus a target escalation rate.
- Section 6 has an effective date, a first meeting date, and a quarterly review cadence for the charter itself with named evidence.
- Section 7 explicitly lists at least three non-goals.
- The standing-agenda template covers the meeting with time boxes and pre-reads.
- The mod-402 Section-9 roll-up sentence is present.
- The charter has a signature block (even if filled with TBDs).

## Stretch goals

- **Draft the verdict-log template.** One page. Fields the meeting chair fills per decision: date, experiment / team, case type, verdict, rationale, dissents, follow-up actions, escalated? This is the artifact that turns the body's decisions into an audit trail; a body with no verdict log has no memory and no accountability.
- **Simulate three weeks of the body.** Three one-page meeting minutes. Week 1: three new-experiment approvals, one clean accept, one nudge, one block. Week 2: an interference dispute between two teams' concurrent launches. Week 3: a trust-breach investigation triggered by an SRM alarm on a live experiment. Each minutes doc names the verdict, the rationale, and any escalation triggered. This is the rehearsal that catches whether the charter actually works when things get complicated.
- **Draft the first-meeting agenda.** One page. The first meeting's opening 45 minutes: charter walkthrough, seat introductions, agreement on the async-channel escalation trigger, one dry-run arbitration on a made-up experiment. First meetings that do not include a dry-run turn into "how does this work?" for the first month; the dry-run compresses that.
- **Take the draft to the peer EM for pre-review.** Per the mod-401 partnership contract, the peer EM is a charter seat. Ask them: is the composition right? Is the cadence sustainable? Would they escalate at the triggers you named? Absorb the pushback; adjust. A charter without the peer EM's sign-off is not a charter that will survive the first hard week.
- **Cross-reference to mod-408.** In an appendix, name the incident types (from exercise-01's mod-408 authoring, when the learner reaches that module) that a review-body decision could route into. A guardrail-breach halt is both a review-body verdict and a mod-408 incident; the two mechanisms must know about each other. If mod-408 has not been read yet, mark the appendix as TBD and set a review-body agenda item to close it later.
- **Draft the exec one-liner.** In one sentence at the top of the doc: what the body is, what it decides, when it first meets. "The cross-team experiment review body meets weekly starting Q3 to arbitrate cross-team eval and experiment disputes across the ranker, chat, and recommender teams." That sentence is what the director reads before signing.
