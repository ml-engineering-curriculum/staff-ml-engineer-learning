# Prerequisites — Staff Machine Learning Engineer Track

**Role level:** 40 (staff-plus IC altitude)

This track is a **level-up** curriculum. It assumes the entire level-30 [`senior-ml-engineer-learning`](https://github.com/ml-engineering-curriculum/senior-ml-engineer-learning) curriculum, and the level-20 [`ml-engineer-learning`](https://github.com/ml-engineering-curriculum/ml-engineer-learning) foundation beneath that, in hand. It does not re-teach any of it. Do not start here without the prerequisites below.

## Assumed foundation (level 20 — Machine Learning Engineer)

Owned by [`ml-engineer-learning`](https://github.com/ml-engineering-curriculum/ml-engineer-learning). You should already be able to:

- Frame a business problem as a regression, classification, ranking, or retrieval task.
- Ingest, profile, clean, and validate data; engineer features for tabular, text, and image data.
- Train and tune classical models (regression, trees, gradient-boosted ensembles) with correct cross-validation and hyperparameter search.
- Write a PyTorch training loop, build an MLP / CNN / small Transformer encoder, and apply regularisation and mixed-precision training.
- Track experiments with MLflow, version datasets and models, and produce reproducible runs.
- Package a model behind FastAPI, containerise with Docker, load-test the service, and instrument drift monitoring.
- Walk through an ML system design from business goal to blueprint at a working-level altitude.

## Assumed foundation (level 30 — Senior Machine Learning Engineer)

Owned by [`senior-ml-engineer-learning`](https://github.com/ml-engineering-curriculum/senior-ml-engineer-learning). You should already be able to:

- Take a business problem to a defensible single-system ML architecture, write and defend the RFC, and diagnose training/serving skew at architectural altitude.
- Choose the modeling regime for a problem shape (multi-task, multi-modal, self-supervised, transfer, ensembling), calibrate model outputs, and handle cold-start / long-tail.
- Integrate LLMs into production classical ML (prompt vs. RAG vs. fine-tune vs. hybrid), treat prompts as engineering artifacts, and instrument cost / latency guardrails.
- Design a single-team offline evaluation harness with slice/holdout/adversarial guardrails and run production-readiness reviews against the Google ML Test Score rubric.
- Design and diagnose A/B tests — power/MDE, SRM, CUPED variance reduction, interference, and causal-inference framings when A/B is impossible.
- Define single-team ML SLOs (quality drift, prediction freshness, retraining SLA), run incident response, and treat retraining as a first-class deploy.
- Author a model card, run a responsible-AI review packet for a single system, and threat-model that system for realistic misuse.
- Lead a single-team ML initiative — roadmap, code review at bar, mentorship of mid-level engineers, and rubric-based ML interview authoring.

If any of the above is shaky, complete `senior-ml-engineer-learning` first. **This track will assume — and lift each of those dimensions to multi-team scope — but will not re-teach them.**

## Assumed lived experience

The staff-plus IC role is defined as much by the situations the engineer has been in as by the skills they hold. You should have shipped at least one senior-tech-lead-scope ML system in a team setting, meaning you have:

- Authored and defended an architecture RFC that another senior engineer or a staff engineer pushed back on.
- Owned a system through at least one production incident and led the postmortem.
- Sat in a hiring loop for an ML engineer role and given a hire / no-hire signal.
- Mentored at least one mid-level engineer through an incident, a design review, or a launch.
- Partnered with a platform team as a paved-road consumer and written at least one contribute-back or gap RFC.

Without these lived experiences, the multi-team modules (mod-402, mod-405, mod-406), the program-leadership modules (mod-403, mod-410), and the staff-plus tech-leadership simulation project (project-403) will feel abstract.

## Recommended reading before you start

- Will Larson, *[Staff Engineer: Leadership beyond the management track](https://staffeng.com/book)* (Stripe Press, 2021) — canonical staff-plus IC ladder text; mod-401 and mod-410 are grounded in this.
- The [StaffEng.com stories corpus](https://staffeng.com/stories/) — real staff-plus engineer interviews from Google, Slack, Stripe, Netflix, Fastly, and others.
- Chip Huyen, *Designing Machine Learning Systems* (O'Reilly, 2022) — reinforcement for portfolio-scope architecture framing.
- Kaplan et al., *[Scaling Laws for Neural Language Models](https://arxiv.org/abs/2001.08361)* (2020) and Hoffmann et al., *[Chinchilla](https://arxiv.org/abs/2203.15556)* (2022) — mod-403 is grounded in these.
- Google Cloud, *[MLOps: continuous delivery and automation pipelines in machine learning](https://cloud.google.com/architecture/mlops-continuous-delivery-and-automation-pipelines-in-machine-learning)* — mod-405 and mod-408 build on this.
- [NIST AI Risk Management Framework](https://www.nist.gov/itl/ai-risk-management-framework) — mod-409 is grounded in this and the [Gen-AI Profile overlay](https://nvlpubs.nist.gov/nistpubs/ai/NIST.AI.600-1.pdf).
- Google SRE Book — [Service Level Objectives](https://sre.google/sre-book/service-level-objectives/) and [Managing Incidents](https://sre.google/sre-book/managing-incidents/) — mod-408 borrows structure from both.
- FinOps Foundation, *[FinOps Framework](https://www.finops.org/framework/)* — mod-407 borrows its operating-model discipline.

## Recommended tooling access

- The tooling stack from the level-30 track (MLflow, PyTorch, Docker, an LLM API key or local inference stack).
- Access to a distributed-training environment (a single multi-GPU node or a small cluster is enough — mod-404 does not require production-scale hardware, only the vocabulary and the strategy).
- Access to a public FinOps or cloud-cost dashboard (many providers publish sample data) for mod-407.

## Peer specialist and peer platform tracks worth knowing exist

The staff ML engineer routinely delegates to and coordinates these owners. You do not need to master them, but you should be able to read the shape of their contract and set one:

- `training-pipeline-engineer-learning`, `ai-infra-performance-learning` — distributed training implementation and cluster tuning.
- `fine-tuning-engineer-learning`, `nlp-engineer-learning`, `rag-engineer-learning`, `llm-application-developer-learning`, `applied-ai-engineer-learning` — LLM and applied-ML specialisations.
- `model-evaluation-engineer-learning`, `ai-eval-engineer-learning` — evaluation-platform depth.
- `ai-infra-ml-platform-learning`, `ai-infra-mlops-learning` — the paved-road ML platforms this staff role sets the org contract for.
- `ai-infra-security-learning`, `ai-governance-analyst-learning`, `head-of-ai-governance-learning` — the security and governance counterparts for mod-409.

## Peer level-40 tracks worth understanding

The staff-plus IC path is one of several level-40 paths. Understand which one you are on:

- **This track** — Staff ML Engineer (IC-staff, ML engineering ladder).
- [`ai-infra-team-lead-learning`](https://github.com/ai-infra-curriculum/ai-infra-team-lead-learning) — engineering-manager path (people leadership). Mod-410 covers the partnership between the two paths but does not teach direct people management.
- [`senior-agentic-ai-engineer-learning`](https://github.com/ai-infra-curriculum/senior-agentic-ai-engineer-learning) — staff-plus IC on the agentic-systems ladder.
- [`ai-infra-senior-architect-learning`](https://github.com/ai-infra-curriculum/ai-infra-senior-architect-learning) — staff-plus IC on the infrastructure-architecture ladder.

If a posting reads as engineering-manager scope (direct reports, headcount ownership, performance reviews), or as a peer level-40 track's centre of gravity (agentic systems, infra architecture), the fit is with that track rather than this one.
