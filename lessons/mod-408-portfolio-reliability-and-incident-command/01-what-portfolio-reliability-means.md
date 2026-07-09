# What "ML Reliability at Portfolio Scope" Means

## Motivation

The Senior tech-lead who kept "the fraud model" green — a five-nines availability number, a p99 latency budget, a page when the Triton pod restarted — is now the Staff engineer over fraud, recommender, ranking, and email personalization. Their single-team SLO instincts do not transfer cleanly. Fraud paged once in the last year: a Kafka partition rebalance dropped features for four minutes. Nobody paged for the fact that the recommender's calibration has been drifting for six weeks and the CTR is quietly down 3%. Neither of those is a classical availability incident. The org already has a peer platform SRE team that owns latency and uptime and pods. What the ML engineer at L40 owns is the ML-specific failure surface *nobody else in the org is instrumented to see*.

This chapter names that scope of work. The rest of the module — the SLI/SLO catalogue in chapter 2, the severity matrix in chapter 3, the multi-team incident-command playbook in chapter 4, and the postmortem template in chapter 5 — builds on the definition that lands here.

## Portfolio reliability, defined

**Portfolio-scope ML reliability** in this module means:

> The set of standards, SLIs, incident-command practices, and postmortem discipline that the Staff ML engineer applies across three-to-five production ML systems so that (a) each system's ML-specific failure modes are detectable, pageable-when-appropriate, and recoverable, and (b) the same failure does not have to be re-learned per team.

Two properties are load-bearing in that definition:

- **ML-specific.** Not availability, not latency, not pod restarts — those belong to the peer platform SRE team. Portfolio reliability owns *prediction quality, drift, calibration, feature freshness, retraining hygiene* — the failure surface that only shows up if you know what to instrument.
- **Portfolio-scope.** Not one system. The Staff engineer is not on-call for fraud; they set the *standard* fraud, recommender, and ranking all report against, run point when an incident crosses team boundaries, and carry forward the lesson so recommender does not re-learn what fraud learned last quarter.

## How this differs from single-service SRE

Google's *Site Reliability Engineering* (Beyer, Jones, Petoff, Murphy — 2016) and the follow-on *Site Reliability Workbook* (2018) establish the vocabulary — SLI, SLO, SLA, error budget, toil, blameless postmortem — that this module builds on. Read them before this module. Their model of reliability, however, is built around a specific failure mode: **the service is up or the service is down, and up is measured by requests that returned successfully within a latency budget**. That model was designed for search, Gmail, and Ads-serving; it works well when the failure surface is "the RPC returned an error" or "the RPC took too long".

ML systems break in *four* ways the classical SRE model doesn't natively name:

1. **Bad predictions.** The service returned 200 in under 50ms, and the prediction was wrong. Fraud let a fraudulent transaction through; ranking put an irrelevant item on top. No page fires because every classical SLI is green.
2. **Silent drift.** The distribution the model was trained on has moved. Predictions are still *plausible* but their quality has degraded. Nobody paged because nobody instrumented the *distribution* of predictions vs. the training distribution.
3. **Feature-pipeline breakage.** The upstream event stream started emitting a schema-drifted payload. The feature pipeline still runs — it fills nulls, or defaults, or picks up the wrong join key. The model retrains on a subtly corrupted view of reality and quality decays over two weeks.
4. **Retraining-loop stall.** The nightly retrain hasn't landed successfully in nine days. Nobody noticed because the *serving* model is still there, still returning fresh predictions, still under latency budget. The world moved; the model didn't.

Classical SRE's page-on-error-and-latency instinct catches zero of these four. The peer platform SRE team, doing their job well, will report clean availability numbers while all four are compounding underneath. This is why the peer platform track can't own portfolio ML reliability — not because they aren't good, but because the failure surface isn't in their instrumentation.

The Staff engineer's contribution is to make *these* four failure classes first-class — with SLIs, with SLOs, with alerts, with severity, and with a rehearsed incident-command response.

## How this differs from the peer platform SRE track

The peer platform tracks — `ai-infra-mlops-learning`, `ai-infra-ml-platform-learning`, `ai-infra-performance-learning` — own the *build side* of reliability tooling. They ship the monitoring substrate, the feature-store freshness metrics, the retraining orchestrator, the model-registry contract, the canary rollout mechanics. That is legitimate platform-team work and this module does not re-teach it. See [`../mod-405-ml-platform-strategy/`](../mod-405-ml-platform-strategy/) for the portfolio-scope contract with those teams.

Portfolio ML reliability sits *on top of* what the platform tracks ship. The Staff ML engineer's questions are:

- Which SLIs must every team's model expose to be considered production-ready in this portfolio?
- What severity is a silent-quality-drift incident, and does it page or does it ticket?
- When ranking, recommender, and fraud all degrade at once because the shared user-embedding pipeline broke, who is Incident Commander?
- How do we make sure recommender doesn't repeat the postmortem action item fraud filed six months ago?

None of those are platform-team build questions. They are portfolio-standard, portfolio-severity, portfolio-command, and portfolio-lesson-carry questions. The platform team can and should implement the tools; the Staff ML engineer authors the contract and enforces the discipline.

## The four ML failure classes carried through the module

This module uses the same four failure classes in every chapter, mapping them to SLIs (chapter 2), severity (chapter 3), incident-command shape (chapter 4), and action-item categories (chapter 5). Naming them here so the vocabulary is stable:

- **Bad predictions.** Individual predictions are wrong. Detected via ground-truth-label backfill, human review sample, downstream business-metric shift, or a canary champion-challenger disagreement. Almost never a page in the moment (no ground truth yet); almost always a Sev-2 ticket once labels arrive.
- **Silent drift.** The distribution of inputs or predictions has moved from the training distribution. Detected via population-stability index, KL divergence, calibration curves, feature histograms. Rarely a page (slow-moving); a rolling ticket that escalates by severity as the drift grows.
- **Feature-pipeline breakage.** A shared feature is stale, wrong, or missing. Detected via feature-freshness SLI, feature-value distribution check, upstream schema-registry alarm. Frequently a page — it is the ML-side failure class most likely to cascade into a user-visible latency or error.
- **Retraining-loop stall.** The scheduled retrain hasn't produced a promotable candidate. Detected via a retraining-SLA SLI and a "model-age vs. SLO" alarm. Ticket the first day, page by day three; a stalled retrain compounds silently and always shows up in a postmortem eventually.

Each of these has an analogue in Breck, Cai, Nielsen, Salib, Sculley — *The ML Test Score: A Rubric for ML Production Readiness and Technical Debt Reduction* (IEEE Big Data, 2017). That paper is the closest thing the industry has to a production-readiness checklist; the SLI catalogue in chapter 2 is a portfolio-standard subset of it.

## What the Staff ML engineer owns vs. what the platform SRE track owns

**Staff ML engineer owns (portfolio scope).**

- The **ML-specific SLI catalogue** every team's model must expose (chapter 2).
- The **portfolio SLOs** for those SLIs and the **error-budget policy** that pauses launches when the budget is spent.
- The **severity matrix** for ML incidents including the silent-failure column (chapter 3).
- **Incident Command** when the incident spans two or more ML teams or involves an ML-specific failure class the platform SRE isn't instrumented for (chapter 4).
- The **org-standard postmortem template** with ML-specific action-item categories (chapter 5).
- The **retraining-as-deploy contract** every model registry entry must satisfy (chapter 6).

**Peer platform SRE / MLOps track owns (build scope).**

- **Availability, latency, and pod-level SLIs** for the inference-serving substrate.
- The **monitoring substrate** — Prometheus / Grafana / Datadog / evaluation stack — that the ML SLIs are computed on top of.
- **Feature-store freshness metrics** as instrumentation (not policy).
- **Model-registry** implementation and canary-rollout mechanics.
- **On-call rotation and page routing** at the substrate level.

**Peer team-lead / EM track owns (people scope).**

- Staffing the on-call rotation across the ML teams.
- Coaching the L30 tech-leads through their incident-command turns.
- Owning the *headcount* consequence of a repeated postmortem action item.

See [`../mod-401-staff-ml-role-scope/`](../mod-401-staff-ml-role-scope/) chapter 5 for the peer-EM partnership contract this module leans on during multi-team incidents.

## The "am I doing portfolio-reliability work?" heuristic

Sibling to the heuristics in mod-401, mod-402, and mod-404:

> **If my document names an SLI, a severity level, an incident-command role, or an action-item category that two or more ML teams will adopt, and defends why it belongs at portfolio scope instead of at platform-team scope or single-team scope, I am doing portfolio-reliability work. If I am adjusting the alert thresholds on one team's model, I am doing L30 tech-lead work. If I am designing the metrics-collection substrate, I am doing platform-team work.**

The two altitude errors to catch in yourself:

- **Drift down into single-team tuning.** Rewriting fraud's alert thresholds is L30 work. The Staff engineer's contribution is the *standard* the thresholds are set against — the SLI, the SLO, the severity — not the numeric threshold on one team's dashboard.
- **Drift sideways into the platform build.** Writing the drift-detection library is the platform / MLOps track's work. The Staff ML engineer's contribution is *which drift metric belongs in the portfolio SLI catalogue* and *what SLO it must meet*, not the library implementation.

## Summary

- **Portfolio-scope ML reliability** is the set of standards, SLIs, incident-command practices, and postmortems the Staff ML engineer applies across three-to-five ML systems so ML-specific failures are visible, actionable, and not re-learned per team.
- Classical SRE vocabulary — SLI, SLO, error budget, blameless postmortem — is load-bearing prerequisite reading, but its default failure surface (RPC error, latency exceeded) covers **zero of the four ML failure classes** this module owns.
- The four ML failure classes carried through the module: **bad predictions, silent drift, feature-pipeline breakage, retraining-loop stall**. Chapters 2-6 hang SLIs, severities, and commands off them.
- Staff owns the portfolio *standard*: the SLI catalogue, the severity matrix, the incident-command playbook, the postmortem template, the retraining-as-deploy contract. The platform SRE / MLOps track owns the build. The peer EM owns the people rotation.
- The heuristic: if two or more ML teams will adopt the artifact, and it defends why it belongs at portfolio scope, it is portfolio-reliability work.

The next chapter turns the four failure classes into a concrete SLI/SLO catalogue every team's model must ship against — the artifact exercise-01 asks the learner to produce.
