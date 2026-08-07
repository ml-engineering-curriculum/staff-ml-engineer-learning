# exercise-04: Coach a Senior Tech Lead Plan

**Estimated effort:** 3 hours

## Objective

Author a **coaching plan for one real (or realistically-composed) L30 senior tech lead** on the learner's roster, in the shape chapter 5 named — mode selection (review-don't-rewrite / pair-then-withdraw / sponsor-not-sponsor-as-shield) per artifact, a quarterly technical-bar read, a peer-EM partnership protocol, and an explicit boundary block naming what stays on the EM's side. After this exercise the learner should hold a two-to-four-page document they could actually use with a real L30 — a coaching plan that grows the L30's judgement on the four L30-owned artifacts (team RFCs, team roadmap, team design reviews, L20 mentorship) without doing the L30's work for them and without crossing into the EM's mandate.

Pairs with chapter 5. Reads against exercise-01's roadmap (the L30 owns one or more programs from Section 2). Feeder for [`project-403-staff-plus-tech-leadership-simulation`](../../../projects/project-403-staff-plus-tech-leadership-simulation/).

## Prerequisites

- Chapter 05 — the four L30-owned artifacts, the three coaching modes (review-don't-rewrite / pair-then-withdraw / sponsor-not-sponsor-as-shield), the Staff/EM boundary-4 split, the weekly Staff/EM 1:1 + quarterly technical-bar read + real-time signal cadence, the *taking the work back under deadline pressure* failure mode, the *coaching an L30 deeper than the staff engineer* special case, and the five coaching-side failure modes. Read in full.
- [`mod-401 chapter 5`](../../mod-401-staff-ml-role-scope/05-staff-and-em-partnership.md) — re-read before drafting. The nine-boundary partnership contract is the ground rule for this exercise; the coaching plan cannot be written coherently without the boundary in view.
- Recommended: skim Camille Fournier, *The Manager's Path* (O'Reilly, 2017), chapters on managing senior engineers and on tech leadership; and Lara Hogan, *Resilient Management* (2019), on the sponsor / mentor / coach distinction the three modes borrow from.
- Optional but grounding: Tanya Reilly, *The Staff Engineer's Path* (O'Reilly, 2022), on coaching peers and on influence-without-authority.

## Choose the L30

Pick **one** L30 senior tech lead as the subject of the coaching plan. Priority order:

1. **A real L30 on the learner's current roster** — the coaching plan will actually get used. This is the highest-signal option and is worth the anonymisation cost if the plan will be shared with peers.
2. **A recently-departed L30 or a past L30 the learner coached** — the plan can be written retrospectively and calibrated against what actually happened.
3. **A realistically-composed L30** built from an amalgam of past coaching engagements. Stipulate the profile at the top of the plan. Do not invent a fictional profile — chapter 5's failure modes are subtle enough that a fictional profile will not surface them.

If reusing exercise-01's roadmap, the L30 should own one or more of the Section-2 programs. Consistency across exercises is worth more than novelty.

For the composed-L30 fallback, this exercise stipulates a shape you can adopt:

> *L30 senior tech lead, 6 years post-degree, promoted to L30 12 months ago. Currently tech lead on the ranking team. Deep in ranking-model architecture and offline eval; strong on team-scope RFC authoring; weaker on cross-team coordination and on exec-audience framing. Ambition read: wants to be considered for the L40 promotion in 18-24 months. Peer EM is a mid-tenure L40, well-partnered with the staff engineer.*

Do not scope the plan to more than one L30 in this exercise. Chapter 5's coaching investment is per-person; a plan that averages across a roster is a roster-management document, which is an EM artifact.

## Steps

1. **State the L30 profile in one paragraph.** Named: role, level, tenure at level, current team scope, one-line technical strength, one-line technical growth edge, ambition read (does the L30 want L40 in the next 18-24 months, is content at L30, or wants to move to a different scope). The technical-strength and growth-edge sentences are what everything below reads against; if they are vague, the coaching investments will be too. If the L30 is real, anonymise names and any details that would be identifying.
2. **State the peer EM and the current partnership state in one paragraph.** Who the peer EM is (named), the current cadence of the Staff/EM 1:1 (weekly per mod-401 chapter 5), the current shape of the shared read on this L30 (aligned, drifting, unshared), and one recent moment where the partnership held or failed. Chapter 5's coaching plan reads against the partnership contract, not around it; if the partnership is drifting, the coaching plan cannot repair it — but the plan should name that state and forward-point to the partnership refresh.
3. **Assess the L30 against the four L30-owned artifacts.** For each of chapter 5's four artifacts, one paragraph:
    - **Team-scope RFCs.** Where does the L30 sit — do they draft RFCs at bar and defend them in review, do they draft but need coaching on the political-cost model, do they avoid RFC authorship? Cite one recent RFC (real or from the composed profile) as evidence.
    - **Team roadmap.** Do they hold a defensible quarterly plan with named non-goals, or does their roadmap read as a feature list? Chapter 2's rubric applies to team roadmaps at team scope.
    - **Team design reviews.** Do they run the reviews, call the verdicts, and hold disagreement between team members without deferring to the staff engineer?
    - **L20 mentorship.** Are the L20s on the team growing? Is the L30 doing the mentorship or is the staff engineer being pulled in directly?
   Chapter 5's rubric: the honest read on each artifact is what determines coaching investment; the plan is not a blanket "coach on everything" statement.
4. **Assign a coaching mode per artifact.** For each of the four artifacts, name the coaching mode chapter 5 defined:
    - **Mode 1 — Review, don't rewrite.** For artifacts the L30 authors at close-to-bar quality; the staff engineer reviews with comments and lets the L30 revise. This is the majority mode for an L30 in mid-tenure.
    - **Mode 2 — Pair, then withdraw.** For artifact shapes the L30 has never authored (first exec one-pager, first multi-team RFC, first architecture-review facilitation); the staff engineer pairs on the first one, does most of the work in a shared session, then explicitly withdraws for the second.
    - **Mode 3 — Sponsor, not sponsor-as-shield.** For high-stakes artifacts the L30 authored and needs public backing on (a program the L30 wants leadership to commit to, a cross-team RFC, a career-visible design review). The staff engineer co-signs and stands behind it in the room.
   For each mode assignment, one sentence on *why this mode for this artifact for this L30*. Not every artifact is in the same mode; the whole point of chapter 5's three modes is that the coaching investment is calibrated to the L30's current bar on that artifact.
5. **Author the quarterly technical-bar read.** Chapter 5's specific artifact — two paragraphs the staff engineer would send to the peer EM as part of the L30's growth-plan input. Structure:
    - **Paragraph 1 — Where the L30's technical bar sits.** Against the L30 rubric on the four artifacts. Specific: which dimensions are at bar, which are below, which are above. This is the staff engineer's honest read; chapter 5 was clear that watery reads dilute the EM's growth-plan quality.
    - **Paragraph 2 — Where the L30 is stretching toward L40.** The one dimension the staff engineer is investing coaching on this quarter, and the specific artifact that dimension shows up on. Cite the mode assignment from step 4.
   The read is written *for the EM*, not the L30. Chapter 5 was direct: the read goes to the EM in writing; the EM uses it as input to the L30's growth plan; the L30 does not read this document directly.
6. **Design the partnership protocol with the peer EM.** Three cadences chapter 5 named, adapted to this L30:
    - **Weekly Staff/EM 1:1.** One recurring agenda item on the L30 roster — what is the L30 working on, where are they stretching, where are they blocked, where does the staff engineer need to invest coaching. Named who owns each part of the agenda (the EM owns the 1:1, per mod-401 chapter 5).
    - **Quarterly technical-bar read** (step 5's artifact). Named delivery cadence (typically the second week of the quarter, so the EM has it in time for the L30's growth-plan conversation).
    - **Real-time signal.** Two specific triggers — when a review or design surfaces material positive or negative signal, when the staff engineer's coaching investment needs to change shape. Named channel (a specific Slack DM, a stand-up-agenda item, a same-week 1:1).
7. **Author the boundary block — what stays on the EM's side.** Chapter 5's boundary-4 split (career and coaching) applied to this specific L30. In three bullets:
    - **Career questions from the L30.** If the L30 asks the staff engineer *"should I try for staff this cycle"*, the redirect line the staff engineer will use. Verbatim. Chapter 5 gave the template: *"That is a conversation for you and [EM name]. I can tell you where I think your technical bar sits against the L40 rubric — which I have shared with your EM. The career plan is theirs."*
    - **Performance issues.** If the L30 has a performance issue the staff engineer notices in code or design review, the protocol: give the read to the EM privately, coach the L30 on the technical work, do not have the performance conversation directly.
    - **Interpersonal conflict.** If the L30 has an interpersonal conflict with another engineer, the boundary — Staff engineer arbitrates technical disagreements only; interpersonal conflict is the EM's per mod-401 chapter 5 boundary-8. Named forward channel (staff engineer flags to EM, does not engage with the L30 on the interpersonal side).
8. **Author the "if coaching does not work" protocol.** Chapter 5's hardest section. In one paragraph:
    - The **technical-bar read** the staff engineer commits to giving in writing to the EM if coaching is not closing the gap after N months of investment. Named horizon (typically two quarters).
    - The **staff engineer's commitment to continue coaching** until the EM tells them the growth plan has changed. Chapter 5 was explicit: silent withdrawal is not a signal the staff engineer is empowered to give.
    - The **performance-conversation boundary.** The staff engineer does not run the performance conversation with the L30 even in the absence of the EM (unfilled EM slot, etc.). Named mitigation (pro-tem EM from a peer team, per mod-401 chapter 5 special-case guidance).
9. **Address the "L30 deeper than staff on a dimension" special case, if applicable.** If the L30 is deeper than the staff engineer on any technical axis (common in mature ML orgs — a pretraining specialist, an eval-methodology specialist, a distributed-systems specialist), one paragraph:
    - Which axis the L30 is deeper on.
    - What the staff engineer is *not* coaching on (do not fake depth on this axis).
    - What the staff engineer *is* coaching on — chapter 5's answer: the cross-team / portfolio / exec-audience axis where the staff engineer's leverage lives.
   If the L30 is not deeper than the staff engineer on any dimension, one sentence noting so and moving on.
10. **Sanity-check against chapter 5's five failure modes.** In one final paragraph, confirm the plan does not exhibit:
    - **Staff engineer as shadow EM** — the plan does not have the staff engineer running 1:1s, giving direct performance feedback, or running growth-plan conversations.
    - **Staff engineer as re-author** — the mode assignments in step 4 do not have the staff engineer doing the L30's work; mode 1 is *review, not rewrite*.
    - **Coaching only the strongest L30** — flag if this L30 is the staff engineer's default because they are already close to L40; ensure the roster also has coaching investment named for the L30s who need it more.
    - **Silent withdrawal** — the "if coaching does not work" protocol in step 8 has an explicit EM-first escalation.
    - **Coaching drifted into friendship** — flag if the staff engineer's coaching cadence has become socially comfortable rather than bar-holding; the mitigation is refreshing the cadence in the presence of the EM.

## Deliverable

A single document, 2-4 pages, in this structure:

- **Section 1 — L30 profile.** One paragraph (step 1).
- **Section 2 — Peer EM and partnership state.** One paragraph (step 2).
- **Section 3 — L30 read across the four artifacts.** One paragraph per artifact — team RFCs, team roadmap, team design reviews, L20 mentorship — with cited evidence (step 3).
- **Section 4 — Coaching mode per artifact.** Four mode assignments (one per artifact) with one-sentence rationale each (step 4).
- **Section 5 — Quarterly technical-bar read.** Two paragraphs the staff engineer would send to the peer EM (step 5).
- **Section 6 — Partnership protocol.** Weekly Staff/EM 1:1 (with agenda item), quarterly read (with delivery cadence), real-time signal (with named channel and triggers) (step 6).
- **Section 7 — Boundary block.** Three bullets: career questions (with verbatim redirect line), performance issues (with protocol), interpersonal conflict (with forward channel) (step 7).
- **Section 8 — "If coaching does not work" protocol.** One paragraph (step 8).
- **Section 9 — Deeper-L30 special case.** One paragraph if applicable, one sentence if not (step 9).
- **Section 10 — Failure-mode audit.** One paragraph confirming chapter 5's five failure modes do not apply (step 10).
- **Reflection.** Two-to-three sentences: which of the four artifacts was hardest to name a mode for? Where does the coaching plan most depend on the partnership with the EM being in good shape?

## Starter guidance

- **The plan is a working document, not a performance review.** Chapter 5 was direct: the coaching plan is the staff engineer's investment structure; it is not a document the L30 reads and it is not an evaluation. Write it as if you will use it — plain, specific, honest. If you find yourself softening a read because "the L30 might see this", the document has drifted into performance-review territory; the staff engineer's read of the L30 is honest input to the EM's growth plan, not narrative for the L30 to read.
- **Not every artifact is in the same mode.** Chapter 5's three modes are calibrated per artifact per L30 per moment. An L30 might be at mode 1 (review, don't rewrite) on team RFCs, at mode 2 (pair, then withdraw) on their first exec one-pager, at mode 1 on team design reviews, and at mode 3 (sponsor) on a cross-team RFC that ships this quarter. The plan should reflect that granularity; a plan with all four artifacts in mode 1 is a plan that has not thought carefully about the L30's growth edge.
- **The verbatim redirect line matters.** Chapter 5's boundary-4 split fails silently under conversational pressure — the L30 asks the staff engineer *"should I try for staff"* in the hallway, and the staff engineer answers reflexively before recalling the boundary. Writing the redirect line verbatim in Section 7 is the mitigation; the staff engineer says the line before their instincts override it.
- **The technical-bar read is written for the EM, not the L30.** Chapter 5 was emphatic: the read is a growth-plan input, not a performance narrative. Two paragraphs, specific, evidence-cited. If the L30's growth-edge dimension is *cross-team RFC authorship*, the read names the specific recent RFC that surfaced the gap and the specific coaching investment planned. Waves at "improve cross-team collaboration" is not a technical-bar read.
- **The "if coaching does not work" protocol is where the boundary is most tested.** Under coaching-failure pressure, the staff engineer's temptation is either to withdraw silently or to have the performance conversation directly. Both cross the boundary. The written protocol in Section 8 is the guardrail; write it before the pressure arrives, not after.
- **The deeper-L30 case is more common than newly-staff engineers expect.** Chapter 5 named it as the special case that causes imposter syndrome. If your L30 is a deeper pretraining specialist, a deeper distributed-systems specialist, or a deeper evaluation specialist than the staff engineer, name it in Section 9. The coaching is on the *cross-team / portfolio / exec-audience* axis where the staff engineer's leverage is — not on the L30's deep-technical axis where it is not.
- **Cross-reference to the roadmap.** The programs the L30 owns on exercise-01's roadmap are the artifacts the coaching plan lands on. If the L30 owns Program A (pretraining), the coaching on team RFCs is against Program-A-shaped RFCs; the coaching on the team roadmap includes the L30's contribution to the portfolio roadmap. Consistency across exercises is the module's capstone; the coaching plan should not stand independent of the roadmap.
- **Do not conflate coaching with mentorship.** Coaching (chapter 5's meaning, borrowed from Hogan) is asking questions that help the L30 find their own answer. Mentorship is giving advice. Sponsorship is opening the door publicly. All three are staff-engineer activities and all three are separate from EM work. The plan should be a coaching plan primarily, with named mentorship or sponsorship moments where the shape calls for it. A plan that is all mentorship (advice-giving) is a plan that under-develops the L30's judgement.

## Acceptance criteria

- Section 1 names L30 role, level, tenure at level, team scope, technical strength, technical growth edge, and ambition read. Real L30s are anonymised.
- Section 2 names the peer EM (or *"no EM peer, pro-tem is X"*), the current 1:1 cadence, and the current partnership state.
- Section 3 has one paragraph per L30-owned artifact (team RFCs, team roadmap, team design reviews, L20 mentorship) with a cited concrete piece of recent evidence for each.
- Section 4 assigns a specific coaching mode (mode 1 / mode 2 / mode 3) per artifact with a one-sentence rationale. At least two different modes appear across the four artifacts (not all four in mode 1).
- Section 5 (quarterly technical-bar read) is two paragraphs written for the peer EM, not for the L30. It names specific rubric dimensions the L30 is at, below, or above bar on, and one specific coaching investment for the quarter.
- Section 6 (partnership protocol) names the weekly Staff/EM 1:1 agenda item, the delivery cadence of the quarterly read, the real-time signal triggers, and the named channel.
- Section 7 (boundary block) includes the verbatim career-question redirect line, the performance-issue protocol, and the interpersonal-conflict forward channel.
- Section 8 ("if coaching does not work" protocol) names: the technical-bar-read commitment, the horizon (typically 2 quarters), the "keep coaching until the EM tells me otherwise" rule, and the performance-conversation boundary.
- Section 9 addresses the deeper-L30 special case with one paragraph if applicable, or one sentence noting non-applicability.
- Section 10 audits the plan against chapter 5's five coaching-side failure modes and names any mitigations.
- The plan does not have the staff engineer running 1:1s, performance conversations, or growth-plan conversations directly with the L30.
- The plan cites at least one program from exercise-01's roadmap (Section 2 programs) that the L30 owns or contributes to.

## Stretch goals

- **Take the plan to the peer EM for co-authoring.** Chapter 5's partnership contract works when the staff engineer and the EM have a shared read on the L30. Share Sections 1, 3, and 5 with the peer EM (or a peer role-playing the EM) and ask them to write a one-paragraph disposition — is the read consistent with theirs, is the growth-edge diagnosis one they agree with, is there anything material the read is missing. Absorb the disposition. This is the exercise's highest-signal check on whether the plan is a real coaching plan or a staff-engineer soliloquy.
- **Draft the second L30's plan in half a page.** Chapter 5 named "coaching only the strongest L30" as a failure mode. If your primary L30 is the strongest of the roster, draft the short-form plan for the L30 who needs coaching more. Do not skip this; the calibration for coaching investment across the roster is the actual staff-plus capacity question.
- **Author the "coaching engagement retrospective" template.** For quarterly use: after each coaching cycle, what did the staff engineer learn about their own coaching — where did mode 1 (review, don't rewrite) drift into mode-1.5 (review-then-rewrite-in-a-comment), where did the pair-then-withdraw work, where did sponsorship land vs. shield. One page. This is the ongoing discipline; the plan is the starting point.
- **Compose the "L30 who does not want L40" case.** Sometimes the L30 is content at L30 and not aiming for L40. Draft the coaching plan for that case. Chapter 5's growth-toward-L40 framing does not apply; the plan is about sustaining L30 bar and about the L30's own growth interests, not about promotion prep. This is a common shape and the module should train against it.
- **Author the "special case: L30 on a program the staff engineer thinks is misdefined" scenario.** Sometimes coaching means helping the L30 escalate a program-shape concern that the staff engineer shares but has been out-voted on. Half a page: how does the staff engineer coach the L30's escalation without abandoning the shared read, and without positioning the L30 as a proxy for the staff engineer's own disagreement? This is a nuanced coaching case that chapter 5 does not walk explicitly.
- **Cross-reference to exercise-03.** The hiring loop that hired this L30 (or a hypothetical replacement) is exercise-03's rubric shape. Note in the reflection: which of exercise-03's rubric dimensions is this L30's growth edge (and thus the coaching target here)? This is the *hiring → coaching* consistency loop that chapter 5 depends on.
- **Draft the succession plan sketch.** If this L30 is on the L40 promotion path, who takes their L30 role on the ranking team when they are promoted? Who is the L20 the L30 is currently mentoring toward L30? Half a page. This is L30-scoped succession planning; it lives on the EM's side of the org boundary but the technical-shape input to it is the staff engineer's coaching read.
