# The Five ML-Platform Components: What Each Is, What Build-vs-Adopt Means for Each

## Motivation

Chapter 1 named this module's scope — build / adopt / contribute-back / defer decisions on the ML org's platform components, carried in written consumer contracts. Before the rubric in chapter 3 or the contract in chapter 4 will make sense, the learner needs a shared vocabulary for the five components those decisions live against. This chapter is that vocabulary.

The point is not to teach *how* to build any of these components — the peer platform tracks own that — but to give the staff-plus IC enough of each component's shape that the build-vs-adopt call in chapter 3 does not collapse into hand-waving. A build-vs-adopt call on "the feature store" that cannot say what point-in-time correctness is, what an online-offline skew SLA looks like, or what an entity-key schema is, is not a call an engineering director will fund on.

Every real ML org has exactly these five conversations. Not four, not six. The names differ across companies (Uber's Michelangelo, LinkedIn's Feathr, Netflix's Metaflow-plus-Titus each stitch the five components differently), but the conceptual slots are the same.

## The five components, named

The five components mod-405's decisions live against:

1. **Feature store** — the offline feature repository and its online serving layer, plus the point-in-time-correct join contract between them.
2. **Model registry** — the versioned store for trained models, their metadata, their lineage, and their stage (dev / staging / prod / archived).
3. **Eval platform** — the offline evaluation harness plus the online experimentation platform (A/B, canary, shadow).
4. **Training platform** — the substrate that turns a training job description into an accelerator reservation, a running loop, checkpoints, and a returned trained artifact.
5. **Inference gateway** — the traffic-shaping layer in front of production models: routing, canary, rate limiting, feature-flag-driven cutover, observability.

The rest of this chapter walks each in turn. For each, three things: what the component actually is; what "build vs. adopt" means for it in practice; and what the peer platform tracks own versus what the ML org's Staff engineer owns.

## Component 1 — Feature store

**What it is.** A feature store has two halves that share a contract. The **offline half** materialises historical features (batch and, sometimes, streaming aggregated views) suitable for training — the join a model trainer performs against labels to produce a training dataset. The **online half** serves the same features at prediction time to production models with low-latency reads. The contract between them is **point-in-time correctness**: the value served at time `t` in production must equal the value materialised for training that would have been available at `t` in the training join. If it does not, the model trains on one feature distribution and serves against another — the *online-offline skew* failure mode, one of the most expensive silent-bug classes in production ML.

Also part of the feature-store surface: entity-key vocabulary (user, item, session, order, whatever), freshness SLA per feature, feature versioning, and access-control on features that are PII-sensitive or regulator-scoped.

**What "build vs. adopt" means.** Choices in the market as of the module's publication:

- **Build in-house.** Uber's Michelangelo (<!-- needs-research: verify current internal status; the public writeups are the reference points -->), Airbnb's Zipline, LinkedIn's Feathr (later open-sourced), Meta's FBLearner. Reserved for orgs with 20+ ML teams and a distinctive point-in-time-correctness or freshness requirement that off-the-shelf tools do not meet.
- **Adopt open-source.** Feast is the most common OSS choice. LinkedIn's open-sourced Feathr and Hopsworks are alternatives. Adoption cost is real — Feast especially requires the org to bring its own compute and materialisation infra — but the point-in-time-correctness primitives are well-tested.
- **Adopt managed / commercial.** Tecton (a managed-Feast lineage tool), Databricks Feature Store, Vertex AI Feature Store, SageMaker Feature Store. Higher $ per team-month, near-zero build cost.
- **Defer.** Small orgs with two-to-three ML systems and no online serving typically do not need a feature store yet — a shared feature-computation library plus a materialised table in the data warehouse gets them 80% of the value. Naming this as an explicit defer in the RFC is the honest move.

**What the peer platform tracks own vs. what Staff owns.** `ai-infra-ml-platform-learning` owns building the feature store if the org's decision is build; owning it operationally either way. Staff (this module) owns the *decision* — build / adopt-Feast / adopt-managed / defer — and the consumer contract that binds the ML org's teams to the chosen implementation.

## Component 2 — Model registry

**What it is.** A registry stores versioned trained models with:

- **The artifact itself** — weights, config, and any tokenizer / vocabulary / feature-schema baggage the model needs at load time.
- **Metadata** — training run ID, training data snapshot, hyper-parameters, eval metrics, author, timestamp.
- **Lineage** — pointer graph back to the data snapshot, feature versions, training code commit. This is what makes drift investigations and audits tractable.
- **Stage** — a lifecycle label (`dev`, `staging`, `prod`, `archived`) with rules on who can promote across which stages. MLflow calls this "aliases and tags" in its current schema; TFX / ML Metadata (MLMD) treats it as a first-class typed graph.

Portfolio-scope: mod-402's registry-fragmentation failure mode is when two teams in the org run two different registries with two different metadata schemas. Mod-405's job is to decide the org's single registry contract and negotiate it.

**What "build vs. adopt" means.**

- **Adopt OSS.** MLflow is the default answer for the majority of ML orgs; the model-registry surface is well-covered and the integration surface (Databricks, PySpark, PyTorch, TF) is broad. TFX + MLMD is the strong alternative for orgs already living in TF-land.
- **Adopt managed / commercial.** Weights & Biases model registry, Neptune.ai, Comet, or the model-registry surface of the major cloud ML platforms (Vertex, SageMaker, Databricks).
- **Build.** Rarely justified. If your org is building its own registry, it is usually because a legacy homegrown registry exists and consolidation has not happened yet — mod-402 chapter 3's registry-fragmentation failure mode. Building a fresh registry from scratch in 2026 is almost always wrong.
- **Defer.** Not really available for orgs above two ML systems. Registry-lite via git-LFS or an S3-hierarchy-of-checkpoints stops scaling past two teams; the audit / lineage story falls apart first.

**What the peer platform tracks own vs. what Staff owns.** `ai-infra-mlops-learning` owns the registry operationally. Staff (this module) owns the contract on **metadata schema** (what every registered model must carry), **stage-promotion rules** (who can promote what where), and **lineage granularity** (does the registry carry the data-snapshot hash and the feature-version hash, or only the training-code commit).

## Component 3 — Eval platform

**What it is.** Two sub-layers that many orgs conflate to their detriment:

- **The offline eval harness.** The pipeline that, given a model and a held-out dataset, produces the eval metrics (offline accuracy, calibration, slice metrics, LLM-as-judge scores). Includes the slice / holdout / adversarial-set discipline.
- **The online experimentation platform.** The A/B test / canary / shadow / progressive-rollout substrate. SRM diagnostics, CUPED variance reduction, sequential testing, guardrail metrics, interference detection. Kohavi et al. (*Trustworthy Online Controlled Experiments*, 2020) is the canonical reference.

Mod-406 is the deep-dive on the eval-and-experimentation program — its charter, its cross-team review cadence, its LLM-as-judge policy. Mod-405 covers the **platform strategy** side: build or adopt the substrate, and what consumer contract binds the ML org's teams to it.

**What "build vs. adopt" means.**

- **Adopt an experimentation platform.** Statsig, Eppo, LaunchDarkly (feature-flags plus experiments), Optimizely, GrowthBook (OSS). For most non-hyperscale orgs this is the right call — the statistics engine is hard to get right and vendor implementations have institutional knowledge you do not.
- **Build an experimentation platform.** Reserved for orgs with distinctive interference structure (multi-sided marketplaces, ranking systems where treatment leaks across users) or scale requirements (billion-events-per-day) that vendors do not handle well. Airbnb's ERF, Netflix's XP, LinkedIn's XLNT, Uber's Morpheus <!-- needs-research: verify current name / status --> are the reference points.
- **Adopt OSS for the offline eval harness.** MLflow evaluate, TFX Evaluator, DeepEval, Ragas (for RAG-specific), Weights & Biases eval, custom-on-top-of-pytest. The offline harness is closer to a coding convention than a service — adoption usually means "we all use this framework" rather than "we all call this API."
- **Build the offline eval harness.** Common at scale — most large ML orgs have an org-specific eval harness because their slice / holdout structure is product-specific. But "build the harness" is often really "define a schema and a set of pytest conventions"; the difference between build and adopt collapses.

**What the peer platform tracks own vs. what Staff owns.** `ai-infra-ml-platform-learning` and `ai-infra-mlops-learning` split the experimentation platform. Staff (this module) owns the *build-vs-adopt call* on both sub-layers and the *consumer contract* — the SRM diagnostic every experiment must pass, the guardrail-metric list every team must ship, the review body that arbitrates conflicts (mod-406 owns the body itself).

## Component 4 — Training platform

**What it is.** The substrate that turns a training job description (data plan, model architecture, parallelism recipe, resource ask) into a running training job on accelerators and a returned trained artifact registered in the model registry. It has several stitched-together layers:

- **Compute reservation.** How does the job get its 8 or 512 or 4096 accelerators for hours-to-weeks? Kubernetes + a scheduler (Kueue, Volcano, Slurm-on-K8s), a bespoke reservation service, or a managed cloud offering (Vertex Training, SageMaker Training, Anyscale, TogetherAI).
- **Job orchestration.** Kubeflow Pipelines, Metaflow, Airflow, Argo Workflows, Ray. Ray is now the dominant modern answer for large-model training and RLHF-style workflows; Kubeflow for batch retraining pipelines.
- **Training loop and parallelism library.** PyTorch/FSDP, DeepSpeed, Megatron-LM, JAX. Mod-404 is where the parallelism-recipe decision lives; mod-405 covers whether that recipe is *served* by an org-standard training substrate or by per-team scripts.
- **Multi-cloud / burst.** SkyPilot for multi-cloud portability; managed hyperscaler answers otherwise.

**What "build vs. adopt" means.**

- **Adopt managed cloud training.** Vertex AI Training, SageMaker Training, Databricks. Adoption is fast and time-to-first-run is measured in days; costs at scale (spot vs. reserved, gradient-checkpointing efficiency, MFU realised) are the trade.
- **Adopt OSS orchestration on your own accelerators.** Ray on a self-managed K8s cluster, Kubeflow, Metaflow-on-your-cluster. Higher build cost, lower marginal $ per training-hour, more control at the parallelism / checkpoint layer.
- **Build the training platform.** Almost never right unless your org is a hyperscaler with >10k accelerators sustained and a distinctive scheduling requirement. Even Meta and Google publish increasingly Ray-and-K8s-shaped stacks rather than pure homegrown ones.
- **Defer the platform question.** For orgs whose training runs are all <64 accelerators for <1 day, a shared library + a Kubernetes namespace is sometimes enough. Naming that as an explicit defer with a re-visit trigger is honest.

**What the peer platform tracks own vs. what Staff owns.** `ai-infra-ml-platform-learning` owns the training-platform build. `ai-infra-performance-learning` owns kernel-level tuning and MFU optimisation. Mod-404 owns the parallelism-recipe decision itself. Mod-405 owns the *strategy call* — do we build, adopt Ray/Kubeflow, adopt Vertex/SageMaker — and the consumer contract about which paved-road path the ML org's programs use.

## Component 5 — Inference gateway

**What it is.** The traffic-shaping layer that sits between the product and the running model containers. It typically owns:

- **Routing** — request → model instance, with model-version selection driven by the model-registry stage.
- **Canary and progressive rollout** — 1% → 10% → 50% → 100% traffic ramps, integrated with the online experimentation platform.
- **Feature-flag-driven cutover** — the ability to toggle a specific model version off, redirect traffic to a fallback, or serve a shadow copy for offline eval.
- **Rate limiting, quotas, and cost accounting** — especially in LLM-serving contexts where per-request cost is heterogeneous.
- **Observability** — latency, throughput, error-rate, prediction-distribution telemetry, per-model tags.

Under the gateway sit the actual serving runtimes: NVIDIA Triton, vLLM (LLM-optimised), TGI (Hugging Face's Text Generation Inference), TensorRT-LLM, Ray Serve, TorchServe, KServe, or hosted APIs (OpenAI, Anthropic, Bedrock, Vertex).

**What "build vs. adopt" means.**

- **Adopt OSS.** KServe + Istio, or Ray Serve, or a stitched Envoy + custom control-plane. Solid answers for orgs with the platform engineering to run them.
- **Adopt managed inference.** Vertex Inference, SageMaker Inference, Databricks Serving, Anyscale. Fastest time-to-adoption; $ per QPS is the trade.
- **Build.** Almost the only strategy call in this module where "build" is regularly the right answer for large ML orgs — because inference gateways touch product SLOs directly and the interference with routing, canary, and cost accounting is very organisation-specific. Netflix, Uber, Meta, LinkedIn, Airbnb all run bespoke inference gateways in front of open-source serving runtimes.
- **Defer.** For orgs with two-to-three models and no canary discipline, direct routing to a KServe / vLLM instance without a gateway is sometimes fine; the tell that this has expired is the first incident where you can't easily roll back a bad model version.

**What the peer platform tracks own vs. what Staff owns.** `ai-infra-ml-platform-learning` owns the gateway build. `ai-infra-performance-learning` owns per-model latency / throughput tuning inside the serving runtime. Staff (this module) owns the *strategy call* — build the gateway, or adopt KServe / Ray Serve / Vertex — and the consumer contract on canary discipline, model-version pinning, and rollback SLO.

## What the peer tracks own for each component, summarised

| Component          | Peer platform track (build)                             | Peer specialist track (implementation depth)           | Mod-405 (this module) owns                              |
| ------------------ | ------------------------------------------------------- | ------------------------------------------------------ | ------------------------------------------------------- |
| Feature store      | `ai-infra-ml-platform-learning`                         | (feature engineering shared across ML)                 | Build-vs-adopt call; PIT-correctness contract           |
| Model registry     | `ai-infra-mlops-learning`                               | `model-evaluation-engineer` for eval-metadata schema   | Metadata schema; promotion rules; lineage granularity   |
| Eval platform      | `ai-infra-ml-platform-learning` + `ai-infra-mlops-learning` | `model-evaluation-engineer`, `ai-eval-engineer`    | Build-vs-adopt; mod-406 owns the charter                |
| Training platform  | `ai-infra-ml-platform-learning`                         | `training-pipeline-engineer`; `ai-infra-performance-learning` | Build-vs-adopt; org-standard training paths       |
| Inference gateway  | `ai-infra-ml-platform-learning`                         | `ai-infra-performance-learning`                        | Build-vs-adopt; canary and rollback SLO contract        |

The `training-pipeline-engineer`, `model-evaluation-engineer`, and `ai-infra-performance-learning` tracks are peer specialist / peer platform curricula; the paths above are the hand-off contracts mod-401 chapter 4 already established.

## The artifact this chapter produces

An **inventory sheet**: for the learner's ML org, name the current implementation, the current owner, and the current pain-level for each of the five components. This becomes the input to exercise-01's rubric. It is not a build-vs-adopt *decision* — that is chapter 3 — but a candid current-state map. Learners without a real ML org to draw on can use a public reference architecture (Uber Michelangelo, Netflix Metaflow, LinkedIn Feathr posts) the same way mod-402 exercise-01 allows.

## Summary

- Every ML org's platform-strategy conversation lives against five components: **feature store, model registry, eval platform, training platform, inference gateway**.
- Each has its own build-vs-adopt vocabulary and its own dominant vendor / OSS options; the rubric in chapter 3 applies to each but pays out different weights.
- The peer platform track (`ai-infra-ml-platform-learning`, `ai-infra-mlops-learning`) owns the *build* of whichever answer the org lands on; Staff (this module) owns the *call* and the *consumer contract*.
- The training platform and inference gateway are the two components where "build" is most often the right answer at scale; the feature store, model registry, and eval platform are where "adopt" is more often defensible.
- The inventory sheet — current implementation, current owner, current pain — is the artifact this chapter produces and the input to the exercise-01 rubric.

The next chapter walks the build-vs-adopt rubric itself: the seven criteria, the weighted-scoring template, and two worked examples on components from the inventory sheet.
