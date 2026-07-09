# Retraining as Deploy: The Portfolio Hygiene Contract

## Motivation

Every ML system in the portfolio ships a new model version somewhere between hourly and monthly. Some do it on a Jenkins-shaped schedule, some on a Airflow DAG, some on a MLflow-promote-button, some on a full continuous-training pipeline. In every incident postmortem the pattern is the same: the retrained model was promoted, it silently degraded a metric, nobody caught it for six to sixty hours. Chapter 1's failure class 1 (bad predictions) and failure class 2 (silent drift) most commonly show up *because a new model version shipped without the same deploy hygiene the platform SRE team requires of a code change*.

This chapter authors the portfolio-scope contract that makes retraining a *deploy* — subject to the canary, rollback, and offline-replay discipline that a code change would be subject to. The contract is what the model-registry entry every team ships to must satisfy. It ties back to [`../mod-402-multi-team-ml-architecture/`](../mod-402-multi-team-ml-architecture/) chapter 2's shared-registry substrate and to [`../mod-406-cross-team-eval-and-experimentation/`](../mod-406-cross-team-eval-and-experimentation/)'s progressive-rollout contract.

## Framing: a retrain is a deploy

Say it out loud in the RFC: **a retrained model version is a deploy**. Everything that follows is the operationalisation of that framing. Three consequences:

- If a code deploy has a canary path, a retrain has a canary path.
- If a code deploy has a rollback trigger, a retrain has a rollback trigger.
- If a code deploy has a pre-merge test suite, a retrain has an offline-replay evaluation.

Teams that resist this framing usually resist because they retrain on a much faster cadence than they deploy code, and the canary / rollback / replay discipline feels expensive. That is exactly why the platform contract is portfolio-standard, not per-team — you cannot let three of five teams opt out of the discipline because their retrain is nightly. Chapter 4 postmortems for the third-time-this-year same-incident are usually traceable to a team that opted out.

## The three contract clauses

The retraining-as-deploy contract has three load-bearing clauses. Every team's model-registry entry must satisfy all three to be considered production-ready.

### Clause 1 — The canary path

**What.** A new model version cannot serve 100% of traffic on promotion. It serves some low-percentage slice (traditionally 1%, sometimes 5-10% for lower-traffic systems) alongside the previous champion for a canary window (traditionally 24 hours for a fast-moving system, up to 7 days for a slow-moving one). During the canary window, the SLIs from chapter 2's catalogue are computed on the canary slice and compared to the champion.

**Portfolio-standard requirements.**

- The canary path must exist on the *serving substrate* — the platform-team's model server, the KServe / Triton / vLLM / Ray Serve gateway that owns traffic-splitting. Per-team canary implementations are a portfolio smell; they diverge, they under-instrument, they get bypassed under pressure.
- The canary window is *not* the same as the offline eval slice. A retrain that improves offline metrics can still fail the canary; the canary is where the online guardrails from chapter 2 are the referee.
- The canary criterion is written into the registry entry as a signed contract: which SLIs are checked, what breach criteria trigger a rollback vs. a hold, who signs off on promotion past canary.

**What the platform team enforces.** The registry entry cannot be promoted to `production` state without a canary window having completed under the contract. The platform team's model-serving gateway is the enforcement point; the ML on-call cannot bypass the canary except through an explicit "emergency promotion" flag that is audited and reviewed monthly.

**Reference implementations.** KServe's canary rollout documentation; NVIDIA Triton's model-repository staged-rollout patterns; the deployment strategies in Ray Serve documentation. These are prior art the RFC reads against, not gospel — the portfolio-standard contract sits *above* the specific implementation the platform team ships.

### Clause 2 — The rollback trigger

**What.** Every canary window has explicit rollback criteria written into the registry entry. When the canary breaches, the platform gateway rolls back to the champion within a bounded time (target: 5 minutes for a Sev-0-risk system, 30 minutes for a lower-risk system). Rollback is *automatic* on hard SLI breaches; it is *human-approved* on soft SLI degradations that require judgement.

**Portfolio-standard requirements.**

- Rollback is *always available*. A champion model that has been retired more than 24 hours ago is not a rollback target; the registry entry must keep the last N champions warm-and-servable. The platform team owns the "N" and the "warmable" contract.
- The rollback trigger criteria are the same shape across the portfolio, differing only in numeric thresholds. Every team's canary has a "hard-red" set that auto-rolls back (calibration SLI drops below the SLO by 2×; feature-freshness SLI breaches; business metric drops >5% on the canary slice) and a "yellow" set that pauses promotion and pages the on-call.
- The rollback event triggers a Sev-2 automatically, per chapter 3's severity matrix. Rollback-and-forget is a chapter-5 anti-pattern; every rollback is a postmortem input.

**What the platform team enforces.** The gateway's rollback tooling. The registry keeps the previous champion registered as `rollback-target` for a portfolio-standard duration.

### Clause 3 — The offline replay

**What.** Before the canary can start, the retrained model must have been evaluated on an **offline replay slice** — a fixed, versioned slice of production traffic (typically 7-30 days depending on the model's cadence) replayed through the new model, with the SLIs from chapter 2's catalogue computed on the replay. If the replay's SLIs are worse than the current champion's on the same slice, the retrain fails and does not promote to canary.

**Portfolio-standard requirements.**

- The replay slice is *versioned* — every registry entry records which replay-slice version was used. When the slice changes (semi-annually), a burn-in window applies before the new replay is trusted.
- The replay is *the same* across teams' models where possible. Shared replay slices catch shared-feature failures — if the recommender's retrain fails replay because the shared user-embedding pipeline is degrading, the fraud team's retrain will fail the same replay soon, and the pattern is visible at portfolio scope.
- The offline replay is *diagnostic*, not sufficient. A retrain that passes replay still runs canary; a retrain that passes canary still runs progressive rollout per mod-406. Chapter 5's postmortems catch every case where a team treated offline replay as the sole gate.

**Reference implementations.** Every mature retraining pipeline in the industry — Uber's Michelangelo, Netflix's Metaflow, Meta's Ax / FBLearner — has offline-replay as a first-class step. The portfolio-standard contract sits above the implementation; it names *what* must be true of the replay, not *which* platform ships it.

## What the model-registry entry must record

Bringing the three clauses back to the artifact: every entry in the shared model registry (mod-402 substrate #3) must record:

- **Canary contract.** SLI set, breach criteria, canary duration, promotion signoff.
- **Rollback contract.** Rollback-target-champion, hard-red criteria, yellow criteria, auto vs. manual, time-to-rollback SLO.
- **Offline-replay contract.** Replay-slice version, replay SLI set, replay pass/fail thresholds, replay-vs-champion comparison result.
- **Training-data lineage.** Which snapshot the model was trained on (required for postmortem forensics — chapter 5 depends on this).
- **Model-owner + on-call.** Who owns this version. Who gets paged on a canary breach. Who gets paged on a rollback.

The mod-402 chapter-3 failure mode **model-registry fragmentation** shows up here: if the org has three registries, the contract has three variants; if two variants disagree on required fields, the contract is decorative. Portfolio reliability depends on the mod-402 substrate consolidation; you cannot enforce the retraining-as-deploy contract on top of a fragmented registry. Sequencing matters — the portfolio consolidation RFC that resolves mod-402's registry fragmentation usually needs to land before this contract can be enforced.

## The exemption pattern — how a team opts out cleanly

Not every model can carry the full contract. A rules-based fraud rule does not "retrain". A batch-scored monthly propensity model may not warrant a five-minute rollback SLO. The contract must have an **exemption pattern** so the exceptions are visible rather than silently bypassing.

The exemption pattern has three properties:

- **Explicit.** The registry entry has an `exemption` field that names the clauses it does not satisfy and why. `exemption: canary-path — no online serving, batch scoring only` is a legitimate entry; a blank field is not.
- **Reviewed.** Exemptions are re-visited semi-annually by the Staff ML engineer with the affected team's L30. New exemptions require an RFC or a lightweight decision doc.
- **Signalled in postmortem review.** When a postmortem involves an exempted system, the review meeting from chapter 5 explicitly checks whether the exemption contributed. If so, the exemption is a candidate for revocation.

## Ties to other modules

- **mod-402 chapter 2** (shared model-registry substrate) is the substrate this contract lives on. Chapter 3 of mod-402 (failure mode: model-registry fragmentation) is what happens when the substrate is not consolidated.
- **mod-406 chapter on progressive rollout** owns the multi-team progressive-rollout contract that sits on top of the canary. The canary in this chapter is *this system's canary*; mod-406's rollout is *across systems' shared traffic*. Both apply.
- **mod-405 chapter on platform contracts** owns the peer platform team conversation that turns this contract into a piece of the paved road they ship. The platform team implements canary tooling and rollback triggers; the Staff ML engineer authors the portfolio-standard contract they enforce.
- **Chapter 5 of this module** is where a violation of the contract lands as a postmortem action item — usually a "catalogue change" or a "retraining contract change" per the seven categories.

## Summary

- **A retrained model version is a deploy.** The retraining-as-deploy contract makes the discipline of code deploy — canary, rollback, offline replay — mandatory on every model version.
- The contract has **three clauses**: canary path, rollback trigger, and offline replay. Each is written into the model-registry entry the platform team enforces.
- The **canary path** must live on the platform serving substrate, not per-team. The **rollback** is bounded (5 minutes for high-risk systems), auto on hard-red SLI breaches, human on yellow. The **offline replay** is versioned and shared across systems where possible so shared-substrate failures are visible at portfolio scope.
- The **model-registry entry** must record canary contract, rollback contract, replay contract, training-data lineage, and on-call ownership. Registry fragmentation from mod-402 chapter 3 makes this uncomputable; the contract depends on mod-402's substrate consolidation.
- The **exemption pattern** is explicit, reviewed semi-annually, and re-visited when a postmortem involves an exempted system.
- The contract closes the module: chapter 2's catalogue defines the SLIs; chapter 3 defines when a breach pages; chapter 4 runs the incident; chapter 5 files the lesson; and chapter 6 makes the class of retrain-caused incidents rarer at the source.

Return to [`README.md`](README.md) for the exercise sequencing and the pointers into project-403.
