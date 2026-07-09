# mod-406-cross-team-eval-and-experimentation: Cross-Team Evaluation & Experimentation Programs

**Estimated effort:** 12 hours

The systemic fix for the eval-metric-conflict failure mode named in [`mod-402/03-portfolio-failure-modes.md`](../mod-402-multi-team-ml-architecture/03-portfolio-failure-modes.md). Names what a "cross-team eval and experimentation program" is as scope of work — a document set (offline eval harness standard, LLM-as-judge policy, rollout contract, experimentation-platform consumer contract) plus a review body, not a codebase and not a platform build — and walks the four contracts plus the delegation contract to the peer specialist tracks that keeps the Staff engineer out of specialist scope. Grounded in Kohavi, Tang, and Xu (*Trustworthy Online Controlled Experiments*, Cambridge 2020) for the experimentation side and in the LLM-as-a-Judge literature (Zheng et al., NeurIPS 2023) for the generative-eval side.

Builds on [`mod-402-multi-team-ml-architecture`](../mod-402-multi-team-ml-architecture/) — specifically the eval-contract substrate column of the portfolio blueprint and the eval-metric-conflict failure mode. Feeds forward into [`mod-408-portfolio-reliability-and-incident-command`](../mod-408-portfolio-reliability-and-incident-command/) (the reliability side of the same rollout contract), [`mod-410-staff-plus-technical-leadership`](../mod-410-staff-plus-technical-leadership/) (the roadmap-and-comms lens on the program), and the capstone [`project-403-staff-plus-tech-leadership-simulation`](../../projects/project-403-staff-plus-tech-leadership-simulation/).

## Learning objectives

- Standardise the offline eval harness across multiple ML teams — pick the required slice / holdout / adversarial guardrails every team must run, and set the review-gate the eval must pass before a model deploys.
- Own the LLM-as-judge policy at org scope — where it is allowed as a primary metric, where it is allowed as a diagnostic, and how bias in the judge model is tracked.
- Set the shadow / canary / progressive-rollout contract every team's deploy adheres to, and the review body that arbitrates when two teams' experiments interfere.
- Set the experimentation-platform contract: SRM diagnostics, CUPED, sequential testing, interference / novelty effects, and the multi-team review cadence that catches trust breaches early.
- Set the delegation contract to model-evaluation-engineer / ai-eval-engineer for platform build depth.

## Lecture chapters

1. [`01-what-cross-team-eval-means.md`](01-what-cross-team-eval-means.md) — What a cross-team eval and experimentation program is as scope of work. Distinct from a single team's eval harness and from the peer specialist tracks. The eval-metric-conflict failure mode from mod-402 chapter 3 is what this module systematically fixes. The four artifacts this module produces and the "am I doing eval-program work?" heuristic.
2. [`02-org-scope-offline-eval-harness.md`](02-org-scope-offline-eval-harness.md) — The org-scope offline eval harness standard. The five required eval components (primary, slices, held-out / regression, adversarial / guardrail, drift / freshness). The metadata schema every eval run must attach. The review-gate before deploy (accept / block / defer / nudge). Where per-team flexibility is preserved. Pairs with exercise-01.
3. [`03-llm-as-judge-org-policy.md`](03-llm-as-judge-org-policy.md) — Where LLM-as-judge is allowed as a primary metric (Tier A), where only as a diagnostic (Tier B), where forbidden (Tier C). The four bias classes tracked (position, verbosity, self-enhancement, rubric-drift). Calibration cadence against human raters. Judge-model vendor-lock-in and the fallback-judge requirement. Pairs with exercise-02.
4. [`04-shadow-canary-progressive-rollout-contract.md`](04-shadow-canary-progressive-rollout-contract.md) — The rollout contract every team's deploy follows: shadow → canary → progressive-rollout. Gate criteria and rollback criteria at each stage. Pre-flight requirements (registered version, accepted eval verdict, pre-registered rollout plan, on-call, cross-team-impact declaration). Interaction with the mod-402 model-registry contract.
5. [`05-experimentation-platform-consumer-contract.md`](05-experimentation-platform-consumer-contract.md) — The seven capabilities the ML org demands the experimentation platform expose (SRM diagnostics, CUPED, sequential testing, guardrail-metric monitoring, interference detection, novelty / primacy visibility, metric registry). The cross-team experiment review body — composition, cadence, what it arbitrates, escalation path. Pairs with exercises 03 and 04.
6. [`06-delegation-to-eval-platform-specialists.md`](06-delegation-to-eval-platform-specialists.md) — The delegation contract to the peer specialist tracks (model-evaluation-engineer, ai-eval-engineer). What Staff owns vs. what specialists own. What flows in each direction. The three cadences (weekly / monthly / quarterly). The three escalation triggers. The specialists' expectations of Staff.

## Exercises

- [`exercises/exercise-01-org-scope-eval-harness-standard.md`](exercises/exercise-01-org-scope-eval-harness-standard.md) — Author the org-scope offline eval harness standard document for a real three-to-five-system portfolio. 3 hours.
- [`exercises/exercise-02-llm-as-judge-org-policy.md`](exercises/exercise-02-llm-as-judge-org-policy.md) — Author the LLM-as-judge org policy with tier classifications for the portfolio's current judge deployments. 3 hours.
- [`exercises/exercise-03-cross-team-experiment-review-charter.md`](exercises/exercise-03-cross-team-experiment-review-charter.md) — Author the cross-team experiment review body charter: composition, cadence, arbitration rules, escalation. 3 hours.
- [`exercises/exercise-04-experimentation-platform-consumer-contract.md`](exercises/exercise-04-experimentation-platform-consumer-contract.md) — Author the experimentation-platform consumer contract naming the seven required capabilities and the platform-team review cadence. 3 hours.

## Structure

```
mod-406-cross-team-eval-and-experimentation/
├── 01-…md … 06-…md    lecture chapters
├── exercises/          per-exercise prompts (solutions live in the paired solutions repo)
├── labs/               long-form hands-on labs (planned)
├── quizzes/            knowledge checks (planned)
├── resources.md        curated external references
└── README.md           this file
```

## Suggested sequencing

Read all six chapters in order. Chapters 1-3 (scope, offline eval standard, judge policy) form the "before-deploy" half and can be read in one sitting; chapters 4-6 (rollout contract, experimentation platform + review body, delegation) form the "at-and-after-deploy" half and reward a break in between. Take exercise-01 immediately after chapter 2 — the standard is easiest to author while the chapter's five-bucket framing is fresh. Take exercise-02 after chapter 3 on the same portfolio; the judge deployments the portfolio already runs are the raw material. Exercises 03 and 04 sit on top of chapter 5's material and should be authored together, because the review-body charter (exercise 03) references capabilities in the platform contract (exercise 04) and vice versa.

The four exercises assemble into the eval-program section of the mod-402 portfolio RFC and, for learners taking the capstone, into [`project-403-staff-plus-tech-leadership-simulation`](../../projects/project-403-staff-plus-tech-leadership-simulation/)'s exec-audience roll-up. Learners planning to take project-403 should keep their exercise-portfolio choice consistent with their capstone-project scope, and with their mod-402 exercise choice if they authored one.

## What comes next

- **[`mod-408-portfolio-reliability-and-incident-command`](../mod-408-portfolio-reliability-and-incident-command/)** — the reliability side of chapter 4's rollout contract; where a canary halt or guardrail breach becomes an incident under the org's SLO vocabulary. Read after mod-406.
- **[`mod-410-staff-plus-technical-leadership`](../mod-410-staff-plus-technical-leadership/)** — the roadmap-and-exec-comms lens on the eval program; how the mod-406 artifacts get communicated to leadership, defended in resourcing conversations, and used as calibration signal in hiring.
- **[`project-403-staff-plus-tech-leadership-simulation`](../../projects/project-403-staff-plus-tech-leadership-simulation/)** — the capstone that folds the mod-406 artifacts into a Staff-plus leadership simulation, including the exec-audience one-pager on the eval program.

## Related tracks

- [`model-evaluation-engineer-learning`](https://github.com/ml-engineering-curriculum/model-evaluation-engineer-learning) — the peer specialist track that owns classical-ML eval-set construction, annotator programs, metric implementation, and regression-test authoring. Chapter 6 walks the delegation contract in detail.
- [`ai-eval-engineer-learning`](https://github.com/ml-engineering-curriculum/ai-eval-engineer-learning) — the peer specialist track that owns generative-AI eval — judge prompts, preference-collection protocols, judge-bias measurement, adversarial and red-team sets. Also covered in chapter 6.
- [`ai-infra-mlops-learning`](https://github.com/ai-infra-curriculum/ai-infra-mlops-learning) — the peer platform track that owns the experimentation-platform build and the rollout-tooling substrate that chapter 4's contract runs on.
- [`ai-infra-team-lead-learning`](https://github.com/ai-infra-curriculum/ai-infra-team-lead-learning) — the peer L40 EM track; chapter 5's review body includes the peer EM per the mod-401 chapter 5 partnership contract.
- [`senior-ml-engineer-learning`](https://github.com/ml-engineering-curriculum/senior-ml-engineer-learning) (L30) — the prerequisite single-team tech-lead track; see [`PREREQUISITES.md`](../../PREREQUISITES.md).
