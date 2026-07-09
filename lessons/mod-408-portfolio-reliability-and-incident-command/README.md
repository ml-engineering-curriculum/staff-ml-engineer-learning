# mod-408-portfolio-reliability-and-incident-command: Portfolio-Scope ML Reliability, SLOs, and Multi-Team Incident Command

**Estimated effort:** 12 hours

The runtime-and-reliability analog to mod-402: the module where the Staff ML engineer standardises how three-to-five ML systems in the portfolio detect, alert on, command through, and learn from ML-specific failures. Names why the classical SRE vocabulary — SLI, SLO, error budget, page-vs-ticket — doesn't natively cover the four ML failure classes (bad predictions, silent drift, feature-pipeline breakage, retraining-loop stall), authors the portfolio-scope SLI/SLO catalogue every team's model ships against, authors the severity matrix that includes the silent-failure column classical SRE lacks, adapts Google SRE's Incident Command System to multi-team ML incidents, authors the org-standard postmortem template with a portfolio-lessons section that stops the same failure recurring on a different team's system, and closes on the retraining-as-deploy contract the platform team enforces on the shared model registry.

Builds on [`mod-401-staff-ml-role-scope`](../mod-401-staff-ml-role-scope/) — specifically the L30 → L40 altitude vocabulary and the peer-EM partnership contract from chapter 5. Builds on [`mod-402-multi-team-ml-architecture`](../mod-402-multi-team-ml-architecture/) — the portfolio substrate the SLIs and severity apply to, and specifically the model-registry substrate the retraining-as-deploy contract in chapter 6 sits on. Sits alongside [`mod-406-cross-team-eval-and-experimentation`](../mod-406-cross-team-eval-and-experimentation/) — mod-406 owns the progressive-rollout contract across shared traffic; this module owns the per-system canary and the SLI catalogue the rollout reads against. Feeds into [`mod-409-responsible-ai-at-portfolio-scope`](../mod-409-responsible-ai-at-portfolio-scope/) — the pre-launch gate in mod-409 reads the reliability contract this module authors — and the capstone [`project-403-staff-plus-tech-leadership-simulation`](../../projects/project-403-staff-plus-tech-leadership-simulation/).

## Learning objectives

- Standardise ML-specific SLI/SLO vocabulary across a portfolio — prediction quality drift, retraining SLA, feature-freshness, calibration decay — and set the required SLIs every team's model must ship with.
- Author the severity matrix for ML incidents at portfolio scope; distinguish silent-failure incidents (quality drift no one paged for) from user-visible incidents.
- Run multi-team incident command using Google SRE incident-management structure — Incident Commander, Ops Lead, Comms Lead — adapted to ML failure classes (bad predictions, silent drift, feature-pipeline breakage, retraining-loop stall).
- Author the postmortem template the ML org adopts, including a portfolio-lessons section that the next incident should not repeat.
- Set the retraining-as-deploy hygiene the platform enforces — canary path, rollback, offline replay — as a contract every team's model registry entry must satisfy.

## Lecture chapters

1. [`01-what-portfolio-reliability-means.md`](01-what-portfolio-reliability-means.md) — Portfolio-scope ML reliability as scope of work. How it differs from single-service SLO (single-team L30) and from the peer SRE / platform team. The four ML failure classes (bad predictions, silent drift, feature-pipeline breakage, retraining-loop stall) and why classical SRE vocabulary doesn't natively cover them. The "am I doing portfolio-reliability work?" heuristic.
2. [`02-ml-specific-sli-slo-catalogue.md`](02-ml-specific-sli-slo-catalogue.md) — The five SLI families the Staff engineer standardises across the portfolio: prediction-quality drift, retraining SLA, feature freshness, calibration/quality proxy, judge-model agreement. SLO-vs-SLA vocabulary from Google SRE Book specialised for ML. Error-budget policy per family. The three failure modes when setting an SLO (aspirational, unmeasurable, single-team-shaped). Pairs with exercise-01.
3. [`03-incident-severity-matrix-for-ml.md`](03-incident-severity-matrix-for-ml.md) — Authoring the severity matrix. Two shapes of ML incident (user-visible, silent-failure). Sev-0 through Sev-3 tiers. The page-vs-ticket-vs-log line. The exec-notification threshold. The silent-failure column classical SRE matrices lack. Pairs with exercise-02.
4. [`04-multi-team-incident-command.md`](04-multi-team-incident-command.md) — Running multi-team incident command using Google SRE's Incident Commander / Ops Lead / Comms Lead structure, itself borrowed from wildland-fire ICS. Role assignment by ML failure class. Keeping a five-hour incident coherent. When to escalate, when to hand off, when the two rotations (platform SRE and ML) merge or run parallel. Six pathologies to prevent. Pairs with exercise-03.
5. [`05-postmortem-and-portfolio-lessons.md`](05-postmortem-and-portfolio-lessons.md) — The org-standard postmortem template. Blameless framing (Allspaw + SRE Book) with the two ML-specific blame pitfalls (blaming the model, blaming the data). The portfolio-lessons section that prevents the same failure recurring on a different team's system. Seven ML-specific action-item categories. The two-week review meeting. Pairs with exercise-04.
6. [`06-retraining-as-deploy-hygiene.md`](06-retraining-as-deploy-hygiene.md) — The retraining-as-deploy contract: canary path, rollback trigger, offline replay. What the model-registry entry every team ships must record. The exemption pattern for models that legitimately cannot carry the full contract. Ties back to mod-402's registry substrate and mod-406's progressive-rollout contract.

## Exercises

- [`exercises/exercise-01-ml-sli-slo-standard-catalogue.md`](exercises/exercise-01-ml-sli-slo-standard-catalogue.md) — Author the portfolio SLI/SLO catalogue over the three-to-five systems the learner carried from mod-402. 3 hours.
- [`exercises/exercise-02-ml-incident-severity-matrix.md`](exercises/exercise-02-ml-incident-severity-matrix.md) — Author the ML-specific severity matrix layered on top of the org's SRE matrix, with the silent-failure column. 3 hours.
- [`exercises/exercise-03-multi-team-incident-command-drill.md`](exercises/exercise-03-multi-team-incident-command-drill.md) — Run a tabletop drill of a multi-team ML incident using the ICS-borrowed command structure. 3 hours.
- [`exercises/exercise-04-portfolio-postmortem-template.md`](exercises/exercise-04-portfolio-postmortem-template.md) — Author the org-standard postmortem template with the portfolio-lessons section, seven action-item categories, and the two-week review-meeting agenda. 3 hours.

## Structure

```
mod-408-portfolio-reliability-and-incident-command/
├── 01-…md … 06-…md    lecture chapters
├── exercises/          per-exercise prompts (solutions live in the paired solutions repo)
├── labs/               long-form hands-on labs (planned)
├── quizzes/            knowledge checks (planned)
├── resources.md        curated external references
└── README.md           this file
```

## Suggested sequencing

Read chapters 1-3 in order; they define the vocabulary (failure classes, SLI catalogue, severity matrix) that the rest of the module hangs off. Take **exercise-01** immediately after chapter 2 so the catalogue is written while the SLI vocabulary is fresh — the exercise input reuses the portfolio the learner drew in mod-402's exercise-01. Take **exercise-02** immediately after chapter 3 for the same reason; the severity matrix reads directly off the chapter-2 catalogue.

Chapters 4 and 5 are the runtime-and-postmortem block; read them together, with **exercise-03** after chapter 4 and **exercise-04** after chapter 5. The chapter-4 tabletop drill is worth doing with at least one other learner or a peer if possible — the coordination failures the drill catches are not visible when the same person plays every role.

Chapter 6 is a short close; read it after finishing the four exercises. Its content ties back to mod-402's registry substrate and mod-406's rollout contract, and its exemption pattern is what the learner will be asked to defend in project-403's simulation.

The four exercises assemble into the reliability-and-incident-command deliverable in [`project-403-staff-plus-tech-leadership-simulation`](../../projects/project-403-staff-plus-tech-leadership-simulation/). Learners planning to take project-403 should keep the portfolio choice consistent with their mod-402 portfolio.

## What comes next

- **[`mod-409-responsible-ai-at-portfolio-scope`](../mod-409-responsible-ai-at-portfolio-scope/)** — the pre-launch review gate mod-409 authors reads the reliability contract this module produces; the two modules compose into a portfolio-scope launch-readiness bar.
- **[`mod-410-staff-plus-technical-leadership`](../mod-410-staff-plus-technical-leadership/)** — the exec-comms and roadmap chapters use the reliability catalogue and the postmortem review as inputs; a portfolio without a reliability standard has no defensible roadmap.
- **[`project-403-staff-plus-tech-leadership-simulation`](../../projects/project-403-staff-plus-tech-leadership-simulation/)** — the capstone simulation includes the SLI catalogue, severity matrix, and postmortem template from this module.

## Related tracks

- [`senior-ml-engineer-learning`](https://github.com/ml-engineering-curriculum/senior-ml-engineer-learning) (L30) — the prerequisite single-team tech-lead track that owns runbook authoring, per-team monitoring, and single-system on-call craft; see [`PREREQUISITES.md`](../../PREREQUISITES.md).
- [`ai-infra-mlops-learning`](https://github.com/ai-infra-curriculum/ai-infra-mlops-learning) — the peer platform track that owns the *build side* of ML reliability tooling: drift-detection substrate, retraining orchestrator, model-registry implementation, canary rollout mechanics. This module owns the portfolio-standard contract those tools enforce.
- [`ai-infra-ml-platform-learning`](https://github.com/ai-infra-curriculum/ai-infra-ml-platform-learning) — the peer platform track for feature-store and serving-substrate build; chapter 2's feature-freshness SLI and chapter 6's canary path both live on their substrate.
- [`ai-infra-performance-learning`](https://github.com/ai-infra-curriculum/ai-infra-performance-learning) — the peer platform track for inference-substrate performance; latency SLIs the classical SRE matrix owns live here.
- [`ai-infra-team-lead-learning`](https://github.com/ai-infra-curriculum/ai-infra-team-lead-learning) — the peer L40 EM track; chapters 4-5's Incident Commander role assignments and the peer EM as Comms Lead sit inside the partnership contract from mod-401 chapter 5.
