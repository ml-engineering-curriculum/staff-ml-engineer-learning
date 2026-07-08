# exercise-04: Hand-Off Map to Peer Specialist and Peer Platform Tracks

**Estimated effort:** 2 hours

## Objective

Author three concrete hand-off contracts — one to a peer *specialist* track, one to a peer *platform* track, one to a peer *governance or security* track — for a real or realistic Staff ML program. Each contract must follow the six-section shape from chapter 4 (peer-track owns end-to-end / Staff owns on their side / the API / the escalation path / explicit non-goals / header) and must survive the "would this document be identifiable after both incumbents rotated?" test.

## Prerequisites

- Chapter 04 — Hand-Off Contracts to Peer Specialist and Peer Platform Tracks. The six-section shape, the three worked examples, and the failure modes come from this chapter.
- Chapter 01 — for the L40 altitude reference and the "Staff owns strategy, program, and consumer contract; peer track owns implementation, build, or policy" pattern.
- The [`../../PREREQUISITES.md`](../../PREREQUISITES.md) list of peer tracks, which is the source of the actual peer-track names.

## Scenario

Pick **one** of the following two scenarios. Use the same one you used for exercise-03 if you did that exercise, so that the artifacts stack.

### Scenario A — your current portfolio

Author the three hand-off contracts for a Staff ML program you (or a Staff ML engineer you observe) actually own — e.g., the ranking-plus-retrieval program, the fine-tune program, the eval program, the platform-consumer program.

### Scenario B — a foundation-model training program (composite)

Author the three hand-off contracts for a foundation-model or large-scale fine-tune training program. This scenario connects directly to mod-403 and project-402.

- **Program.** Ship a domain-specialised model — say, a 7-13B mid-size LLM fine-tuned on internal corpus for a customer-facing product. Six-month program horizon.
- **Program owner.** You, as Staff ML Engineer (Architect archetype).
- **Peer specialist track.** `training-pipeline-engineer-learning` — owns the parallelism-recipe implementation, checkpoint sharding, NCCL configuration, and cluster-level distributed-training operations.
- **Peer platform track.** `ai-infra-ml-platform-learning` — owns the training platform (job orchestration, artifact tracking) and the model-registry service.
- **Peer governance / security track.** Choose one — `head-of-ai-governance` (for the compliance side) *or* `ai-infra-security-learning` (for the ML-security-red-team side).

Both scenarios produce the same three-contract deliverable.

## Steps

1. **Restate the Staff program mandate in one paragraph.** What is the object of the verb the Staff engineer holds end-to-end on this program? This paragraph frames what stays on the Staff side across all three contracts.
2. **For each of the three peer tracks, author a full six-section contract.** Follow the chapter-4 template — header / peer track owns / Staff owns / API / escalation / non-goals. Two-to-three pages per contract is the working range.
3. **Cross-reference the three contracts.** Are there any verbs or objects that appear in *two* of them? If so, one of two things is true: either you've written a hand-off ambiguity that will cause conflict, or the two peer tracks legitimately share ownership and the contract must say so explicitly. Fix by either handing the verb-object cleanly to one side or writing the shared-ownership clause explicitly.
4. **Check the non-goals list.** For each contract, name at least three non-goals. Every non-goal should point to a track that *does* own the deferred work — usually one of the two other peer tracks in this exercise, the peer L40 EM, or a peer L40 track from the same-altitude ladder.
5. **Stress-test with the failure modes.** Chapter 4 lists five failure modes when writing hand-off contracts (only one side named, individuals not roles, non-goals missing, aspirational cadence, Staff-side implementation veto). For each contract, note in one sentence whether the draft protects against each failure mode.
6. **Simulate the "peer track not staffed yet" case.** For one of the three peer tracks in your draft, imagine the peer track is *not* yet staffed at your organisation. Rewrite the contract accordingly (chapter 4's rule: do not pretend; write the contract as the peer-role hiring spec).

## Deliverable

A single markdown document, 6-8 pages, structured:

- **Section 1 — Staff program mandate.** One paragraph naming what the program owner holds end-to-end.
- **Section 2 — Contract A: peer specialist track.** Six-section contract.
- **Section 3 — Contract B: peer platform track.** Six-section contract.
- **Section 4 — Contract C: peer governance or security track.** Six-section contract.
- **Section 5 — Cross-contract consistency check.** Table of verbs-and-objects that appear in more than one contract; resolution for each (cleanly reassigned to one side, or explicitly shared-ownership with the mechanism named).
- **Section 6 — Failure-mode checklist.** One line per contract per failure mode from chapter 4.
- **Section 7 — Unstaffed-peer scenario.** The rewritten contract for the "peer track not staffed yet" case.
- **Section 8 — Program-level dependency graph.** A simple sketch showing which peer track feeds which artifact into which stage of the Staff program (one page or one image).

## Starter guidance

- **The pattern is always the same on the Staff side: strategy, program, consumer contract.** The peer specialist owns *implementation*; the peer platform owns *build*; the peer governance track owns *policy*. If a Staff-side row of your contract does not fit one of "strategy, program, consumer contract," something is wrong.
- **Watch for the "I know this better than the specialist" temptation.** Chapter 4 flags this as the single most common way the Staff/specialist contract dissolves. Every implementation call the Staff engineer holds is a call the peer specialist did not make.
- **Non-goals kill more scope arguments than any other section.** Learners systematically underweight this. Three per contract minimum, each pointing at where the work actually lives.
- **Roles, not people.** If a section names a specific engineer by first name, rewrite it in role vocabulary. The contract should read the same after either incumbent rotates.
- **Cadences should be realistic.** A "weekly 30-minute program sync" that is not on the actual calendar is a wish-list, not a contract. Chapter 4 flags "aspirational cadence" as a failure mode. If you cannot commit the calendar, do not write the cadence.
- **The unstaffed-peer scenario is where the exercise pays off longest.** Most Staff ML engineers *will* face a case where the peer track is not yet built. Writing that contract *now* as a hiring spec is what turns a personal knowledge hoard into an org-scope artifact.

## Acceptance criteria

- Three complete six-section contracts, one to a peer specialist track, one to a peer platform track, one to a peer governance or security track.
- Every contract has an explicit peer-track-owns section, an explicit Staff-owns section, an API section with real cadences and artifacts, an escalation section, and at least three non-goals.
- The Staff-side rows in every contract fit the pattern "strategy, program, or consumer contract" — no implementation, build, or policy rows on the Staff side.
- The cross-contract consistency check identifies at least one verb-object that appears in more than one contract and resolves it (either reassigned cleanly or written as explicit shared ownership).
- The failure-mode checklist confirms each contract protects against all five failure modes from chapter 4.
- The unstaffed-peer scenario is written out.
- The program-level dependency graph is sketched.

## Stretch goals

- **Author the reverse contract — from the peer track's seat.** Pick one of your three contracts and rewrite it as if you were the peer specialist, peer platform, or peer governance owner. Compare the two drafts. Divergences are usually where the real boundary work is (same pattern as exercise-03's stretch goal).
- **Extend to a five-peer program.** Add a fourth and a fifth peer track — for example, `fine-tuning-engineer` and `model-evaluation-engineer` for the composite scenario. Do the contracts still hold, or do the fourth and fifth reveal boundaries you missed?
- **Draft the program-scope RFC that lives *on top of* these contracts.** Chapter 4 notes the hand-off contract is not an RFC; it does not defend a specific technical choice. But the choices the RFC defends have to fit the contract. Sketch the mod-402 / mod-403 multi-team RFC for the program and cross-check that every technical decision fits inside the boundaries you drew here.
- **Time-slice the contracts.** How would the Staff-side ownership boxes change if the program is in month 1 vs. month 3 vs. month 6? Some Staff verbs are heaviest at program start (the strategy RFC) and lightest at program end (the consumer-contract steady state); the reverse is true for others. Chapter-4-shape contracts *can* be time-sliced — some organisations write "start-phase" and "steady-state" versions.
- **Compare against the Staff/EM partnership contract from exercise-03.** The Staff/EM contract binds two roles *inside* one team; these three peer-track contracts bind the Staff role to *other* teams entirely. Where do they intersect — where does an EM in your team have to co-sign a hand-off contract to a peer track? Chapter 4's contract template calls out a peer L40 EM co-signer specifically because of this.
