# Staff Machine Learning Engineer Curriculum

**Role level:** 40 (staff-plus IC altitude, ML engineering ladder)
**Status:** planned — modules and projects below are the planned scope authored from [`.aicg/curriculum-plan.json`](.aicg/curriculum-plan.json). Lessons and projects will be drafted by subsequent autonomous content cycles.

## Overview

This track lifts a senior tech-lead Machine Learning Engineer to staff-plus IC altitude. It does **not** re-teach the single-team tech-lead scope that the level-30 [`senior-ml-engineer-learning`](https://github.com/ml-engineering-curriculum/senior-ml-engineer-learning) track owns, nor the build-altitude workflow the level-20 [`ml-engineer-learning`](https://github.com/ml-engineering-curriculum/ml-engineer-learning) track owns beneath that. Instead it adds the multi-team architecture, foundation-model / large-scale training program leadership, distributed-training strategy, ML platform strategy at org scope, cross-team eval and experimentation programs, GPU capacity / TCO framing, portfolio-scope reliability with multi-team incident command, portfolio-scope responsible-AI, and staff-plus technical leadership scope that separates a level-40 staff IC from a level-30 senior tech-lead — and from the peer level-40 engineering-manager path owned by [`ai-infra-team-lead-learning`](https://github.com/ai-infra-curriculum/ai-infra-team-lead-learning).

Total planned commitment: **126 hours** across 10 modules + **105 hours** across 3 projects = **~231 hours**.

## Ownership rule

Following the project-wide ownership rule, this curriculum:

- **Inherits** from `senior-ml-engineer-learning` (level 30) and `ml-engineer-learning` (level 20) — single-team tech-lead scope and build-altitude fundamentals are prerequisites, not modules.
- **Owns** multi-team ML architecture, foundation-model and large-scale training program leadership, distributed-training strategy at program scope, ML platform strategy at org scope, cross-team eval and experimentation programs, GPU capacity and TCO framing, portfolio-scope reliability with multi-team incident command, portfolio-scope responsible-AI, and staff-plus technical leadership.
- **Defers up** to `principal-ml-engineer-learning` (level 50) for org- and industry-wide technical strategy and keynote-grade external influence.
- **Defers sideways to the peer manager path** — direct people management is owned by [`ai-infra-team-lead-learning`](https://github.com/ai-infra-curriculum/ai-infra-team-lead-learning). This track is the IC-staff counterpart. Mod-410 covers the staff-plus-and-EM partnership; it does not teach direct people management.
- **Defers sideways to peer specialist tracks** for individual-contributor depth in their domain — `training-pipeline-engineer`, `fine-tuning-engineer`, `model-evaluation-engineer`, `ai-eval-engineer`, `nlp-engineer`, `rag-engineer`, `llm-application-developer`, `applied-ai-engineer`.
- **Defers sideways to peer platform tracks** — `ai-infra-ml-platform-learning`, `ai-infra-mlops-learning`, `ai-infra-performance-learning` own the paved-road build; this track owns the org-scope consumer contract.
- **Partners with** `ai-infra-security-learning` (level 35) for ML/AI security depth and with `ai-governance-analyst` / `head-of-ai-governance` for governance depth. Mod-409 authors the ML-side portfolio-scope risk register; the analysts author the policy.

See [`JOB_REQUIREMENTS.md`](JOB_REQUIREMENTS.md) for the requirements-to-coverage map and the cited public references the catalogue is grounded in.

## Module Plan

| Module | Title | Hours | Status |
|---|---|---|---|
| mod-401-staff-ml-role-scope | The Staff ML Engineer Role: Scope, Archetypes, and Multi-Team Contracts | 10 | planned |
| mod-402-multi-team-ml-architecture | Multi-Team ML Systems Architecture — Portfolios, Shared Substrates, Consolidation | 14 | planned |
| mod-403-foundation-model-training-programs | Foundation-Model & Large-Scale Fine-Tune Training Programs | 16 | planned |
| mod-404-distributed-training-systems-at-scale | Distributed-Training Strategy at Scale — Parallelism, MFU, Failure Budgets | 14 | planned |
| mod-405-ml-platform-strategy | ML Platform Strategy at Org Scope — Build vs. Adopt, Contribute-Back, Consumer Contract | 12 | planned |
| mod-406-cross-team-eval-and-experimentation | Cross-Team Evaluation & Experimentation Programs | 12 | planned |
| mod-407-capacity-cost-and-tco | GPU Capacity, Cost, and TCO — Portfolio Management for an ML Org | 12 | planned |
| mod-408-portfolio-reliability-and-incident-command | Portfolio-Scope ML Reliability, SLOs, and Multi-Team Incident Command | 12 | planned |
| mod-409-responsible-ai-at-portfolio-scope | Responsible AI at Portfolio Scope — Risk Register, Policy, Pre-Launch Gate | 10 | planned |
| mod-410-staff-plus-technical-leadership | Staff-Plus Technical Leadership — Roadmaps, Exec Comms, Hiring Calibration, Coaching Seniors | 14 | planned |

## Project Plan

| Project | Title | Hours | Status |
|---|---|---|---|
| project-401-multi-team-ml-blueprint | Multi-Team ML Blueprint and TCO Model | 35 | planned |
| project-402-foundation-model-training-program | Foundation-Model or Large-Scale Fine-Tune Training Program End-to-End | 40 | planned |
| project-403-staff-plus-tech-leadership-simulation | Staff-Plus Technical Leadership Simulation | 30 | planned |

## Module summaries

### mod-401 — Staff ML Engineer Role Scope
The staff-plus IC archetypes (Tech Lead, Architect, Solver, Right Hand), the staff-defining verbs ("architect across teams", "set multi-quarter direction", "calibrate senior engineers", "sponsor the RFC", "partner with executives"), the hand-off contracts to peer specialist and peer platform tracks, and the partnership contract with the peer level-40 engineering-manager path.

### mod-402 — Multi-Team ML Architecture
Blueprint 3–5 production ML systems as one portfolio: shared data planes, shared feature layer, shared model-registry contract, shared eval contract. Diagnose portfolio-scope pipeline-jungle, duplicated-feature drift, and model-registry fragmentation. Author multi-system RFCs that defend merge/split/share decisions and their migration cost.

### mod-403 — Foundation-Model Training Programs
Scope pretraining and large-scale fine-tune programs: compute-optimal parameter/data trade-offs (Chinchilla-style), scaling-law-driven roadmap, data mixture and curation, ablation sequencing, capability vs. safety post-training investment, and executive framing of the program's cost and risk.

### mod-404 — Distributed-Training Strategy at Scale
Pick the parallelism recipe (DDP, FSDP/ZeRO, tensor-parallel, pipeline-parallel, 3D-parallel) for a given (model, cluster, dataset) shape; estimate MFU; enumerate failure modes and set the mitigation cadence; author the cluster-hour reserve strategy; set the hand-off contract to the training-pipeline / performance tracks.

### mod-405 — ML Platform Strategy
Org-scope build-vs-adopt calls on feature store, model registry, eval platform, training platform, and inference gateway. Negotiate the paved-road roadmap with the peer platform teams. Author the multi-team gap RFC and the contribute-back RFC that carry the ownership and API-stability contract.

### mod-406 — Cross-Team Evaluation & Experimentation
Standardise the offline eval harness across ML teams. Set the LLM-as-judge org policy. Set the shadow / canary / progressive-rollout contract and the experiment-review body that arbitrates cross-team interference. Set the delegation contract to the eval-platform specialist tracks.

### mod-407 — Capacity, Cost, and TCO
Portfolio-level demand forecast across training and inference; capacity portfolio (reserved / on-demand / spot / multi-cloud / self-hosted); FinOps operating discipline; TCO model a CFO can read.

### mod-408 — Portfolio Reliability & Incident Command
Standardise ML-specific SLI/SLO vocabulary across a portfolio. Author the ML-incident severity matrix. Run multi-team incident command using the Google SRE structure adapted to ML failure classes. Author the org-standard postmortem template.

### mod-409 — Responsible AI at Portfolio Scope
Portfolio-scope AI risk register (NIST AI RMF + Gen-AI profile). EU AI Act classification of each system. Org-standard model card and datasheet-for-datasets format. Pre-launch review gate and escalation path to head-of-ai-governance. Delegation contract to `ai-infra-security-learning` for deep red-team operations.

### mod-410 — Staff-Plus Technical Leadership
Multi-quarter ML program roadmap that leadership commits to. Executive-audience one-pagers and RFCs. Staff-plus hiring loops (calibrate senior engineers, decide the bar, coach hiring committees). Coach senior tech leads (L30) and partner with the EM peer. Vendor and partnership judgement calls. Sustain the technical bar without sliding into IC-only or management-only work.

## Assessment

Each module ships **1 quiz** plus the exercises and (where present) a lab listed in [`.aicg/curriculum-plan.json`](.aicg/curriculum-plan.json). Each project ships a portfolio-grade README and an explicit assessment rubric covering the staff signals — multi-team blueprint defence, program leadership artifact, platform-consumer-contract clarity, TCO defensibility, exec-audience RFC quality, hiring calibration artifact, and roadmap defensibility.

## Where to go after this curriculum

- **`principal-ml-engineer-learning`** — next-up role on the ML engineering ladder; org- and industry-wide technical strategy.
- **`ai-infra-team-lead-learning`** — sideways move to the level-40 engineering-manager path if the learner prefers the people-leadership branch.
- **`senior-agentic-ai-engineer-learning`** — peer level-40 track for staff ICs whose portfolio centres on agentic systems.
- **`ai-infra-senior-architect-learning`** — peer level-40 track for staff ICs whose portfolio centres on infrastructure architecture.
- **`ai-infra-principal-engineer-learning`** / **`ai-infra-principal-architect-learning`** — level-50 infrastructure paths for staff ICs who cross over into the platform side.

<!-- needs-research: backfill industry-frequency evidence into JOB_REQUIREMENTS.md once the autonomous research loop runs with WebSearch / WebFetch permissions; demote any module or exercise whose underlying requirement does not show up in ≥3 in-window Staff ML Engineer postings. -->
