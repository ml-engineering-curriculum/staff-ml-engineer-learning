# exercise-05: Training-Pipeline Hand-Off Contract

**Estimated effort:** 2 hours

## Objective

Write the **two hand-off contracts** that turn the four preceding deliverables (recipe, MFU plan, failure-mode budget, reserve-and-checkpoint strategy) into a signed downward hand-off to the peer specialist and platform tracks: contract A to `training-pipeline-engineer` (implementation, on-call, checkpoint mechanics, six-mode mitigations) and contract B to `ai-infra-performance-learning` (kernel-level tuning, profile-guided optimisation, hardware-generation-specific bake). Roll the exercise-01-through-04 headline numbers into a **one-page recipe summary** for the program brief's technical appendix.

This is section 5 of the mod-404 five-artifact stack. The deliverable is short: two one-page contracts and one one-page summary. Anything longer is a smell.

## Prerequisites

- Chapter 07 — Hand-off contract to training-pipeline and performance tracks (the two-contract shape, deliverables both directions, acceptance criteria, review cadence, hand-back triggers, the one-page recipe summary).
- Exercise-01, exercise-02, exercise-03, exercise-04 all delivered — the contracts depend on the numbers those exercises commit to.
- Chapter 07 of mod-401 (hand-off contract shape at the abstract level) as background.

## Steps

1. **Draft contract A (training-pipeline-engineer).** Use chapter 7's template:
   - Recipe pointer (link to exercise-01).
   - Deliverables from author to receiver: the recipe tuple, MFU plan, failure-mode budget, reserve-and-checkpoint strategy, program-scope defaults (precision, tokenizer, dataloader shape, optimizer, LR schedule).
   - Deliverables from receiver to author: measured MFU at three points, measured HFU and step-time decomposition, implemented failure-mode mitigations with acceptance tests, measured restart-latency, on-call rotation and runbook derived from exercise-03.
   - Acceptance criteria: recipe implemented as specified, measured MFU inside planned band, all six mitigations tested on scratch slice, restart-latency within 1.5× target, runbook covers all six modes.
   - Review cadence: weekly during ablation; twice-weekly for first two weeks of main run; weekly thereafter; out-of-cadence on any re-plan trigger.
   - Hand-back triggers: MFU > 20% below plan with recipe-side root cause; failure-mode rate > 1.5× budget; storage / scheduler substrate cannot meet chapter 6 acceptance criteria; hardware-generation change mid-program.
   - Escalation path: shared manager or portfolio-level architecture review.
2. **Draft contract B (ai-infra-performance-learning).** Same template, lighter cadence:
   - Recipe pointer.
   - Deliverables from author to receiver: recipe tuple and MFU planning band, step-time decomposition, list of operations to profile / tune, expected bottleneck.
   - Deliverables from receiver to author: measured-MFU baseline on target hardware with supporting profile, profile-guided report with prioritised uplift estimates, implemented kernel-level optimisations with before/after MFU deltas, hardware-generation-specific tuning notes.
   - Acceptance criteria: MFU planning band cited in exercise-02 is ai-infra-performance's measured number (not a plausibility guess), bottleneck named in exercise-02 is confirmed or refuted by profile evidence, required optimisations are on their roadmap with dates.
   - Review cadence: at recipe-signing, at ablation-phase-end, as-needed on re-plan trigger.
   - Hand-back triggers: measured baseline materially below planning band with no closable optimisation gap; hardware substitution changes the achievable band by > 10%.
3. **Compose the one-page recipe summary.** Chapter 7's template:
   - Top line: recipe tuple `(TP, PP, DP, SP, EP, precision, recompute, batch, seq)` from exercise-01.
   - MFU and wall-clock: planned MFU with band; wall-clock forecast — from exercise-02.
   - Failure-mode risk multiplier: percentage overhead on wall-clock — from exercise-03.
   - Reserve posture: reserved / burst / spot split; warm-standby percentage — from exercise-04.
   - Hand-off status: contract A and B signed on `<date>`; next review `<date>`.
4. **Sanity-check for anti-patterns.** Chapter 7 names three: Staff-engineer-implements, specialist-re-derives, contract-not-read. State how each contract prevents its named anti-pattern (short paragraph per contract).
5. **State the escalation-and-review calendar.** Concrete: which review meeting the recipe-signing happens in, which team review the ablation-phase MFU check happens in, who is called if a hand-back trigger fires. Two-line calendar entry is fine.

## Deliverable

Three artifacts, delivered as one 3-4 page document:

- **Section 1 — Contract A: hand-off to training-pipeline-engineer.** One page in the chapter 7 template.
- **Section 2 — Contract B: hand-off to ai-infra-performance-learning.** One page in the same template.
- **Section 3 — One-page recipe summary** for the program brief's technical appendix.
- **Section 4 — Anti-pattern-prevention note.** One paragraph per contract naming how it prevents the associated anti-pattern from chapter 7.
- **Section 5 — Escalation-and-review calendar.** Concrete entries; who is called when.
- **Section 6** — Three-sentence personal reflection: which of the two contracts felt harder to write and why? (Usually contract A because it is longer; sometimes contract B because it depends on numbers ai-infra-performance has not measured yet.)

## Starter guidance

- **Keep it short.** Chapter 7 is emphatic that a two-page-total contract is the target. If your contracts exceed one page each, you are probably including implementation detail the training-pipeline engineer should own.
- **Numbers, not prose.** Every acceptance criterion should be numeric and testable. "MFU within band" is not a criterion; "MFU ≥ 35% (band lower bound from exercise-02) at ablation-phase-end" is.
- **Signatures matter as a ritual.** In a real hand-off, the two contracts are signed at a recipe-signing meeting with the author, the specialist receiver, and both engineering managers. State the ceremony explicitly — a contract that nobody explicitly signed is a contract that nobody read.
- **The one-page summary is what leadership reads.** Test it against the executive-review question *"can the CFO understand what we are committing to in 90 seconds?"* If it takes longer, tighten.
- **Contract B is often shorter than A** because ai-infra-performance's deliverable is a measured baseline and a profile report, not an implementation of the entire training loop. A short contract B is not under-scoped; it is correctly scoped.
- **Do not re-derive.** The contracts point to exercises 01-04 by reference. Do not repeat their numbers except in the one-page summary. Duplication invites drift when exercise-02 gets revised and the contract does not.
- **Do not delegate ownership.** The Staff engineer signs both contracts as the accountable party for the recipe. If contract A reads like the training-pipeline engineer owns the recipe, the boundary has moved wrong; re-read chapter 1's Staff-vs-specialist split.

## Acceptance criteria

- Both contracts follow the chapter 7 template with all seven sections (pointer, deliverables both directions, acceptance criteria, review cadence, hand-back triggers, escalation).
- All acceptance criteria are numeric and testable — no "within reason", "as needed", or "high quality" language.
- Contract A includes a runbook hand-off derived from exercise-03's failure-mode narratives.
- Contract B cites ai-infra-performance's measured MFU baseline as the source of the planning band in exercise-02.
- The one-page recipe summary contains the top-line recipe tuple, MFU + wall-clock, failure-mode multiplier, reserve posture, and hand-off status.
- Anti-pattern-prevention paragraphs name each of chapter 7's three anti-patterns and state a specific contract mechanism that prevents each.
- Escalation-and-review calendar has at least three concrete entries: recipe-signing, ablation-phase-end review, main-run-week-1 review.
- Reflection paragraph makes a specific observation about the contract-authoring experience.

## Stretch goals

- **Contract C, upward.** Author a third contract that mirrors chapters 1 and 7's upward-contract framing: the Staff engineer's commitment back to mod-403's program-brief owner (usually the same person, but a different hat). Deliverables: revised cluster-hour rollup, revised wall-clock forecast, revised risk multiplier, revised re-plan-trigger inventory. This is the contract that closes the feedback loop into the program budget.
- **Runbook draft.** Take exercise-03's per-mode narratives and turn them into a two-page on-call runbook — the derived artifact contract A commits to. Each mode is a section with signature, decision tree, and page-out criteria. This is the actual document the 3-AM-on-call reads.
- **Dry-run the recipe-signing meeting.** Write a five-minute agenda for the meeting: recipe walkthrough (recipe author leads), MFU-and-bottleneck check (ai-infra-performance leads), failure-mode-and-runbook check (training-pipeline lead), reserve-and-checkpoint check (platform lead), sign-off. Include the specific numbers each track must confirm during the meeting.
- **Compare with a real hand-off.** If your program brief is real (option 1 or 2 from exercise-01), compare your contract shape against how your organization actually handles the recipe-to-implementation transition. Where does your contract include something that would be surprising to your training-pipeline team? Where does it *omit* something they would expect?
- **Portfolio-scope escalation path.** Author the escalation path for the case where both contract A and contract B trigger a hand-back simultaneously (e.g., MFU is well below plan and ai-infra-performance's measured baseline is also below plan). Who chairs the re-plan meeting? What is the mod-410-scope leadership move? Chapter 7 gestures at this; you fill in the specifics.
