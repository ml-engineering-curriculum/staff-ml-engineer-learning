# Resources for mod-405-ml-platform-strategy

Real, citable references for the topics this module covers. Read the canonical texts before the module; treat the platform-team writeups as prior art you can point at in an RFC's Section 7 (Alternatives Considered); treat the process references as templates whose *shape* you can adopt without adopting their ceremony.

## Canonical platform-strategy and team-topology texts

- **Skelton, Matthew and Pais, Manuel.** *Team Topologies: Organizing Business and Technology Teams for Fast Flow.* IT Revolution, 2019. [teamtopologies.com/book](https://teamtopologies.com/book). The load-bearing reference for the consumer/producer framing in chapters 1 and 4; the *stream-aligned team* and *platform team* pair, and the four interaction modes (X-as-a-service, collaboration, facilitating, enabling), are what the consumer contract in chapter 4 operationalises.
- **Larson, Will.** *Staff Engineer: Leadership beyond the management track.* Stripe Press, 2021. [staffeng.com/book](https://staffeng.com/book). The Architect and Right Hand archetypes chapter 1 anchors on. Larson's chapters on writing RFCs and building executive-adjacent relationships apply directly to chapter 5's gap and contribute-back RFCs.
- **Reilly, Tanya.** *The Staff Engineer's Path.* O'Reilly, 2022. Chapters on influence, alignment, and cross-team writing apply to the consumer-contract negotiation in chapter 4 and to running the review meetings this module's RFCs land in.
- **Kim, Gene; Humble, Jez; Debois, Patrick; Willis, John.** *The DevOps Handbook.* Second edition, IT Revolution, 2021. The producer-team operating discipline (SLOs, error budgets, deprecation windows) chapter 4's producer commitments reflect; useful for the platform-team counter-party's vocabulary.
- **Sculley, D. et al.** *Hidden Technical Debt in Machine Learning Systems.* NeurIPS 2015. [Paper (PDF)](https://papers.nips.cc/paper/2015/hash/86df7dcfd896fcaf2674f757a2463eba-Abstract.html). The CACE (Change Anything, Change Everything) framing chapter 3's exit-cost criterion reads against, and the platform-level version chapter 5's contribute-back RFC guards against.

## Feature-store prior art

- **Uber Engineering blog.** [Meet Michelangelo — Uber's Machine Learning Platform](https://www.uber.com/blog/michelangelo-machine-learning-platform/). The canonical company-scale ML platform writeup; the feature-store section is what chapter 2's feature-store definition reads against.
- **Airbnb.** *Zipline: Airbnb's Machine Learning Data Management Platform.* Public talks at Spark+AI Summit and later engineering-blog posts. <!-- needs-research: verify canonical URL for the most recent Zipline / successor Chronon writeup. -->
- **LinkedIn Engineering.** *Feathr: Building a High-Performance Feature Store at LinkedIn.* [Blog post](https://engineering.linkedin.com/blog/2022/open-sourcing-feathr---linkedin-s-feature-store-for-productive-m). The build-then-open-source path chapter 3 and chapter 5's contribute-back discussion reference.
- **Feast documentation.** [feast.dev/documentation](https://docs.feast.dev/). Open-source feature-store implementation; the [Feast concepts](https://docs.feast.dev/getting-started/concepts) page is the shortest precise reference for the feature-store vocabulary chapter 2 uses.
- **Tecton.** [What is a Feature Store?](https://www.tecton.ai/blog/what-is-a-feature-store/). A commercial-vendor summary useful as triangulation of the Feast concepts; Tecton is the "adopt-managed" reference option in chapter 3's worked example 1.
- **Hopsworks blog.** [Time-travel for the Feature Store](https://www.hopsworks.ai/post/time-travel-for-feature-stores). The point-in-time-correctness property chapter 2 requires the feature layer to provide.

## Model-registry prior art

- **MLflow.** [Model Registry documentation](https://mlflow.org/docs/latest/model-registry.html). The primitives — registered model, model version, alias, tag — chapter 2 refers to as "the vocabulary every registry ends up needing." Default answer for the adopt-OSS verdict.
- **Google.** [ML Metadata (MLMD) documentation](https://www.tensorflow.org/tfx/guide/mlmd). The lineage-graph model chapter 2's metadata-schema section leans on.
- **Baylor, Denis et al.** *TFX: A TensorFlow-Based Production-Scale Machine Learning Platform.* KDD 2017. [Paper (PDF)](https://dl.acm.org/doi/10.1145/3097983.3098021). The canonical write-up of production ML pipelines as a component graph and the metadata-store contract that flows through them.
- **Weights & Biases.** [W&B Model Registry docs](https://docs.wandb.ai/guides/registry). Commercial alternative registry; a plausible adopt-managed candidate for orgs already living in W&B for experiment tracking.

## Eval and experimentation platforms

- **Kohavi, Ron; Tang, Diane; Xu, Ya.** *Trustworthy Online Controlled Experiments: A Practical Guide to A/B Testing.* Cambridge University Press, 2020. The canonical reference for the experimentation-platform side of chapter 2's component 3; SRM diagnostics, guardrail metrics, sequential testing, and interference structure are what the eval-platform contract must cover. Mod-406 is the deep-dive.
- **Netflix Tech Blog.** *Experimentation Platform at Netflix.* [Blog series](https://netflixtechblog.com/tagged/experimentation) — several posts spanning the platform's architecture and statistical guarantees.
- **LinkedIn Engineering.** *XLNT: LinkedIn's Experimentation Platform.* <!-- needs-research: verify canonical URL for LinkedIn's XLNT writeup; multiple posts exist. -->
- **Airbnb Engineering.** *Experimentation Reporting Framework (ERF) and Democratizing Data Products.* Several posts on the [Airbnb Tech blog](https://medium.com/airbnb-engineering). The build-side reference for chapter 3's inference-gateway-scale build case.
- **Statsig, Eppo, GrowthBook.** Commercial and OSS experimentation platforms cited in chapter 2 as the adopt options. [statsig.com](https://statsig.com/), [geteppo.com](https://www.geteppo.com/), [growthbook.io](https://www.growthbook.io/).
- **DeepEval, Ragas.** LLM-and-RAG-specific offline eval libraries chapter 2 cites; [github.com/confident-ai/deepeval](https://github.com/confident-ai/deepeval), [github.com/explodinggradients/ragas](https://github.com/explodinggradients/ragas).

## Training and inference platforms

- **Kubeflow.** [kubeflow.org/docs](https://www.kubeflow.org/docs/). The dominant OSS ML-orchestration substrate; adopt-OSS reference for the training platform.
- **Ray.** [ray.io/docs](https://docs.ray.io/). The modern OSS choice for large-model training, RLHF, and distributed inference; increasingly the default for orgs building on top of accelerators they control.
- **Metaflow.** [Netflix Tech Blog — Open-sourcing Metaflow](https://netflixtechblog.com/open-sourcing-metaflow-a-human-friendly-lib-for-data-science-fa72e04a5d9). Netflix's ML orchestration substrate; contrast with Kubeflow / Ray for orgs where data-scientist ergonomics dominate.
- **SkyPilot.** [skypilot.co](https://skypilot.co/). Multi-cloud burst-training reference cited in chapter 2's training-platform section; useful for orgs whose capacity strategy spans clouds (mod-407 territory).
- **NVIDIA Triton Inference Server.** [github.com/triton-inference-server](https://github.com/triton-inference-server/server). Serving-runtime reference for the inference-gateway chapter, primarily for non-LLM models.
- **vLLM.** [github.com/vllm-project/vllm](https://github.com/vllm-project/vllm). LLM-serving runtime; the reference implementation cited in chapter 2 for LLM inference under a gateway.
- **Hugging Face Text Generation Inference (TGI).** [github.com/huggingface/text-generation-inference](https://github.com/huggingface/text-generation-inference). Alternative LLM-serving runtime; often the operational simplicity choice.
- **KServe.** [kserve.github.io](https://kserve.github.io/website/). OSS inference platform on Kubernetes; the reference for chapter 2's adopt-OSS option for the inference gateway.
- **Ray Serve.** [ray.io/docs](https://docs.ray.io/en/latest/serve/index.html). Ray-native serving; the alternative to KServe for Ray-adopting orgs.

## Build-vs-adopt frameworks

- **Fowler, Martin.** [*Utility vs. Strategic Dichotomy*](https://martinfowler.com/bliki/UtilityVsStrategicDichotomy.html). Bliki post that grounds chapter 3's differentiation criterion.
- **Wardley, Simon.** [*Wardley Maps* (Medium series)](https://medium.com/wardleymaps). Wardley Mapping as an analytical instrument for the utility / product / custom axis chapter 3's rubric intersects; useful advanced reading for learners who want a more visual framing of the same trade-off.
- **Fowler, Martin.** *Refactoring.* Second edition, Addison-Wesley, 2018. The 1.5-2× underestimate factor chapter 3 applies to naive build estimates is Fowler-tradition; the preface is the shortest primary reference.

## RFC and consumer-contract templates

- **Rust RFC template.** [`rust-lang/rfcs` on GitHub](https://github.com/rust-lang/rfcs). The section shape chapter 5's gap and contribute-back RFC templates are closest to; the [README](https://github.com/rust-lang/rfcs/blob/master/README.md) has the process overview.
- **Kubernetes Enhancement Proposals (KEP).** [`kubernetes/enhancements` on GitHub](https://github.com/kubernetes/enhancements). The lifecycle stages (`provisional`, `implementable`, `implemented`, `deferred`, `rejected`, `withdrawn`, `replaced`) are the vocabulary chapter 5 recommends for RFCs that live longer than one quarter.
- **Kubernetes API Review Process.** [Community docs on api-review-process](https://github.com/kubernetes/community/blob/master/sig-architecture/api-review-process.md). Chapter 5's counter-proposal discussion is closest to the K8s API review's design-review stage.
- **The IETF RFC series.** [rfc-editor.org](https://www.rfc-editor.org/). RFC 2026 (*The Internet Standards Process*) is the shortest read on why RFC processes exist and how status distinctions carry into contract shape.

## Company platform-strategy writeups (build / contribute-back prior art)

- **Uber Engineering blog.** [Michelangelo](https://www.uber.com/blog/michelangelo-machine-learning-platform/) and follow-ups. The reference build-case for the training platform and feature store at scale.
- **Netflix Tech Blog.** [Metaflow open-sourcing post](https://netflixtechblog.com/open-sourcing-metaflow-a-human-friendly-lib-for-data-science-fa72e04a5d9). The reference contribute-back-shaped writeup — a build that then made it into the broader community.
- **LinkedIn Engineering.** [Open-sourcing Feathr](https://engineering.linkedin.com/blog/2022/open-sourcing-feathr---linkedin-s-feature-store-for-productive-m). Similar pattern for the feature store.
- **Meta / Facebook AI.** [Applied ML at Facebook: A Datacenter Infrastructure Perspective](https://research.facebook.com/publications/applied-machine-learning-at-facebook-a-datacenter-infrastructure-perspective/) (HPCA 2018). The hyperscale-org platform-strategy case; useful for grounding claims about what "build" looks like at the outer end of chapter 3's rubric.
- **Google.** [TFX and MLMD publications](https://www.tensorflow.org/tfx/guide/mlmd). The reference for how a hyperscaler thinks about model registry and metadata as platform-team-owned components.
- **Winters, Manshreck, Wright.** *Software Engineering at Google.* O'Reilly, 2020. Free HTML edition at [abseil.io/resources/swe-book](https://abseil.io/resources/swe-book). The chapters on Deprecation and on Large-Scale Changes are directly load-bearing for chapter 4's API-stability-window and migration-support commitments.

## In-track references

- [`../../CURRICULUM.md`](../../CURRICULUM.md) — the module and project plan this module sits inside.
- [`../../PREREQUISITES.md`](../../PREREQUISITES.md) — the L20 and L30 foundations assumed for this module.
- [`../mod-401-staff-ml-role-scope/`](../mod-401-staff-ml-role-scope/) — the introductory module whose Architect and Right Hand archetypes and hand-off contracts this module builds on.
- [`../mod-401-staff-ml-role-scope/03-reading-job-postings-for-staff-verbs.md`](../mod-401-staff-ml-role-scope/03-reading-job-postings-for-staff-verbs.md) — the staff-verb vocabulary chapter 1's "am I doing platform-strategy work?" heuristic reads against.
- [`../mod-402-multi-team-ml-architecture/`](../mod-402-multi-team-ml-architecture/) — the portfolio-blueprint module whose painful substrate cells become the input to this module's build-vs-adopt calls.
- [`../mod-406-cross-team-eval-and-experimentation/`](../mod-406-cross-team-eval-and-experimentation/) — the deep-dive on the eval-platform strategy component this module names in chapter 2 and covers at strategic altitude only.
- [`../mod-407-capacity-cost-and-tco/`](../mod-407-capacity-cost-and-tco/) — the FinOps and TCO discipline chapter 3's TCO criterion depends on.
- [`../../projects/project-401-multi-team-ml-blueprint/`](../../projects/project-401-multi-team-ml-blueprint/) — the capstone that combines this module's four artifacts with mod-402 and mod-407.

<!-- needs-research: add two or three published public case studies of ML-org contribute-back RFCs — the actual RFC or the equivalent public writeup where an ML org handed a built component to a platform team or an OSS community. Candidates to check first: LinkedIn (Feathr), Uber (Michelangelo components), Airbnb (Chronon / successor to Zipline), Netflix (Metaflow), Stripe, Shopify. -->

<!-- needs-research: verify canonical URLs and current status of Uber Morpheus, Airbnb ERF, LinkedIn XLNT experimentation platforms once WebSearch / WebFetch permissions are available. Chapter 2 and this file cite them as reference build cases; the citations should point at durable canonical write-ups. -->
