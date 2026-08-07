# exercise-02: Paved-Road Consumer Contract Authoring

**Estimated effort:** 3 hours

## Objective

Author the **paved-road consumer contract** for the component whose build-vs-adopt call landed in exercise-01, using chapter 4's eight-section template. The deliverable is a three-to-five-page bilateral, signed, per-component, time-bounded agreement between the ML org and the peer platform team (or the equivalent counter-party for adopt-managed / adopt-OSS) that turns the exercise-01 verdict from a design decision into a durable partnership.

The contract's success criterion is not literary. It is: *the ML-org Staff engineer and the peer platform-team staff-plus IC would both sign this on the last page without renegotiating a comma.* If either side would push back on a clause, the clause is either wrong or under-specified.

## Prerequisites

- Exercise-01 complete — a landed verdict, a specific implementation option, a re-visit trigger, and the two-to-four contract-implication bullets from Section 7.
- Chapter 04 — the three-column responsibility matrix, the four consumer-commitment categories, the four producer-commitment categories, the eight-section template, and the two failure modes (over-specification and under-specification). Read in full.
- Chapter 01 — refresher on the consumer/producer split and the "silently becoming the platform team" failure mode.
- Recommended: skim one chapter of *Software Engineering at Google* on Deprecation ([abseil.io/resources/swe-book](https://abseil.io/resources/swe-book)) and the [Team Topologies](https://teamtopologies.com/book) interaction-modes summary. Both are the vocabulary the peer platform team will use.
- If the verdict was `build-inside-the-ML-org`: chapter 4 was explicit that build-inside verdicts do not need a consumer contract — the artifact is a cross-team API contract instead. In that case, pivot this exercise to a **cross-team API contract**; the template below still applies, but the two sides are two ML-org teams instead of ML-org and platform-team.

## Steps

1. **Restate the exercise-01 verdict at the top of the doc.** One paragraph: component, specific implementation, verdict, the two-to-four contract implications from exercise-01 Section 7. This anchors the contract to the decision that generated it; a contract disconnected from the verdict drifts.
2. **Fill the three-column responsibility matrix** from chapter 4 for this component. Every row from chapter 4's matrix, plus any additional rows this component surfaces (e.g., feature-store-specific "freshness SLO ownership", inference-gateway-specific "canary progression policy ownership"). Every cell is either ML-org, platform-team, or shared/negotiated — no cell empty. Chapter 4 warned that shared is where drift lives; use it deliberately, not by default.
3. **Draft the four consumer commitments.** For each of the four chapter 4 categories (adoption volume, feedback and review time, consumption discipline, migration commitment), name specific, measurable commitments. Named counter-party (which teams commit), named number (how many hours per month, how many teams by when), named accountable party (which named engineer holds it).
4. **Draft the four producer commitments.** For each of the four chapter 4 categories (API stability window, on-call SLO, migration support, roadmap transparency and gap-RFC responsiveness), name specific, measurable commitments. Named SLO number, named deprecation window in months, named migration-support hours per week, named gap-RFC response SLA in weeks. If you do not know what the platform team would realistically commit to, name the number you would negotiate for and mark it `NEGOTIATE`.
5. **Draft the escalation-and-dispute-resolution path.** Chapter 4 named the typical staircase: ML-org Staff → platform-team Staff → both EMs → both directors. Name the specific humans for the two counter-parties (or the roles if names are unknown), and name the specific triggers for each escalation step.
6. **Draft the success metrics and review triggers.** Two lists. **Success metrics** — three-to-five measurable indicators the contract is working after six months (adoption percentage, incident count, gap-RFC turnaround, migration-in-window rate, monthly consumer-review-meeting attendance). **Out-of-cycle review triggers** — three-to-five conditions that force an earlier renegotiation (SEV-1 outage on the substrate, doubling of gap-RFC backlog, adoption stall, platform-team roadmap change).
7. **State the contract term.** Effective date (`YYYY-MM-DD`), review date (typically six months out), renewal / renegotiation triggers. A contract that says "in effect until further notice" is not time-bounded and violates chapter 4's third property.
8. **Sign the contract.** Named signers on both sides with role and named human. If the peer platform-team staff engineer's name is not available to you, use `<Platform-team staff engineer, TBD>` and note who would be the counter-party.
9. **Sanity-check against chapter 4's two failure modes.** Count your consumer commitments and your producer commitments. If either side has more than 10 commitments, prune to the 6-10 enforceable band — chapter 4 was explicit that over-specification produces a contract nobody enforces. If either side has fewer than 6, the contract is under-specified and either you missed a category or a commitment is too vague; check whether any commitment reads "reasonable", "timely", or "as appropriate" — if so, put a number on it.

## Deliverable

A single document, 3-5 pages, containing the chapter 4 eight-section template with the additional exercise-context wrapper:

- **Section 0 — Verdict pointer.** One paragraph restating the exercise-01 verdict and the two-to-four contract implications (step 1).
- **Section 1 — Component and scope.** One paragraph naming component, implementation, and portfolio boundary — which ML-org teams are in scope, which are explicitly out.
- **Section 2 — Contract term.** Effective date, review date, renewal / renegotiation triggers (step 7).
- **Section 3 — Responsibility matrix.** The three-column table with every cell assigned (step 2).
- **Section 4 — Consumer commitments.** The four categories from chapter 4, each with 1-3 named, measurable commitments (step 3).
- **Section 5 — Producer commitments.** The four categories from chapter 4, each with 1-3 named, measurable commitments (step 4). Any commitment the ML-org side wants to negotiate for is marked `NEGOTIATE`.
- **Section 6 — Escalation and dispute resolution.** The named staircase with specific humans (or roles) and specific triggers (step 5).
- **Section 7 — Success metrics and review triggers.** Two lists: three-to-five success metrics, three-to-five out-of-cycle review triggers (step 6).
- **Section 8 — Signatures.** ML-org signer, platform-team signer, counter-signers (step 8).
- **Appendix — Failure-mode audit.** One paragraph confirming the commitment counts fall in the 6-10 enforceable band per side and naming any clause you flagged as `NEGOTIATE` (step 9).

## Starter guidance

- **Every commitment carries a number.** Chapter 4's under-specification failure mode is caught here. "Timely response" fails; "written response within 4 weeks" passes. "Reasonable availability" fails; "15-minute page-to-first-response for SEV-1 during business hours" passes. If you cannot put a number on a commitment, either the commitment is aspirational (delete it) or you have not thought it through (do the thinking).
- **The matrix is the pre-contract exercise.** Chapter 4 was emphatic that the matrix goes in the room with the counter-party synchronously. For this exercise, if you cannot get the counter-party in the room, simulate their perspective adversarially: for each cell you marked ML-org, ask "would the platform team's staff IC push back and claim this?" For each cell you marked shared, ask "which side does chapter 4's default assign this to, and why did I deviate?"
- **`NEGOTIATE` is a legitimate marker for this exercise.** You do not have the actual platform-team staff engineer sitting next to you. When a producer commitment is a number you would advocate for but do not know the platform team would accept, mark it `NEGOTIATE` in Section 5 and record your reasoning in the appendix. A contract with three `NEGOTIATE` clauses is honest; one that pretends to know the platform team's on-call SLO commitment without asking is fantasy.
- **Do not include implementation detail.** Chapter 4 was explicit — technical implementation lives in the platform team's design docs, pricing lives in mod-407, individual-team migration steps live in per-team docs derived from this contract. If your contract has a code snippet, a schema definition, or a per-team migration timeline, delete it and point to the sibling document.
- **The signature is what makes the escalation path work.** Chapter 4 named signatures as non-ceremonial. For this exercise, if the ML-org side would require the ML-org director's counter-signature (typical for contracts touching more than one team), name the director's role and add a signature line. The exercise of asking "who counter-signs?" surfaces whether the contract has the authority to bind the commitments it names.
- **Six months is the default review date.** Do not extend to 12 months to "reduce overhead." The chapter 4 default reflects the observation that platform-team roadmaps and ML-org portfolios both drift in ~two-quarter cycles; a contract that reviews after four quarters routinely is a contract that quietly stops being enforced by month 9.

## Acceptance criteria

- Section 0 restates the exercise-01 verdict and the contract implications; the contract is bound to the decision that generated it.
- The responsibility matrix has every cell assigned to ML-org, platform-team, or shared/negotiated — no empty cells.
- Consumer commitments cover **all four** chapter 4 categories; producer commitments cover **all four** chapter 4 categories.
- Every commitment has a number, a named counter-party, or a named accountable role. No commitment uses "reasonable", "timely", "as appropriate", or "high quality" without a numeric backstop.
- The commitment count per side sits in the 6-10 enforceable band (chapter 4's failure-mode guard). The appendix confirms the count.
- The escalation path names specific humans or roles at each step, plus specific triggers.
- Success metrics and out-of-cycle review triggers each have 3-5 items, each measurable.
- The contract term is bounded — effective date, review date (typically 6 months out), and renewal trigger stated.
- Signature lines are present for both sides plus any required counter-signers.
- The contract avoids implementation detail, pricing, and per-team migration steps — those belong in sibling documents.

## Stretch goals

- **Take the draft to a peer role-playing the platform-team staff engineer.** For each producer commitment, they push back or accept. Absorb the pushback and revise. This is the single highest-signal way to catch producer-side commitments that would not survive a real negotiation.
- **Draft the mirror-image contract for the second-best verdict from exercise-01.** One page, section 1 and sections 4-5 only. Compare against your primary contract: which consumer commitments would you keep? Which producer commitments changed? The comparison is a check on whether the exercise-01 verdict is materially different from the runner-up — sometimes it turns out the contract shape is invariant to the verdict, which is a signal the verdict was a coin flip.
- **Simulate the six-month review meeting.** Write a one-page agenda: contract-clause-by-clause review, adoption percentage against Section 7's success metrics, gap-RFC backlog check, escalation-path stress test (has any escalation fired?). Include the specific numbers you would want to see. This is the artifact that turns Section 7's success metrics into a running discipline.
- **Author the "adoption ramp" appendix.** For consumer-commitment 1 (adoption volume), draw the ramp: which ML-org teams adopt in which month over the six-month contract term. Include a critical-path arrow — the team whose adoption blocks the next team's adoption. This is the artifact the ML-org EM will use to sequence engineering effort against the contract's commitments.
- **Author the deprecation-window worked example.** For producer-commitment 1 (API stability window), pick one plausible breaking change the platform team might ship in the next four quarters. Walk through the deprecation-window mechanics: announcement date, migration tooling shipped by, deprecation-enforcement date, ML-org migration-owner assignment, extension-protocol trigger. This turns an abstract SLO into a rehearsed workflow.
- **Cross-reference to project-401.** Note in an appendix which contract clauses will land in project-401's platform-strategy section. Consistency across the module's four artifacts is the deliverable at project scope; catching drift early is worth an hour.
