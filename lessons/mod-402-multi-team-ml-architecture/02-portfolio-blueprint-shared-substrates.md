# The Portfolio Blueprint: Four Substrates and How to Draw Them

## Motivation

Chapter 1 named the artifact: a **portfolio blueprint** across three-to-five production ML systems. This chapter shows *how to draw it*. Learners arrive at Staff having drawn plenty of single-system diagrams — a data-flow diagram, a model-serving stack, a feature-pipeline DAG. The blueprint is not any of those; it is a **portfolio × substrate matrix** whose cells are the actual shared or divergent implementations, and whose colour code names which cells are painful.

The rest of the module depends on this matrix. Chapter 3's failure catalogue reads columns of the matrix. Chapter 4's merge/split/share decision proposes edits to specific cells. Chapter 5's RFC defends the edits. Chapter 6's review walks the matrix in the meeting.

## The four substrate layers, precisely

The blueprint walks the portfolio across the four substrate layers named in chapter 1. This chapter names each layer precisely enough that "shared" and "duplicated" mean the same thing across the review meeting.

### 1. Shared data planes

**Definition.** The batch and streaming pipelines that pull raw events, product-catalogue snapshots, user-attribute snapshots, and label sources into the offline analytics warehouse and the online feature-serving store.

**What to record per system.** For each of the three-to-five systems, list:

- Source event streams consumed (e.g. `impression_events_v3`, `purchase_events_v2`).
- Batch snapshots consumed (e.g. `user_dim_daily`, `product_catalog_hourly`).
- The compute substrate the pipeline runs on (Spark, Beam, Flink, a homegrown DAG framework).
- The scheduler (Airflow, Prefect, Dagster, Argo Workflows, a homegrown CRON).
- The output landing zone (warehouse table, feature store, object store).
- Owner team and on-call rotation.

**Prior art worth reading before drawing this row.** Google's original TFX paper (Baylor et al., "TFX: A TensorFlow-Based Production-Scale Machine Learning Platform", KDD 2017) is the canonical write-up of what production ML pipelines look like as a *component graph*. Uber's Michelangelo posts describe the same shape at Uber scale. The point is not to copy their diagrams; it is to have the same vocabulary as the RFC readers.

**What "shared" means at this layer.** The rows are shared when two systems consume the *same event stream at the same freshness* through the *same materialised table or Kafka topic*. Two systems each independently subscribing to the same raw stream and each computing their own filtered projection are **not** shared; they are **parallel**. That distinction is what makes the failure-mode diagnosis in chapter 3 precise.

### 2. Shared feature layer

**Definition.** The offline feature repository and its online serving surface. This is the layer where features are *named*, *materialised*, and *joined into training data*.

**What to record per system.** For each of the three-to-five systems, list:

- The feature-store implementation: managed (Vertex AI Feature Store, SageMaker Feature Store, Databricks Feature Store), open-source (Feast, Tecton), or homegrown.
- The count of features consumed, split into (a) features the team defines and materialises itself, (b) features consumed from another team's namespace, (c) features consumed from a shared platform namespace.
- The online-serving latency budget (p99) the system requires.
- The point-in-time-correctness guarantee the training-time join provides (this is the property Feast, Tecton, Hopsworks, and every production feature-store paper stress; see Hopsworks' "Time-travel Feature Store" write-up for the canonical framing).

**Prior art worth reading before drawing this row.** Uber's Michelangelo Palette blog posts, Airbnb's Zipline talks (Spark+AI Summit), the Feast documentation, and the Tecton "What is a Feature Store" material converge on a short list of properties every production feature store must provide: point-in-time-correct joins, offline-online parity, feature discoverability, and a lineage record. The blueprint's job is to record whether each system has those properties and whether the implementation is shared or forked.

**What "shared" means at this layer.** The feature layer is shared between two systems when both systems (a) read from the same namespace with the same feature definitions, (b) rely on the same point-in-time-join implementation for training data, and (c) share the same online-serving fetch path. Anything less than all three is a **partial share** and belongs on the diagnostic list in chapter 3.

### 3. Shared model-registry contract

**Definition.** The system-of-record for trained model artefacts, their metadata, their promotion state (development / staging / production / archived), and their lineage back to training data.

**What to record per system.** For each of the three-to-five systems, list:

- The registry implementation: MLflow, SageMaker Model Registry, Vertex AI Model Registry, Weights & Biases Model Registry, a homegrown S3 hierarchy, or a Git-LFS repo.
- The metadata schema attached to each model: at minimum training run ID, training data snapshot ID, evaluation report ID, hyperparameters, git SHA, and framework version.
- The promotion state machine (who can promote a model to `production`, how it is triggered, what audit trail it leaves).
- The serving-side contract that reads from the registry (which inference service pulls which registry entry, how versioning is expressed to callers).

**Prior art worth reading before drawing this row.** The MLflow Model Registry documentation is the shortest precise reference for the primitives — registered model, model version, stage, alias — that every registry system, homegrown or vendor, ends up needing. Google's *ML Metadata* and TFX's `MetadataStore` cover the lineage-graph side. Neither of those documents *is* the contract; they are the vocabulary the RFC readers already know.

**What "shared" means at this layer.** The registry contract is shared between two systems when both write into the same registry backend with the same metadata schema and the same promotion state machine. Two systems that both happen to use MLflow but attach different metadata are *not* sharing a registry contract; they are colocating storage. That distinction becomes the argument in chapter 4.

### 4. Shared eval contract

**Definition.** The set of offline evaluation harnesses each system runs before shipping a new model, plus the online experimentation contract (shadow, canary, A/B) each system uses to expose that model to traffic.

**What to record per system.** For each of the three-to-five systems, list:

- The offline harness: what dataset slices it evaluates on, what metrics it reports, whether the harness is reproducible from a checkpoint and a data snapshot ID.
- Whether the system uses an **LLM-as-judge** eval component and, if so, which judge model and whose team owns the judge prompt. (Mod-406 owns the deeper LLM-as-judge policy conversation.)
- The online rollout contract: is there a shadow stage, a canary stage, an A/B stage? What is the guardrail metric set? Who owns the rollout tooling?
- The stopping-rule / early-halt policy on live experiments.

**Prior art worth reading before drawing this row.** Kohavi, Tang, and Xu, *Trustworthy Online Controlled Experiments* (Cambridge, 2020) is the canonical text on running A/B experiments at scale — how randomisation is validated, what metrics are trustworthy, how false positives are controlled. Microsoft's ExP platform posts and Netflix's XP platform posts describe the same primitives in practice. The blueprint records which of these primitives each system in the portfolio actually has today.

**What "shared" means at this layer.** The eval contract is shared between two systems when both (a) evaluate on comparable dataset slices with comparable metrics that the org's leadership can compare directly, and (b) roll out through the same experimentation platform with the same guardrail-metric conventions. Anything less produces the eval-metric-conflict failure mode of chapter 3.

## The portfolio × substrate matrix

The blueprint's centrepiece is a single matrix whose rows are the three-to-five systems in the portfolio and whose columns are the four substrate layers above. Each cell records the concrete implementation and a colour code.

A minimal template:

```
                    | Data plane            | Feature layer         | Model registry        | Eval contract
--------------------|-----------------------|-----------------------|-----------------------|-----------------------
Search ranking      | Spark + Airflow       | Feast (own namespace) | MLflow (own project)  | Own harness, own A/B
                    | consumes eng_events   | 320 features          | promotion by TL       | metrics: NDCG@10
--------------------|-----------------------|-----------------------|-----------------------|-----------------------
Home-feed recommend | Spark + Airflow       | Feast (own namespace) | MLflow (own project)  | Own harness, own A/B
                    | consumes eng_events   | 280 features, 140     | promotion by TL       | metrics: recall@50
                    |                       | duplicate Search team |                       |
--------------------|-----------------------|-----------------------|-----------------------|-----------------------
Personalization     | Beam + Argo           | Homegrown feature svc | Homegrown S3 registry | Own harness, no shadow
(email/push)        | consumes eng_events   | 120 features          | promotion by SRE      | metrics: CTR proxy
                    | (own filtered stream) |                       |                       |
```

Each cell is then tagged with one of the four colour codes below.

### Cell colour codes

Every cell falls into one of four labels; they are what chapter 3's failure catalogue and chapter 4's decision framework read against.

- **`shared` — the substrate is shared with at least one other system in the portfolio and the share is providing value.** No action required from this module.
- **`duplicated` — the substrate is nominally independent but is doing the same job as another cell, and the two implementations have drifted or will drift.** This is the highest-frequency portfolio failure. Chapter 3 diagnoses it, chapter 4 decides whether to merge.
- **`forked` — the substrate was once shared and was deliberately forked, but the fork is no longer justified.** Chapter 4 decides whether to un-fork (merge) or make the fork explicit (split).
- **`unique-by-design` — the substrate is unique to this system for a defensible reason** (regulated data plane, hard latency budget, foreign-region data-residency, and so on). No action required, but the *reason* is recorded so the next person to look at the matrix does not try to consolidate it.

Two anti-labels the blueprint deliberately avoids: `TBD` and `mixed`. Anything you would want to call `mixed` decomposes into per-property labels — `feature-definitions:shared, online-serving:forked` — and belongs in the notes column of the row. `TBD` means the blueprint author has not finished the interview yet; the review meeting should not accept a matrix with `TBD` cells.

## Cross-system dependency diagram

The matrix records *state*. A companion diagram records *dependency* — which system's output feeds which other system's input. This is the diagram that reveals the pipeline-jungle failure mode of chapter 3.

The diagram has two node types:

- **System nodes** — one per system in the portfolio (rectangle).
- **Substrate nodes** — one per shared or nominally-shared substrate cell (oval): "impression events stream", "user embeddings feature-store namespace", "MLflow instance", "eval harness v3".

The edges are labelled with the direction: `writes`, `reads`, `co-owns`. The diagram becomes actionable when you can point at any substrate node and answer, in one sentence, who owns it and who is on the hook when it breaks. If more than one substrate node has an ambiguous owner, the portfolio has a *governance* problem and no amount of technical consolidation will fix it.

An ASCII sketch — the real diagram is a graphviz or Excalidraw file:

```
[Search ranking team]---writes--->(impression_events_v3 stream)<---reads---[Home-feed team]
        |                                                                          |
        writes                                                                   writes
        |                                                                          |
        v                                                                          v
(feature-store: search_ns)                                             (feature-store: feed_ns)
   |                                                                          |
   contains 140 features                                                      contains those same
   with the same semantics as ---duplicated--->                               140 features
   feed_ns                                                                    (drift starts here)
```

The interesting decisions in chapter 4 happen where the diagram shows a substrate node with more than one writer, or a duplicated pair whose owners cannot name each other in the review meeting.

## Interviews to run before the matrix is drawable

The matrix is not sittable-and-typable. It requires a short round of interviews so the labels are grounded in what the teams *actually* do rather than in what the last team-lead-off-site claimed. Budget one 45-minute interview per team, plus a 90-minute writeup afterwards. The interview questions:

1. **Which upstream events / snapshots does your training pipeline consume?** Ask for concrete table names or topic names.
2. **Where do your training features live?** — feature-store, warehouse, S3, homegrown.
3. **Which feature namespaces are your team's, and which are you consuming from another team?** Ask specifically about the top ten features by importance.
4. **Where do your model checkpoints live and how do you know which version is in production?** Ask specifically for a link to the "current prod model" record.
5. **Which offline metrics do you gate a launch on?** Ask which slices and thresholds are the deal-breakers.
6. **Which online experimentation platform do you ship through?** Ask for the guardrail metric set and the stopping-rule.
7. **If the shared upstream stream you consume were renamed tomorrow, who would you page?** This is the question that reveals whether the ownership boundary is real or nominal.

Learners often want to skip step 1 because they "already know" — they were on the team six months ago. Do not skip it; do not trust memory across a six-month gap.

## A worked example — three-system consumer portfolio

Concretely, a plausible portfolio blueprint for a consumer product with three ML systems (search ranking, home-feed recommender, email personalization) and 3–5 teams:

- **Data plane.** All three consume the same `impression_events_v3` and `purchase_events_v2` streams. The Search and Home-feed teams read them through the same Spark job maintained by the Data Platform team (`shared`). The Personalization team reads them through their own Beam pipeline that applies a bounce-filter — labelled `duplicated` because the filter logic is a slightly-different version of the same de-duplication the Spark job already does.
- **Feature layer.** Search and Home-feed both use Feast against a shared feature-store cluster but with separate namespaces. 140 features overlap in semantics but have different names — labelled `duplicated`. Personalization uses a homegrown feature-fetching service because the online latency budget for email personalization was higher than the online path Feast targeted at ship time — labelled `forked` (was originally shared).
- **Model registry.** Search and Home-feed both use MLflow, but in different projects with different metadata schemas — labelled `duplicated`. Personalization writes to an S3 hierarchy — labelled `forked` (predates the MLflow decision by a year).
- **Eval contract.** Each system has its own harness and its own online experimentation setup; metrics are not comparable across systems — labelled `duplicated` on the online side (they all use the same A/B platform but with team-specific guardrail sets) and `unique-by-design` on the offline side for now (mod-406 revisits this).

That blueprint is the input to chapter 3 (diagnose the `duplicated` and `forked` cells) and chapter 4 (decide, for each painful cell, whether to merge, split, or share behind a contract).

## What the blueprint is *not*

Three things the blueprint deliberately does not do — each is a mode where authors reliably burn a week and produce a document no one adopts:

- **It is not a target-state diagram.** The blueprint records the *current* state honestly and the *proposed* delta explicitly. Skipping the current-state audit produces a target-state document that no team recognises and no team adopts.
- **It is not a platform roadmap.** The blueprint proposes what these three-to-five systems should share now. A platform roadmap proposes what every future ML system should consume off-the-shelf. Mod-405 covers the platform roadmap; do not conflate the two.
- **It is not a code review of any individual team's implementation.** The blueprint records *what* each team implements at each layer, not *how well*. Team-internal code quality is L30 scope. Chapter 6's review actively defends this line — a Staff engineer who lets the review turn into "here is what is wrong with team A's ETL" has abandoned portfolio scope.

## Summary

- The blueprint's centrepiece is a **portfolio × substrate matrix** — rows are systems, columns are the four substrate layers (data planes, feature layer, model registry, eval contract).
- Each cell is labelled `shared`, `duplicated`, `forked`, or `unique-by-design`. Avoid `TBD` and `mixed`.
- The matrix is accompanied by a **cross-system dependency diagram** whose actionable observations are substrate nodes with more than one writer or duplicated pairs with unclear ownership.
- The matrix is grounded in seven-question interviews with each team lead; do not skip the interviews even if you were on the team six months ago.
- The blueprint is *current-state honest* — it is not a target-state diagram, not a platform roadmap, and not a per-team code review.

The next chapter takes the `duplicated` and `forked` cells of the matrix and walks the four canonical portfolio-scope failure modes: pipeline-jungle, duplicated-feature drift, model-registry fragmentation, and eval-metric conflict.
