# mod-404-distributed-training-systems-at-scale: Distributed-Training Strategy at Scale — Parallelism, MFU, Failure Budgets

**Estimated effort:** 14 hours

The Staff Machine Learning Engineer's third archetype-centre after multi-team architecture and training-program scoping: **the parallelism recipe and its operational plan**. Names what a distributed-training strategy is above single-run training-loop code, walks the six parallelism primitives (DDP, FSDP/ZeRO, TP, PP, SP, EP) as the recipe vocabulary, walks a decision tree that turns a (model, cluster, dataset) triple into a specific `(TP, PP, DP, SP, EP, precision, recompute, batch, seq)` tuple, plans the MFU with a bottleneck and re-plan trigger, budgets the six failure modes any long-running large-cluster run will meet, sets the reserve-and-checkpoint strategy that makes the wall-clock forecast robust to hardware failure, and finishes on the hand-off contract to the training-pipeline and infra-performance specialist tracks.

Builds on [`mod-401-staff-ml-role-scope`](../mod-401-staff-ml-role-scope/) (staff-plus altitude, the hand-off-contract shape) and [`mod-403-foundation-model-training-programs`](../mod-403-foundation-model-training-programs/) (the program brief this module receives — the scaling-law bet, the cluster-hour budget with risk multiplier, the data plan, the ablation cadence). Feeds forward into [`mod-407-capacity-cost-and-tco`](../mod-407-capacity-cost-and-tco/) (the org-scope capacity portfolio the reserve strategy sits inside) and [`mod-408-portfolio-reliability-and-incident-command`](../mod-408-portfolio-reliability-and-incident-command/) (the portfolio-scope incident-command framework the failure-mode SLOs feed).

## Learning objectives

- Choose the parallelism recipe (DDP, FSDP/ZeRO, tensor-parallel, pipeline-parallel, sequence-parallel, 3D-parallel) for a given (model, cluster, dataset) shape and defend the choice.
- Estimate throughput and Model FLOPs Utilisation for a proposed configuration; identify the bottleneck (compute, memory, network, IO).
- Enumerate the failure modes a program-owner budgets for — silent NaN, straggler nodes, network partition, checkpoint corruption, gradient explosion, dataloader stalls — and set the mitigation cadence.
- Author the cluster-hour reserve strategy: burst vs. reserved capacity, checkpoint frequency vs. throughput, restart-latency budget.
- Set the hand-off contract to `training-pipeline-engineer` and `ai-infra-performance-learning` for implementation and kernel-level tuning.

## Lecture chapters

1. [`01-scope-and-hand-off.md`](01-scope-and-hand-off.md) — What "distributed-training strategy" means at Staff scope. The program → recipe → implementation stack. What the Staff engineer owns vs. what belongs to the training-pipeline and infra-performance specialist tracks. The five artifacts this module produces. The "am I doing recipe-scope work?" heuristic.
2. [`02-parallelism-primitives.md`](02-parallelism-primitives.md) — The six primitives as a vocabulary the recipe pulls from: DDP, FSDP / ZeRO, tensor-parallel, pipeline-parallel, sequence-parallel, expert-parallel. Cost and benefit of each expressed as a communication-memory-compute trade-off. The recipe as a tuple `(TP, PP, DP, SP, EP, precision, recompute, batch, seq)`.
3. [`03-choosing-the-parallelism-recipe.md`](03-choosing-the-parallelism-recipe.md) — The decision tree that turns a (model, cluster, dataset) triple into a recipe. TP-inside-NVLink and PP-across-InfiniBand as the load-bearing mesh-nesting rule. Four worked recipes (7B fine-tune, 70B pretrain, MoE, frontier-scale). The recipe-decision doc's section shape and the re-plan trigger.
4. [`04-mfu-throughput-and-bottleneck-analysis.md`](04-mfu-throughput-and-bottleneck-analysis.md) — The roofline model, one page. MFU vs. HFU defined precisely. Published MFU bands per recipe class. The step-time decomposition. Four candidate bottlenecks — compute, HBM-bandwidth, interconnect, IO — with signature diagnostics. The instrumentation plan. Wall-clock forecast from MFU plan.
5. [`05-failure-modes-and-mitigation-cadence.md`](05-failure-modes-and-mitigation-cadence.md) — The six named failure modes with signature, detection cadence, response SLO, and mitigation cost each. The failure-mode budget table. Cost-per-undetected-event vs. cost-per-detection-cycle framework. How pretraining and fine-tune programs weigh the modes differently.
6. [`06-cluster-reserve-and-checkpoint-strategy.md`](06-cluster-reserve-and-checkpoint-strategy.md) — Reserved / burst / spot posture split. Warm-standby pool sizing via MTBF and a 3-sigma calculation. The Young/Daly checkpoint-interval derivation. Restart-latency budget with component decomposition. Storage substrate acceptance criteria. Cluster-hour rollup back into mod-403's budget.
7. [`07-hand-off-contract-to-training-pipeline-and-performance.md`](07-hand-off-contract-to-training-pipeline-and-performance.md) — The two hand-off contracts: to `training-pipeline-engineer` (implementation, on-call) and to `ai-infra-performance-learning` (kernel-level tuning). Deliverables, acceptance criteria, review cadence, hand-back triggers. Three anti-patterns to catch. The one-page recipe summary for the program brief's technical appendix.

## Exercises

- [`exercises/exercise-01-parallelism-recipe-decision-doc.md`](exercises/exercise-01-parallelism-recipe-decision-doc.md) — Author the parallelism-recipe decision doc for a concrete (model, cluster) point. 3 hours.
- [`exercises/exercise-02-mfu-and-bottleneck-analysis.md`](exercises/exercise-02-mfu-and-bottleneck-analysis.md) — Turn the recipe into an MFU planning number with a bottleneck and a wall-clock forecast. 3 hours.
- [`exercises/exercise-03-failure-mode-budget-and-mitigation-plan.md`](exercises/exercise-03-failure-mode-budget-and-mitigation-plan.md) — Fill in the six-row failure-mode budget with per-mode detection cadence, response SLO, and cost. 3 hours.
- [`exercises/exercise-04-cluster-reserve-and-checkpoint-strategy.md`](exercises/exercise-04-cluster-reserve-and-checkpoint-strategy.md) — Set the reserved / burst / spot split, warm-standby sizing, Daly-derived checkpoint interval, and restart-latency budget. 3 hours.
- [`exercises/exercise-05-training-pipeline-handoff-contract.md`](exercises/exercise-05-training-pipeline-handoff-contract.md) — Write the two-page hand-off contract to training-pipeline-engineer and ai-infra-performance-learning. 2 hours.

## Structure

```
mod-404-distributed-training-systems-at-scale/
├── 01-…md … 07-…md    lecture chapters
├── exercises/          per-exercise prompts (solutions live in the paired solutions repo)
├── labs/               long-form hands-on labs (planned)
├── quizzes/            knowledge checks (planned)
├── resources.md        curated external references
└── README.md           this file
```

## Suggested sequencing

Read chapters 1-2 as one block: they define the artifact and load the vocabulary. Chapter 3 (the decision tree) is the substantive centre — read it slowly and with a specific program brief in mind. Chapter 4 (MFU) is where the recipe becomes a wall-clock forecast; do not skim it, because the bottleneck-identification framework is what makes the recipe defensible against measurement disagreement. Chapters 5 and 6 are the operational shoulders — failure-mode budgeting and the reserve-and-checkpoint strategy — and they compose into the cluster-hour rollup that feeds back to mod-403. Chapter 7 is the hand-off; read it with the mod-401 hand-off-contract chapter open in a second tab.

The five exercises assemble into a single recipe-plus-operations packet for one (model, cluster, dataset) point. Keep the program brief constant across the five — do not swap it between exercises. Exercise-01 immediately after chapter 3; exercise-02 after chapter 4; exercise-03 after chapter 5; exercise-04 after chapter 6; exercise-05 after chapter 7. Learners taking [`project-403-distributed-training-run-plan`](../../projects/project-403-distributed-training-run-plan/) (if scaffolded in the projects layer) roll all five into the capstone deliverable.

## What comes next

- **[`mod-405-ml-platform-strategy`](../mod-405-ml-platform-strategy/)** — zoom out from a single program to the portfolio of programs the org runs and the shared training-platform investments (feature-store, model-registry, run-tracking) that make each program's recipe faster to author.
- **[`mod-406-cross-team-eval-and-experimentation`](../mod-406-cross-team-eval-and-experimentation/)** — the cross-team eval-contract depth that chapter 4's instrumentation-plan hand-off feeds.
- **[`mod-407-capacity-cost-and-tco`](../mod-407-capacity-cost-and-tco/)** — the org-scope capacity portfolio chapter 6's reserved / burst / spot posture sits inside.
- **[`mod-408-portfolio-reliability-and-incident-command`](../mod-408-portfolio-reliability-and-incident-command/)** — the portfolio-scope incident-command framework chapter 5's failure-mode SLOs and chapter 7's on-call rotation feed.

## Related tracks

- [`senior-ml-engineer-learning`](https://github.com/ml-engineering-curriculum/senior-ml-engineer-learning) (L30) — the prerequisite tech-lead track; single-node and small-cluster training-pipeline scope. See [`PREREQUISITES.md`](../../PREREQUISITES.md).
- `training-pipeline-engineer` — the peer specialist track that owns the *implementation* of the training loop this module scopes the recipe around; contract A of chapter 7 is the hand-off boundary.
- [`ai-infra-performance-learning`](https://github.com/ai-infra-curriculum/ai-infra-performance-learning) — the peer platform track that owns kernel-level tuning and profile-guided optimisation; contract B of chapter 7 is the hand-off boundary and the source of the measured-MFU baseline chapter 4's plan quotes.
- `ai-infra-ml-platform` / `ai-infra-mlops` — the peer platform tracks that own the training cluster itself, the reservation-and-scheduling API, and the checkpoint storage substrate chapter 6 states acceptance criteria against.
