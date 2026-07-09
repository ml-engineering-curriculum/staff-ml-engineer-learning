# The Experimentation-Platform Consumer Contract and the Cross-Team Review Body

## Motivation

Chapter 4's rollout contract said *how* traffic ramps. It named canary and progressive-rollout as A/B experiments in the statistical sense but deliberately did not walk the statistical machinery. That machinery — SRM diagnostics, CUPED variance reduction, guardrail metrics, interference detection, novelty effect handling — is the trustworthy-experimentation literature that Kohavi, Tang, and Xu named as canonical in their 2020 Cambridge book. Every ML org that runs A/B experiments at any serious scale ends up with a home-grown or vendor experimentation platform (Microsoft ExP, LinkedIn XLNT, Booking.com's platform, Airbnb's ERF, Netflix's platform are the public prior art); the ML org's job at Staff scope is not to build that platform but to **consume it well**, and to arbitrate when two teams' experiments interfere.

This chapter walks two artifacts. The **experimentation-platform consumer contract** — what the ML org demands the platform expose — pairs with [`exercises/exercise-04-experimentation-platform-consumer-contract.md`](exercises/exercise-04-experimentation-platform-consumer-contract.md). The **cross-team experiment review body charter** — the multi-team review that arbitrates interference and enforces trust — pairs with [`exercises/exercise-03-cross-team-experiment-review-charter.md`](exercises/exercise-03-cross-team-experiment-review-charter.md).

The two artifacts compose. The contract names the machinery; the review body catches when the machinery is misused, misread, or unavailable.

## The experimentation-platform consumer contract

The contract is a document — two-to-three pages — that names the specific capabilities the ML org requires the experimentation platform to expose. It is analogous to mod-405's platform-consumer-contract vocabulary: the ML org is the consumer, the platform team is the vendor, and the document is the SLA.

### Required capability 1 — SRM (Sample Ratio Mismatch) diagnostics

Every experiment must report an SRM check on assignment. SRM — the observed traffic split between arms diverges from the intended split by more than a chance-explained margin — is Kohavi/Tang/Xu's canonical first trust-check on an experiment. An experiment with an unexplained SRM is not a valid experiment; the metric numbers cannot be trusted, and the review body's default verdict is *invalidate and re-run*.

The contract requires the platform expose:

- A per-experiment SRM p-value on the assignment log.
- An alerting mechanism when SRM crosses the org threshold (typical: p < 0.001) that pages the experiment owner before the experiment analysis is trusted.
- A dashboard visible to the review body that shows SRM status across every active experiment in the portfolio.

Without SRM diagnostics as a first-class platform feature, half the review body's disputes will be "the numbers look wrong" arguments that never reach a resolution. The contract makes SRM undebatable.

### Required capability 2 — CUPED (or equivalent variance reduction)

CUPED — Controlled-experiments Using Pre-Existing Data, from Deng, Xu, Kohavi, Walker 2013 — is the reference variance-reduction technique. It uses pre-experiment covariates to shrink metric variance and materially reduces the sample size needed to detect a given effect. Every ML org at nontrivial traffic scale runs CUPED or an equivalent.

The contract requires:

- CUPED (or a documented equivalent) available as a first-class platform capability, not an ML-team-authored notebook.
- Documentation of the pre-period, the covariate list, and the guardrails on when CUPED is or is not appropriate (Kohavi et al. caution against applying CUPED to metrics whose pre-period distribution has changed substantially).
- The platform reports both the CUPED-adjusted and the unadjusted numbers, so the review body can see when CUPED is doing the load-bearing work.

### Required capability 3 — sequential testing

Naive A/B testing assumes a fixed horizon. Teams that peek at their experiment before the horizon and stop early on a favourable-looking result inflate their false-positive rate substantially. Sequential testing — always-valid p-values, mixture sequential probability ratio tests, Bayesian equivalents — is the mechanically-correct fix.

The contract requires:

- The platform supports at least one sequential-testing method with published false-positive properties. The specific method (mSPRT, group sequential, always-valid inference, Bayesian) is negotiable; the property "always-valid at every interim look" is not.
- Halt thresholds registered before the experiment starts are what the sequential test evaluates against. Post-hoc thresholds fail the review-body's trust check.
- The platform surfaces the peek-count in the experiment metadata — how many times the results have been inspected — as a review-body diagnostic.

### Required capability 4 — guardrail metrics with automatic monitoring

Every experiment must have a **guardrail set** — the org-wide guardrail metrics from chapter 2 (safety, latency, cost) plus the cross-team guardrails from the chapter-4 declaration. The platform monitors these automatically for the experiment's duration and halts the ramp if any crosses its threshold.

The contract requires:

- Guardrails are a first-class experiment configuration, not a per-team dashboard hack.
- The platform runs the halt logic (with paging) rather than requiring the team's on-call to notice.
- A guardrail-breach event is recorded in the experiment's metadata and visible to the review body.

### Required capability 5 — interference detection

When two teams' experiments run on overlapping traffic and their treatments interact, both teams' analyses are biased. The trustworthy-experimentation literature discusses interference at length. The platform must expose:

- A per-experiment interference report: which other active experiments share more than a threshold fraction of traffic, and whether the treatments are known to interact.
- A cross-experiment orthogonalisation view — the ability to see one experiment's metric conditioned on the presence or absence of another. This is what the review body needs to arbitrate when Team A's launch depresses Team B's guardrail.
- A pre-experiment declaration of expected interference, checked against actual overlap after the experiment starts.

### Required capability 6 — novelty and primacy effect visibility

New treatments produce transient user response — novelty (positive) or primacy (negative) — that decays over time. Fixed-horizon reports at day 1 do not generalise. The platform must expose:

- Time-series views of the treatment effect over the ramp, so the review body can see when a positive effect is fading toward null.
- A minimum ramp duration setting for the primary metric that guards against novelty-dominated launch decisions.

### Required capability 7 — metric registry with owner and stable definition

Every metric the platform reports must have an entry in a metric registry — owner, definition, computation, unit, direction of goodness. Two teams' arguments about "what does this metric mean" are common enough that the metric registry pays for itself in prevented arguments. The registry composes with mod-402's substrate blueprint (the eval column) as the eval-time counterpart of the feature-store's feature registry.

## What the ML org does *not* demand

The contract is explicit about the boundary:

- **The platform's internal architecture is the platform team's business.** The contract names capabilities, not implementations.
- **New statistical methods can be adopted at the platform team's cadence.** The contract's list is the minimum, not the ceiling.
- **The platform team's roadmap is negotiated in the mod-405 platform-strategy conversation**, not in this consumer contract. This document is what the ML org requires; the platform-team's build is mod-405 / peer-platform-track territory.

## The cross-team experiment review body

The review body is the operational counterpart to the contract. Where the contract says "the platform exposes SRM diagnostics", the review body says "and someone with authority to invalidate an experiment is watching those diagnostics on the org's active experiments every week".

### Composition

The review body is small — five to seven people. Recommended composition:

- **Two rotating Staff-plus ML engineers** from teams in the portfolio, rotating quarterly. Rotation matters because the review body's authority depends on it not being one team's fiefdom.
- **The platform team's experimentation-platform lead.** Non-voting on ML judgement calls; voting on whether a platform capability is delivering as contracted.
- **A data-science / analytics lead** with statistics depth — often the person who actually understands sequential-testing and interference in enough detail to arbitrate a technical dispute.
- **The responsible-AI lead** (from the mod-409 counterpart) as an ex-officio member when an experiment intersects a Tier-C system from chapter 3 or the responsible-AI risk register.
- **The peer EM** — as the mod-401 chapter-5 partnership contract requires — non-voting on technical calls, voting on process and escalation.

### Cadence

Weekly synchronous meeting (30-45 minutes) plus an asynchronous channel for urgent halts. Kohavi et al.'s prior art at Microsoft and the LinkedIn / Airbnb / Booking equivalents run something similar; the specific cadence is less load-bearing than the discipline of running it every week without skipping.

### What the review body arbitrates

Four routine cases:

1. **New experiment approval.** Pre-experiment plan review: primary metric, guardrails, halt thresholds, cross-team-impact declaration. Emits `accept / nudge / block / defer`, same vocabulary as the eval-gate.
2. **In-flight interference disputes.** Team A's experiment appears to be depressing Team B's guardrail. The review body reads the interference report from the platform, decides whether Team A must halt, ramp down, or defer to a windowed re-run.
3. **Trust-breach investigations.** SRM alarms, halt-threshold violations, guardrail breaches. The body decides whether the experiment is invalidated, whether the underlying platform issue is a build-back item, and what the org-wide follow-up is.
4. **Standard-of-care exceptions.** A team requests to run an experiment that does not conform to the contract (no CUPED, short ramp, no guardrails on a low-risk system). The body decides whether the exception is granted, logged, and time-bound.

### Escalation

When the review body cannot resolve — usually a political dispute where two teams' interests genuinely conflict on how a launch should proceed — the escalation path is:

1. First, to the two teams' L40 EMs and the peer platform lead, per the mod-401 partnership contract.
2. Second, to the director / VP of the product area.
3. Third, to the mod-410 leadership-communication channel — this is the point where a Staff engineer authors the exec-audience one-pager that summarises the dispute and asks for a call.

Escalations are rare in a well-functioning review body — five to ten per year in a five-team portfolio is typical.

## The interaction between the two artifacts and the rest of the module

The consumer contract (this chapter) and the review-body charter (this chapter) compose with:

- **Chapter 2's offline eval standard** — the eval-gate before the rollout is the same review body's upstream check on the same launch.
- **Chapter 3's LLM-as-judge policy** — the review body enforces the tier classification when a judge score is proposed as an experiment's primary metric.
- **Chapter 4's rollout contract** — canary and progressive-rollout are what the platform contract's statistical machinery watches; the review body arbitrates when the ramp halts on a guardrail.
- **[`mod-408`](../mod-408-portfolio-reliability-and-incident-command/)** — a guardrail-breach halt is an incident under mod-408's SLO-and-incident vocabulary; the review body and the incident-command chain interoperate.

## The artifacts this chapter produces

- **The experimentation-platform consumer contract document** (exercise-04). Two-to-three pages naming the seven capabilities, the review cadence with the platform team, and the exception path.
- **The cross-team experiment review body charter** (exercise-03). One-to-two pages naming composition, cadence, what the body arbitrates, and the escalation path.

Both roll up into the mod-402 portfolio RFC's Section 9 as the operational side of "how the eval-metric-conflict cell is fixed at portfolio level".

## Summary

- The **experimentation-platform consumer contract** names seven capabilities the ML org demands the platform expose — SRM diagnostics, CUPED (or equivalent), sequential testing, guardrail-metric monitoring, interference detection, novelty/primacy visibility, metric registry with owner.
- The contract is a *consumer* contract in the mod-405 sense — it names capabilities, not implementations. The platform team's build is mod-405 / peer-platform-track territory.
- The **cross-team experiment review body** is a small, rotating Staff-plus body that meets weekly. It arbitrates new-experiment approvals, in-flight interference disputes, trust-breach investigations, and standard-of-care exceptions.
- Escalation flows through the mod-401 EM-partnership channel first, then to the product director, then to the mod-410 exec-comms channel.
- The two artifacts compose with the chapter-2 eval standard, the chapter-3 judge policy, the chapter-4 rollout contract, and mod-408's incident vocabulary.

The next chapter walks the delegation contract to the peer specialist tracks (model-evaluation-engineer, ai-eval-engineer) that keeps the Staff engineer out of the build and in the program-scope work.
