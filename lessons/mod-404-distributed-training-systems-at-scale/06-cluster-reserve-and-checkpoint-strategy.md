# Cluster Reserve and Checkpoint Strategy

## Motivation

Chapter 5 named the failure modes and set the detection cadence. Two of those modes — straggler eviction and network partition — resolve to *replace-a-node* actions. Two others — silent NaN and loss spike — resolve to *restart-from-checkpoint* actions. Both classes of action require the recipe to have pre-committed to two things chapter 5 pointed forward at: a **warm-standby pool** to draw replacement nodes from, and a **checkpoint interval** that bounds how much wall-clock a restart costs.

This chapter is where those commitments get their numbers. It answers three questions the recipe author has to answer before signing the plan:

1. **How is the cluster reserved?** Reserved vs. burst vs. spot, and how the mix supports the failure-mode budget.
2. **How often does the program checkpoint?** The Young/Daly-style optimum, restated for LLM-training shape.
3. **How long does a restart take, worst case?** The restart-latency budget that the failure-mode wall-clock arithmetic in chapter 5 multiplies against.

The trap to avoid: treating reserve strategy and checkpoint policy as separate. They are two knobs on the same dial. A recipe that reserves aggressively but checkpoints infrequently pays for standby capacity that the restart-latency budget wastes; a recipe that checkpoints every 5 minutes but has no warm standby has to wait an hour for a replacement node before the checkpoint even helps. The two knobs get turned together, and this chapter walks the joint optimisation.

## The three capacity postures

Every cluster provisioning model for a training program is a mix of three postures. Chapter 6 recommends stating the mix as an explicit percentage split in the recipe doc.

### Reserved capacity

**What it is.** Accelerators dedicated to the program for the whole main-run window. Reserved via a formal allocation from the platform team (peer track ai-infra-ml-platform) or via a cloud-provider commitment (AWS Capacity Reservations, GCP TPU reservations, Azure Reserved VM Instances, Oracle bare-metal reservations).

**What it buys.** Predictable placement, topology guarantees (the nodes are wired into the same InfiniBand island, so PP boundary-activation traffic and DP AllGather stay inside the fabric the recipe planned for), no preemption. This is the posture the main run's core cluster occupies.

**What it costs.** The full accelerator-hour price for the full window, whether the program uses every hour or not. Idle time is paid for.

**When it is right.** Every main run of every serious pretraining or large-scale fine-tune program. If the accelerators are not reserved, the recipe cannot commit to a wall-clock number.

### Burst capacity

**What it is.** Additional accelerators available on-demand from the same platform or provider, above the reserved base. Priced at on-demand rates, no long-term commitment.

**What it buys.** Elasticity for the ablation phase (chapter 6 of mod-403 walks the ablation cadence), for parallel eval runs, for one-off compute (data curation, tokenisation, checkpoint format conversion). Burst capacity is what absorbs the *variance* around the main run.

**What it costs.** On-demand rates are typically 1.5-3× the reserved rate for the same hardware. Availability is not guaranteed at burst call-time — a busy provider region can refuse the allocation.

**When it is right.** Every program's ablation and pre-run compute belongs on burst, not on the reserved main-run pool. Also right for the checkpoint verification load-test slice (chapter 5 mode 4) — a small dedicated slice for a few hours a week.

### Spot / preemptible capacity

**What it is.** Extremely-discounted accelerator-hours that can be reclaimed by the provider with short notice (typically 30 seconds to a few minutes). Sold at 30-70% off on-demand rates. All major clouds sell some form of preemptible instance (AWS Spot, GCP Spot / Preemptible VMs, Azure Spot VMs).

**What it buys.** Cheap compute for genuinely-interruptible workloads: data curation, tokenisation, offline eval, hyperparameter sweeps that can accept individual-run losses without ruining the sweep.

**What it costs.** Preemption. If a spot node dies mid-run, the work on that node is lost. For a synchronous distributed training step, losing any single node is catastrophic — the whole step must restart.

**When it is right.** *Almost never for the main run.* Spot is a data-and-eval-side tool. The recipe should explicitly state that the main run does not sit on spot capacity, because that is the assumption most reviewers will want stated out loud.

There is one exception: a checkpoint-and-restart-friendly training loop with a very short checkpoint interval (minutes) on a *single-node* fine-tune could tolerate spot. Any multi-node synchronous training program cannot. A hybrid arrangement — reserved for the main-run core, spot for auxiliary compute — is standard.

### The mix that most recipes converge on

For a typical Staff-scope pretraining or large-scale fine-tune program on 128-1024 accelerators:

- **~85-90% reserved.** The main-run core plus the warm-standby pool.
- **~10-15% burst.** Ablation, eval, offline data compute, format conversion.
- **~0-5% spot.** If used, only for data-side pre-processing.

The exact split is program-specific and the recipe should state it with the rationale. A recipe that says only "we'll reserve capacity" without saying how much and with what standby ratio has left the platform-team conversation for later.

## The warm-standby pool

Chapter 5's straggler-eviction and network-partition responses both draw from a **warm-standby pool** — accelerators reserved, powered-on, and joined to the training-job's launch scope, ready to take the place of an evicted node. Cold standby (provisioning a new node from scratch when needed) is impractical at LLM scale because the provisioning latency (minutes to tens of minutes on cloud, sometimes an hour on bare metal) is far larger than the restart-latency budget.

**How much standby?** Two heuristics, both worth citing in the recipe doc:

- **The ByteDance / MegaScale heuristic.** MegaScale (Jiang et al. 2024) reports operating at roughly 5% standby (e.g., ~500 warm-standby GPUs on a 10 000-GPU cluster). This absorbs the observed rate of node failures on their fleet with margin.
- **The N-nines calculation.** If per-node MTBF is `M` hours and the cluster has `K` reserved-plus-standby nodes, the probability of running the full main run of duration `T` hours without exhausting standby of size `S` follows a Poisson calculation on the failure arrival rate `K/M`. For `M = 10 000 hours`, `K = 512`, `T = 168 hours`, expected failures = `168 × 512 / 10 000 ≈ 8.6`. A standby of 32 (~6%) gives a 3-sigma buffer.

For most Staff-scope programs on H100-class hardware, **5-10% standby** is the defensible number. Below 3% and the recipe has under-reserved for the observed failure rate; above 15% and the recipe is paying for idle capacity that would be better spent on the main run.

The recipe doc should state:

- The reserved main-run node count.
- The warm-standby node count as an absolute number and a percentage.
- The per-node MTBF assumption the number is anchored on (cite: the vendor's published MTBF, or a specific program's logbook).
- The 3-sigma calculation (or equivalent).

## Checkpoint frequency — the Young/Daly derivation

Checkpoint frequency is a canonical optimisation. Checkpoint too frequently and the IO cost eats MFU; checkpoint too rarely and each failure costs more work-in-progress. The optimum has been derived independently multiple times; the Young 1974 and Daly 2006 papers (both from the HPC community) give the closed form most recipes cite.

The setup:

- **T_c** = wall-clock cost of a single checkpoint (dump time, including verification of chapter 5 mode 4).
- **T_r** = expected wall-clock cost of a restart-from-checkpoint (chapter 5 restart-latency budget).
- **M** = mean time between failures (MTBF) that trigger a restart. Compose the modes that force a full-cluster restart from chapter 5 (modes 1, 3, 5, and some 4 events) — typically 6-48 hours for a well-instrumented program on healthy hardware, shorter for programs with elevated failure rates.

The Daly first-order optimum for checkpoint interval `τ` is:

> **τ\* = √(2 · T_c · M)**

Interpretation: the optimum trades one unit of expected checkpoint IO cost against one unit of expected re-work cost. For `T_c = 60 seconds` and `M = 24 hours = 86400 seconds`:

- `τ* = sqrt(2 · 60 · 86400) ≈ 3220 seconds ≈ 54 minutes`.

Real programs tend to round to a convenient number (30 minutes, 1 hour, 2 hours) rather than picking the exact optimum.

For `T_c = 300 seconds` (a slower, verified checkpoint) and `M = 24 hours`:

- `τ* = sqrt(2 · 300 · 86400) ≈ 7200 seconds = 2 hours`.

The recipe should state:

- The `T_c` assumption with a reference (specialist-track measurement from a prior run, or a rough estimate for a first-time program that gets refined during the ablation phase).
- The `M` assumption pulled from the failure-mode budget in chapter 5.
- The computed `τ*` and the rounded interval the recipe commits to.
- The **checkpoint retention policy**: how many old checkpoints are kept (chapter 5 mode 4 recommends at least 3 — the last known-good plus two prior — because a corruption caught late needs a rollback further back than one step).

**When the Daly formula does not apply.** If `T_c` is very small compared to `M` (async checkpointing with well-hidden IO), the recipe can safely go tighter than Daly-optimum because the IO cost has been amortised off the critical path. If `T_r` dominates the failure budget (a warm-restart from checkpoint takes an hour because model shard reshuffling is slow), the practical checkpoint interval should be tuned against a *joint* Young/Daly-style expression including `T_r`. See the Daly 2006 paper for the second-order form.

## Restart-latency budget

The wall-clock cost of getting the training loop back into productive computation after a failure. Chapter 5's per-mode "restart within one restart-latency budget" SLO all reference this number.

Components:

1. **Detection latency.** From failure occurrence to the on-call / automation deciding to restart. For per-step-detected modes (silent NaN, straggler), this is one step (~1 second). For collective-timeout modes (network partition), this is one timeout window (~30-120 seconds). For loss-spike modes with on-call decision, this is the N-minute human-decision window (~5-30 minutes).
2. **Job teardown.** Killing the current training-job pods across the cluster (scheduler-dependent; ~10-60 seconds on Kubernetes-shape schedulers, longer on batch schedulers with pod-eviction delays).
3. **Job reboot.** Launching a fresh set of pods on the cluster (~30-180 seconds, dominated by container-image pull if the image is not already cached and by pod-scheduling latency on the platform).
4. **Framework initialisation.** Process-group setup, NCCL handshake, tensor-parallel mesh construction (~30-120 seconds at 512+ ranks; NCCL cold-init is a load-bearing latency source).
5. **Checkpoint load.** Reading the checkpoint from object storage or shared file system, sharding into the recipe's mesh, resuming optimizer state (~30 seconds to 20 minutes, dominated by object-store throughput and shard-count).
6. **Warmup steps.** Depending on the framework, the first few steps after resume may run at reduced throughput while caches warm and JIT-compiled kernels are recompiled.

A well-instrumented program on modern H100 clusters can compress the total to 15-30 minutes. A poorly-instrumented program on a busy scheduler can take 1-3 hours per restart. Published programs (LLaMA-3, MegaScale) cite numbers in the low-tens of minutes.

The recipe doc should state:

- The **target restart-latency budget** as a single number (the SLO the training-pipeline engineer commits to).
- The **component decomposition** as a first cut (each of the six above with an estimate).
- The **worst-case budget** for use in the chapter 5 wall-clock arithmetic — typically ~2× the target, to absorb variance.

**Why the component decomposition matters.** If measurement during the ablation phase shows restart-latency at 45 minutes but the recipe planned 20, the decomposition tells you which component blew the budget. Object-store checkpoint-load latency is a platform-team conversation; NCCL-init latency is a specialist-track tuning; container-image pull is a build-side fix. The decomposition tells the on-call *who to talk to*.

## Composing reserve, checkpoint, and restart-latency

The three numbers compose into the failure-mode wall-clock accounting from chapter 5. Concrete example, chapter 5's straggler mode:

- Per-week straggler-eviction event count: `168 × K / MTBF_straggler`. For `K = 512`, `MTBF_straggler = 24 hours`, this is `168 × 512 / 24 ≈ 3584` node-events per week — but not all become evictions. Assume 1 in 100 (only chronic slowdowns evict, not transient blips): ~36 evictions per week.
- Restart-latency per eviction (if the training loop restarts from checkpoint rather than elastic-substituting the node): 20 minutes.
- Wall-clock cost per week: `36 × 20 min = 12 hours per week` — which on a 168-hour week is *7% of wall-clock*, unacceptable for a program that budgeted 15% total risk.

The two levers to bring this down:

- **Elastic node substitution** (framework-supported hot-swap, if the training-pipeline engineer can implement it): reduces per-event restart to ~1 minute. Weekly cost: 36 × 1 min = 36 minutes ≈ 0.4%. Big win.
- **More aggressive straggler filter** (evict on the tail of P99 with wider margin): reduces event count by ~2×. Weekly cost: 18 × 20 min = 6 hours ≈ 3.5%. Modest win.

The recipe author picks the lever, cites the cost, and commits the number. This is the joint optimisation between checkpoint policy, restart-latency, and reserve size that the chapter opened by naming.

Same shape of calculation for the other modes; the exercise-03 deliverable folds all six into the risk-multiplier line item.

## The checkpoint storage substrate

The recipe does not own the object-store implementation, but does own the *acceptance criteria* the object store has to satisfy. The chapter 7 hand-off contract to peer platform tracks (ai-infra-ml-platform / ai-infra-mlops) is where these land; the recipe pre-commits them so the platform team has a target to design against.

Load-bearing acceptance criteria:

- **Aggregate write bandwidth.** `checkpoint_size / T_c_target`. For a 70B model with FP32 optimizer state (~980 GB shard-summed), a 60-second `T_c` target requires ~16 GB/s aggregate write. Modern object stores (GCS, S3, Ceph) hit this with parallel shard writes; smaller in-house stores may not.
- **Aggregate read bandwidth.** Same shape, for the restart-latency budget. Object-store read throughput is what dominates step 5 of the restart-latency decomposition.
- **Durability.** 11 nines (99.999999999%) is the object-store standard. Do not accept less; a lost checkpoint is a re-run.
- **Retention.** At least 3 recent checkpoints + one weekly-snapshot; long-term retention of the final checkpoint is a compliance-and-safety consideration (mod-409 discusses).
- **Atomicity.** A partial checkpoint should not be visible until all shards land. Modern object-store multi-part uploads with a manifest-write-last convention is the standard pattern.

The recipe committing to these bandwidth numbers upfront lets the platform team choose the storage backend (GCS bucket, S3 Express One Zone, Weka, VAST, DDN, in-house Ceph) with the recipe's requirements as input rather than a post-hoc renegotiation.

## The reserve-and-checkpoint doc

Section shape of the cluster-reserve-and-checkpoint-strategy doc (exercise-04):

1. **The capacity split.** Reserved / burst / spot as percentages of total planned accelerator-hours, with the rationale for each.
2. **The main-run node count and the warm-standby pool.** Absolute numbers, percentage, MTBF assumption, 3-sigma calculation.
3. **The checkpoint interval.** Daly derivation with the `T_c` and `M` inputs and the rounded committed interval. The retention policy.
4. **The restart-latency budget.** Target number, component decomposition, worst-case budget.
5. **The storage acceptance criteria.** Bandwidth (write and read), durability, retention, atomicity.
6. **The composed failure-mode wall-clock rollup.** Per-mode events per week × restart-latency, summed. Reconciled with the mod-403 risk multiplier.
7. **The re-plan trigger.** At what measured deviation from any of the above numbers does the recipe re-open?

Two-to-four pages. Exercise-03's failure-mode table is a peer-doc — exercise-04 references its per-mode event rates rather than re-deriving them.

## The re-plan trigger, this chapter's version

Same shape as chapters 3 and 4. The chapter-6 numbers that trigger re-plan:

- **Measured checkpoint IO latency > 1.5× the `T_c` used in the Daly derivation.** Storage substrate did not meet its bandwidth target; either accept a lower checkpoint frequency and higher per-failure re-work, or escalate to platform.
- **Measured restart-latency > 1.5× the target.** Decompose using the component list; escalate the responsible sub-component to its owner.
- **Observed failure rate > 1.5× the MTBF assumption for warm-standby sizing.** Increase standby, and (upstream) revisit whether the hardware batch is on a bad build.

Numeric triggers, in the doc, at authoring time. Without them the recipe is not re-plannable when measurement lands.

## The cluster-hour rollup — what mod-403 chapter 3 gets back

Chapter 3 of mod-403 defended a total cluster-hour budget with a risk multiplier. This chapter's outputs revise that number. The rollup:

> **total_cluster_hours = main_run_hours + ablation_hours + eval_hours + (main_run_hours × failure_mode_wall_clock_fraction) + (main_run_hours × standby_fraction)**

Where:

- **failure_mode_wall_clock_fraction** = sum of the per-mode weekly wall-clock costs / 168, from this chapter and chapter 5.
- **standby_fraction** = warm-standby-pool-size / main-run-pool-size.

Concrete: 100 000 accelerator-hour main-run + 20 000 hours ablation + 5 000 hours eval + 10% failure fraction × 100 000 + 6% standby × 100 000 = **141 000 accelerator-hours**. Which is what mod-403 chapter 3's risk-multiplier defense should have anticipated. If it did not, the recipe re-opens the budget doc as chapter 1 of this module set up.

## Summary

- **Capacity is a mix of reserved / burst / spot.** Main runs sit on reserved (85-90%); ablation and eval sit on burst (10-15%); spot is a data-side tool, not a training-side one.
- The **warm-standby pool** absorbs straggler and partition evictions. **5-10% of the main-run node count** is the defensible band, anchored on MTBF and a 3-sigma-style calculation.
- The **Daly / Young checkpoint interval** — `τ* = √(2 · T_c · M)` — is the closed-form optimum. Round to a convenient interval; keep at least 3 recent checkpoints for corruption rollback.
- The **restart-latency budget** decomposes into detection, teardown, reboot, framework init, checkpoint load, and warmup. State the target, the decomposition, and a worst-case (~2× target) for use in the chapter 5 arithmetic.
- The **storage substrate** has to meet write and read bandwidth targets derived from `T_c` and the restart-latency target — the recipe states them, the platform team designs against them.
- **Reserve, checkpoint interval, and restart-latency are one joint optimisation.** A tight interval without warm standby is wasted; warm standby without a tight interval is over-reserved. The exercise-04 doc names both together.
- The chapter's outputs **roll into the mod-403 chapter-3 cluster-hour budget** as the risk-multiplier line-item — with per-mode arithmetic replacing the mod-403 rule-of-thumb multiplier.
- Numeric re-plan triggers — measured checkpoint IO, restart-latency, failure rate — belong in the doc at authoring time.

The next chapter walks the hand-off contract to the training-pipeline-engineer and ai-infra-performance-learning peer tracks — what deliverables cross the boundary, what acceptance criteria the recipe author signs against, and what triggers a hand-back.
