# The Cost / Throughput / Utilisation Review — Finding the Workloads That Should Move

## Motivation

Chapters 2 and 3 planned the demand and the capacity to meet it. This chapter is about what to do when the *plan* is right and the *execution* is quietly wrong — when an inference workload is running at 12% of the throughput MLPerf-derived math says the hardware can sustain, when a hosted-API line item on the cloud bill has grown to the point where self-hosting would pay for itself in six months, when a batch pipeline is running on H100s that a T4 could serve, or when a workload is sitting on the wrong cloud because it was launched there historically and nobody has re-priced it since. Each of these is a **workload-migration decision**, and each is Staff-scope work because it arbitrates across teams and workloads that no L30 tech-lead is chartered to decide.

The primary artifact of this chapter is the **quarterly cost / throughput / utilisation review** — a review meeting run against a written diagnostic, from which one or more **workload-migration docs** are commissioned. Pairs with [`exercise-03`](exercises/exercise-03-utilisation-review-and-migration-doc.md).

## What the review is as an artifact

The **quarterly cost / throughput / utilisation review** is a two-hour meeting run against a written diagnostic (5-8 pages) that answers, for every workload class in the portfolio, three questions:

1. **What does it cost?** Dollars per quarter, broken down by capacity bucket (chapter 3's five modes), attributed to a specific owning team.
2. **What is its throughput?** For inference: requests per second per GPU, tokens per second per GPU, dollars per 1000 requests. For training: MFU, dollars per accelerator-hour spent, hours to converge. Compare each to a reference — MLPerf for inference, program-brief plan for training.
3. **What is its utilisation?** For inference: GPU utilisation of the serving fleet during peak and off-peak, tail latency at target SLO. For training: pod utilisation, checkpoint / restart overhead as fraction of wall-clock.

Any workload whose cost, throughput, or utilisation is materially off from the reference is a **candidate for migration** — the next section walks the four migration types.

The review does not decide migrations. It *surfaces candidates*. Migration decisions require a **migration doc** — a written case for moving a specific workload from one capacity mode (or one implementation) to another, defended against alternatives, with an implementation plan and a rollback. The migration doc is what chapter 4 pairs the review meeting to.

## The four migration types

Not every workload migration is the same call. Naming the four types up front makes the diagnostic sharper and the migration doc easier to write.

### 1. Managed API ↔ Self-hosted

The archetype: an ML team is calling a hosted LLM API for inference (OpenAI, Anthropic, Google Vertex, Bedrock, etc.). The line item is $X per month. At what usage does the org self-host on vLLM / TGI / Triton against an open-weights model of comparable quality and pay less?

**When managed-API wins.** Low or bursty volume. Model quality that open-weights cannot yet match on the relevant benchmark. Regulatory / audit requirements that the hosted API already satisfies and self-hosted would need to re-earn. Product that has not yet hit the traffic to justify the operational overhead of hosting.

**When self-hosted wins.** High, predictable volume. An open-weights model of adequate quality. Existing GPU capacity that self-hosting can utilise. A peer platform team already running an inference gateway. Latency requirements the hosted API cannot meet (e.g., sub-100ms token latency on a large model).

**Rough break-even.** The break-even math is workload-specific and shifts every quarter as hosted-API prices drop and open-weights model quality improves. A defensible order-of-magnitude: at hosted-API rates of ~$1-15 per million output tokens (as of late 2024) <!-- needs-research: verify current hosted-API pricing tiers -->, self-hosting on vLLM against a similar-quality open-weights model on H100/H200 hardware becomes cost-competitive somewhere between **$10K/month and $100K/month** of hosted-API spend, depending on model size, throughput, and the org's already-in-place platform. Below $10K/month, self-hosting almost never pays for the operational overhead. Above $100K/month, self-hosting almost always wins on dollars but may lose on quality if the open-weights alternative is a generation behind. The **PagedAttention / vLLM** paper (Kwon et al. 2023) documents the throughput uplift that makes self-hosting inference competitive at all, and its followups on KV-cache economics inform the break-even.

**What to watch for.** Do not include *only* the direct dollar comparison. Include: engineering time to build and maintain the serving stack, the SLO regression from managed → self (uptime, latency, moderation stack), and the *contract-out* option if the workload is small enough. Do not migrate a $20K/month workload; the migration doc will consume more engineering than the workload spends.

### 2. GPU-generation migration (H100 ↔ H200 ↔ B200, or older-to-newer)

The archetype: workloads are running on the hardware they were launched on. A new hardware generation ships with materially better performance-per-dollar. When does the org re-plan against the new generation?

**When to migrate.** The new generation's price-performance is materially better *for the specific workload* (not for MLPerf's reference workload). The new generation is available on the org's capacity plan (in reserved or in supply). The workload can be recompiled / re-tuned against the new generation without regressing quality. NVIDIA's public datasheets (H100 → H200 raised memory bandwidth ~40% and capacity 76% while keeping compute the same; B200 raised compute ~2-2.5× depending on precision) give the ceiling for the performance uplift; the real number is workload-dependent and requires benchmarking. <!-- needs-research: verify the H200 memory-bandwidth and B200 compute-uplift ratios against NVIDIA's current whitepapers. -->

**When to defer.** The workload is small enough that the migration cost dominates the savings. The workload is running on a reserved commit that will not roll off for another year (the commit is a sunk cost; the migration only helps *new* demand). The new generation is supply-constrained and the org cannot get enough of it to move meaningfully.

**Rough sizing.** If a workload is above ~$100K/quarter in on-demand or committed-use spend, the migration doc is usually worth writing. Below that, revisit next quarter.

### 3. Cross-cloud migration

The archetype: an inference workload was launched on AWS because the product team was already there; the same workload could run 20% cheaper on GCP because of a specific reserved-commit differential. Or the reverse.

**When to migrate.** The cross-cloud price differential is meaningfully larger than the egress / re-platforming cost (usually 20%+ on the target line item). The workload's data locality does not force a specific cloud. The org's ML platform already runs on the target cloud (see [`mod-405 chapter 5`](../mod-405-ml-platform-strategy/) if the platform-team side is uncertain).

**When to defer.** The workload is data-heavy and the egress cost eats the savings. The org lacks a platform on the target cloud. The migration would require re-training against a different accelerator (see #2). The differential is small (<20%) and the operational overhead of running a workload on a non-primary cloud is real.

### 4. Consolidation within a cloud (right-sizing / co-tenanting)

The archetype: the org runs three separate inference fleets for three related models, each at 20% utilisation. Consolidating onto a shared serving stack (Triton or vLLM with model-level routing) reduces the fleet size by 40-60% at the same SLO.

**When to consolidate.** Utilisation of an existing fleet is materially below reference. A shared serving substrate exists or is being built by the platform team. The SLO consequences are understood (co-tenanting can raise tail latency).

**When to defer.** Models have genuinely different SLOs. The platform substrate is not ready. The consolidation would sit across team ownership boundaries in a way the [`mod-402`](../mod-402-multi-team-ml-architecture/) portfolio blueprint has not resolved.

This migration type is the most closely bound to peer-track work — the *implementation* is the peer performance track's or peer platform track's job. The Staff engineer's contribution is the diagnostic, the migration doc, and the arbitration if two teams disagree.

## The diagnostic — what the review reads against

The review's diagnostic is a table with one row per workload and columns for:

1. **Owning team.** Named. Present in the review meeting.
2. **Capacity mode.** Reserved / committed / on-demand / spot / self-hosted / hosted-API.
3. **Current quarter spend.** From FinOps chargeback / showback.
4. **Requests per second (inference) or accelerator-hours (training).**
5. **Throughput vs. reference.** For inference: measured throughput / MLPerf-derived reference throughput. For training: measured MFU / program-brief planned MFU.
6. **Utilisation.** Peak / off-peak GPU utilisation for inference; pod utilisation for training.
7. **Flag.** One of `green` (within 20% of reference), `yellow` (20-50% below), `red` (>50% below or a hosted-API workload above the $100K/quarter threshold).

Every `yellow` or `red` row is a migration candidate. The review meeting's output is a decision, per candidate, of one of:

- **Ship migration doc.** The workload merits a full migration doc; assign an owner and a target review date.
- **Defer.** The candidate is real but not this quarter's priority. Re-review next quarter with a specific trigger.
- **Explain away.** The candidate's numbers look bad but there is a legitimate reason (e.g., a workload is running below-reference throughput because the model was recently swapped and re-tuning is in flight). Document the reason so the next quarter's review does not raise it again.
- **Escalate.** The candidate implicates cross-team ownership disagreements that the review body cannot resolve. Escalate to leadership.

The review is not a place to *do* the migration — it is a place to *commission* the migration doc. Migrating a workload in a review meeting is the same class of mistake as merging an RFC in an architecture-review meeting.

## The migration doc — section shape

The migration doc mirrors the multi-system RFC template from [`mod-402 chapter 5`](../mod-402-multi-team-ml-architecture/) but is workload-scoped rather than portfolio-scoped:

1. **Executive summary** — one paragraph and a table. Current cost, projected cost, savings, engineering cost, payback period.
2. **Current-state description** — the workload, its owning team, its capacity mode, its current cost / throughput / utilisation numbers with references.
3. **Proposed migration** — the target capacity mode / hardware / cloud / implementation, and the migration path from current to target.
4. **Alternatives considered** — the other migration types that were considered and rejected, and the "do nothing" alternative with its cost.
5. **Cost model** — dollar comparison over one year and three years, including one-time migration costs and the ongoing operational delta. Sensitivity to the two most-uncertain assumptions.
6. **SLO / quality impact** — what regressions are expected, what quality re-tuning is required, what the acceptance criteria are for the migrated workload.
7. **Implementation plan** — who does what, in what order, with which peer platform / peer specialist track involvement. Rollback plan. Kill switches.
8. **Political cost** — who loses control of what, who has to change process, who is on-call for the migrated workload. Same discipline as mod-402 chapter 5.
9. **Success metrics** — the numbers the next review will read to decide whether the migration succeeded.

Sections 8 and 9 are the most often skipped and the most often the reason a migration doc gets rejected. A migration doc without a political cost section will be blocked in review by whichever team feels blindsided; a migration doc without success metrics will get quietly forgotten and the same workload will re-appear as a red-flag row in a future quarter's review.

## What Staff owns vs. what peer tracks own

**Peer FinOps team owns** the chargeback and showback substrate the review reads from. If the diagnostic column "current quarter spend" is unreliable, the review cannot run; that is a FinOps gap the Staff engineer names but does not fill.

**Peer performance track ([`ai-infra-performance-learning`](https://github.com/ai-infra-curriculum/ai-infra-performance-learning))** owns the actual throughput optimisation once the migration is commissioned. The Staff engineer's job is the diagnostic — "throughput is 12% of reference" — not the fix.

**Peer platform track ([`ai-infra-ml-platform-learning`](https://github.com/ai-infra-curriculum/ai-infra-ml-platform-learning))** owns the shared serving substrate that consolidation targets. If the migration doc's target requires a substrate the platform team has not built, the migration is blocked on the platform roadmap.

**Owning-team L30 tech lead** owns the workload today and will own it after the migration. They are the primary reviewer of the migration doc. If they resist, the migration is contested and often the correct output is escalation, not force.

**Staff at portfolio scope owns** the diagnostic, the quarterly review body, the commissioning of the migration doc, the arbitration when teams disagree, and the migration doc if the workload does not have a clear owner willing to author it.

## The review cadence and the escalation gates

The review cadence is quarterly — matched to the demand-forecast refresh cadence from chapter 2. Monthly is too frequent (numbers do not move fast enough to justify the meeting overhead); annual is too infrequent (migrations that could have paid off in Q1 sit until Q4).

The review body is small — the Staff engineer, the finance partner, one representative from the peer platform team, one from FinOps, and (for that meeting only) the L30 owner of any workload with a `yellow` or `red` flag. Larger bodies make the meeting political rather than diagnostic.

Escalation gates:

- **Workload spends >$500K/quarter and is red-flagged for two consecutive reviews.** Escalate to VP / director.
- **A migration doc is blocked on cross-team disagreement for more than one review cycle.** Escalate to the peer-EM per the mod-401 chapter 5 partnership contract.
- **The diagnostic is unreliable (FinOps allocation gaps).** Escalate to leadership as a FinOps investment ask; the review cannot proceed usefully until fixed.

Chapter 5 walks the FinOps operating discipline that supplies the review with reliable data and closes the escalation loop.

## Summary

- The **cost / throughput / utilisation review** is a quarterly artifact that surfaces workload-migration candidates. The review does not migrate workloads; it *commissions migration docs*.
- Four migration types: **managed API ↔ self-hosted, GPU-generation, cross-cloud, consolidation.** Each has a distinct decision framework and break-even threshold.
- The diagnostic uses **MLPerf-derived references for inference throughput** and **program-brief plan for training MFU**. A workload materially off reference (>20% for yellow, >50% for red) is a candidate.
- The **migration doc** mirrors the mod-402 chapter 5 RFC template with a workload-scoped variant. Political cost and success metrics are the most-skipped and most-load-bearing sections.
- The Staff engineer owns the diagnostic, the review body, and the arbitration; the peer performance / platform / FinOps tracks own the underlying substrate and the actual throughput fix.
- The review cadence is quarterly, the review body is small, and the escalation gates are named up front.

The next chapter walks the FinOps operating discipline — Inform / Optimise / Operate — that keeps the review supplied with reliable data and keeps the ML org's cost story defensible year over year.
