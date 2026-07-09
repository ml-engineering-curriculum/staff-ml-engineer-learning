# ML Platform Strategy at Org Scope: What This Module Is (and Is Not)

## Motivation

The Staff ML engineer who finished mod-402 is holding a portfolio blueprint that names four painful substrate cells and probably three merge / split / share verdicts. The blueprint's next question is not *"how do I build the shared feature store I proposed"* — that is the peer platform team's job — but *"what do I as the ML org commit to, in writing, so that the platform team is willing to build it, and what do I do when the answer is no?"* That question is what this module owns.

Two failure modes to guard against right out of the gate:

- **Silently becoming the platform team.** The Staff engineer whose blueprint proposed a shared feature store, and who then goes and builds it because the platform team is "too slow," has just re-scoped themselves as an under-resourced platform-team lead without a headcount to run it. This is the single most common mis-scope at L40 in ML orgs that grew organically. Mod-401's chapter 4 hand-off contracts are the vaccine; this module is the booster shot.
- **Wishing the platform team into existence.** The opposite mistake: a blueprint that lists every painful cell as "the platform team will fix" without any negotiation, adoption commitment, or contract behind it. That blueprint gets ignored — correctly — by the platform PM whose queue has ten teams making the same ask.

The mod-405 recipe sits between those two extremes. The **ML platform strategy at org scope** is the set of decisions and contracts by which the ML org decides *what to build itself, what to adopt off-the-shelf, what to ask the platform team to build, and what to contribute back when the ML org has built something the platform should own long-term*.

## What "ML platform strategy at org scope" means here

> A **platform-strategy decision** in this module is a written call — build / adopt-managed / adopt-OSS / contribute-back / defer — on one of the five ML-platform components (feature store, model registry, eval platform, training platform, inference gateway), made by the ML org's Staff engineer(s) in negotiation with the peer platform team, and carried in a **consumer contract** that names adoption volume, feedback cadence, on-call SLO, and API stability window.

Three things fall out of that definition and are worth naming explicitly.

**It is a document, not a codebase.** The output of mod-405 is a handful of RFCs — the build-vs-adopt decision doc, the consumer contract, the multi-team gap RFC, the contribute-back RFC. It is not a feature-store implementation. If the learner's mod-405 output is a Python package, the scope has slid down into the peer platform track and the RFCs went unwritten.

**It is org-scope, not portfolio-scope.** Mod-402 owns the portfolio blueprint — *what these three-to-five current systems should share now*. Mod-405 owns the platform-strategy layer — *what every ML system in the org should be able to consume off-the-shelf, and what my portfolio commits to adopting*. The two overlap heavily, and a healthy org runs them in lock-step; but they answer different questions and produce different artifacts.

**It is a consumer stance, not a producer stance.** Peer platform tracks ([`ai-infra-ml-platform-learning`](https://github.com/ai-infra-curriculum/ai-infra-ml-platform-learning), [`ai-infra-mlops-learning`](https://github.com/ai-infra-curriculum/ai-infra-mlops-learning), [`ai-infra-performance-learning`](https://github.com/ai-infra-curriculum/ai-infra-performance-learning)) own the producer stance — the actual paved-road build. This module owns the consumer stance — the ML org's collective posture toward the paved road. Team Topologies (Skelton & Pais, 2019) names this pair as *platform team* and *stream-aligned team*; the Staff ML engineer in this module is the senior technical voice for the stream-aligned side of the pair.

## Consumer vs. producer, drawn crisply

The most useful framing to keep in your head this whole module is the **consumer / producer split**. Every one of the five components has both sides in play. What the consumer commits to and what the producer commits to is the trade the consumer contract writes down.

| Concern                       | Producer (peer platform team) owns                                   | Consumer (ML org, this module) owns                                        |
| ----------------------------- | -------------------------------------------------------------------- | -------------------------------------------------------------------------- |
| API design                    | The interface itself, versioning, deprecation policy                 | Review time and staff-level feedback on the design                         |
| Implementation                | The code, the operational tooling, the on-call rotation              | Nothing (this is the whole point of a paved road)                          |
| Migration                     | Migration tooling, backward-compat window, migration support         | Committed adoption plan and per-team migration slot                        |
| Roadmap prioritisation        | Ordering across all their consumers                                  | Business-impact and adoption-volume evidence per gap RFC                   |
| Reliability                   | SLO, error budget, incident response                                 | Consumption discipline (retry, backoff, respect quotas)                    |
| Feature depth                 | Depth of feature per the roadmap                                     | The gap RFC when a needed feature is missing                               |
| Ownership when platform can't | Escalation path                                                      | Willingness to fork *or* contribute-back an ML-team-built implementation   |

The line between the two columns is what the consumer contract in chapter 4 makes explicit. When it is not written down, both sides quietly assume the other owns some cell in the middle, and the failure mode you get is either a platform team building things nobody adopts (producer without a real consumer) or an ML org running four homegrown feature stores because the platform "isn't ready" (consumer without a real producer commitment).

## What Staff owns vs. what the peer platform track owns

The staff-plus IC operating at mod-405 owns:

- **The build-vs-adopt call.** For each of the five components in the ML org's near-term horizon, is the org's default to build, adopt-managed, adopt-OSS, or defer? Chapter 3's rubric is the substantive chapter.
- **The consumer contract with the platform team.** What adoption volume, feedback cadence, and staff-level review time does the ML org commit to; what API stability, on-call SLO, and migration support does the platform team commit to in return. Chapter 4.
- **The gap RFC.** When a specific paved-road feature is missing that materially blocks the portfolio, the multi-team RFC that argues for it with business-impact evidence and an adoption commitment. Chapter 5.
- **The contribute-back RFC.** When the ML org has built a thing the platform should own long-term, the RFC that offers it back with the ownership, on-call, and API-stability contract that makes acceptance possible. Chapter 5 (or 6 if the material splits).

The peer platform track owns:

- **The paved-road build itself.** The feature-store service, the model-registry service, the eval platform, the training-cluster reservation API, the inference gateway.
- **The multi-consumer roadmap.** Not just the ML org — also the analytics org, the fraud org, and any other consumer group. The platform team's prioritisation is over all of them; the ML org's gap RFC is one input among several.
- **Operational SLOs.** On-call, error budget, upgrade cadence, deprecation policy.
- **API design ownership.** The platform team's API-review discipline. The ML org influences it via review, not by unilateral change.

The peer specialist tracks (training-pipeline-engineer, model-evaluation-engineer, etc.) own **implementation depth inside a specific component** — writing the actual training-pipeline library, the actual LLM-as-judge grader, the actual model-registry ingest. Their scope is one component's build; mod-405's scope is the org's *stance* on all five components together.

If the mod-405 learner catches themselves writing an inference-gateway config or an MLflow ingest DAG, either (a) they are pair-programming with the peer platform team as part of a hand-off — legitimate, time-box it — or (b) the RFC did not get written and the strategy work is being replaced with build work. Case (b) shows up in staff-level career reviews as "did lots of tactical work; unclear strategic impact." The RFC is the promotion packet.

## The Architect and Right Hand archetype overlap

Mod-401 chapter 2 named four staff-plus IC archetypes — Tech Lead, Architect, Solver, Right Hand (from Larson, *Staff Engineer*, 2021). Mod-405 sits at the overlap of **Architect** and **Right Hand**.

- **Architect,** because build-vs-adopt calls, consumer contracts, and multi-team RFCs are architecture artifacts by any reasonable definition. The mod-402 blueprint is the sibling artifact; this module's RFCs are its downstream siblings.
- **Right Hand,** because the counter-party to every artifact in this module is another leader — the platform team's staff engineer, the platform PM, the platform-team EM, sometimes the ML org's director. The RFC that argues a build-vs-adopt call is fundamentally an executive-adjacent document. Larson's Right Hand chapter and Reilly (*The Staff Engineer's Path*, 2022) chapter on influence apply directly.

If a learner's mod-401 exercise-02 self-placement was Tech Lead or Solver, mod-405 is still required — every staff-plus IC will make at least one platform-strategy call over a two-year period — but the module will feel more like a stretch than the Architect / Right Hand's home turf. That is a feature, not a bug: this module is meant to build the muscle even for learners whose default archetype is elsewhere.

## The "am I doing platform-strategy work?" heuristic

Sibling to the mod-401 and mod-402 heuristics:

> **If my document changes what the platform team commits to build (or not build), and what my ML org commits to adopt (or not adopt), and both commitments are legible enough that either side could hold the other accountable, I am doing platform-strategy work. If it only changes what my team builds, I am doing Senior tech-lead work. If I am building the thing myself instead of writing the contract, I have become the platform team.**

The failure modes to catch in yourself:

- **Drifting down into build.** *"I'll just prototype the feature store and if it works, they can adopt it."* Sometimes this is right — see the contribute-back RFC in chapter 5 — but it must be a **planned** prototype paired with a contribute-back RFC in draft. Unplanned, it is scope drift.
- **Drifting up into vision.** *"In five years every ML system will use a unified evaluation platform."* That is a Principal / mod-410 conversation. Mod-405 stays inside the two-to-four quarter window where the ML org and the platform team can hold each other accountable.
- **Confusing platform-strategy with portfolio.** *"We should merge the three feature pipelines."* That is a mod-402 verdict. Mod-405 asks the different question: *"once we merge them, do we build the shared implementation ourselves, adopt an OSS one, or ask the platform team to build it?"*

## The artifacts this module produces

Four written artifacts, one per exercise, all bearing the Staff engineer's name:

1. **The build-vs-adopt decision doc** (exercise-01, chapter 3). A weighted-rubric call on one of the five components — feature store, model registry, eval platform, training platform, or inference gateway — with team-size, differentiation, TCO, vendor-lock-in, time-to-adoption, contribution-ergonomics, and exit-cost analysis.
2. **The paved-road consumer contract** (exercise-02, chapter 4). What the ML org guarantees the platform team (adoption volume, feedback cadence, staff-level review time) in exchange for what the platform team guarantees back (API stability window, on-call SLO, migration support).
3. **The multi-team gap RFC** (exercise-03, chapter 5). The RFC that asks the platform team to build a specific missing feature, with defended business impact and adoption commitment.
4. **The contribute-back RFC with ownership contract** (exercise-04, chapters 5-6). The RFC that offers an ML-org-built component back to the platform team with a written ownership, on-call, and API-stability contract that makes acceptance a realistic proposition rather than a giveaway with hidden operational load.

The four artifacts assemble into project-401's platform-strategy section alongside the mod-402 blueprint and the mod-407 TCO model.

## Summary

- **Platform strategy at org scope** is the set of build / adopt / contribute-back / defer decisions on the five ML-platform components, negotiated with the peer platform team, and carried in written consumer contracts. It is document work, not build work.
- The consumer / producer split is the load-bearing framing; every one of the module's artifacts writes some part of that split down.
- Staff owns the calls, contracts, and RFCs; the peer platform track owns the paved-road build; the peer specialist tracks own per-component implementation depth.
- The archetype centre is the overlap of **Architect** and **Right Hand**.
- The heuristic: if the document changes what both sides commit to, and both can hold the other accountable, it is platform-strategy work; if you are building the thing yourself, you have become the platform team.

The next chapter walks the five components — feature store, model registry, eval platform, training platform, inference gateway — as the vocabulary every decision in this module reads against.
