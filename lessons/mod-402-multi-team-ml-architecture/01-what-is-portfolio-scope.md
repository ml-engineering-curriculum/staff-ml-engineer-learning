# From One System to a Portfolio: What Multi-Team ML Architecture Actually Means

## Motivation

The Senior tech-lead who owned "the ranking system" and is now Staff over "ranking, retrieval, and personalization" is usually surprised by how quickly the familiar habits stop working. The instinct that took them to Senior — sit next to the model, understand every feature, land the change themselves — becomes actively harmful at portfolio scope. There is too much to sit next to. The habits that made the promotion happen make the next set of decisions worse.

This chapter names what "multi-team ML architecture" is as a **scope of work**, in enough detail that the rest of the module — the blueprint template in chapter 2, the failure-mode catalogue in chapter 3, the merge/split/share decision in chapter 4, the RFC in chapter 5, and the review in chapter 6 — has a stable definition to build on. It re-uses the L30 → L40 altitude framing from [`mod-401`](../mod-401-staff-ml-role-scope/) and specialises it to *the Architect archetype's core artifact* — the portfolio blueprint.

## What "portfolio" means here

A **portfolio of ML systems** in this module means **three to five production ML systems, each owned by its own team, that share enough substrate that a decision about one system materially affects the others**. Two representative shapes:

- A consumer-product ML org: a **search ranking** system, a **home-feed recommender**, an **email-and-push personalization** system, and an **ads relevance** model. Each has its own team; each pulls the same behavioural events; each computes overlapping user embeddings; each ships eval against user-facing engagement.
- A B2B / marketplace ML org: a **fraud** model, a **credit / underwriting** model, a **document classification** model, and a **support-ticket routing** model. Each has its own team; each depends on the same customer graph; each is regulated on a related axis; each ships eval against different labelled review data.

Three to five is not magic; it is the range that shows up in real staff postings and where consolidation decisions genuinely matter. Below three, the systems can usually be reasoned about pairwise and no portfolio blueprint is needed. Above five or six, you are actually authoring a *platform* — mod-405 territory — and the artifact is different.

The portfolio is *not*:

- **The whole ML organisation.** That is director / VP / Principal scope.
- **One team's system decomposed into services.** That is Senior tech-lead architecture and belongs to the L30 track.
- **A generic platform for future ML systems.** That is ML platform strategy — mod-405. The portfolio blueprint answers *what the current three-to-five systems should share now*, not *what future ML systems will need*.

## Why single-system framing breaks at three-to-five systems

Every failure mode in this module — pipeline-jungle, duplicated features that drift, model-registry fragmentation, eval-metric conflicts (chapter 3) — has the same root cause: the systems were designed *serially*, each by a Senior tech lead making locally correct decisions, and no one held the whole picture. The classic reference is Sculley et al., "Hidden Technical Debt in Machine Learning Systems" (NIPS 2015), whose central observation is that in production ML **only a tiny fraction of the code is the model itself**; the rest is the surrounding data plumbing, monitoring, configuration, and glue. Every one of those surrounding boxes is a place where two ML systems inside the same org will independently reinvent the same shape.

Three concrete illustrations of the break:

**1. Two teams recompute the same feature and it drifts.** Team A's ranking model needs `user_7d_purchase_count`. Team B's recommender needs the same signal, spelled slightly differently — `user_purchase_count_7d`. Both are correct on the day they ship. Six months later Team A silently patches a bug in the join and Team B does not. Both models now train on subtly different `user_7d_purchase_count` and neither team notices; the ranking team's offline eval improves while the recommender's degrades. No single team owns the definition, so no single team owns the drift.

**2. Two teams pick different model-registry conventions.** Team A stores checkpoints in MLflow with semantic-version tags. Team B stores them in a homegrown S3 hierarchy keyed by commit SHA. Neither convention is wrong; the failure is that the org has *two* registries. When leadership asks "which model version is serving fraud traffic right now", the answer requires reading two different documents. When a shared inference-serving team tries to build a canary rollout tool, it has to support both. Sculley calls this **CACE — Change Anything, Change Everything** — dragged to the org level: changing one team's registry format makes every downstream tool re-plumb.

**3. Two teams' offline metrics disagree, and the exec cannot pick.** Team A ships against NDCG@10 on their held-out slice. Team B ships against a recall-at-k on a different held-out slice. Both metrics are defensible. When the director asks *"which of the two experiments should we ship next quarter"*, there is no legitimate way to compare. Chapter 3 calls this the **eval-metric conflict** failure mode; mod-406 owns the systemic fix.

The pattern is the same in each case. At L30 the Senior tech-lead's job was to make one system's boxes fit together well. At L40 the Staff engineer's job is to see the **implicit contract between systems** that no L30 was hired to see.

## The four substrates you will blueprint

The blueprint in chapter 2 walks the portfolio across four substrate layers. Naming them up front so this chapter can end with a stable outline:

- **Shared data planes** — the feature-and-events pipelines under all three-to-five systems (batch and streaming). This is where pipeline-jungle grows.
- **Shared feature layer** — the offline feature store and its online serving contract. This is where duplicated features drift.
- **Shared model-registry contract** — the format and metadata every system's checkpoints agree on. This is where registry fragmentation shows up.
- **Shared eval contract** — the offline eval harness and the online experimentation contract. This is where metric conflicts arise; the deeper cross-team eval program is mod-406.

These are the four layers where consolidation, sharing, or splitting is a *staff-scope* decision. Other things do get shared across teams (Docker images, model-serving base classes, feature-flag systems), but they are usually L30 or platform-team decisions; they are not what the staff-plus IC gets paid to arbitrate.

## What the portfolio blueprint is as an artifact

The **portfolio blueprint** is the primary artifact this module produces. Concretely it is a single document (typically a page of diagram plus five-to-ten pages of text) that answers, for the three-to-five systems in scope:

1. **What each system is** — a one-paragraph description per system: business object, upstream data sources, downstream consumers, on-call team.
2. **What each system's per-layer substrate is today** — for each of the four layers (data planes, features, registry, eval), state what each system uses. Name the ownership boundary.
3. **What is shared, what is duplicated, what is forked** — at each layer, mark the substrate as `shared`, `duplicated`, `forked`, or `unique-by-design`. The nuance between the middle two is the point of chapter 3.
4. **Where the coupling hurts** — the two-to-five substrate cells where cross-system coupling is materially painful today (drift, registry mismatch, eval fight).
5. **The merge / split / share proposal** — for each painful cell, the shape of the fix (chapter 4).
6. **The migration and political cost** — what does it cost to get from the current state to the proposal, in engineering weeks *and* in team politics (chapter 5).

Exercise 01 walks the learner through step 1-3. Exercise 02 drills step 3-4. Exercise 03 covers step 5. Exercise 04 rolls it all up into a defensible multi-team RFC.

## What Staff owns vs. what the platform team owns

A common misfire when a newly-Staff engineer sees pipeline-jungle across four teams is to *build a fifth service that unifies them all* — that is, to become the platform team. Two reasons that is usually wrong.

**First, the peer platform tracks exist.** [`ai-infra-ml-platform-learning`](https://github.com/ai-infra-curriculum/ai-infra-ml-platform-learning), [`ai-infra-mlops-learning`](https://github.com/ai-infra-curriculum/ai-infra-mlops-learning), and [`ai-infra-performance-learning`](https://github.com/ai-infra-curriculum/ai-infra-performance-learning) are the tracks that own paved-road *build* — the actual feature store, the actual model registry, the actual training platform. A staff ML engineer who quietly builds a fifth service is now competing with the platform team's roadmap and has taken on operational load that is neither their weekly job nor covered by their promotion packet.

**Second, the portfolio decision is different from the platform decision.** The platform team's mandate is *"what should every future ML team be able to consume off-the-shelf"*. The portfolio staff engineer's mandate is *"what should these three-to-five current systems share, given what they actually look like today"*. The two answers overlap heavily in a healthy org and diverge in an unhealthy one. Mod-405 covers the platform-strategy conversation. This module stays on the portfolio side.

The right shape is almost always **decide the portfolio contract, then negotiate whether the platform team ships it, adopts an OSS implementation, or explicitly defers to a per-team implementation**. Chapter 5's RFC template makes that negotiation explicit.

## The Architect archetype's centre of gravity

Mod-401 chapter 2 named four staff-plus IC archetypes — Tech Lead, Architect, Solver, Right Hand (from Larson, *Staff Engineer*, 2021). Multi-team ML architecture is the **Architect** archetype's centre of gravity. The signature artifact is the portfolio blueprint; the signature failure mode is *the beautiful blueprint no team adopted* — technically pristine, operationally unmarketable. Chapter 6's architecture review, and chapter 5's political-cost defence, are the fixes for that failure.

If your Cut 2 from mod-401 exercise-02 was Architect, this module is your target artifact. If your Cut 2 was Tech Lead or Right Hand, this module still matters — every Staff ML engineer authors at least one portfolio blueprint over a two-year period — but you will lean more on the RFC and review chapters than on the blueprint template itself.

## The "am I doing portfolio work?" heuristic

A one-sentence sibling to the mod-401 heuristic:

> **If my proposal changes what two or more teams do next quarter, and defends why it is worth the coordination cost, I am doing portfolio work. If it only changes what my former team does, I am doing Senior tech-lead work.**

The coordination cost — the RFC drafts, the review meeting, the migration plan, the political trade — is not overhead. It *is* the work. A staff engineer who resents coordination cost is either mis-scoped or in the wrong archetype.

## Summary

- A **portfolio** here is three-to-five production ML systems, each with its own team, sharing enough substrate that decisions on one system affect the others.
- Portfolio-scope failures — pipeline-jungle, feature drift, registry fragmentation, eval conflict — appear because no L30 was hired to see the implicit contract between systems. That contract is what staff owns.
- The blueprint walks four substrate layers: **data planes, feature layer, model registry, eval contract**. The remaining chapters drill each layer.
- The portfolio staff engineer does not become the platform team. They decide the portfolio contract and negotiate implementation with the peer platform team.
- The **Architect** archetype's centre of gravity is this module. The signature failure is a technically-clean blueprint no team adopted.

The next chapter walks the four-layer blueprint template and shows a worked example on a plausible three-system portfolio.
