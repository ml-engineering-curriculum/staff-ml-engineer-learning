# What "Distributed-Training Strategy" Means at Staff Scope

## Motivation

The learner arrives at this module from [`mod-403-foundation-model-training-programs`](../mod-403-foundation-model-training-programs/) holding a program brief. The brief names an (N, D, C) target, a cluster-hour budget with a risk multiplier, a data plan, and an ablation cadence. What the brief *does not* say is *how* the accelerators are wired into a training job that turns 6ND FLOPs of theoretical work into a converged checkpoint at the promised wall-clock and the promised MFU. That answer — the **parallelism recipe** and everything that hangs off it — is what this module owns.

It is worth being explicit about the altitude. This is *not* the module where you learn to write a NCCL AllReduce or hand-tune a fused-attention kernel; that belongs to the peer specialist tracks [`training-pipeline-engineer`](https://github.com/ml-engineering-curriculum/training-pipeline-engineer) and [`ai-infra-performance-learning`](https://github.com/ai-infra-curriculum/ai-infra-performance-learning). This *is* the module where you decide what recipe those specialists are going to implement, defend the throughput number leadership signed off on, budget the failure modes you know will occur at 512+ accelerators for weeks, choose the reserve strategy that makes the wall-clock forecast survive contact with hardware, and write the hand-off contract that turns the recipe into a specialist's build order rather than a request for magic.

The five artifacts this module produces sit *between* the program brief and the code the training-pipeline engineer writes. They are the artifacts the Staff engineer signs their name to and defends in review. They are not implementation, and they are not vision — they are the operational plan that makes the program run.

## What "distributed training strategy" means here

A **distributed-training strategy** in this module means:

> A written recipe that (a) picks a parallelism configuration for a specific (model shape, cluster, dataset) point, (b) states a planned MFU with an uncertainty band and the bottleneck the plan expects to sit against, (c) enumerates the failure modes the program will spend cluster-hours mitigating and the cadence of that mitigation, (d) commits to a reserve-and-checkpoint strategy that makes the wall-clock forecast robust to hardware failure, and (e) hands the recipe to the training-pipeline and infra-performance specialist tracks under a written contract with measurable acceptance criteria.

The strategy is a document, not a codebase. If the Staff engineer's output is a training script, either they have taken on the specialist's job or the specialist is not staffed and the program has a resourcing problem the strategy did not disclose.

## The program → recipe → implementation stack

Mod-403 owns the program envelope; mod-404 owns the recipe; the peer specialist tracks own the implementation. The three layers compose and pass artifacts downward.

```
mod-403 (program)     scaling-law bet ─ cluster-hour budget ─ data plan ─ ablation cadence
        │                              │              │
        ▼                              ▼              ▼
mod-404 (recipe)      parallelism recipe ─ MFU plan ─ failure budget ─ reserve strategy ─ hand-off contract
        │                                                                                       │
        ▼                                                                                       ▼
specialist tracks     training loop ─ checkpoint mechanics ─ kernel tuning ─ operational tooling ─ on-call
```

The vertical arrows carry two things: (a) the artifact itself and (b) the *contract* — what the receiving layer is entitled to assume and what it is on the hook to deliver back. Mod-401 named these hand-off contracts abstractly; this module operationalises the two contracts most load-bearing to a training program.

**The upward contract from mod-404 to mod-403.** The recipe commits to an MFU planning number ± band, a wall-clock estimate for the main run, a failure-mode-adjusted cluster-hour total that reconciles with the budget from mod-403 chapter 3, and a set of restart-latency assumptions. If the recipe cannot hit the budget's assumed MFU, the recipe re-opens the budget doc — not the other way around.

**The downward contract from mod-404 to the specialist tracks.** The recipe commits to a specific configuration (parallelism dimensions, precision, activation-recomputation policy, communication pattern), a measurement plan (what MFU is measured on what run and how it converts to the acceptance gate), a failure-mode SLO table (silent-NaN detection latency, straggler eviction cadence, checkpoint corruption verification), and a hand-off review cadence. The specialist tracks commit back to a measured MFU with error bars, an implementation of the failure-mode budget, a benchmark of restart latency, and an escalation path for the recipe assumptions that fail under measurement.

Chapter 7 walks the hand-off contract in template detail.

## What the Staff ML engineer owns vs. what the specialist tracks own

The temptation, especially for a learner arriving from a Senior tech-lead track that owns single-node training pipelines, is to reach for the training-pipeline-engineer or ai-infra-performance vocabulary and start writing kernel-level code. That is a scope error. The Staff engineer's ownership at mod-404 is:

**Staff owns.**

- **The recipe choice.** DDP vs. FSDP-ZeRO vs. 3D-parallel vs. MoE + expert-parallel, with the model-shape and cluster-shape argument for the choice. Chapter 3 is the substantive chapter.
- **The MFU planning number.** The band, the bottleneck the plan sits against, the re-plan trigger if measurement disagrees with plan. Chapter 4.
- **The failure-mode budget.** Which of the six named modes gets what mitigation cadence, and how much compute and wall-clock the mitigation consumes. Chapter 5.
- **The reserve and checkpoint strategy.** Reserved vs. burst vs. spot, checkpoint interval, restart-latency envelope. Chapter 6.
- **The hand-off contract.** Deliverables, acceptance criteria, escalation, hand-back triggers. Chapter 7.

**Training-pipeline-engineer (peer specialist track) owns.**

- **The training loop implementation.** The actual PyTorch / JAX / Megatron / DeepSpeed configuration. The dataloader. The checkpointing code. The restart logic.
- **The parallelism library configuration.** Wiring FSDP into the training script, setting up Megatron's TP × PP mesh, tuning the pipeline schedule.
- **The observability substrate for the run itself.** Loss-and-gradient-norm telemetry, throughput dashboards, checkpoint metadata tracking.
- **Operational on-call for the main run.** The engineer who gets paged at 3 AM when the loss goes NaN or NCCL times out.

**ai-infra-performance-learning (peer platform track) owns.**

- **Kernel-level tuning.** Custom fused kernels, FlashAttention configuration, `torch.compile` / XLA optimisation, precision-specific tuning.
- **Profile-guided optimisation.** Reading Nsight / XLA / PyTorch profiler traces, identifying the specific fused-op or memory-copy pattern eating the throughput, proposing the fix.
- **Hardware-generation-specific optimisations.** When H100 → H200 → B200 requires the kernels to be re-tuned, this is their work.
- **The measured MFU number** that the Staff engineer's plan cited as a target.

**Peer platform tracks (ai-infra-ml-platform, ai-infra-mlops) own.**

- **The training cluster itself.** Physical topology, InfiniBand fabric, storage backend, scheduler.
- **The reservation-and-scheduling API.** How the training job actually gets a 512-H100 pod for a week.
- **The checkpoint storage substrate.** Object store, throughput SLO, durability guarantees.

If the Staff engineer at mod-404 finds themselves reading a Nsight trace, either (a) they are pair-programming with the ai-infra-performance engineer as part of the specialist hand-off review — legitimate but should be time-boxed — or (b) the hand-off contract has broken and the recipe is being enforced by the recipe author on the specialist's turf, which is a scope smell. Case (b) usually shows up in program retrospectives as "the training-pipeline engineer felt second-guessed."

## The five artifacts this module produces

The staff-scope deliverables of mod-404, one-to-one with the five exercises:

1. **The parallelism-recipe decision doc** (exercise-01, chapter 3). The chosen (TP, PP, DP, SP, EP) configuration for the program's (model, cluster, dataset) point, with the isoflop-style reasoning that argues no alternative recipe is materially better. Two-to-four pages.
2. **The MFU-and-bottleneck analysis** (exercise-02, chapter 4). The planned MFU with band, the bottleneck the plan sits against, the roofline sketch, the instrumentation plan for validating the number. Two-to-three pages plus one plot.
3. **The failure-mode budget and mitigation plan** (exercise-03, chapter 5). Six named modes with detection cadence, response SLO, mitigation cost, and the compute-and-wall-clock line-items rolled into the program's risk table. Three-to-five pages plus a table.
4. **The cluster-reserve-and-checkpoint strategy** (exercise-04, chapter 6). Reserved vs. burst vs. spot split, checkpoint interval with the Young/Daly-style derivation, restart-latency budget, warm-standby node count. Two-to-four pages.
5. **The hand-off contract to training-pipeline-engineer and ai-infra-performance-learning** (exercise-05, chapter 7). Deliverables, acceptance criteria, review cadence, escalation, hand-back triggers. One-to-two pages.

Add one integrating artifact — a **one-page recipe summary** for the program brief's technical appendix that leadership reads. It is not a separate exercise; it is the top of exercise-01's document with the four other exercises' headline numbers folded in.

The five artifacts feed back into mod-403's cluster-hour budget: if the recipe's MFU is lower than the budget assumed, the budget grows; if the failure-mode budget is larger than the budget's risk multiplier, the budget grows; if the reserve strategy needs warm-standby nodes, the budget grows. Chapter 3's re-plan trigger in mod-403 is what protects the program from the cascade.

## The "am I doing recipe-scope work?" heuristic

Sibling to the mod-401 and mod-403 heuristics:

> **If my document says what parallelism configuration will run on what cluster, at what MFU planning number, with what failure-mode SLOs, defended against alternative recipes and re-planned against measurement, I am doing recipe-scope work. If it says how to write the AllReduce, I am doing training-pipeline work. If it says why the program exists, I am doing program-scope work.**

The two failure modes to catch in yourself:

- **Drifting up into program-scope re-litigation.** *"The scaling law is wrong; we should go smaller."* That is a mod-403 conversation. The recipe author feeding back into the program is welcome — the *re-plan trigger* is the mechanism — but arguing the program at recipe-review time is a scope error.
- **Drifting down into training-pipeline scope.** *"The FSDP unit should be at the transformer-block granularity."* That is a legitimate opinion but not the recipe author's call unless the alternative would change the MFU planning number materially. The specialist owns the call; the Staff engineer owns the number.

## Pretraining vs. large-scale fine-tune — what's the same, what's different for this module

Same:

- The parallelism primitives (chapter 2). DDP, FSDP, TP, PP, SP, EP all apply.
- The MFU / roofline framework (chapter 4).
- The six failure modes (chapter 5), though gradient explosion is far less common in fine-tune than pretraining.
- The cluster reserve strategy (chapter 6).
- The hand-off contract (chapter 7).

Different:

- **Recipe complexity.** A 7B-continued-pretrain on 128 H100s often does not need TP or PP; FSDP alone wins. A 175B pretrain from scratch on 4096 H100s needs 3D-parallel at minimum. Chapter 3 walks the decision tree; the fine-tune column exits earlier than the pretrain column.
- **MFU expectations.** Fine-tune programs starting from a base checkpoint can often hit higher MFU faster because the recipe is inherited; pretraining programs on a new architecture pay a ramp cost. Chapter 4 quotes bands for both.
- **Failure-mode weights.** Fine-tune has lower silent-NaN risk (loss is starting near a stable point) and lower straggler cost (shorter wall-clock); pretraining has higher of both. Chapter 5 walks the weights.
- **Reserve horizon.** Fine-tune's shorter wall-clock changes the reserved-vs-burst maths; a two-week reservation is expensive relative to a two-day fine-tune, cheap relative to a two-month pretrain. Chapter 6.

Where the module treats both together, that is deliberate. Where it splits, the chapter calls it out.

## Summary

- The **distributed-training strategy** is the recipe-scope artifact that sits between mod-403's program brief and the specialist tracks' implementation. Its output is a document, not a codebase.
- The Staff ML engineer owns five things: the recipe choice, the MFU planning number, the failure-mode budget, the reserve strategy, and the hand-off contract. Kernel-level tuning, training-loop implementation, and operational on-call belong to peer specialist and platform tracks.
- The five artifacts of this module map one-to-one with its five exercises and roll up into the program brief's technical appendix. Chapters 2 through 7 build them in order.
- The "am I doing recipe-scope work?" heuristic catches drift up into program-scope re-litigation and drift down into training-pipeline implementation — the two most common altitude errors at this module.
- Pretraining and large-scale fine-tune programs use the same primitives and the same framework; the recipe complexity, MFU band, failure-mode weights, and reserve horizon differ, and the chapters call out the split.

The next chapter walks the parallelism primitives — DDP, FSDP/ZeRO, tensor-parallel, pipeline-parallel, sequence-parallel, expert-parallel — as a vocabulary the recipe will pull from.
