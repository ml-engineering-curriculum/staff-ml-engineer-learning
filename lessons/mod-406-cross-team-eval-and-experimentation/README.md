# mod-406-cross-team-eval-and-experimentation: Cross-Team Evaluation & Experimentation Programs

> Scaffolded by `aicg org execute-plan`. Lecture chapters and exercise content are authored on subsequent autonomous cycles.

**Estimated effort:** 12 hours

## Learning objectives

- Standardise the offline eval harness across multiple ML teams — pick the required slice/holdout/adversarial guardrails every team must run, and set the review-gate the eval must pass before a model deploys
- Own the LLM-as-judge policy at org scope — where it is allowed as a primary metric, where it is allowed as a diagnostic, and how bias in the judge model is tracked
- Set the shadow / canary / progressive-rollout contract every team's deploy adheres to, and the review body that arbitrates when two teams' experiments interfere
- Set the experimentation-platform contract: SRM diagnostics, CUPED, sequential testing, interference / novelty effects, and the multi-team review cadence that catches trust breaches early
- Set the delegation contract to model-evaluation-engineer / ai-eval-engineer for platform build depth

## Structure

- `01-…md` … `0N-…md`: lecture chapters.
- `exercises/`: per-exercise prompts.
- `labs/`: long-form hands-on labs.
- `quizzes/`: knowledge checks.
- `resources.md`: external references.
