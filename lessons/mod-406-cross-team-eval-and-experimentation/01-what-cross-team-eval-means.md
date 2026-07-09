# What "Cross-Team Eval and Experimentation Program" Means at Staff Scope

## Motivation

The learner arrives at this module holding a portfolio blueprint from [`mod-402`](../mod-402-multi-team-ml-architecture/) and a specific finding from that module's chapter 3: **the eval-metric-conflict failure mode**. Two teams' offline metrics do not compare, their online experiments interfere on shared traffic, and no arbitration body exists. Chapter 3 named the failure and pointed at Kohavi/Tang/Xu; this module is where the fix actually gets built.

Nothing in mod-402 tells you *what* the eval-program contract should say. Nothing in [`mod-405`](../mod-405-ml-platform-strategy/) does either — mod-405 is about the platform teams whose paved-road *build* backs the substrate; it is about roadmap and API stability, not about the semantic contract of "what does an eval have to prove before it deploys". That semantic contract is what a **cross-team evaluation and experimentation program** is, and it is what this module owns.

This chapter names the scope of work in enough detail that chapters 2 through 6 have a stable definition to build on. The tone here matches [`mod-402/01-what-is-portfolio-scope.md`](../mod-402-multi-team-ml-architecture/01-what-is-portfolio-scope.md) and [`mod-404/01-scope-and-hand-off.md`](../mod-404-distributed-training-systems-at-scale/01-scope-and-hand-off.md): what "program" means, what it produces, and the "am I doing eval-program work?" heuristic.

## What "cross-team eval and experimentation program" means here

A **cross-team eval and experimentation program** in this module means:

> A written set of contracts — offline eval harness standard, LLM-as-judge policy, shadow / canary / progressive-rollout rules, and experimentation-platform consumer contract — that every ML team in a three-to-five-system portfolio agrees their models must satisfy before they train, evaluate, ship, or launch, plus the review body that arbitrates when two teams' evaluations or experiments interfere.

The program is a document set and a review body, not a codebase and not a platform. If the Staff engineer's output is an eval framework in Python that other teams import, either they have taken on the model-evaluation-engineer or the platform team's job, or the platform team is missing and the program has a resourcing problem the contract did not disclose. Chapter 6 handles the delegation contract that keeps the Staff engineer out of the build.

## What this is *not*

Three adjacent things that live in adjacent modules or peer tracks:

- **A single team's eval harness.** That is Senior tech-lead scope on one team. The staff-scope program is the *contract* that every team's harness satisfies, not the harness itself.
- **The model-evaluation-engineer or ai-eval-engineer's specialist depth.** Those peer specialist tracks own the individual-contributor eval-authoring depth for a *single* team or *single* eval framework — the actual eval-set construction, the actual judge-model prompt, the actual annotator disagreement analysis. This module owns the *org-wide policy* those specialists work under. Chapter 6 walks the hand-off.
- **The experimentation platform itself.** The A/B platform is built by a platform team (mapped to `ai-infra-mlops-learning` or an in-house data-platform team). This module owns the consumer contract the ML org demands the platform expose; it does not own the platform's build. Chapter 5 walks the consumer contract in detail.

Naming the negations up front matters because the failure mode among newly-Staff engineers is to reach for the specialist or platform vocabulary and start building. That is a scope error the review body will notice.

## Why the eval-metric-conflict failure mode requires a *program* fix

Mod-402 chapter 3's fourth failure mode — eval-metric conflicts — has a structural property that distinguishes it from the other three portfolio failure modes:

- Pipeline-jungle, feature drift, and registry fragmentation can each be fixed by a single well-scoped **substrate consolidation** — write the contract, migrate the teams, retire the duplicated implementations. The RFC template in mod-402 chapter 5 carries that consolidation.
- Eval-metric conflict cannot be fixed the same way, because *the metrics themselves are not fungible*. Team A's NDCG@10 is not a re-implementation of Team B's recall@50; they are genuinely different objects tied to genuinely different business tasks. You cannot merge them into one metric without destroying information the product needs.

The fix, therefore, is not consolidation of the metrics. It is a **program**: an offline-eval standard that says which slices, holdouts, and adversarial guardrails every team's eval must include (chapter 2); a policy for when it is legitimate to substitute an LLM-as-judge score for a human-labelled metric (chapter 3); a rollout contract that says how model changes reach production traffic and when a launch is halted (chapter 4); and an experimentation-platform contract with a review body that arbitrates cross-team interference (chapter 5).

The metrics themselves stay per-team. The *scaffolding around them* — the guardrails, the review, the rollout, the arbitration — is what the program standardises.

## The four artifacts this module produces

Mapped one-to-one with the four exercises:

1. **The org-scope offline-eval harness standard** (exercise-01, chapter 2). What every team's eval must run before a training-and-deploy is allowed to proceed: required slices, held-out sets, adversarial guardrails, freshness and drift checks, the metadata attached to every eval run. Two-to-four pages.
2. **The LLM-as-judge org policy** (exercise-02, chapter 3). Where judge-model scores are allowed as a primary metric, where they are allowed only as a diagnostic, where they are forbidden. How judge-model bias is tracked and calibrated against human raters. Two-to-three pages.
3. **The cross-team experiment review charter** (exercise-03, chapter 5). Composition of the review body, the meeting cadence, the arbitration rules when two teams' experiments interact on shared traffic, the escalation path when arbitration fails. One-to-two pages.
4. **The experimentation-platform consumer contract** (exercise-04, chapter 5). What SRM diagnostics, CUPED, sequential-testing, and interference-detection features the ML org demands the platform expose, and the review cadence with the platform team that keeps the contract honest. Two-to-three pages.

The rollout contract in chapter 4 (shadow → canary → progressive) is a load-bearing artifact that appears across exercises 01 and 03 rather than as its own exercise; it composes with the eval-harness gate on one side and the experiment-review body on the other.

An integrating fifth artifact — a **one-page eval-program summary** for the portfolio RFC's Section 9 — is authored as part of exercise-01's document; it is what the mod-402 architecture review body reads when it evaluates the eval-conflict cell of the blueprint matrix.

## What Staff owns vs. what the specialist and platform tracks own

The temptation is to reach for the vocabulary of the peer specialist tracks (model-evaluation-engineer, ai-eval-engineer) or the peer platform tracks (ai-infra-mlops-learning) and start authoring in *their* register. That is a scope error. The Staff engineer's ownership at mod-406 is:

**Staff owns.**

- **The org-wide eval-harness contract.** What every team's eval must produce; what the review gate looks for; what per-team flexibility is preserved. Chapter 2.
- **The LLM-as-judge policy.** Where allowed, where forbidden, calibration cadence, bias-tracking cadence. Chapter 3.
- **The rollout contract.** Shadow → canary → progressive-rollout stages, gate criteria, rollback criteria. Chapter 4.
- **The experimentation-platform consumer contract.** What the ML org demands the platform expose; the review cadence with the platform team. Chapter 5.
- **The cross-team experiment review body's charter.** Composition, cadence, arbitration rules, escalation. Chapter 5.

**Peer specialist tracks own.**

- **The actual eval-set construction.** [`model-evaluation-engineer-learning`](https://github.com/ml-engineering-curriculum/model-evaluation-engineer-learning) — how a specific held-out set is drawn, how annotator agreement is measured, how a judge prompt is authored and iterated. The Staff engineer says *what must be produced*; the specialist says *how to produce it well*.
- **The actual LLM-as-judge implementation.** [`ai-eval-engineer-learning`](https://github.com/ml-engineering-curriculum/ai-eval-engineer-learning) — the judge-model prompt, the pairwise-vs-scalar rubric, the bias-calibration harness itself. The Staff engineer sets the policy; the specialist implements it.
- **Single-team eval on a specific model.** The team's Senior tech-lead owns the day-to-day model-quality work. The program's contract is what they check against, not a replacement for their judgement.

**Peer platform tracks own.**

- **The experimentation-platform build.** The A/B system's traffic-splitting, event-recording, and analysis pipeline. Peer platform team territory.
- **The rollout tooling.** Feature-flag system, canary controller (Argo Rollouts / Flagger / homegrown), the plumbing the rollout contract runs on. Peer platform team.

If the Staff engineer at mod-406 finds themselves authoring an eval-set annotation guideline or debugging a judge prompt, either they are pair-programming with the specialist as part of the delegation review — legitimate but time-boxed — or the delegation contract has broken and the program is being enforced on the specialist's turf. Chapter 6 walks the delegation contract that prevents that.

## The "am I doing eval-program work?" heuristic

Sibling to the mod-401, mod-403, and mod-404 heuristics:

> **If my document says what every ML team in the portfolio must prove before shipping, how disagreements between two teams' evals or experiments get arbitrated, and how the eval-platform and rollout contracts stay honest across teams, I am doing eval-program work. If it says how to construct a specific held-out set for one team's model, I am doing model-evaluation-engineer work. If it says how the experimentation platform's variance-reduction module works internally, I am doing platform-team work.**

Two drift patterns to catch in yourself:

- **Drifting down into single-team eval work.** *"Team B's ranking eval should include a fresh-cold-start slice."* If Team B's Senior tech-lead has not asked for that specific slice, and if the org-wide standard does not require *any* cold-start slice, you are second-guessing a team's own eval author. The right move is either to add "cold-start slice" to the org standard (chapter 2 territory) or to leave it to Team B's judgement.
- **Drifting up into product-strategy re-litigation.** *"Team A should not optimise NDCG@10 at all — the business object is wrong."* That is a product-strategy conversation with the PM and director, not an eval-program conversation. The program says *how* metrics are measured, guarded, arbitrated, and rolled out; it does not pick the business objective.

## Summary

- A **cross-team eval and experimentation program** at Staff scope is a set of four contracts — offline eval harness standard, LLM-as-judge policy, rollout contract, experimentation-platform consumer contract — plus a review body, that every ML team in a three-to-five-system portfolio operates under.
- The program is the **systemic fix** for the eval-metric-conflict failure mode from [`mod-402/03-portfolio-failure-modes.md`](../mod-402-multi-team-ml-architecture/03-portfolio-failure-modes.md). Unlike the other portfolio failure modes, eval conflict cannot be fixed by substrate consolidation; the metrics are genuinely per-team. The scaffolding around them is what standardises.
- The Staff engineer owns four documents plus a review charter. Kernel-level eval implementation belongs to model-evaluation-engineer and ai-eval-engineer; the platform build belongs to the peer platform tracks. Chapter 6 walks the delegation contract.
- The "am I doing eval-program work?" heuristic catches drift down into single-team eval and drift up into product-strategy re-litigation — the two most common altitude errors here.

The next chapter walks the offline eval harness standard: the required slices, the held-out and adversarial guardrails, the metadata every eval run must attach, and the review-gate that the eval must pass before a model deploys.
