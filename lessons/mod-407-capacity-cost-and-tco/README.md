# mod-407-capacity-cost-and-tco: GPU Capacity, Cost, and TCO — Portfolio Management for an ML Org

**Estimated effort:** 12 hours

The finance-partner-facing centre of gravity for the Staff Machine Learning Engineer track. Names what "GPU capacity, cost, and TCO at portfolio scope" is as scope of work, hands the learner the demand-forecast method that separates baseline, program-driven, and speculative demand and quotes them at three confidence tiers, walks the five-bucket capacity portfolio (reserved / committed-use / on-demand / spot / self-hosted, plus multi-cloud as an orthogonal risk overlay) with sizing rules of thumb, teaches the quarterly cost / throughput / utilisation review that surfaces the four workload-migration types, walks the FinOps Foundation *Inform / Optimise / Operate* operating model adapted to an ML org's specific patterns (retraining as a workload class, token-based pricing, KV-cache economics, hardware-generation transitions), and closes on the CFO-readable TCO model whose one-page executive summary is the artifact leadership actually reads.

Builds on [`mod-403-foundation-model-training-programs`](../mod-403-foundation-model-training-programs/) — specifically the single-program cluster-hour budget defence from [`mod-403 chapter 3`](../mod-403-foundation-model-training-programs/03-cluster-hour-budget-defense.md), which the portfolio TCO model aggregates over — and on [`mod-402-multi-team-ml-architecture`](../mod-402-multi-team-ml-architecture/) for the portfolio-scope vocabulary. Feeds forward into [`mod-408-portfolio-reliability-and-incident-command`](../mod-408-portfolio-reliability-and-incident-command/) (the reliability-and-SLO side of the same portfolio), [`mod-410-staff-plus-technical-leadership`](../mod-410-staff-plus-technical-leadership/) (the exec-framing lens for the TCO one-pager), and the capstone [`project-401-multi-team-ml-blueprint`](../../projects/project-401-multi-team-ml-blueprint/).

## Learning objectives

- Build a portfolio-level demand forecast across training and inference workloads; separate baseline, program-driven, and speculative demand and articulate each.
- Choose the capacity portfolio: reserved vs. on-demand vs. spot vs. multi-cloud vs. self-hosted; use MLPerf-grade throughput data to size the fleet.
- Run the cost / throughput / utilisation review across teams; identify the workloads that should move between managed API and self-hosted and defend the migration.
- Apply the FinOps operating-model discipline (Inform / Optimise / Operate) to an ML org; set the review cadence and the escalation gates.
- Author the TCO model a CFO can read — training amortisation, inference marginal cost, per-request cost target, sensitivity to model-size and hardware-generation shifts.

## Lecture chapters

1. [`01-what-portfolio-capacity-and-tco-means.md`](01-what-portfolio-capacity-and-tco-means.md) — What "GPU capacity, cost, and TCO at portfolio scope" is as scope of work. How it differs from a single-training-program cluster-hour defence (mod-403 chapter 3) and from the peer FinOps / platform teams. The Staff engineer as the ML-side counterpart to the finance partner. The "am I doing portfolio TCO work?" heuristic.
2. [`02-portfolio-demand-forecast.md`](02-portfolio-demand-forecast.md) — Building the portfolio-level demand forecast across training and inference. Separating baseline (steady-state), program-driven (bought against a specific brief), and speculative (uncertain payoff) demand. One-year detail and three-year envelope. Three confidence tiers (P50 / P75 / P90). Pairs with exercise-01.
3. [`03-capacity-portfolio-reserved-ondemand-spot-multicloud-selfhosted.md`](03-capacity-portfolio-reserved-ondemand-spot-multicloud-selfhosted.md) — The five capacity buckets — reserved / committed-use / on-demand / spot / self-hosted — with multi-cloud as an orthogonal risk overlay. Trade-offs and sizing rules of thumb; MLPerf as a size input; vendor lock-in and hardware-generation-transition risk priced explicitly. Pairs with exercise-02.
4. [`04-utilisation-review-and-workload-migration.md`](04-utilisation-review-and-workload-migration.md) — The quarterly cost / throughput / utilisation review. The four workload-migration types (managed-API ↔ self-hosted, GPU-generation, cross-cloud, consolidation). The migration doc's section shape and the review's escalation gates. Pairs with exercise-03.
5. [`05-finops-discipline-for-an-ml-org.md`](05-finops-discipline-for-an-ml-org.md) — Applying the FinOps Foundation *Inform / Optimise / Operate* operating model to an ML org specifically. Review cadence, chargeback / showback dynamics, escalation gates, and the three adaptations general FinOps needs for ML (unit-economic vocabulary, retraining as a workload class, hardware-generation transitions priced in).
6. [`06-cfo-readable-tco-model.md`](06-cfo-readable-tco-model.md) — The TCO model a CFO can actually read. Training amortisation over model-family lifetime, inference marginal cost, per-request cost target, sensitivity to model-size and hardware-generation shifts, the one-page executive summary. Pairs with exercise-04.

## Exercises

- [`exercises/exercise-01-portfolio-demand-forecast.md`](exercises/exercise-01-portfolio-demand-forecast.md) — Portfolio demand forecast across training and inference, three-tier confidence, one-year detail plus three-year envelope. 3 hours.
- [`exercises/exercise-02-capacity-portfolio-selection.md`](exercises/exercise-02-capacity-portfolio-selection.md) — Capacity-portfolio selection against the exercise-01 forecast, with vendor-lock-in and hardware-generation-transition analysis. 3 hours.
- [`exercises/exercise-03-utilisation-review-and-migration-doc.md`](exercises/exercise-03-utilisation-review-and-migration-doc.md) — Cost / throughput / utilisation review and one full migration doc against a specific candidate. 3 hours.
- [`exercises/exercise-04-cfo-readable-tco-model.md`](exercises/exercise-04-cfo-readable-tco-model.md) — CFO-readable TCO model (workbook, companion doc, one-page executive summary) rolling up the prior three exercises. 3 hours.

## Structure

```
mod-407-capacity-cost-and-tco/
├── 01-…md … 06-…md    lecture chapters
├── exercises/          per-exercise prompts (solutions live in the paired solutions repo)
├── labs/               long-form hands-on labs (planned)
├── quizzes/            knowledge checks (planned)
├── resources.md        curated external references
└── README.md           this file
```

## Suggested sequencing

Read chapters 1 and 2 in one sitting — chapter 2's demand forecast is the input to every other chapter and locks in a shared vocabulary. Take exercise-01 immediately after chapter 2; do not wait to read chapters 3-6 first, because the forecast skill decays with delay and the downstream exercises reuse the same portfolio. Chapters 3-4 are the "buying and moving" pair and can be read together; take exercise-02 after chapter 3 and exercise-03 after chapter 4, each against the same portfolio the exercise-01 forecast used. Chapter 5 is a shorter operational chapter that reads well on its own and does not have a paired exercise. Chapter 6 is the roll-up; take exercise-04 after it and use the exercise-01 through exercise-03 outputs as inputs — do not re-derive.

The four exercises assemble into a single portfolio-grade artifact suitable for [`project-401`](../../projects/project-401-multi-team-ml-blueprint/) as the TCO half of its deliverable (the portfolio-blueprint half comes from mod-402).

## What comes next

- **[`mod-408-portfolio-reliability-and-incident-command`](../mod-408-portfolio-reliability-and-incident-command/)** — the reliability side of the same portfolio. Uses the same review-and-escalation discipline this module grounds, applied to SLIs / SLOs and incident command instead of dollars.
- **[`mod-410-staff-plus-technical-leadership`](../mod-410-staff-plus-technical-leadership/)** — the exec-audience framing lens; where the one-page executive summary from chapter 6 is workshopped as a first-class staff-plus communication artifact.
- **[`project-401-multi-team-ml-blueprint`](../../projects/project-401-multi-team-ml-blueprint/)** — the capstone that combines this module's TCO model with mod-402's portfolio blueprint into one portfolio-grade deliverable.

## Related tracks

- [`senior-ml-engineer-learning`](https://github.com/ml-engineering-curriculum/senior-ml-engineer-learning) (L30) — the prerequisite single-team tech-lead track; see [`PREREQUISITES.md`](../../PREREQUISITES.md).
- [`ai-infra-mlops-learning`](https://github.com/ai-infra-curriculum/ai-infra-mlops-learning) — the peer platform track for MLOps tooling; owns the retraining / registry substrate that chapter 5's Inform phase depends on.
- [`ai-infra-performance-learning`](https://github.com/ai-infra-curriculum/ai-infra-performance-learning) — the peer platform track for inference-throughput and MFU depth; owns the fixes chapter 4's utilisation review commissions.
- [`ai-infra-ml-platform-learning`](https://github.com/ai-infra-curriculum/ai-infra-ml-platform-learning) — the peer platform track for training-cluster and inference-gateway build; owns the paved-road implementations the portfolio TCO model prices.
- [`ai-infra-team-lead-learning`](https://github.com/ai-infra-curriculum/ai-infra-team-lead-learning) — the peer L40 EM track; chapter 4's escalation gates and chapter 5's cross-team review body run against the mod-401 chapter 5 partnership contract with this track.

<!-- needs-research: if a peer FinOps-for-ML learning track exists in the curriculum ecosystem (level 30 or 40), cross-link it here. Current search shows no dedicated FinOps track — the FinOps function is typically embedded in ai-infra-mlops-learning or in the org's finance function rather than as a separate learning track. -->
