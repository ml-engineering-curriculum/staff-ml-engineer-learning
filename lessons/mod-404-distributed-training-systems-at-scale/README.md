# mod-404-distributed-training-systems-at-scale: Distributed-Training Strategy at Scale — Parallelism, MFU, Failure Budgets

> Scaffolded by `aicg org execute-plan`. Lecture chapters and exercise content are authored on subsequent autonomous cycles.

**Estimated effort:** 14 hours

## Learning objectives

- Choose the parallelism recipe (DDP, FSDP/ZeRO, tensor-parallel, pipeline-parallel, sequence-parallel, 3D-parallel) for a given (model, cluster, dataset) shape and defend the choice
- Estimate throughput and Model FLOPs Utilisation for a proposed configuration; identify the bottleneck (compute, memory, network, IO)
- Enumerate the failure modes a program-owner budgets for: silent NaN, straggler nodes, network partition, checkpoint corruption, gradient explosion, dataloader stalls — and set the mitigation cadence
- Author the cluster-hour reserve strategy: burst vs. reserved capacity, checkpoint frequency vs. throughput, restart-latency budget
- Set the hand-off contract to training-pipeline-engineer / ai-infra-performance-learning for implementation and kernel-level tuning

## Structure

- `01-…md` … `0N-…md`: lecture chapters.
- `exercises/`: per-exercise prompts.
- `labs/`: long-form hands-on labs.
- `quizzes/`: knowledge checks.
- `resources.md`: external references.
