# mod-409-responsible-ai-at-portfolio-scope: Responsible AI at Portfolio Scope — Risk Register, Policy, Pre-Launch Gate

**Estimated effort:** 10 hours

The Right Hand archetype's centre of gravity in the Staff Machine Learning Engineer track. Names what "responsible AI at portfolio scope" is as scope of work — distinct from single-model risk assessment (an L30 artifact) and from the governance analyst / head-of-ai-governance / security-partner roles — and hands the learner the five composable artifacts that make an ML org's responsible-AI posture legible to a governance analyst, a regulator, or an incoming head of AI without a per-team walk-through: the portfolio risk register aligned to NIST AI RMF 1.0 (Govern / Map / Measure / Manage) and the Gen-AI Profile overlay; the EU AI Act engineering classification table with the tier-driven obligation surface; the org-standard model card and datasheet templates grounded in Mitchell et al. 2019 and Gebru et al. 2021; the pre-launch review gate charter with signoff matrix and escalation path; and the delegation contract to the peer security track for the deep adversarial-eval work.

Builds on [`mod-401-staff-ml-role-scope`](../mod-401-staff-ml-role-scope/) — specifically the Right Hand archetype, the L30 → L40 altitude vocabulary, and the hand-off contracts to peer partner tracks. Composes with [`mod-402-multi-team-ml-architecture`](../mod-402-multi-team-ml-architecture/) (the portfolio blueprint whose systems become register rows) and with [`mod-408-portfolio-reliability-and-incident-command`](../mod-408-portfolio-reliability-and-incident-command/) (the incident-severity vocabulary the register's Manage column reuses). Feeds forward into [`mod-410-staff-plus-technical-leadership`](../mod-410-staff-plus-technical-leadership/) (the executive-comms craft the gate charter's political defence draws on), [`project-402-foundation-model-training-program`](../../projects/project-402-foundation-model-training-program/) (which integrates a register entry into the training-program deliverable), and [`project-403-staff-plus-tech-leadership-simulation`](../../projects/project-403-staff-plus-tech-leadership-simulation/) (which integrates the gate charter into a leadership simulation).

## Learning objectives

- Author a portfolio-scope AI risk register aligned to NIST AI RMF (Govern / Map / Measure / Manage) and the Gen-AI Profile overlay.
- Classify each system in the portfolio against the EU AI Act risk tiers and identify obligations that apply per system (transparency, human oversight, logging, conformity assessment).
- Standardise the model-card and datasheet-for-datasets format across the org so a governance analyst can review any team's launch packet without adaptation.
- Set the pre-launch review gate the ML org uses — required artifacts, required signoffs (product, legal, security, governance), and the escalation path to head-of-ai-governance for policy conflicts.
- Set the red-teaming / adversarial-eval cadence and the delegation contract to `ai-infra-security-learning` for deep security operations.

## Lecture chapters

1. [`01-what-portfolio-responsible-ai-scope-means.md`](01-what-portfolio-responsible-ai-scope-means.md) — What "responsible AI at portfolio scope" is as scope of work. Distinct from single-model risk assessment and from the governance-analyst / head-of-ai-governance / security-partner roles. The Right Hand archetype's centre of gravity. The "am I doing portfolio RAI work?" heuristic. The five artifacts this module produces.
2. [`02-portfolio-ai-risk-register-with-nist-rmf.md`](02-portfolio-ai-risk-register-with-nist-rmf.md) — The portfolio AI risk register: one row per production system, columns aligned to NIST AI RMF 1.0 (Govern / Map / Measure / Manage) and the Gen-AI Profile (NIST AI 600-1) overlay. What Staff owns vs. what the analyst owns. What a healthy row looks like. Cadence and drift.
3. [`03-eu-ai-act-portfolio-classification.md`](03-eu-ai-act-portfolio-classification.md) — Classifying each system against the four EU AI Act tiers (unacceptable / high-risk / limited-risk / minimal-risk). The obligation surface for a high-risk system. Extraterritorial reach for non-EU-headquartered orgs. Staff owns the engineering read; legal owns the binding classification. Composition with the register.
4. [`04-org-standard-model-card-and-datasheet.md`](04-org-standard-model-card-and-datasheet.md) — The org-standard model card (Mitchell et al. 2019) with the three portfolio-specific extensions; the org-standard datasheet for datasets (Gebru et al. 2021) with licensing and PII sections. Staff owns the templates; the model author owns the instances; the analyst reviews in one shape across teams. The over-specification temptation. Cross-linking as a first-class artifact.
5. [`05-pre-launch-review-gate-and-escalation.md`](05-pre-launch-review-gate-and-escalation.md) — The pre-launch review gate charter: four always-required artifacts, two conditional, the signoff matrix by tier, block-vs-flag rule, escalation to the head of AI governance. Staff owns the charter and operational running; the analyst owns the policy the gate enforces. The gate as a career signal.
6. [`06-red-teaming-and-security-delegation.md`](06-red-teaming-and-security-delegation.md) — Red-teaming and adversarial-eval cadence at portfolio scope, proportional to tier. The artifact schema the gate consumes. OWASP LLM Top 10 and NIST AI 100-2 as shared vocabulary. The delegation contract to `ai-infra-security-learning`. What the Staff engineer keeps rather than delegating.

## Exercises

- [`exercises/exercise-01-portfolio-ai-risk-register-with-nist-rmf.md`](exercises/exercise-01-portfolio-ai-risk-register-with-nist-rmf.md) — Draft a portfolio-scope AI risk register with one row per production system, aligned to NIST AI RMF and the Gen-AI Profile overlay. 3 hours.
- [`exercises/exercise-02-eu-ai-act-portfolio-classification.md`](exercises/exercise-02-eu-ai-act-portfolio-classification.md) — Classify each system in the portfolio against the four EU AI Act tiers and document the obligation surface and legal-review escalation for the high-risk rows. 2 hours.
- [`exercises/exercise-03-org-standard-model-card-and-datasheet.md`](exercises/exercise-03-org-standard-model-card-and-datasheet.md) — Adapt the Mitchell et al. and Gebru et al. shapes into two-page org-standard templates and apply them to one system in the portfolio. 3 hours.
- [`exercises/exercise-04-pre-launch-review-gate-charter.md`](exercises/exercise-04-pre-launch-review-gate-charter.md) — Author the pre-launch review gate charter for the portfolio: required artifacts, signoff matrix, block-vs-flag rule, escalation path. 2 hours.

## Structure

```
mod-409-responsible-ai-at-portfolio-scope/
├── 01-…md … 06-…md    lecture chapters
├── exercises/          per-exercise prompts (solutions live in the paired solutions repo)
├── labs/               long-form hands-on labs (planned)
├── quizzes/            knowledge checks (planned)
├── resources.md        curated external references
└── README.md           this file
```

## Suggested sequencing

Read all six chapters in order. Chapters 1-3 establish scope and the two per-system artifacts (register row, classification-table row); they can be read in one sitting. Chapters 4-6 shift to the launch-packet artifacts and the delegation contract; they reward a break in between. Take exercise-01 after chapter 2, using the same three-to-five-system portfolio you carried through mod-402 if possible — the mod-402 blueprint's per-system paragraph flows straight into the register's Map column. Take exercise-02 immediately after exercise-01 on the same portfolio; the two artifacts are locked at the row level. Exercise-03 lands after chapter 4 on one system in that portfolio — a high-risk row from exercise-02 gives the model card the most demanding constraints. Exercise-04 lands after chapter 5 and consumes all three prior exercises' artifacts as the packet the gate is defined against.

The four exercises assemble into deliverables for `project-402-foundation-model-training-program` (the trained model's register row and gate packet) and `project-403-staff-plus-tech-leadership-simulation` (the gate charter and its executive framing). Learners planning either capstone should keep their exercise portfolio choice consistent with their capstone scope.

## What comes next

- **[`mod-410-staff-plus-technical-leadership`](../mod-410-staff-plus-technical-leadership/)** — the executive-comms and roadmap craft the gate charter's political defence draws on; where the promotion-packet framing for the artifacts in this module gets its polish.
- **[`project-402-foundation-model-training-program`](../../projects/project-402-foundation-model-training-program/)** — the training-program capstone that integrates this module's register into a foundation-model or large-scale fine-tune deliverable.
- **[`project-403-staff-plus-tech-leadership-simulation`](../../projects/project-403-staff-plus-tech-leadership-simulation/)** — the leadership-simulation capstone that integrates the gate charter into a multi-team, multi-role decision-making artifact.

## Related tracks

- [`ai-infra-security-learning`](https://github.com/ai-infra-curriculum/ai-infra-security-learning) (L35) — the peer security track that owns deep red-team, adversarial-ML, model-supply-chain, and prompt-injection work. Chapter 6's delegation contract is the interface.
- [`ai-governance-analyst`](https://github.com/ai-infra-curriculum/ai-governance-analyst) — the peer governance track that owns the org's responsible-AI *policy*. This module owns the ML-side implementation of that policy; the analyst reviews the register and signs off at the gate.
- [`head-of-ai-governance`](https://github.com/ai-infra-curriculum/head-of-ai-governance) — the governance-leadership track. Chapter 5's escalation path routes policy conflicts here; chapter 6's delegation contract for high-risk systems is co-signed here.
- [`senior-ml-engineer-learning`](https://github.com/ml-engineering-curriculum/senior-ml-engineer-learning) (L30) — the prerequisite single-team tech-lead track that owns single-model risk assessment; see [`PREREQUISITES.md`](../../PREREQUISITES.md).
- [`ai-infra-team-lead-learning`](https://github.com/ai-infra-curriculum/ai-infra-team-lead-learning) — the peer L40 EM track; the gate's product-owner signoff and the delegation contract's headcount negotiation happen in partnership with the EM peer per the mod-401 chapter 5 contract.
