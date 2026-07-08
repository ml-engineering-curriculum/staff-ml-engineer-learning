# Resources for mod-402-multi-team-ml-architecture

Real, citable references for the topics this module covers. Read the canonical texts before the module; treat the platform-team writeups as prior art you can point at in an RFC's Section 9; treat the process references as templates whose *shape* you can adopt without adopting their ceremony.

## Canonical ML-systems texts

- **Sculley, D. et al.** *Hidden Technical Debt in Machine Learning Systems.* Advances in Neural Information Processing Systems (NeurIPS) 2015. [Paper (PDF)](https://papers.nips.cc/paper/2015/hash/86df7dcfd896fcaf2674f757a2463eba-Abstract.html). The origin text for the pipeline-jungle, CACE (Change Anything, Change Everything), and glue-code failure modes chapter 3 diagnoses at portfolio scope.
- **Chip Huyen.** *Designing Machine Learning Systems.* O'Reilly, 2022. The current single-book synthesis of production ML systems architecture; chapters on feature engineering, model deployment, and monitoring supply the vocabulary the blueprint reads against.
- **Kleppmann, Martin.** *Designing Data-Intensive Applications.* O'Reilly, 2017. Not ML-specific but the canonical reference on the batch / streaming distinction, data-plane guarantees, and the shape of shared-substrate contracts.

## Trustworthy experimentation and evaluation

- **Kohavi, Ron; Tang, Diane; Xu, Ya.** *Trustworthy Online Controlled Experiments: A Practical Guide to A/B Testing.* Cambridge University Press, 2020. The canonical reference for the eval-metric-conflict failure mode and the arbitration-body split verdict. Chapters on OEC (Overall Evaluation Criterion), guardrail metrics, and interference are the most load-bearing for this module.

## Feature-store and data-plane prior art

- **Uber Engineering blog.** [Meet Michelangelo — Uber's Machine Learning Platform](https://www.uber.com/blog/michelangelo-machine-learning-platform/). The canonical company-scale ML platform writeup; the feature-store and model-registry sections are what chapter 2's substrate definitions read against.
- **Airbnb.** *Zipline: Airbnb's Machine Learning Data Management Platform.* Public talks at Spark+AI Summit and later engineering-blog posts. The point-in-time-correct-join framing from chapter 2's feature-layer section is Zipline-shaped.
- **Feast documentation.** [feast.dev/documentation](https://docs.feast.dev/). Open-source feature-store implementation; the [Feast concepts](https://docs.feast.dev/getting-started/concepts) page is the shortest precise reference for the feature-store vocabulary chapter 2 uses.
- **Tecton.** *What is a Feature Store?* [tecton.ai/what-is-a-feature-store](https://www.tecton.ai/blog/what-is-a-feature-store/). A commercial-vendor summary of the feature-store contract that lines up with the Feast concepts, useful as triangulation.
- **Hopsworks blog.** [Time-travel for the Feature Store](https://www.hopsworks.ai/post/time-travel-for-feature-stores). The point-in-time-correctness property chapter 2 requires the feature layer to provide.

## Model-registry and metadata prior art

- **MLflow.** [Model Registry documentation](https://mlflow.org/docs/latest/model-registry.html). The primitives — registered model, model version, stage, alias, promotion — that chapter 2 and chapter 3 refer to as "the vocabulary every registry ends up needing".
- **Google.** [ML Metadata (MLMD) documentation](https://www.tensorflow.org/tfx/guide/mlmd). The lineage-graph model that chapter 2's metadata-schema requirement leans on.
- **Baylor, Denis et al.** *TFX: A TensorFlow-Based Production-Scale Machine Learning Platform.* KDD 2017. [Paper (PDF)](https://dl.acm.org/doi/10.1145/3097983.3098021). The canonical write-up of production ML pipelines as a component graph and the metadata-store contract that flows through them.
- **Weights & Biases.** [W&B Model Registry docs](https://docs.wandb.ai/guides/registry). Commercial alternative registry with a lineage model; useful for portfolios where more than one registry is in play (the registry-fragmentation failure mode).

## RFC and design-doc templates

- **The IETF RFC series.** [rfc-editor.org](https://www.rfc-editor.org/). RFC 2026 (*The Internet Standards Process*) is the shortest read on why RFC processes exist; BCP 9 status distinctions still shape modern RFC processes.
- **Rust RFC template.** [`rust-lang/rfcs` on GitHub](https://github.com/rust-lang/rfcs). The section shape chapter 5's multi-system RFC template is closest to. [README](https://github.com/rust-lang/rfcs/blob/master/README.md) has the process overview.
- **Kubernetes Enhancement Proposals (KEP).** [`kubernetes/enhancements` on GitHub](https://github.com/kubernetes/enhancements). The stages (`provisional`, `implementable`, `implemented`, `deferred`, `rejected`, `withdrawn`, `replaced`) chapter 5 recommends for portfolio-scope RFCs that live longer than one quarter.
- **Kubernetes API Review Process.** [Community docs on api-review-process](https://github.com/kubernetes/community/blob/master/sig-architecture/api-review-process.md). Chapter 6's block-vs-nudge distinction is closest to the K8s API review's stage gates.

## Engineering practice texts adjacent to this module

- **Winters, Manshreck, Wright.** *Software Engineering at Google.* O'Reilly, 2020. Free HTML edition at [abseil.io/resources/swe-book](https://abseil.io/resources/swe-book). The chapters on Design Documents and Code Review carry over to architecture-review meetings; chapter 6 leans on them for the review-culture properties.
- **Fowler, Martin.** *Refactoring.* Second edition, Addison-Wesley, 2018. The 1.5× migration-cost underestimate factor chapter 4 uses is Fowler-tradition and the preface is the shortest primary reference for it.
- **Larson, Will.** *Staff Engineer: Leadership beyond the management track.* Stripe Press, 2021. [staffeng.com/book](https://staffeng.com/book). The Architect archetype chapter 1 anchors on — its centre of gravity is this module's blueprint. See also mod-401's chapter 2 for the deeper archetype treatment.
- **Reilly, Tanya.** *The Staff Engineer's Path.* O'Reilly, 2022. Chapter 6's "review as a career signal" section shares its framing with Reilly's chapters on influence and communication.

## Company engineering-blog posts worth reading as prior art

- **Netflix Tech Blog.** [Metaflow](https://netflixtechblog.com/open-sourcing-metaflow-a-human-friendly-lib-for-data-science-fa72e04a5d9). Netflix's ML orchestration substrate; representative of a data-plane and pipeline-orchestration shape distinct from Uber's Michelangelo. Useful contrast for the blueprint's data-plane column.
- **LinkedIn Engineering.** *Feathr: Building a High-Performance Feature Store at LinkedIn.* [Blog post](https://engineering.linkedin.com/blog/2022/open-sourcing-feathr---linkedin-s-feature-store-for-productive-m). LinkedIn's feature-store substrate; useful for triangulating the feature-layer contract in chapter 2 across a different consumer profile.
- **Meta / Facebook AI.** [Applied ML at Facebook: A Datacenter Infrastructure Perspective](https://research.facebook.com/publications/applied-machine-learning-at-facebook-a-datacenter-infrastructure-perspective/) (HPCA 2018). Portfolio-scope ML from a hyperscaler perspective; useful for grounding claims about how the same substrate columns look at very large scale.

## In-track references

- [`../../CURRICULUM.md`](../../CURRICULUM.md) — the module and project plan this module sits inside.
- [`../../PREREQUISITES.md`](../../PREREQUISITES.md) — the L20 and L30 foundations assumed for this module.
- [`../mod-401-staff-ml-role-scope/`](../mod-401-staff-ml-role-scope/) — the introductory module whose Architect archetype and altitude vocabulary this module builds on.
- [`../mod-405-ml-platform-strategy/`](../mod-405-ml-platform-strategy/) — the platform-team side of every substrate consolidation decision; the mod that owns build-vs-adopt and the platform consumer contract.
- [`../mod-406-cross-team-eval-and-experimentation/`](../mod-406-cross-team-eval-and-experimentation/) — the deep-dive on the eval-contract failure mode from chapter 3.
- [`../../projects/project-401-multi-team-ml-blueprint/`](../../projects/project-401-multi-team-ml-blueprint/) — the capstone that combines this module's blueprint and RFC into a portfolio-grade deliverable.

<!-- needs-research: add one or two published cross-team ML consolidation postmortems (public write-ups of a merge or share-behind-a-contract project that landed at portfolio scope) once the autonomous research loop has WebSearch permissions. Candidates to check first: Shopify, Stripe, LinkedIn, Instacart, Doordash engineering blogs. -->
