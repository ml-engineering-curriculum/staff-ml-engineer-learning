# The Hand-Off Contract to Training-Pipeline and Performance Tracks

## Motivation

The four preceding chapters produced four artifacts: the recipe, the MFU plan, the failure-mode budget, the reserve-and-checkpoint strategy. This chapter turns them into a *contract* — a signed document that says what crosses the boundary from the Staff ML engineer to the peer specialist tracks, what the specialists are entitled to assume, what they commit to deliver back, and what triggers a hand-back if the contract's assumptions break under measurement.

The trap this chapter exists to avoid: a recipe hand-off that is a Slack thread and a wiki page nobody re-reads. Programs that hand off that way spend the first two weeks of the ablation phase re-litigating the recipe because the training-pipeline engineer built to a mental model that diverged from the Staff engineer's plan on some load-bearing detail (which precision, which pipeline schedule, which checkpoint format). A written contract with acceptance criteria at each interface stops the drift.

This is the same shape of contract mod-401 introduced abstractly and mod-403 chapter 3 operationalised for the program-brief hand-off. Chapter 7 is the mod-404 instantiation for the two most load-bearing downward hand-offs: to **training-pipeline-engineer** (peer specialist track, implements the training loop) and **ai-infra-performance-learning** (peer platform track, owns kernel-level throughput). The contract is not adversarial; it is the mechanism that lets both sides do their jobs without spending review time re-deriving what was already decided.

## What a hand-off contract is (and is not)

A hand-off contract, in the mod-404 sense, is:

- A **short written document** (one to two pages per hand-off) that names what artifacts move, what acceptance criteria apply, what the review cadence is, and what triggers escalation.
- **Signed** — or explicitly acknowledged, at whatever ceremony the program uses — by the Staff ML engineer (author), the receiving specialist / platform engineer, and their engineering manager or tech lead.
- **Living** in the sense that its numeric parameters (planned MFU, restart-latency target, checkpoint interval) can be renegotiated when measurement disagrees with plan — but the *shape* of the contract does not shift mid-program.

It is not:

- **An implementation spec.** The contract does not say *how* the training-pipeline engineer wires FSDP; it says *what* the FSDP wiring must achieve (MFU band, restart-latency target).
- **A one-way document.** Both sides are on the hook. If the training-pipeline engineer's measured MFU misses the recipe's plan by 20%, they owe the Staff engineer a diagnostic direction; the Staff engineer owes them a re-plan or an accepted new baseline.
- **A legal agreement.** The tone is professional and specific but not lawyerly. The point is shared understanding, not enforceable claims.

## The two contracts this chapter names

### Contract A — Staff ML engineer → training-pipeline-engineer

The specialist track that implements the training loop. Owns the code, the configuration, and the operational on-call.

**Staff engineer delivers to training-pipeline-engineer.**

1. **The recipe tuple** from chapter 3, in the form `(TP, PP, DP, SP, EP, precision, recompute, batch, seq)`, with the mesh-to-topology mapping and the choice of pipeline schedule (GPipe / 1F1B / interleaved 1F1B with V, etc.).
2. **The MFU plan** from chapter 4: planned MFU with band, step-time decomposition, expected bottleneck, the instrumentation categories the engineer must build dashboards for, and the numeric re-plan trigger.
3. **The failure-mode budget** from chapter 5: the six-row table, with detection cadence, response SLO, and compute-and-wall-clock line items per mode.
4. **The reserve and checkpoint strategy** from chapter 6: capacity split, warm-standby size, checkpoint interval and retention, restart-latency budget with component decomposition, storage acceptance criteria.
5. **The program-scope defaults** the training-pipeline engineer inherits without re-litigating: precision policy, tokenizer, dataloader shape, optimizer choice, learning-rate schedule.

**Training-pipeline-engineer delivers back to Staff engineer.**

1. **Measured MFU** at three points: end of the ablation phase, end of the first 5% of the main run, and rolling throughout the main run. Reported against the recipe's planned band.
2. **Measured HFU and step-time decomposition** matching the categories chapter 4 named. If the measured decomposition disagrees with the plan, the diagnostic direction the engineer reads out of it.
3. **Implemented failure-mode mitigations** for each of the six modes: the NaN detector, the straggler eviction integration, the NCCL watchdog, the checkpoint verification pipeline, the loss-spike alerting, the dataloader configuration. Each with an acceptance test the Staff engineer can inspect.
4. **Measured restart-latency** with the component decomposition, benchmarked at least once on a scratch slice of the reserved cluster before the main run begins.
5. **On-call rotation and runbook** — the training-pipeline team is on the pager for the six-mode responses. The Staff engineer is the escalation point for recipe-level re-plans, not for individual incident response.

**Acceptance criteria** — how the Staff engineer signs off the specialist's work before the main run starts:

- Recipe implemented as specified. Mesh geometry matches; the actual pod-to-topology mapping matches the mesh-to-topology map in the recipe doc.
- Measured MFU at ablation-phase-end lands inside the planned band, or a diagnostic and re-plan is delivered.
- All six failure-mode mitigations have acceptance tests that pass on a scratch slice — a NaN injected into the loss triggers the detector; a straggler simulated by artificial delay triggers eviction; a checkpoint written with corruption fails the read-back verification.
- Measured restart-latency lands within 1.5× of the target.
- Runbook covers each of the six modes with an on-call decision tree.

**Review cadence.** Weekly during ablation; twice-weekly for the first two weeks of the main run; weekly thereafter. Any measurement that trips the re-plan trigger from chapters 3, 4, 5, or 6 escalates to an immediate review outside cadence.

**Hand-back triggers.** Situations where the training-pipeline engineer returns to the Staff engineer for a recipe change rather than continuing under the existing contract:

- Measured MFU is more than 20% below the recipe's planned band and the diagnostic direction points at the recipe (e.g., pipeline bubble larger than budgeted, TP AllReduce not fitting in NVLink) rather than at implementation.
- Observed failure-mode rate is more than 1.5× the budgeted rate for any single mode.
- Storage or scheduler substrate cannot meet the reserve-and-checkpoint chapter's acceptance criteria — recipe re-plans with a longer restart-latency budget or a different checkpoint cadence.
- Any hardware-generation change during the program (H100 → H200 mid-run, for example) — the recipe is re-derived from scratch.

### Contract B — Staff ML engineer → ai-infra-performance-learning

The peer platform track that owns kernel-level tuning and profile-guided optimisation. The relationship is *lighter-touch* than contract A but load-bearing at the top of the MFU band.

**Staff engineer delivers to ai-infra-performance.**

1. **The recipe tuple and the MFU planning band.** ai-infra-performance is the source-of-truth for the *achievable* MFU on the specific hardware generation; the recipe's plan quotes their number and cites their measurement.
2. **The step-time decomposition** from chapter 4. Which operations the recipe expects to dominate the wall-clock and which the recipe is willing to accept as slow.
3. **The list of specific operations** the recipe wants profiled and (if warranted) tuned: attention (FlashAttention version), MLP GEMMs, LayerNorm-and-dropout fusion, optimizer step, NCCL collectives, custom kernels (if any).
4. **The bottleneck the recipe expects to sit against** (chapter 4). ai-infra-performance validates or refutes.

**ai-infra-performance delivers back to Staff engineer.**

1. **A measured-MFU baseline** on the target hardware and target model shape, with the profile that supports the number. This is the number the recipe's plan quotes.
2. **A profile-guided report** identifying the operations that would benefit from tuning and the projected MFU uplift per optimisation. Prioritised by cost-benefit.
3. **Implemented kernel-level optimisations** where cost-benefit warrants and the training-pipeline engineer can integrate. Each with a before-and-after measured MFU delta.
4. **A hardware-generation-specific tuning note** for each new hardware generation the program considers — H100 vs. H200 vs. B200 differ substantially in HBM bandwidth, tensor-core throughput, and NVLink topology, and the per-generation tuning matters.

**Acceptance criteria** — before the recipe is signed:

- The MFU planning band cited in chapter 4 is the ai-infra-performance measured number, not a plausibility-guess from a published paper on different hardware.
- The bottleneck the recipe names in chapter 4 is either confirmed or refuted by profile evidence.
- Any kernel-level optimisations required to hit the planning band are on ai-infra-performance's roadmap with dates, not implicit in the recipe.

**Review cadence.** At recipe-signing time, then at ablation-phase-end (measured MFU review), then as-needed if measurement trips the re-plan trigger.

**Hand-back triggers.** The recipe re-opens if:

- Ai-infra-performance's measured baseline is materially below the recipe's planning band, and no optimisation identified in the profile-guided report closes the gap by the ablation-phase deadline.
- A hardware substitution changes the achievable band by more than 10%.

## What is *not* in either contract

Explicitly out of scope for the mod-404 hand-off:

- **Cluster provisioning, IB fabric topology, storage backend selection.** These are peer platform-track (ai-infra-ml-platform / ai-infra-mlops) hand-offs, handled through the mod-401-style peer-track contract, not this chapter's contract A or B.
- **Data curation, data-quality filtering, tokenization decisions.** These are mod-403 program-scope decisions the recipe author does not re-decide.
- **Evaluation-benchmark choice, eval-waterfall design.** These are mod-406 concerns; the recipe consumes the eval cadence rather than defining it.
- **Post-training / safety / RLHF-adjacent decisions.** These are mod-403 chapter 7 and mod-409 concerns.

If the Staff engineer finds the hand-off contract expanding to cover any of the above, either the recipe is over-scoped or the peer-track boundary is unclear. Chapter 1's heuristic — *am I doing recipe-scope work?* — is the check.

## The one-page contract template

The contract shape distilled to a template the exercise-05 deliverable adopts.

```
Contract: <program name> recipe hand-off
Between: <Staff ML engineer>, author
     and: <training-pipeline-engineer / ai-infra-performance-engineer>, receiver
Approved: <manager / tech lead of both>
Date: <YYYY-MM-DD>; Next review: <YYYY-MM-DD>

1. Recipe pointer
   [link to the recipe-decision doc, MFU plan, failure-mode budget, reserve strategy]

2. Deliverables from author to receiver
   [enumerated list of the artifacts in section "delivers" above]

3. Deliverables from receiver to author
   [enumerated list of the artifacts in section "delivers back" above]

4. Acceptance criteria
   [enumerated, numeric where possible, testable]

5. Review cadence
   [regular cadence + trigger-driven escalation]

6. Hand-back triggers
   [enumerated conditions that re-open the recipe]

7. Escalation path
   [who is called when a hand-back trigger fires but the two teams cannot agree
    on the re-plan direction; usually the shared manager or a portfolio-level
    architecture review]
```

One page for contract A, one page for contract B. Two pages total is the exercise-05 deliverable. If it takes more than two pages, either the recipe is under-specified (a lot of implicit assumptions are being written down for the first time in the contract, which is a smell) or the contract is doing implementation work the training-pipeline engineer should own.

## The review-cadence rhythm

The chapter's contracts specify review cadence explicitly because programs that let cadence slip end up rediscovering the recipe assumptions in incident retros. Concrete rhythm for a typical Staff-scope program:

- **Week 0 (recipe sign-off).** Contract A and B signed. Ai-infra-performance's measured baseline is on the recipe doc.
- **Week 1-2 (ablation phase).** Weekly reviews. The training-pipeline engineer runs 1-2 ablation-scale test runs; the Staff engineer looks at measured-MFU-vs-plan; both sides check the failure-mode mitigations under adversarial injection.
- **Week 3 (main-run start).** Twice-weekly reviews. First 5% of main run is the highest-risk window; anything catastrophic surfaces here.
- **Week 4 onward (main run).** Weekly reviews with per-review scan of MFU drift, failure-mode incident count, checkpoint IO drift. Any re-plan trigger escalates outside cadence.
- **Main-run end and post-mortem.** The Staff engineer and the specialists jointly write the post-run report; the recipe's numeric assumptions are compared to the measured reality; the delta feeds the next program's recipe.

The rhythm is not decorative. Programs that skip week-1-2 reviews find out at week 4 that the failure-mode mitigations were not implemented for two of the six modes. Programs that skip week-3 twice-weekly reviews miss the loss-spike-early-in-training that a longer-checkpoint-interval could have absorbed.

## When the contract goes wrong — three anti-patterns

Failure modes of the hand-off contract itself, worth naming so the recipe author catches them:

### Anti-pattern 1 — The Staff engineer implements

The Staff engineer, frustrated by the training-pipeline engineer's implementation pace or worried about a subtle recipe detail, starts writing training-loop code. The specialist track loses ownership; the Staff engineer starts spending time on implementation reviews instead of the next program's recipe. The recipe author's altitude drops. Retros later blame "handoff friction" — the diagnosis is scope drift by the recipe author.

Prevention: the recipe author's calendar time should be spent on the *next* recipe and on hand-off review, not on the current recipe's implementation. If more than ~10% of a week is going to training-loop code, the recipe author is doing training-pipeline-engineer work.

### Anti-pattern 2 — The specialist re-derives the recipe

The training-pipeline engineer, given a recipe they disagree with, quietly changes the parallelism configuration during implementation. Measurement disagrees with the recipe's plan. Both sides spend two weeks arguing about whose plan was right, when the actual issue is that they implemented different plans.

Prevention: the contract's *deliverable 1* — the recipe tuple — is treated as spec-not-suggestion. If the specialist has a substantive objection, they file a hand-back trigger and the two re-plan together; they do not silently change the config.

### Anti-pattern 3 — The contract exists but is not read

The contract is written, signed, filed, and never re-read. Six weeks into the main run, an on-call engineer facing a loss spike does not know whether to auto-restart or page for a human decision, because the response SLO from chapter 5 is in a document nobody has open. Retros later blame "incident response gaps" — the diagnosis is that the contract's failure-mode SLOs did not make it into the runbook.

Prevention: the contract's *deliverable* to the training-pipeline engineer includes a runbook that is *derived from* the contract's failure-mode SLO table. The runbook, not the contract, is what the on-call reads at 3 AM. The contract's job is to make sure the runbook contains the right numbers.

## The five-artifact stack, closed

The five artifacts of this module — recipe (chapter 3), MFU plan (chapter 4), failure-mode budget (chapter 5), reserve-and-checkpoint strategy (chapter 6), hand-off contract (chapter 7) — compose into the one-page recipe summary that sits in the program brief's technical appendix. Structure:

- **Top line.** The recipe tuple `(TP, PP, DP, SP, EP, precision, recompute, batch, seq)`.
- **MFU and wall-clock.** Planned MFU with band; wall-clock forecast.
- **Failure-mode risk multiplier.** Percentage overhead on the wall-clock forecast.
- **Reserve posture.** Reserved / burst / spot split; warm-standby percentage.
- **Hand-off status.** Contract A and B signed on `<date>`; next review `<date>`.

That one-page summary is what mod-403 chapter 3's budget-defense reads. The rest of the five artifacts are what the reviewer of the summary can drill into.

## Summary

- The mod-404 hand-off contract turns the four preceding chapters' artifacts into a **signed, short, living document** that names what crosses the boundary, what the acceptance criteria are, what the review cadence is, and what triggers a hand-back.
- **Contract A** goes to the **training-pipeline-engineer** peer specialist track. Recipe + MFU plan + failure-mode budget + reserve strategy delivered in; measured MFU + implemented mitigations + measured restart-latency + on-call rotation delivered back. Weekly-then-twice-weekly review cadence.
- **Contract B** goes to **ai-infra-performance-learning** peer platform track. Recipe + step-time decomposition delivered in; measured MFU baseline + profile-guided report + kernel-level optimisations delivered back. Lighter cadence.
- **Explicitly out of scope**: cluster provisioning, data curation, evaluation design, post-training. Each has its own peer-track contract elsewhere in the curriculum.
- The **template** is one page per contract. Two pages total is the exercise-05 deliverable.
- The **review-cadence rhythm** — weekly during ablation, twice-weekly at main-run start, weekly thereafter, out-of-cadence on re-plan trigger — is what stops recipe assumptions from drifting silently until the retro.
- Three **anti-patterns** — Staff-engineer implements, specialist re-derives, contract-not-read — are the failure modes to watch. The prevention in each case is disciplined scope and disciplined communication, not more artifacts.
- The five artifacts compose into a **one-page recipe summary** for the program brief's technical appendix. That summary is the top-of-page communication with leadership; the underlying documents are the drill-in.

This closes the module. The learner returns to mod-403 with a defensible recipe, an MFU plan, a failure-mode budget, a reserve strategy, and a hand-off contract — the five artifacts the Staff ML engineer signs their name to on a training program. The next module (mod-405, ML platform strategy) zooms out from a single program to the portfolio of programs the org runs and the platform investments that make them faster.
