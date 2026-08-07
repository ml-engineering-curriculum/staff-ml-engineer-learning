# Failure Modes and the Mitigation Cadence

## Motivation

A recipe defended for its MFU alone is a recipe that will surprise its owner. A cluster of 512 accelerators running for a week is exposed to roughly `512 · 168 = 86 000` accelerator-hours of failure surface, and the empirical mean-time-to-failure of a single H100 in a large-cluster deployment is measured in thousands to tens of thousands of accelerator-hours (Meta's OPT-175B logbook and the LLaMA-3 paper both give first-hand numbers). *Something will fail*. The recipe that plans for it — enumerates the failure modes, sets a detection cadence for each, budgets the compute and wall-clock the mitigations consume — is the recipe that lands the program on time. The recipe that treats failures as one-off exceptions is the recipe that quietly bleeds 20-40% of its cluster-hour budget to unplanned restarts.

This chapter is the failure-mode taxonomy. Six named modes, one per section, each with the same four sub-headings: **signature** (how you know it is happening), **detection cadence** (how the recipe decides how often to look), **response SLO** (how quickly the program owner commits to acting), **mitigation cost** (compute and wall-clock line-items that roll into the mod-403 cluster-hour budget). The chapter closes with the failure-mode budget table that becomes exercise-03's deliverable and the risk column that feeds the mod-403 chapter-3 budget-defense conversation.

The load-bearing framing: **budgeting failure modes is a first-class recipe activity, not an operational-team concern**. If the recipe author leaves the failure budget to "we'll figure it out during the run," the program spends its risk-multiplier on incidents it could have priced upfront.

## The six failure modes this module names

Different training-programs face different distributions of these modes, but the six-mode taxonomy is comprehensive enough that any Staff-scope recipe should have a line for each. The six are:

1. **Silent numerical corruption / NaN loss.** The loss goes to `NaN` (or a gradient does) and, if unchecked, the model's weights are irreversibly poisoned.
2. **Straggler nodes.** One or a small number of accelerators run slower than the rest, dragging the whole synchronous step to its speed.
3. **Network partition.** An IB / NVLink / RoCE partition splits the training-job's communicator; NCCL times out; the job hangs.
4. **Checkpoint corruption.** A saved checkpoint is unreadable, incomplete, or numerically wrong when the job tries to load it.
5. **Gradient explosion / loss spike.** The training loss suddenly increases by orders of magnitude, sometimes without recovering.
6. **Dataloader stall.** Batches stop arriving in time; accelerators sit idle at the start of each step waiting for data.

These six are the historically-attested modes that show up in every published pretraining logbook — OPT-175B (Meta 2022), BLOOM (BigScience 2022), LLaMA-3 (Meta 2024), MegaScale (ByteDance 2024). Ordering matters: silent corruption is first because it is the mode with the *highest cost per undetected incident* — everything else is a wall-clock cost, but silent corruption can invalidate a training run.

## Failure mode 1 — Silent numerical corruption / NaN loss

**Signature.** A single loss value becomes `NaN` or `Inf`. In BF16, the risk is elevated because BF16's dynamic range still includes overflow at extreme activation magnitudes and its low precision magnifies underflow in optimizer state. Silent corruption also includes gradients that become `NaN` on a single rank while other ranks stay clean — a mode that DDP/FSDP will *average through* and quietly poison the weights. It also includes hardware-caused bit flips (single-event upsets from cosmic rays or bad HBM cells) that alter a single weight or activation without any user-visible signal.

**Detection cadence.** Two overlapping strategies, both cheap enough to run on every step:

- **Per-step loss finite-ness check.** After the loss reduction, assert `torch.isfinite(loss).all()`. A single non-finite value is an immediate stop-and-investigate signal. Cost: one all-reduce of a scalar per step — sub-millisecond.
- **Per-step gradient norm finite-ness and threshold check.** After the gradient reduction, compute the global gradient norm; assert finite; assert below a *dynamic* threshold (e.g., 5× the exponential-moving-average of prior-step gradient norms). A norm that spikes 100× is not always corruption — it can also be a legitimate loss spike (mode 5) — but the *detection* is the same, and separating cause is what happens after the stop.

For silent hardware bit-flips, most programs additionally checkpoint frequently enough that the state at last-known-good-loss can be restored, and rely on the loss curve as the delayed detection signal. NVIDIA's DCGM (Data Center GPU Manager) reports ECC-corrected and un-corrected errors; the training-pipeline engineer's telemetry should surface these and the recipe should specify a threshold above which a node is evicted.

**Response SLO.** Two SLOs, per severity tier:

- **Detected non-finite loss or gradient norm.** *Stop within one step, restore from last verified-good checkpoint within one restart-latency budget.* Restart-latency budget is chapter 6.
- **ECC-un-correctable error on any accelerator.** *Evict the node within one step; restart from last verified-good checkpoint on the replacement node within one restart-latency budget.* The replacement comes from the warm-standby pool (chapter 6).

**Mitigation cost.** Compute-wise: the per-step checks are negligible. Wall-clock: each stop-and-restore costs one restart-latency budget (chapter 6 walks the number; a well-instrumented program lands at 15-45 minutes on H100-class clusters). A recipe that budgets, e.g., "two NaN-driven restarts per 100 000-accelerator-hour main run" and multiplies by a 30-minute restart-latency gets a rounding-error cost. A recipe that assumes zero and has to renegotiate the budget mid-run is embarrassing.

**The subtle detail.** *Loss-scale management for mixed-precision.* BF16 does not require dynamic loss scaling the way FP16 did, but FP8 (Transformer Engine) reintroduces the concept via per-tensor scaling factors. If the recipe uses FP8, the failure-mode budget should include a line for FP8 scale-recalibration during the first few thousand steps and a scale-tracking-divergence detector for later.

## Failure mode 2 — Straggler nodes

**Signature.** A synchronous distributed step is bottlenecked by its slowest rank. If one accelerator is thermally throttled, on degraded HBM, or on a bad ECC-recovery loop, its step-time inflates by 5-30% while the rest of the cluster waits at the AllReduce. Aggregated over hours, this looks like an unexplained MFU drop; on a per-step latency histogram, one rank sits at the tail while the rest cluster around the median.

**Detection cadence.** Per-step per-rank timing telemetry is the load-bearing instrumentation:

- **Per-rank per-step wall-clock.** Rank *i*'s forward + backward + optimizer time, logged per step. Compute the P50, P99, and (P99 - P50) rolling over the last 100 steps. A rank whose P99 exceeds 1.15× the cluster median for more than N steps is a straggler.
- **Per-rank GPU utilisation, HBM temperature, HBM-ECC-corrected-error-rate.** DCGM sources. A rank whose temperature has crept above threshold or whose ECC error rate is elevated is a straggler-in-waiting.

**Response SLO.** *Detect within 1000 steps of onset. Evict within one training-step boundary (drain the current step's AllReduce, then the scheduler kills the pod). Replace with warm-standby node and restart within one restart-latency budget.*

The recipe should state a specific N-steps-of-observation threshold. Too tight (10 steps) causes flapping evictions on transient thermal spikes; too loose (10 000 steps) wastes hours of MFU. Published programs typically converge on 200-1000 steps.

**Mitigation cost.** Compute-wise: telemetry is negligible. Wall-clock: each eviction is one restart-latency budget, plus the MFU loss during the observation window. A recipe on a 512-accelerator cluster with a mean-time-between-straggler-events of 24 hours and a 30-minute restart-latency loses roughly `168 / 24 = 7` events per week × 30 min = ~3.5 hours per week to straggler eviction — around 2% of wall-clock. Multiply by the risk multiplier from mod-403 chapter 3.

**The subtle detail.** *Elasticity vs. fixed-topology.* Some frameworks (PyTorch's `torch.distributed.elastic`, DeepSpeed's elastic training) support losing a node without restarting from a checkpoint — the remaining ranks re-form the communicator and continue at a reduced world size. This is a specialist-track implementation call; the recipe simply states whether the program requires bit-exact rerunability (say, for audit reproducibility) or accepts elastic mid-run rescaling.

## Failure mode 3 — Network partition

**Signature.** NCCL kernels do not return. Ranks hang at the next collective. `NCCL_DEBUG=INFO` logs show timeouts and unreachable-peer errors. The training-loop process is alive but consuming no cycles; without a hang-detector, the job appears to still be running while burning cluster-hours to nothing.

**Detection cadence.** Two layers:

- **Watchdog on collective wall-clock.** Every NCCL collective should be wrapped with a timeout. PyTorch's default `pg.timeout` (10 minutes for `nccl` backend in modern versions) is the outer bound; the recipe should specify a tighter timeout (e.g., 30 seconds for AllReduce, 2 minutes for pipeline-boundary sends). A collective that exceeds its timeout is a partition until proven otherwise.
- **External heartbeat.** A separate process (usually the training-pipeline engineer's launcher) that pings every rank every 60 seconds and expects a heartbeat back. A missing heartbeat on any rank triggers a job kill; the launcher restarts.

**Response SLO.** *Detect within one collective-timeout window (typically ≤ 2 minutes). Kill the job. Restart from last verified-good checkpoint within one restart-latency budget.* If the same partition recurs, the recipe escalates to platform (peer track ai-infra-ml-platform / ai-infra-mlops) — a repeated network-fabric problem is not a training-program problem to fix.

**Mitigation cost.** Wall-clock: each partition costs one collective-timeout window plus one restart-latency budget. A well-instrumented program on a healthy fabric may see 0-1 partitions per week; a degraded fabric can see 5+. The failure-mode budget should include a per-week-partition-count assumption and multiply.

**The subtle detail.** *False-positive partitions vs. real partitions.* A slow AllReduce can look like a partition to a tight timeout; a real partition can look like a straggler to a loose timeout. The disambiguation lives in per-rank per-step telemetry (mode 2) and in the NCCL logs. The recipe author does not disambiguate at runtime — the training-pipeline engineer's on-call does — but the recipe should state that the two modes share instrumentation.

## Failure mode 4 — Checkpoint corruption

**Signature.** A saved checkpoint fails to load — file truncated, framework-format mismatch, tensor shape wrong, or (worst case) loads fine but contains numerically-wrong values that only manifest as loss discrepancy several hundred steps into the resumed run. The most-cited primary source is the OPT-175B logbook: multiple checkpoints were unusable and the team had to restore from earlier known-good states.

**Detection cadence.** Detection has to happen at *save* time; catching corruption at load time is too late.

- **Post-save read-back verification.** Immediately after saving, read the checkpoint back into a scratch process and compute a hash / a summary statistic (e.g., the mean and standard deviation of a representative subset of tensors). Compare against the in-memory values. Mismatch = the checkpoint is bad; delete and re-save from the still-live in-memory state.
- **Cross-shard consistency check.** For sharded checkpoints (FSDP, 3D-parallel), each rank saves its shard. A missing or truncated shard should be detected by an all-reduce of per-rank success flags before the checkpoint is declared complete.
- **Periodic load-test.** Every N checkpoints (e.g., every 10th), do a full load-into-a-scratch-cluster-slice test to verify the checkpoint is actually resumable end-to-end.

**Response SLO.** *A save that fails verification is retried within one checkpoint-interval boundary. A corruption caught at load time falls back to the previous checkpoint automatically. A load-test failure escalates immediately.*

**Mitigation cost.** Compute-wise: the read-back verification doubles the checkpoint IO cost. Wall-clock: a well-tuned program overlaps checkpointing with the next training step and the extra IO does not stall training; poorly-tuned programs pay the extra IO synchronously. The load-test is done on a scratch slice, not the main cluster — cost is a small number of accelerator-hours per week.

The higher-order cost is *checkpoint frequency itself*, which chapter 6 optimises. A program that checkpoints too rarely, if a corruption is caught late, loses hours of work per incident; a program that checkpoints too often pays the IO cost every time.

**The subtle detail.** *Async vs. synchronous checkpointing.* Modern frameworks (PyTorch's `dcp.async_save`, DeepSpeed's asynchronous checkpointing, NeMo's checkpoint-io backends) allow the checkpoint save to overlap with training. This shifts the IO cost off the critical path but adds a window where a checkpoint save is in flight when the process dies — and that in-flight checkpoint is potentially the "latest" one. The recipe should state whether async checkpointing is enabled and what the atomicity guarantee is (e.g., "the checkpoint is declared complete only after all ranks' shards land in object storage and the manifest is written").

## Failure mode 5 — Gradient explosion / loss spike

**Signature.** The loss curve, previously monotone-decreasing, jumps upward — sometimes by tenths, sometimes by orders of magnitude. Gradient norms may or may not have spiked in the immediately-preceding steps. Distinguishing a *recoverable* spike from a *terminal* one is the operational judgment; the OPT-175B and PaLM logbooks both document spikes that recovered, and spikes that necessitated restart from an earlier checkpoint.

**Detection cadence.** Same telemetry as mode 1, different response:

- **Loss curve alerting.** Alert on any single-step loss increase above a threshold (e.g., 3× the rolling standard deviation). Also alert on a loss curve that has been flat for more than N steps (a stall, not a spike, but the same root cause of "something changed").
- **Gradient norm history.** A gradient-norm-clipping value is a mitigation, not a detector, but the pre-clip norm history is what the on-call reads when a spike happens.

**Response SLO.** *Alert within one step of spike detection. Do not automatically restart — pause the training loop; on-call decides within N minutes whether to (a) continue and hope for recovery, (b) restart from the last checkpoint before the spike, or (c) restart from an earlier checkpoint with a smaller learning rate.*

The N-minutes-of-on-call-decision is a program-scope commitment. The recipe author names N; the training-pipeline engineer's on-call is the one who pages within N.

**Mitigation cost.** Compute-wise: the mitigations themselves (gradient clipping, skip-batch-on-spike, LR reduction) are cheap. Wall-clock: each spike-driven restart costs one restart-latency budget *plus* the compute the program lost between the spike-precipitating step and the restore-from-checkpoint step. If checkpoints are 30 minutes apart, a spike loses on average 15 minutes of compute per incident. Add a per-week-spike-count assumption; pretraining programs on new architectures typically see 1-5 spikes per week, mature-recipe fine-tunes see 0-1.

**The subtle detail.** *Prevention beats mitigation.* Data-related spikes are often caused by rare pathological inputs — a single document that triggers numerical extremes. If a specific step's data is reproducibly the spike-cause, the recipe should include a "quarantine-and-skip" mechanism (rare batches indexed by seed can be excluded from resume). The BLOOM training chronicles document this in detail. The mod-403 chapter-5 data curation-pipeline discussion feeds this: an under-deduplicated corpus has more spike-causing outliers.

## Failure mode 6 — Dataloader stall

**Signature.** Tensor cores are idle at the *start* of each training step. Nsight timeline shows PCIe or NVMe activity at the step boundary; the forward kernel of each step is preceded by a stall. MFU drops, but the drop is uniform across all ranks (unlike a straggler) and there is no NCCL activity during the stall (unlike a partition).

**Detection cadence.** Two layers:

- **Per-step batch-ready-latency.** Time from `dataloader.__next__` request to batch tensor arriving on device. Log P50, P99. A P99 above one training-step budget is a stall-in-progress.
- **Dataloader worker health.** Number of alive workers; queue depth (batches prefetched but not yet consumed); per-worker fetch-latency histogram.

**Response SLO.** *Detect a chronic (>1000 steps) stall within one shift. Redimension the dataloader: worker count, prefetch depth, per-worker batch buffer.* This is often a specialist (training-pipeline-engineer) tune, not a program-scope restart.

For a hard failure (dataloader crash), the response is the same as a partition: kill and restart from the last checkpoint.

**Mitigation cost.** Wall-clock: chronic stalls kill MFU on the order of 5-20% until resolved; the mitigation itself is a specialist implementation call. Compute-wise: pre-tokenising the dataset upfront (spending compute once instead of per step) can eliminate the largest single cause. That pre-tokenisation compute goes on the mod-403 chapter-3 budget as a data-side line item, not a training-side one.

**The subtle detail.** *Streaming vs. cached datasets.* Programs that stream from cold object storage (S3 / GCS with no local cache) pay per-shard latency indefinitely and are stall-prone. Programs that stage the dataset to hot local NVMe pay a one-time transfer cost and then have no ongoing stall risk. The recipe should state which the program is using; if streaming, the recipe should include a warm-cache pre-run and a per-worker-buffer-depth target.

## The failure-mode budget table

The exercise-03 deliverable is a filled-in version of this table. Each row is one mode; each column is what the recipe commits to. This is the artifact the mod-403 chapter-3 budget-defense conversation reads.

| # | Mode | Detection cadence | Response SLO | Compute cost per week | Wall-clock cost per week |
|---|------|-------------------|--------------|------------------------|--------------------------|
| 1 | Silent NaN / bit-flip | Per-step loss + grad-norm finite-ness; DCGM ECC | Stop within 1 step; restart within one restart-latency | ~negligible | (events × restart-latency) |
| 2 | Straggler node | Per-rank per-step timing; DCGM temp / ECC | Evict within 200-1000 steps; replace within one restart-latency | ~negligible | (events × restart-latency) |
| 3 | Network partition | Collective timeout; external heartbeat | Detect within timeout; restart within one restart-latency | ~negligible | (events × restart-latency) |
| 4 | Checkpoint corruption | Post-save read-back; periodic load-test | Retry save; fall back on load | ~2× checkpoint IO | Rare; catch at save-time avoids the wall-clock cost |
| 5 | Loss spike / gradient explosion | Loss-curve alerting; grad-norm history | Alert within 1 step; on-call decision within N min | ~negligible | (events × (restart-latency + interval/2)) |
| 6 | Dataloader stall | Batch-ready-latency; worker health | Detect within 1000 steps; redimension (specialist) | ~negligible; pre-tokenise offloads to data side | (chronic-stall MFU-loss until resolved) |

The rightmost two columns are what folds into the mod-403 cluster-hour budget as the risk multiplier. A recipe that says "the failure-mode budget adds 8% wall-clock and 3% compute overhead" and shows the arithmetic beats a recipe that quotes a 20% risk multiplier without breakdown.

## Setting the mitigation cadence — the two-question framework

For each of the six modes, the recipe author answers two questions:

1. **What is the cost per undetected event?** *Silent NaN* undetected for 10 000 steps can invalidate the run — cost is unbounded. *Straggler* undetected for 10 000 steps loses ~10% of that period's MFU — cost is bounded. *Dataloader stall* undetected for 10 000 steps loses ~10% MFU — bounded.
2. **What is the cost per detection cycle?** *Per-step loss check* is a sub-millisecond all-reduce — nearly free. *Per-step per-rank timing telemetry* is a moderate log-volume cost — cheap. *Periodic checkpoint load-test* is accelerator-hours on a scratch slice — non-trivial.

Where cost-per-event is unbounded (silent corruption, checkpoint corruption), detection cadence should be as tight as physics allows (per-step, per-save). Where cost-per-event is bounded and detection is non-cheap (load-tests, deep telemetry sweeps), the cadence should be tuned so `cost_per_cycle × cadence` roughly matches the *deferred* cost of a slower cadence.

This is the same Young/Daly logic chapter 6 uses for checkpoint frequency, applied to detection frequency. A recipe that treats the two consistently is a recipe that composes well.

## Mode weights differ between pretraining and large-scale fine-tune

The chapter opened with the six modes as a comprehensive taxonomy. The *weights* — how much of the failure budget each consumes — differ across program types:

- **Pretraining from scratch.** Higher weight on mode 5 (loss spikes on unfamiliar loss landscape), mode 1 (numerical corruption on longer runs), mode 2 (straggler evictions on longer runs). Lower relative weight on mode 6 (dataloader tune is usually inherited from a reference recipe).
- **Continued pretraining / large-scale fine-tune.** Lower weight on mode 5 (starting from a stable point). Higher relative weight on mode 4 (checkpoint compatibility with base-model formats is a common corruption source) and mode 6 (custom data mixture often introduces new stall modes).
- **Instruction fine-tune / RLHF.** Lower weight on mode 5. Higher weight on mode 6 (reward model or preference-data streams are additional stall surfaces).

The recipe should state which regime the program is in and adjust the per-mode budget accordingly. A pretraining program that reuses a fine-tune-shaped failure-mode budget will under-budget for spikes; a fine-tune program that reuses a pretraining-shaped budget will overspend on unlikely mitigations.

## Where the recipe author stops and the specialist track begins

The Staff engineer owns the failure-mode *budget* — the table above, with the numbers filled in and defensible against the mod-403 chapter-3 risk multiplier. The training-pipeline engineer owns the *implementation* of each mitigation: the NaN detector code, the straggler eviction integration with the scheduler, the NCCL watchdog wiring, the checkpoint verification pipeline, the loss-spike alerting hookup, the dataloader worker configuration. The chapter-7 hand-off contract makes this explicit and provides the acceptance criteria the training-pipeline engineer's implementation is measured against.

If the recipe author finds themselves writing the NCCL watchdog code, either (a) they are pair-programming with the training-pipeline engineer for context-transfer purposes — legitimate and time-boxed — or (b) the hand-off has broken and the recipe author is doing implementation work the specialist should do. Case (b) shows up in retros as "the training-pipeline engineer felt second-guessed" — same drift as chapter 1 named for the MFU planning number.

## Summary

- **Six named failure modes** — silent NaN / bit-flip, straggler, network partition, checkpoint corruption, loss spike / gradient explosion, dataloader stall — comprise the failure-budget taxonomy every Staff-scope recipe should have a row for.
- Each mode gets a **signature, detection cadence, response SLO, and mitigation cost**. The recipe commits to numbers; the specialist track implements them.
- **Detection cadence** is set by two questions: cost per undetected event (bounded or unbounded) and cost per detection cycle. Unbounded-cost modes get per-step detection; bounded-cost modes get cadence-matched-to-savings.
- **Silent corruption** is the highest-priority mode because its cost per undetected event is unbounded (an invalidated run). Cheap per-step finite-ness checks are non-negotiable.
- **Stragglers, partitions, and stalls** are wall-clock-cost modes; the mitigation is instrumentation-and-eviction, and the cost rolls into the mod-403 risk multiplier.
- **Loss spikes and gradient explosions** are program-shape-dependent: pretraining programs on new architectures see more, mature fine-tunes see fewer. Mitigation is on-call judgment, not automation.
- The **failure-mode budget table** is the exercise-03 deliverable and the artifact that feeds back into mod-403 chapter 3's risk multiplier. A budget without arithmetic is a wish; a budget with per-mode compute and wall-clock line-items is a plan.

The next chapter walks the reserve-and-checkpoint strategy — the cluster-hour side of the failure budget, plus the Young/Daly-style derivation of checkpoint frequency and the warm-standby-node count that mode 2's eviction response depends on.
