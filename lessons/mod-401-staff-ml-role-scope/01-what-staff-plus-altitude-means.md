# What "Staff-Plus" Altitude Actually Means

## Motivation

The most common failure mode when a Senior ML Engineer is promoted to Staff is *doing the same job harder*: shipping more models, reviewing more pull requests, writing more RFCs for the same team. That work is valuable, but it is still Senior-tech-lead work. It does not clear the Staff bar and it will not get the engineer promoted to Principal.

To clear the Staff bar on an ML engineering ladder, the **object of the verb has to change**. The Senior owns *the ranking model*; the Staff owns *the ranking, retrieval, and personalization portfolio*. The Senior owns *the eval harness for the fraud team*; the Staff owns *the org-wide eval contract that every ML team ships against*. The Senior is judged on *one team's outcome this quarter*; the Staff is judged on *what the ML org is capable of shipping two quarters from now*.

This chapter names that altitude shift in enough detail that the rest of the module — archetypes, verbs, hand-off contracts, self-assessment — has a stable reference point.

## Where Staff sits on the ladder

The AI Career Curriculum uses a numeric level ladder that lines up with common industry frameworks:

| Level | Role (ML ladder) | Scope object | Time horizon |
|---|---|---|---|
| 20 | ML Engineer | *A component* — a feature pipeline, a model, an endpoint | Sprint to quarter |
| 30 | Senior ML Engineer | *A team's system* — the whole ranking system, the whole fraud stack | Quarter to two quarters |
| **40** | **Staff ML Engineer (this track)** | *A portfolio, a program, or a platform contract across teams* | Two to four quarters |
| 50 | Principal ML Engineer | *An org's technical direction*, external influence | One to three years |

The peer level-40 role on the same rung is **Engineering Manager, ML** — the people-leadership path — owned by [`ai-infra-team-lead-learning`](https://github.com/ai-infra-curriculum/ai-infra-team-lead-learning). It is the *same altitude*, not a lower or higher one; it is a *different discipline*. Chapter 5 sets the partnership contract between the two.

Two framings of this ladder are widely-cited in industry and worth reading directly:

- Will Larson, *Staff Engineer: Leadership beyond the management track* (Stripe Press, 2021) — the canonical staff-plus IC ladder text. The four archetypes in the next chapter come from here.
- [Engineering Ladders](https://www.engineeringladders.com/) and [progression.fyi](https://www.progression.fyi/) — collected public engineering-career frameworks. Both cross-check the L30 → L40 shift described below.

## The three altitude shifts that define L40

Three things change simultaneously when the engineer crosses from Senior tech-lead altitude to Staff altitude. All three must show up in the artifacts the engineer produces. If only one or two show up, the bar is not yet cleared.

### 1. The object of the verb widens from "one system" to "a portfolio / program / platform contract"

At Senior altitude the verbs act on **one system that one team ships**:

> *"Owned the ranking model for the marketplace search team."*
> *"Authored the RFC for the fraud detection retraining pipeline."*
> *"Led the incident response for the recommender's latency regression."*

At Staff altitude the verbs act on **a set of systems, a program, or a contract that binds teams together**:

> *"Architected the shared retrieval, ranking, and personalization substrate across the three surfaces teams."*
> *"Sponsored the org-scope build-vs-adopt call on the eval platform and authored the consumer contract with the ML platform team."*
> *"Set the multi-quarter modeling roadmap that the ranking, retrieval, and personalization teams committed to."*

This is the change the rest of the module drills on. It is the single most reliable signal in a job posting (chapter 3), it maps directly onto the four staff archetypes (chapter 2), and it is what the hand-off contracts (chapter 4) protect.

### 2. The time horizon lengthens from "this quarter" to "two to four quarters"

The Senior ML Engineer is judged on *did the model ship and did the metric move this quarter*. The Staff ML Engineer is judged on *is the ML org capable of shipping what leadership committed to two quarters from now*, and *did the direction the engineer set two quarters ago land*. Consequences of a Staff decision — pick this parallelism recipe, adopt this eval platform, invest in this pretraining program — often do not arrive until well after the engineer has moved on to the next portfolio decision.

This forces a different working style. The Staff engineer writes down decisions in enough detail that they can be defended (or falsified) six months later. They read the last two quarters of RFCs and postmortems before deciding anything expensive. They keep a *roadmap document* rather than a *sprint board* as their primary artifact.

### 3. The influence surface widens from "my team" to "peer teams + peer platform teams + peer EM + executives"

Senior tech-leads mostly write, review, and mentor inside one team, with occasional cross-team contact. Staff engineers spend a material fraction of their week outside their home team:

- Peer ML teams, negotiating what the portfolio does and does not share.
- Peer platform teams, negotiating what the paved road covers.
- The peer L40 EM, splitting technical direction from headcount and career.
- Executives (director, VP, sometimes CTO), translating the ML program into a form leadership can commit to.

The Staff engineer does not *own* any of these people or teams. The Staff engineer *influences* them by writing artifacts they can act on — RFCs, roadmaps, one-pagers, hand-off contracts, hiring calibrations — and by lending their attention where it moves the org furthest.

## What Staff owns end-to-end (the L40 mandate)

Concretely, at ML-engineering L40 the Staff IC owns end-to-end the following classes of artifact. Every subsequent module in this track builds one of them:

- **Multi-team ML architecture** — the portfolio blueprint that decides which systems merge, which split, which share substrates. Owned in mod-402; capstoned in project-401.
- **Foundation-model / large-scale fine-tune training programs** — scope, roadmap, data mixture, ablation sequencing, exec framing. Owned in mod-403; capstoned in project-402.
- **Distributed-training strategy** — parallelism recipe, MFU defence, failure-mode budgeting, cluster-hour reserve — as *strategy*, not as *implementation* (implementation is owned by the training-pipeline / performance peer specialists). Owned in mod-404.
- **ML platform strategy at org scope** — build-vs-adopt, contribute-back, consumer contract with the platform teams. Owned in mod-405.
- **Cross-team evaluation and experimentation program** — the org-standard offline eval harness, the LLM-as-judge policy, the shadow / canary / progressive-rollout contract, the experiment-review body. Owned in mod-406.
- **GPU capacity, cost, and TCO** — a portfolio-level demand forecast, capacity mix, FinOps discipline, a TCO model a CFO can read. Owned in mod-407.
- **Portfolio-scope reliability, SLOs, and multi-team incident command** — the ML SLI/SLO catalogue, the ML incident severity matrix, the org-standard postmortem. Owned in mod-408.
- **Responsible AI at portfolio scope** — the AI-risk register (NIST AI RMF + Gen-AI profile), EU AI Act classification of each system, model-card and datasheet standards, pre-launch gate. Owned in mod-409.
- **Multi-quarter roadmap, exec comms, hiring calibration, senior-engineer coaching** — the staff-plus technical leadership package. Owned in mod-410; capstoned in project-403.

Everything above is *the same list a real Staff ML Engineer posting reads against*. mod-401 is the introduction that names the altitude, the archetypes, and the hand-off contracts so that the rest of the track is not just a heap of topics.

## What Staff does not own (defer-up, defer-down, defer-sideways)

Equally important: three classes of work sit next to Staff scope and are explicitly *not* owned by the Staff ML IC. Confusing them is a recurring cause of stuck-Staff promotions.

**Defer down to Senior (L30).** Single-team tech-lead work — running one team's design reviews, mentoring one team's mid-level engineers, authoring one system's model card — is Senior scope. A Staff engineer who spends the majority of their week on this is not clearing the Staff bar; they are backfilling a Senior gap. Fix the gap by hiring or growing the Senior, not by holding the work. This is why [`senior-ml-engineer-learning`](https://github.com/ml-engineering-curriculum/senior-ml-engineer-learning) is a **prerequisite** for this track and is not re-taught here.

**Defer up to Principal (L50).** Org- and industry-wide technical strategy — a three-year ML strategy for the whole company, keynote-grade external influence, industry-standard authorship — is Principal scope. The Staff engineer stops at *multi-team scope inside the ML org* and at *exec-audience communication inside the company*. A Staff engineer who tries to hold Principal scope while carrying full Staff scope is a fast route to burnout and to the org-scope work landing badly.

**Defer sideways to peer tracks.** Three classes of peer track live at the same or adjacent altitude:

- **Peer specialist tracks** — `training-pipeline-engineer`, `fine-tuning-engineer`, `model-evaluation-engineer`, `ai-eval-engineer`, `nlp-engineer`, `rag-engineer`, `llm-application-developer`, `applied-ai-engineer`. These own IC depth inside a single domain. The Staff ML engineer owns the *program that coordinates them*.
- **Peer platform tracks** — `ai-infra-ml-platform-learning`, `ai-infra-mlops-learning`, `ai-infra-performance-learning`. These own the paved-road *build*. The Staff ML engineer owns the *consumer contract*.
- **Peer level-40 EM** — `ai-infra-team-lead-learning`. Direct people management (1:1s, performance reviews, headcount) is EM scope. The Staff ML engineer owns the technical bar and the technical direction.

Chapter 4 spells out the hand-off contract to peer specialists and peer platform teams. Chapter 5 spells out the partnership contract with the peer L40 EM.

## The "am I doing Staff work?" heuristic

A one-sentence heuristic to keep close for the rest of the track:

> **If the artifact I produced this week would still be valid after every specific person on my team left, I probably did Staff work. If it would evaporate the moment I stopped chasing it in the daily stand-up, I probably did Senior tech-lead work.**

Staff work leaves *institutional artifacts* — architecture RFCs, roadmaps, contracts, calibrations, playbooks — that the org keeps using without the engineer's continuous attention. Senior tech-lead work leaves *shipped features and mentored engineers*, which is essential but is L30 scope.

Both are valuable. Only one clears the L40 bar.

## Summary

- L40 Staff ML altitude is defined by three simultaneous shifts from L30 Senior: the object of the verb widens from one system to a portfolio/program/platform contract; the time horizon lengthens from one quarter to two-to-four quarters; the influence surface widens from one team to peer teams, peer platform teams, the peer EM, and executives.
- The Staff ML mandate is a specific set of artifacts — multi-team architecture, training programs, platform-consumer contracts, eval and experimentation programs, capacity/TCO, portfolio reliability, portfolio-scope responsible-AI, and the multi-quarter roadmap. The rest of this track authors each one.
- Staff scope defers *down* to Senior (single-team tech-lead work), *up* to Principal (org-wide and external strategy), and *sideways* to peer specialist tracks (IC depth), peer platform tracks (paved-road build), and the peer L40 EM (people leadership).
- The self-test: does the artifact I produced this week outlive my continuous attention?

The next chapter names the four staff-plus IC archetypes — Tech Lead, Architect, Solver, Right Hand — and shows how each of them expresses this altitude in ML-flavoured terms.
