# The Shadow / Canary / Progressive-Rollout Contract

## Motivation

The eval gate from chapter 2 accepts a candidate model. The team is now ready to deploy. What happens next is where most portfolio-scope launch incidents actually live: a team ships to 100% of traffic immediately because "the offline eval said it was fine", a second team's guardrail metric quietly degrades, the incident-command channel lights up eight hours later, and the postmortem's first line is "there was no canary".

The rollout contract is the third of the four artifacts this module produces. It is deliberately not called a "release process" — a release process is L30 tech-lead territory and is per-team. What this chapter authors is the **org-wide contract** that every team's release process must satisfy: three stages (shadow, canary, progressive-rollout), the gate criteria between them, the rollback criteria at each stage, and the interaction with the mod-402 model-registry contract that carries the version.

The trustworthy-experimentation literature (Kohavi/Tang/Xu, chapters on ramp-up, guardrail metrics, and interference) is the theoretical backing; the tooling literature (Argo Rollouts, Flagger, feature-flag systems) is the operational backing. This chapter names the org-scope contract those two literatures produce.

## The three stages

### Stage 1 — shadow

**What it is.** The candidate model receives production traffic — either full traffic or a representative sample — but its outputs are *not* returned to the user. Instead they are logged and compared against the production model's outputs. Sometimes called "dark launch" in the web-serving literature.

**What it proves.** That the candidate can handle production traffic distribution (feature availability, edge-case inputs, latency envelope) without breaking. Shadow is *not* an eval of model quality — the outputs are not seen by users, so engagement and downstream metrics are not measurable. Shadow is an **operational** gate.

**Gate criteria to proceed to canary.** The candidate must show:

- No exceptions or failures above the org-standard error-budget on the shadow traffic sample.
- Latency p50 / p95 / p99 within the org-standard bounds for the system class.
- Output-distribution parity checks against the production model — the candidate's output distribution on the same inputs should not have moved by more than the calibrated shadow-drift threshold on a set of shape-check statistics (mean, quantiles, and for text outputs a length / vocabulary sketch).

**Rollback criteria.** Shadow is aborted (candidate retired to iteration) if any of the operational metrics blow the budget. No user impact has occurred; rollback is trivial.

**Duration.** Typically hours to a day. Long enough to see multiple traffic cycles; short enough to keep iteration velocity.

### Stage 2 — canary

**What it is.** A small fraction of real traffic (typical starting point: 1%) is routed to the candidate model, its outputs are returned to users, and both the candidate's and the control's downstream metrics are recorded. The canary is a **statistical** stage.

**What it proves.** That on real user impact, the candidate's primary metric and the org-wide guardrail metrics (chapter 5 walks these) are not obviously worse than control. Canary is *not* the full A/B experiment that establishes launch-worthiness; it is the risk-gate that prevents a bad candidate from ever reaching a large user base.

**Gate criteria to proceed to progressive rollout.** The candidate must show:

- Primary metric on the canary slice is directionally not worse than control by more than the pre-registered `canary-halt` threshold, with statistical significance handled by sequential-testing (chapter 5) or by a fixed-horizon test appropriate to the traffic volume.
- Every org-standard guardrail metric is within its guardrail band — where "guardrail band" is defined in the mod-405 experimentation-platform contract.
- Every cross-team guardrail (metrics owned by another team in the portfolio whose behaviour the candidate could affect) is within its band.

**Rollback criteria.** Canary is halted and the candidate rolled back if:

- Any guardrail crosses its halt threshold at any point during canary.
- The primary metric shows a statistically-significant *negative* direction with a magnitude worse than the pre-registered halt-on-loss threshold.
- The team's on-call, the eval-program review body, or the mod-408 incident-command process calls halt.

**Duration.** Typically 24 hours to a week, tuned to the system's traffic volume and the required detection sensitivity.

### Stage 3 — progressive rollout

**What it is.** The candidate is rolled out to increasing fractions of traffic (canonical shape: 1% → 5% → 25% → 50% → 100%) with a hold-period at each step. This is where the full A/B experiment lives.

**What it proves.** That the candidate is the launch — a real, statistically-defensible improvement over control on the primary metric and non-negative on the guardrails at production traffic scale. Progressive rollout is where the launch decision is made and defended.

**Gate criteria at each step.** At each traffic step, the same guardrail-and-halt logic runs as at canary; only the sample size grows and the sensitivity increases. The mod-405-adjacent trustworthy-experimentation vocabulary — SRM diagnostics, CUPED variance reduction, guardrail metrics, interference detection — belongs here, and is treated in chapter 5 as the experimentation-platform's job to expose, not the ML team's job to re-implement.

**Rollback criteria.** Same as canary at each step. The pre-registered thresholds are the same across steps; what changes is the statistical power.

**Duration.** Days to weeks, depending on traffic and effect size. Kohavi/Tang/Xu discuss the trade-off between fast-ramp (business urgency) and slow-ramp (better novelty-effect handling) at length; the org contract picks a default.

## What every team's deploy must have before the rollout can start

The contract requires, before any team pushes the candidate to shadow:

1. **A registered model version.** The candidate exists in the mod-402 model registry with full metadata: training-data snapshot, code SHA, eval-run results, author. If the candidate does not exist in the registry, the rollout does not start.
2. **An accepted eval-gate verdict.** Chapter 2's review-gate emitted `accept` or `nudge`. `block` and `defer` do not proceed.
3. **A pre-registered rollout plan.** A short document (half-page) naming the primary metric to be measured, the guardrails that will be watched, the halt thresholds, and the expected duration at each stage. Pre-registration matters because the trustworthy-experimentation literature is unambiguous that halt thresholds and metrics chosen post-hoc are a common source of false-positive launches.
4. **An on-call rotation for the rollout window.** Someone gets paged when a halt threshold trips. If no on-call is committed, the rollout does not start.
5. **A cross-team-impact declaration.** The team names, in the plan, which other teams' systems could be affected by the candidate — shared upstream data, shared traffic, shared downstream consumers. Named teams are notified before shadow begins. Chapter 5 walks the cross-team review body that arbitrates when the declaration is contested.

## Rollback semantics

Rollback in the contract has three specific meanings depending on the stage:

- **Shadow rollback.** Retire the candidate. Trivial. No user impact.
- **Canary rollback.** Route all traffic back to the control model version. The mod-402 model-registry contract's `alias` mechanism is the mechanical hook — the "prod" alias points at the previous version; traffic-splitting infrastructure re-resolves in seconds. The candidate stays in the registry for post-mortem inspection but is de-aliased from serving.
- **Progressive-rollout rollback.** Same mechanism as canary rollback but at higher traffic. The contract explicitly does *not* allow "roll forward to fix" as an alternative to rollback except when the previous version is known to be worse than the candidate's failure mode — the standard case is roll back, root-cause offline, iterate, re-enter the rollout at shadow.

The rollback tooling — the traffic-router, the feature flag, the canary controller — is *platform-team* build. The contract names the *behaviour* the platform-team tooling must support: single-command rollback, sub-minute traffic re-routing, no orphaned in-flight requests. Chapter 5 walks the consumer contract with the platform team.

## Interaction with the model-registry contract from mod-402

The registry contract is the load-bearing dependency here. Two properties matter:

- **Version identity.** The candidate has a unique registry version identifier that is what the traffic-splitting infrastructure references. If two teams' registries do not agree on version identifiers — the mod-402 chapter 3 registry-fragmentation failure mode — a rollout under this contract cannot execute cleanly. The rollout contract requires the registry contract be honest.
- **Stage / alias vocabulary.** The registry's `stage` and `alias` fields (chapter 2 of mod-402 named them from MLflow's vocabulary) are what the rollout contract manipulates. `shadow`, `canary`, `prod`, `prev-prod` are the four aliases every team's registry must expose per the mod-402 contract.

If a team's registry does not carry the alias vocabulary, the rollout contract cannot bind. This is one of the places the mod-402 registry-fragmentation failure mode has direct operational cost, not just aesthetic cost.

## What the contract does *not* cover

- **Feature-flag semantics for non-model changes.** The contract is for ML model version rollouts. Feature-flag-gated product changes that also toggle model behaviour need to compose with the rollout, but the flag system's own semantics are L30 tech-lead or platform-team territory.
- **The A/B experiment analysis itself.** The rollout contract's canary and progressive stages *are* A/B experiments in the statistical sense; the analysis of those experiments — SRM, CUPED, sequential testing, novelty, interference — is chapter 5's territory as the experimentation-platform consumer contract. This chapter names *how* traffic is ramped; chapter 5 names *what statistical machinery* the ramp uses.
- **Ramp shapes for non-standard systems.** Batch-scored systems (nightly re-scoring of a document corpus, not user-facing traffic) do not fit the shadow / canary / progressive pattern cleanly. The contract's exception path handles them; the default shape covers the modal case.

## The artifact this chapter produces

A single **rollout-contract document**, one-to-two pages, that names the three stages, the gate criteria, the rollback semantics, the pre-flight requirements, and the exception path for systems that legitimately do not fit the default shape. This document is part of exercise-01's deliverable (folded into the standard's Section 5-6) and is referenced by exercise-03's review-body charter.

## Summary

- Every model deploy in the portfolio moves through **shadow → canary → progressive-rollout**. The stages have distinct purposes — shadow is operational, canary is statistical-risk-gate, progressive-rollout is the launch A/B.
- Each stage has explicit **gate criteria** to advance and **rollback criteria** that halt. Halt thresholds are **pre-registered** — chosen after the fact, they become false-positive machinery.
- The contract requires, before shadow begins: registered model version, accepted eval-gate verdict, pre-registered rollout plan, on-call rotation, and cross-team-impact declaration.
- The contract binds to the mod-402 model-registry contract via **version identity** and the **stage/alias vocabulary**. Registry fragmentation breaks the rollout at the operational layer, not just the aesthetic layer.
- Rollback tooling is platform-team build; the contract names the behaviour the platform team must support.

The next chapter walks the experimentation-platform consumer contract and the cross-team experiment review body — the machinery around the canary and progressive stages that makes their statistics trustworthy and their cross-team interference detectable.
