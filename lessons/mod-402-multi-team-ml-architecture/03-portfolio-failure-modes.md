# Portfolio-Scope Failure Modes: The Four Senior Engineers Rarely See

## Motivation

Chapter 2 handed you a matrix. The `duplicated` and `forked` cells are where the portfolio actually hurts. This chapter names the four canonical failure modes that appear in those cells at portfolio scope — the ones a Senior tech-lead on one team is unlikely to see because they only ever look at *their* column of the matrix.

Each failure mode is described the same way: a definition, the mechanism that produces it, three diagnostic questions you ask the team owner in the review, an anti-pattern that looks like the failure but is not, and the primary reference for further reading. Chapter 4 uses the same labels to decide merge / split / share.

The canonical reference behind three of the four is Sculley et al., "Hidden Technical Debt in Machine Learning Systems" (NIPS 2015). Read the paper before this module if you have not. This chapter's contribution is not the observations — they are Sculley's — it is that at *portfolio* scope these observations become failures *between teams*, and the fix requires a staff-plus-scope decision that no single team can make.

## Failure mode 1 — Pipeline-jungle across teams

**Definition.** A tangle of feature-engineering and data-preparation pipelines that connect the same upstream sources to two or more team-owned models, with no single team owning the intermediate transformations. Sculley names this *pipeline jungles*; at portfolio scope the jungle is stitched together from three or four teams' local DAGs, and the load-bearing edges are the ones that cross ownership boundaries.

**Mechanism.** Team A ships their ranking pipeline. Team B ships their recommender pipeline six months later. Team B needs 80% of the same transformations as Team A. Because Team A's pipeline is written for Team A's schedule, Team A's SLOs, and Team A's release cadence, Team B forks. A year later, Team A patches a de-duplication bug in impression counts; Team B does not; Team C, who ships email personalization off of both teams' derived tables, silently gets one team's fix and the other team's bug. No one team owns the discrepancy and no one team has the authority to fix it across all three.

**Three diagnostic questions.**

1. *"If the upstream `impression_events_v3` schema had a column renamed tomorrow, which teams would you page, and in what order?"* If any team leaves out another team that consumes the same source, ownership is broken.
2. *"How many pipelines in this portfolio consume the same source and produce a claim about the same business object (e.g. 7-day impression count per user)?"* More than one, and you have a jungle candidate.
3. *"When was the last time any of these pipelines patched a bug in a de-duplication, join, or fill-forward, and did the patch land in every consuming pipeline?"* If the answer is "we don't cross-check", the jungle is silently drifting.

**Anti-pattern that looks like it.** Not every duplicated pipeline is a jungle. A legitimate per-team pipeline exists when the SLO or the transformation semantics genuinely diverge (e.g. Team A needs impressions at 15-minute freshness for near-real-time ranking; Team C needs impressions at 24-hour freshness for a batch model). Two pipelines with genuinely different SLOs are *parallel*, not tangled. The jungle failure requires (a) the same source, (b) the same downstream semantics, and (c) no single owner.

**Primary reference.** Sculley et al., "Hidden Technical Debt in Machine Learning Systems", NIPS 2015, section 4 ("Data Dependencies Cost More than Code Dependencies") — specifically the pipeline-jungle discussion and the accompanying under-utilised-data-dependency taxonomy.

## Failure mode 2 — Duplicated features that drift

**Definition.** Two or more systems compute the same nominal feature — same business meaning, same intended input, same intended freshness — from independent code paths, and the outputs progressively disagree over time. Sculley discusses this under *entanglement* (CACE — Change Anything, Change Everything); at portfolio scope, the entanglement is across team boundaries, so no team's regression suite catches it.

**Mechanism.** Team A defines `user_7d_purchase_count` in its feature repo. Team B, six months later, needs the same signal and is *aware* Team A defines it, but Team A's namespace is not tagged as reusable, Team A's freshness SLO is 15 minutes and Team B needs it at 1 minute, or Team A's feature is `region-filtered` in a way Team B does not want. Team B forks the definition. On day 0 the two definitions agree to within rounding error. On day 90 they no longer agree, because someone patched a bug in one and not the other, or because the two consumed a slightly-different filter of `purchase_events_v2` after that stream was upgraded. Team A's model retrains and does slightly better; Team B's retrains and does slightly worse; neither team files a bug because the drift is silent.

**Three diagnostic questions.**

1. *"Which features are defined by more than one team in this portfolio?"* If the team leads cannot answer without the blueprint matrix, you already know.
2. *"For your top ten features by SHAP importance, does anyone else in the portfolio define a feature with the same business meaning?"* If yes, is the definition byte-identical, or is it a re-implementation?
3. *"When did you last diff a duplicated feature between teams on the same time slice?"* If the answer is "never", the drift is not a hypothesis; it is a certainty. The only question is by how much.

**Anti-pattern that looks like it.** Two features that share a *name* but have genuinely different semantics — e.g. Team A's `user_7d_purchase_count` filters test users out, Team B's does not — are not duplicates. They are two features that should be renamed so future readers do not conflate them. The failure mode is same-semantics-two-implementations, not same-name-two-semantics.

**Primary reference.** Sculley et al., "Hidden Technical Debt in Machine Learning Systems", section 2 ("Complex Models Erode Boundaries") on CACE and entanglement. For the feature-store-side framing, the Hopsworks "Time-travel Feature Store" and Feast "Feature Reuse" documentation cover why point-in-time-correct joins and namespaced discovery are the mechanical fix.

## Failure mode 3 — Model-registry fragmentation

**Definition.** The portfolio has more than one system-of-record for trained-model artefacts, and the systems disagree on which model is in production, what its lineage is, or what the promotion state machine is. In the worst case there is *no* system of record and the answer to "which version is in prod" requires reading a serving-side config file.

**Mechanism.** Team A adopted MLflow in year 1 because it was the tool of the season. Team B adopted a homegrown S3 hierarchy because MLflow "did not support their needs" (usually a real reason at the time, occasionally a rationalisation later). Team C adopted Weights & Biases as part of a broader experiment-tracking migration. The org now has three registries, none of which agree on what fields must be attached to a model version, what "production" means, or who can promote a model. When leadership asks "which version of the fraud model is currently serving traffic and what data was it trained on", the answer takes an hour and involves three different tools.

**Three diagnostic questions.**

1. *"For each system in the portfolio, name the registry, the metadata schema on each model version, and the promotion state machine."* If any team gives you an evasive answer ("it's in git", "the config file has it"), the registry is not a registry.
2. *"How would an incident commander find, in five minutes, the training data snapshot ID of the model currently serving 90% of the traffic on system X?"* If the five-minute path involves paging a team member, the registry is fragmented.
3. *"When leadership asks the org 'how many models are in production and who owns each', how long does it take to produce that list, and who produces it?"* If the answer is "no one produces it routinely", the fragmentation is invisible to leadership.

**Anti-pattern that looks like it.** Two registries where each is scoped to a different *class* of model — one for large foundation-model checkpoints ordered by cluster storage constraints, one for smaller online-serving models — with a documented rule for which class goes where, is **not** fragmentation. It is deliberate specialisation. The failure mode requires (a) more than one registry, (b) unclear rules about which one to use, and (c) inability to answer "which model is in prod" without team-specific knowledge.

**Primary references.** The MLflow Model Registry documentation for the vocabulary of *registered model*, *version*, *stage*, and *alias*. Google's ML Metadata (MLMD) documentation for the lineage-graph model. Neither is prescriptive of what your org must adopt; both give you the vocabulary the RFC readers already know.

## Failure mode 4 — Eval-metric conflicts

**Definition.** Two or more systems in the portfolio evaluate against metrics that leadership cannot compare, and no shared body decides how the conflict is arbitrated. In practice this shows up as A/B experiment fights (team A's launch degraded team B's guardrail), as roadmap-ranking fights (which of the two proposed launches is a bigger win), and as promotion-packet fights (which model actually improved the product).

**Mechanism.** Team A ships ranking against NDCG@10 on a held-out slice. Team B ships recommendation against recall@50 on a different held-out slice. Team C ships personalization against a click-through-rate proxy. All three metrics are individually defensible. When Team A's experiment shows +0.4% NDCG@10 and Team B's shows +1.2% recall@50, leadership cannot rank them. Worse — Team A's launch depresses Team B's recall@50 in the online A/B by 0.3%, and there is no arbitration body. The two teams argue for a week; product picks the louder one; the org learns that eval metrics do not settle arguments.

**Three diagnostic questions.**

1. *"Which offline metrics does each system in the portfolio gate a launch on, and are they comparable across systems?"* If the answer is "not really comparable", you have a conflict candidate; write down whether that non-comparability is *by design* (e.g. two genuinely different tasks) or *by accident*.
2. *"When two teams' online experiments interact — team A's ships and depresses team B's guardrail — who arbitrates?"* If the answer names no body and no policy, you have the conflict failure.
3. *"When leadership ranks the ML roadmap for next quarter, which portfolio-level metric aggregates across systems, and who owns it?"* If no portfolio-level metric exists, roadmap-ranking is a political fight and not a technical one.

**Anti-pattern that looks like it.** Two teams whose *primary* metrics differ *by design* — because they optimise for genuinely different business objects (fraud recall vs. search NDCG) — are not in conflict. They are in a portfolio that requires *rolled-up* metrics leadership can compare, not *identical* metrics teams can compare directly. The failure mode is when there is no rolled-up view *and* no arbitration body *and* team-level metrics interact through shared traffic.

**Primary reference.** Kohavi, Tang, and Xu, *Trustworthy Online Controlled Experiments* (Cambridge University Press, 2020) — chapter on OEC (Overall Evaluation Criterion), chapter on guardrail metrics, and chapter on interference. Mod-406 owns the deeper cross-team eval program; this chapter only names the failure so chapter 4 can decide the substrate consolidation.

## A fifth failure mode worth naming — silent contract erosion

Not one of the four core objectives, but worth flagging because it appears in many real reviews: **the contract was written, the systems drifted, no one re-read the contract**.

A shared substrate — a feature-store namespace, a registry schema, an eval-harness API — was documented eighteen months ago. Since then the teams have added fields, deprecated fields, changed default values, and none of the changes were reflected in the shared contract document. When a new system tries to join the substrate, the documented contract does not match reality; the new system's engineer talks to each team to reconstruct the *actual* contract; no one updates the document because everyone remembers the state of their own team.

**Diagnostic.** Take the substrate contract you find on your wiki. Diff it against reality with one representative team. If the diff is longer than a paragraph, the contract is decorative. Chapter 5 (RFC authoring) treats a decorative contract as an active liability, not a neutral document.

## How the failure modes compose

The four failure modes are not independent — they compose in predictable orders. Two composition patterns you will see repeatedly:

**Pipeline-jungle → feature drift.** Teams that were forced into forked pipelines by pipeline-jungle later fork their feature definitions. Fixing the feature-drift symptom without fixing the pipeline-jungle root cause produces a new drift a quarter later. The blueprint matrix flags this as a *feature-layer* `duplicated` cell above a *data-plane* `duplicated` cell in the same row pair; chapter 4 must decide the data-plane row before the feature row.

**Registry fragmentation → eval-metric conflict.** Systems with fragmented registries have no shared lineage to compare against; when two teams' offline evals disagree, no one can reconstruct *which training data snapshot each model was trained against*, so no one can attribute the disagreement. Fixing eval-metric conflict without fixing registry fragmentation is fixing the symptom.

**Rule of thumb.** When two cells in the matrix are both painful, fix the leftmost one first (data plane before features; registry before eval). The failure modes propagate left-to-right in the matrix, not right-to-left.

## The failure modes you will *not* fix from this seat

Three failure modes look adjacent but are explicitly *not* portfolio-staff scope in this module:

- **Model performance degradation on a single team's system.** L30 tech-lead scope. The staff engineer notices the pattern across systems, not the specific regression on one system.
- **Vendor lock-in on the underlying feature store, registry, or experimentation platform.** ML platform strategy scope — mod-405. Portfolio consolidation may increase or decrease vendor concentration, but the vendor decision is not the portfolio decision.
- **The training pipeline is slow / expensive on a single team's system.** Peer specialist tracks — [`training-pipeline-engineer`](https://github.com/ml-engineering-curriculum/training-pipeline-engineer) and the [`ai-infra-performance-learning`](https://github.com/ai-infra-curriculum/ai-infra-performance-learning) — own single-system training performance. Multi-team distributed-training strategy shows up in mod-404.

Confusing any of these with a portfolio failure is a fast way to lose credibility in chapter 6's architecture review. The reviewers will point out that the *scope* is wrong before they engage the technical merits.

## Summary

- Four portfolio-scope failure modes recur across ML orgs. Each maps to `duplicated` or `forked` cells in the chapter-2 matrix:
    - **Pipeline-jungle** — tangled feature-prep pipelines across teams; no single owner of intermediate transforms.
    - **Duplicated-feature drift** — same-semantics features re-implemented per team; drift silently over time.
    - **Model-registry fragmentation** — more than one system-of-record for trained models; "which version is in prod" is a multi-hour question.
    - **Eval-metric conflicts** — team-level metrics that leadership cannot compare; no arbitration body when experiments interact.
- The failure modes compose left-to-right in the matrix — pipeline-jungle propagates into feature drift, registry fragmentation propagates into eval conflict. Fix the leftmost cell first.
- Single-system performance regressions, vendor lock-in, and single-system training slowness are *not* portfolio failures. They belong to L30 tech-lead, mod-405, or peer specialist scope.
- The canonical reference is Sculley et al. (NIPS 2015); the eval reference is Kohavi, Tang, and Xu (Cambridge, 2020). Read both before authoring an RFC in chapter 5.

The next chapter turns each `duplicated` and `forked` cell into a decision — merge, split, or share behind a contract — and quantifies the migration cost and the political cost of each choice.
