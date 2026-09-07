# Job Requirements — Staff Machine Learning Engineer

> **Cycle:** 2026-09 (previous: 2026-08 backfill against 30 in-window postings)
> **Postings reviewed:** 28 distinct in-window postings across three segments — AI-native labs, big tech / consumer, and fintech / health / autonomy / auto (last 90 days: 2026-06-09 → 2026-09-07).
> **Delta recommendation:** **no change** — the existing 10-module curriculum still covers every requirement that clears the ≥3-posting AND ≥0.30-frequency bar. Two signals moved this cycle: (a) the agentic-AI theme crossed the 0.40 watch-list line set last cycle (0.41 vs 0.30), but the distinct staff-scope "cross-team agent-eval program" framing this track owns is only present in ~2 postings — below the ≥3 threshold for a new exercise; and (b) a new emergent theme, "product-integrated GenAI / LLM in production", is the dominant signal at 0.69 but reads as inherited prerequisite rather than a staff-scope-only gap. [`.aicg/curriculum-plan-delta.json`](.aicg/curriculum-plan-delta.json) is emitted with empty additions arrays.
> **Machine-readable index:** [`.aicg/job-requirements.json`](.aicg/job-requirements.json)

## How this file is organized

Requirements are grouped by ownership. For each requirement we list:

- **Status** — `covered`, `covered_at_lower_level`, `covered_at_peer_level`, or `covered_at_higher_level`.
- **Frequency** — share of the 28-posting sample that names the requirement (additive-change threshold is 0.30).
- **Δ vs 2026-08** — direction of change since the previous cycle (↑ / ↓ / — / *new*).
- **Owner** — role that primarily teaches it. When ownership sits at a lower or peer level, this file links out rather than duplicating.
- **Curriculum coverage** — module / project paths inside this repo, or a link to the sibling track that owns it.
- **Evidence** — verbatim quotes from postings. Full posting URLs and bullets are in the JSON index.

## Sampling notes

**Title filter applied.** The sample counts only postings with one of: "Staff Machine Learning Engineer", "Staff ML Engineer", "Staff Software Engineer, Machine Learning", "Staff Engineer, ML Systems / ML Platform / ML Infra", "Staff AI/ML Engineer", "Senior Staff Machine Learning Engineer", "Lead ML Engineer", "ML Tech Lead", "ML Research Engineer" (where the primary output is production ML, not publications), or "Staff MLOps Engineer" where the primary verb is ML-modeling or ML-platform (not people management). Excluded: plain "Machine Learning Engineer" (no seniority), "Senior Machine Learning Engineer" (L30), "Principal" (L50), any "Engineering Manager" title, "Research Scientist" / "Applied Scientist" where the primary output is publications, "Staff Data Scientist", and "Tech Lead Manager" (people-management path).

**Title-convention gap.** Pure AI-native labs (OpenAI, xAI, Perplexity, Cohere, Character.AI, ElevenLabs, Runway, and much of Anthropic) use "Member of Technical Staff" (MoTS) as their L4/L5 title rather than "Staff …", so the AI-native segment leans on Databricks, Cresta, and Sentry this cycle. This is not a coverage bug — the requirements gathered from these employers still describe the level-40 IC-staff scope this track owns; the vocabulary of the postings is just different.

**Big-tech FAANG gap.** Apple / Meta / Google / Amazon ATS pages 403/404'd on WebFetch this cycle and were dropped rather than fabricated. The 2026-09 sample skews toward consumer marketplaces (Airbnb ×4, Reddit ×4, plus Uber, DoorDash, Pinterest, Snap, Discord, Chewy, Riot, Hinge, Zocdoc) because those employers index highly on public boards. Segment mix: AI-native (3), consumer (16), fintech (2), autonomy (2), health (2), auto (1), applied-industrial (2). Employers appearing more than once: Reddit (4), Airbnb (4). Full posting list in [`.aicg/job-requirements.json`](.aicg/job-requirements.json).

## Seniority differentiation — what changes from level 30

The single most reliable signal in a Staff Machine Learning Engineer posting is the **shift in the object of the verb**. Cresta 2026-09: *"Define and lead the technical vision for Cresta's next-generation Agentic AI systems."* Plaid 2026-09: *"You will serve as the technical lead for the full machine learning lifecycle, overseeing everything from data curation and experimentation to production deployment."* GM 2026-09: *"Demonstrated track record of leading technically ambiguous, cross-team infrastructure initiatives and driving them to measurable impact."* Reddit LS Embedding 2026-09: *"You will own the technical direction for large-scale embedding models, guiding the development of state-of-the-art graph-based ML architectures."*

A level-30 posting says *"own the ranking model"*, *"lead the fraud detection team"*, *"run the experimentation review"*, *"author the model card"*. A level-40 posting says *"own the ranking, retrieval, and personalization portfolio"*, *"lead the ML org's build-vs-buy on the eval platform"*, *"run the cross-team experiment review body"*, *"author the AI-risk register the CISO signs off on"*, *"shape the multi-quarter modeling roadmap that leadership commits to"*, *"calibrate the senior ML engineers we hire"*.

### Staff-plus IC vs. Engineering Manager at level 40

Two distinct level-40 role families coexist in ML orgs, both hired against the "staff-plus" bar but with different day-to-day scope:

- **Staff Machine Learning Engineer (this track)** — IC-staff path. Multi-team technical leadership without direct reports. Owns architecture, program, platform strategy, and the technical bar.
- **Engineering Manager, ML** — people-leadership path. Owned by the peer track [`ai-infra-team-lead-learning`](https://github.com/ai-infra-curriculum/ai-infra-team-lead-learning).

None of the 28 postings sampled this cycle included direct-report management as a required-skill bullet; the sample was filtered to IC-staff postings before coding.

---

## 1. Staff role scope, staff-plus archetypes, multi-team contracts

**Status:** covered · **Frequency:** 0.79 (Δ ↓ from 0.87) · **Owner:** `staff-ml-engineer`

The staff-plus IC archetypes (Tech Lead, Architect, Solver, Right Hand), the shift from single-team ownership to multi-team ownership, the hand-off contracts to the manager peer (`ai-infra-team-lead`) and to specialist tracks.

**Curriculum coverage**

- [`lessons/mod-401-staff-ml-role-scope/`](lessons/mod-401-staff-ml-role-scope)

**Evidence (selected)**

- Cresta (Staff ML): *"Proven ability to influence technical direction across teams as a senior individual contributor."*
- Wayve (Staff ML, AV Core): *"Staff-level technical leadership: research-literate and pragmatic, setting direction, raising the bar, and leading cross-functional work."*
- Plaid (Staff ML, DFAI): *"Prior technical leadership experience (tech lead, principal, or staff) with demonstrated cross-team influence and mentorship."*
- Reddit (Staff ML, LS Embedding): *"Demonstrated leadership in driving ML strategy, mentoring engineers, and influencing cross-functional teams."*
- Snap (Staff ML, Search Ranking): *"Proven ability to lead complex technical projects across multiple teams."*

## 2. Multi-team ML systems architecture

**Status:** covered · **Frequency:** 0.62 (Δ ↑ from 0.47) · **Owner:** `staff-ml-engineer`

Portfolio-scope blueprints — shared data planes, shared feature layer, shared model-registry contract, shared eval contract; identify duplication and drive consolidation; author multi-system RFCs and defend them in architecture reviews.

**Curriculum coverage**

- [`lessons/mod-402-multi-team-ml-architecture/`](lessons/mod-402-multi-team-ml-architecture)
- Capstone: [`projects/project-401-multi-team-ml-blueprint/`](projects/project-401-multi-team-ml-blueprint)

**Evidence (selected)**

- GM (Staff ML, ML Training Infra): *"Demonstrated track record of leading technically ambiguous, cross-team infrastructure initiatives and driving them to measurable impact."*
- Plaid (Staff ML, DFAI): *"Ability to drive technical alignment across teams: setting standards, defining integration patterns, and influencing beyond your immediate scope."*
- Airbnb (Staff ML, AI Enablement): *"Your insights and technical expertise will shape the future of ML infrastructure, enabling hundreds of engineers to build world-class AI experiences."*
- Reddit (Staff ML, LS Embedding): *"Proven ability to design, implement, and optimize scalable ML architectures, from distributed training to real-time inference."*
- Chewy (Staff ML): *"Ability to set technical direction and influence cross-functional teams through architecture reviews, design documents, and mentorship."*

## 3. Foundation-model / large-scale fine-tune training programs

**Status:** covered · **Frequency:** 0.41 (Δ ≈ 0.40) · **Owner:** `staff-ml-engineer` (program) → peer specialists (`training-pipeline-engineer`, `fine-tuning-engineer`) for implementation

Scope pretraining and large-scale fine-tune programs: compute-optimal parameter/data trade-offs, scaling-law-driven roadmap, data mixture and curriculum, ablation sequencing, capability vs. safety post-training investment, executive framing of the program's cost and risk.

**Curriculum coverage**

- [`lessons/mod-403-foundation-model-training-programs/`](lessons/mod-403-foundation-model-training-programs)
- Capstone: [`projects/project-402-foundation-model-training-program/`](projects/project-402-foundation-model-training-program)
- Peer specialist depth: `training-pipeline-engineer-learning`, `fine-tuning-engineer-learning`

**Evidence (selected)**

- Plaid (Staff ML, DFAI): *"Deep expertise in Transformers/LLMs/Foundation Models, including large-scale training or domain adaptation."*
- DoorDash (Staff ML, Fulfillment Planning): *"Define and lead DoorDash's cutting-edge AI vision for logistics: an LLM-inspired foundation model for intelligence across logistics."*
- Airbnb (Sr Staff ML, Post Training): *"You will be responsible for fine-tuning state-of-the-art LLMs for diverse use cases while optimizing models for high-performance deployment."*
- Wayve (Staff ML, AV Core): *"Hands-on experience with transformer-based and multimodal architectures, including vision-language models (VLM), vision-language-action models (VLA), or equivalent."*
- Cresta (Staff ML): *"7+ years of experience building and deploying machine learning systems in production, including deep hands-on experience with LLMs at scale."*

## 4. Distributed-training strategy at scale

**Status:** covered · **Frequency:** 0.34 (Δ ↓ from 0.50) · **Owner:** `staff-ml-engineer` (strategy) → peer specialists for implementation

Parallelism recipe (DDP, FSDP/ZeRO, tensor-parallel, pipeline-parallel, 3D-parallel), MFU estimation, failure-mode budgeting, cluster-hour defence to leadership.

**Curriculum coverage**

- [`lessons/mod-404-distributed-training-systems-at-scale/`](lessons/mod-404-distributed-training-systems-at-scale)
- Peer implementation depth: `training-pipeline-engineer-learning`, `ai-infra-performance-learning`

**Evidence (selected)**

- GM (Staff ML, ML Training Infra): *"Experience designing and developing training platforms that support FSDP, pipeline parallelism, and other scalable solutions for training large foundational models."*
- Reddit (Staff MLSE): *"Deep experience working with distributed training frameworks, including Ray and Kubernetes."*
- Reddit (Staff ML, ML Understanding): *"Experience with distributed training frameworks (e.g., Ray Training, PyTorch Distributed), and efficient utilization of hardware resources."*
- Plaid (Staff ML, DFAI): *"Distributed training experience and strong Python + software engineering fundamentals at a staff level."*
- Kodiak (Staff ML, Deployment): *"Experience working with large-scale distributed training systems."*

**Frequency note.** The 0.50 → 0.34 drop is likely a sampling artefact — the 2026-09 sample skews toward consumer marketplaces (Reddit / Airbnb / DoorDash / Pinterest / Snap / Discord / Chewy / Riot / Hinge / Zocdoc) where distributed-training language tends to be implicit ("scalable ML architectures") rather than explicit ("FSDP / pipeline parallel / MFU"). GM Training Infra remains the clearest archetype for the staff-scope framing.

## 5. ML platform strategy at org scope

**Status:** covered · **Frequency:** 0.48 (Δ ↑ from 0.40) · **Owner:** `staff-ml-engineer` (consumer strategy) → peer platform tracks for build

Build-vs-adopt on feature store, model registry, eval platform, training platform, inference gateway; paved-road roadmap negotiation; contribute-back and gap RFCs; multi-team consumer contract.

**Curriculum coverage**

- [`lessons/mod-405-ml-platform-strategy/`](lessons/mod-405-ml-platform-strategy)
- Peer platform build depth: `ai-infra-ml-platform-learning`, `ai-infra-mlops-learning`

**Evidence (selected)**

- Riot Games (Staff ML, ML Platform): *"Experience operating inference platforms such as KServe and production ML infrastructure including Feast, Milvus, or similar open-source systems."*
- Reddit (Staff MLSE): *"Hands-on experience administering and integrating MLOps tools for experiment tracking, model serving, and model registries (e.g. MLflow or Wandb)."*
- Airbnb (Staff ML, AI Enablement): *"Your insights and technical expertise will shape the future of ML infrastructure, enabling hundreds of engineers to build world-class AI experiences."*
- Plaid (Staff ML, DFAI): *"Experience defining ML platform capabilities (serving infra, feature stores) used across multiple teams."*
- Chewy (Staff ML): *"Deep knowledge of data pipelines, feature stores, and distributed training/serving systems."*

## 6. Cross-team evaluation & experimentation program

**Status:** covered · **Frequency:** 0.52 (Δ ↑ from 0.33) · **Owner:** `staff-ml-engineer`

Standardise the offline eval harness across ML teams; LLM-as-judge policy; shadow/canary/progressive-rollout contract; cross-team experiment review; evaluation of LLM / agent / VLM systems at program scope.

**Curriculum coverage**

- [`lessons/mod-406-cross-team-eval-and-experimentation/`](lessons/mod-406-cross-team-eval-and-experimentation)
- Peer platform build depth: [`ai-eval-engineer-learning`](https://github.com/ai-infra-curriculum/ai-eval-engineer-learning) (LLM/agent eval platform), [`model-evaluation-engineer-learning`](https://github.com/ml-engineering-curriculum/model-evaluation-engineer-learning) (classical + generative eval depth)

**Evidence (selected)**

- Cresta (Staff ML): *"Experience designing evaluation frameworks for LLM systems beyond single-turn prompts, including robustness testing and production monitoring."*
- Snap (Staff ML, Search Ranking): *"Strong understanding of online experimentation, A/B testing, metric design, model debugging, and tradeoff analysis."*
- Wayve (Staff ML, AV Core): *"5+ years in ML engineering, including pathfinding in ambiguous problems - from scoping and evals to establishing a direction."*
- DoorDash (Staff ML, Fulfillment Planning): *"Designed, launched, and operated mission-critical ML models or systems in production, including monitoring, retraining, reliability, and governance."*
- Airbnb (Sr Staff ML, Post Training): *"Post-training experience in areas like … language model evaluation."*

## 7. GPU capacity, cost, and TCO — portfolio management

**Status:** covered · **Frequency:** 0.21 (Δ ↑ from 0.17) · **Owner:** `staff-ml-engineer`

Portfolio-level demand forecast, capacity portfolio (reserved / on-demand / spot / multi-cloud / self-hosted), FinOps discipline, TCO model a CFO can read.

**Curriculum coverage**

- [`lessons/mod-407-capacity-cost-and-tco/`](lessons/mod-407-capacity-cost-and-tco)

**Evidence (selected)**

- Riot Games (Staff ML, ML Platform): *"Familiarity with GPU orchestration, performance tuning, and cost-aware scheduling."*
- Reddit (Staff MLSE): *"Hands-on experience with ML optimization, including memory and GPU profiling."*
- Cresta (Staff ML): *"Strong systems thinking: ability to design for scalability, latency constraints, cost efficiency, security, and long-term maintainability."*
- Kodiak (Staff ML, Deployment): *"Quantization, pruning, converting to ONNX/TensorRT, custom GPU kernels and profiling."*
- Airbnb (Sr Staff ML, Post Training): *"Runtime optimizations, model quantization, compression, on-device inference, GPU inference."*

**Frequency note.** Crept up from 0.17 → 0.21 with GPU-orchestration and cost-aware-scheduling language at Riot ML Platform; still below the 0.30 additive-change threshold. Explicit TCO / demand-forecast language remains rare. Kept at planned scope; watch on the next cycle in case explicit TCO language becomes a broader requirement.

## 8. Portfolio-scope ML reliability, SLOs, multi-team incident command

**Status:** covered · **Frequency:** 0.28 (Δ ↑ from 0.23) · **Owner:** `staff-ml-engineer`

Standardise ML-specific SLI/SLO vocabulary; severity matrix for ML incidents; cross-team incident command adapted to ML failure classes; org-standard postmortem template; retraining-as-deploy hygiene.

**Curriculum coverage**

- [`lessons/mod-408-portfolio-reliability-and-incident-command/`](lessons/mod-408-portfolio-reliability-and-incident-command)

**Evidence (selected)**

- DoorDash (Staff ML, Fulfillment Planning): *"Designed, launched, and operated mission-critical ML models or systems in production, including monitoring, retraining, reliability, and governance."*
- Sentry (Staff ML, AI): *"You will be at the forefront of integrating AI and machine learning into our core products, from issue triage and resolution to predictive analytics."*
- Riot Games (Staff ML, ML Platform): *"You will architect systems for model deployment, observability, and lifecycle management."*
- Wayve (Staff ML, AV Core): *"Experience with redundant or fallback architectures, safety-critical systems."*
- Kodiak (Staff ML, Deployment): *"Design and deploy machine learning systems that improve our vehicles' ability to understand the world, predict behavior, and make safe driving decisions."*

**Frequency note.** Explicit incident-command language still clusters in safety-critical domains (autonomy) and production-AI-quality domains. DoorDash's "retraining, reliability, and governance" phrasing is the clearest 2026-09 archetype for the retraining-as-deploy hygiene mod-408 owns.

## 9. Responsible AI at portfolio scope

**Status:** covered · **Frequency:** 0.14 (Δ ↓ from 0.20) · **Owner:** `staff-ml-engineer` (ML-side program) → `ai-governance-analyst` / `head-of-ai-governance` / `ai-infra-security-learning` for depth

Portfolio-scope AI risk register (NIST AI RMF + Gen-AI profile); EU AI Act classification; org-standard model card and datasheet format; pre-launch review gate; red-team cadence and escalation path to head-of-ai-governance.

**Curriculum coverage**

- [`lessons/mod-409-responsible-ai-at-portfolio-scope/`](lessons/mod-409-responsible-ai-at-portfolio-scope)

**Evidence (selected)**

- Airbnb (Sr Staff ML, Post Training): *"Post-training experience in areas like data processing for fine-tuning; responsible LLMs; LLM alignment."*
- Reddit (Staff ML, ML Understanding): *"Passion for developing scalable, well-designed, and responsible AI solutions that positively impact society."*
- Cresta (Staff ML): *"Experience designing evaluation frameworks for LLM systems beyond single-turn prompts, including robustness testing and production monitoring."*
- Wayve (Staff ML, AV Core): *"Experience with redundant or fallback architectures, safety-critical systems."*

**Frequency note.** Explicit responsible-AI language dropped from 0.20 → 0.14 this cycle. No NIST AI RMF / EU AI Act / model-card language surfaced explicitly in the 2026-09 sample — the responsible-AI framing showed up as "responsible LLMs / LLM alignment" (Airbnb Post-Training), "responsible AI solutions" (Reddit ML Understanding), and "robustness testing" (Cresta). Below the 0.30 additive-change threshold but non-negotiable at staff scope. **Watch-list:** if this stays below 0.15 for two consecutive cycles (2026-10 and 2026-11), open a proposal to demote mod-409 from 10h.

## 10. Staff-plus technical leadership — roadmaps, exec comms, hiring calibration, coaching seniors

**Status:** covered · **Frequency:** 0.76 (Δ ↑ from 0.67) · **Owner:** `staff-ml-engineer`

Multi-quarter ML program roadmap that leadership commits to; executive-audience one-pagers and RFCs; staff-plus hiring loops; coach senior tech leads and partner with the EM peer; vendor and partnership judgement calls.

**Curriculum coverage**

- [`lessons/mod-410-staff-plus-technical-leadership/`](lessons/mod-410-staff-plus-technical-leadership)
- Capstone: [`projects/project-403-staff-plus-tech-leadership-simulation/`](projects/project-403-staff-plus-tech-leadership-simulation)

**Evidence (selected)**

- Reddit (Staff ML, Notifications): *"Proven ability to identify key opportunities, define roadmaps and drive scalable improvement in notifications relevance."*
- GM (Staff ML, ML Training Infra): *"Excellent communication skills, with the ability to build consensus, navigate controversial decisions, communicate risks clearly, and provide constructive technical feedback."*
- Ramp (Staff ML): *"You will help build core machine learning, design data architectures, and set strategic roadmaps to help Ramp reduce Identity-related threats."*
- ZipRecruiter (Staff ML): *"Partner directly with Engineering and Product Leadership to define and execute technical vision for core marketplace components."*
- Reddit (Staff ML, ML Understanding): *"Mentor and guide senior and mid-level ML engineers, fostering a culture of excellence, innovation, and knowledge sharing."*
- Discord (Sr Staff SWE, ML): *"Experience communicating updates and resolutions to customers and other partners to lead large and complex technical projects cross-functionally."*

---

## Requirements covered at other levels / by peer tracks (do NOT duplicate here)

The following themes appear in the postings but are owned elsewhere per the project-wide ownership rule. This track links to the owner and does not re-teach.

### 11. ML practitioner workflow foundations

**Status:** covered_at_lower_level · **Frequency:** implicit prerequisite (assumed in every posting) · **Owner:** `ml-engineer` (level 20)

Python, PyTorch/scikit-learn, MLflow, FastAPI/Docker, drift monitoring. Listed in [`PREREQUISITES.md`](PREREQUISITES.md).

Depth link: [`ml-engineer-learning`](https://github.com/ml-engineering-curriculum/ml-engineer-learning)

### 12. Single-team ML tech-lead scope

**Status:** covered_at_lower_level · **Frequency:** implicit prerequisite · **Owner:** `senior-ml-engineer` (level 30)

Single-system architecture RFC, offline eval harness for one team, single-model SLOs, one-team roadmap, single-system model-card authoring. Listed in [`PREREQUISITES.md`](PREREQUISITES.md).

Depth link: [`senior-ml-engineer-learning`](https://github.com/ml-engineering-curriculum/senior-ml-engineer-learning)

### 13. Distributed-training implementation depth

**Status:** covered_at_peer_level · **Frequency:** implicit within Distributed-training strategy (§4) · **Owner:** `training-pipeline-engineer` / `ai-infra-performance-learning`

DDP/FSDP APIs, NCCL debugging, checkpoint sharding, activation checkpointing, kernel tuning. Depth links: [`training-pipeline-engineer-learning`](https://github.com/ai-infra-curriculum/training-pipeline-engineer-learning), [`ai-infra-performance-learning`](https://github.com/ai-infra-curriculum/ai-infra-performance-learning).

### 14. LLM fine-tuning / RLHF / DPO / GRPO / PPO / MoE post-training depth

**Status:** covered_at_peer_level · **Frequency:** 0.17 (Δ ↑ from 0.13, below 0.30 threshold) · **Owner:** `fine-tuning-engineer`

Airbnb Post-Training explicitly names "RL, alignment, fine-tune, multimodal" as required; Cresta names "LLM fine-tuning"; Plaid names "domain adaptation"; Databricks names "LLM fine-tuning, prompt engineering, and retrieval-augmented generation (RAG)" (preferred). Program-level trade-off framing is captured in mod-403 exercise-04 (`capability-vs-safety-post-training-plan`). Depth link: [`fine-tuning-engineer-learning`](https://github.com/ml-engineering-curriculum/fine-tuning-engineer-learning).

**Evidence (selected)**

- Airbnb (Sr Staff ML, Post Training): *"Post-training experience in areas like data processing for fine-tuning; responsible LLMs; LLM alignment; reinforcement learning; efficient training and inference; language model evaluation."*
- Cresta (Staff ML): *"Deep hands-on experience with LLMs at scale."*
- Plaid (Staff ML, DFAI): *"Deep expertise in Transformers/LLMs/Foundation Models, including large-scale training or domain adaptation."*

### 15. Agentic AI systems (multi-step LLM workflows, tool use, agent frameworks)

**Status:** covered_at_peer_level · **Frequency:** 0.41 (Δ ↑ from 0.30; crossed the 0.40 watch-list line) · **Owner:** `senior-agentic-ai-engineer` (peer level-40 track) with depth links out to `agentic-ai-engineer` (L30) and `agentic-systems-architect` (L30)

**Movement this cycle.** Twelve of the 28 postings name agentic-systems experience as a staff-level responsibility — Cresta, Sentry, Airbnb Growth Platform, Airbnb GenAI, Airbnb AI Enablement, Reddit Notifications (preferred), Snap (preferred), Plaid, Chewy, Cohere Health, Uber Sr Staff Core Services, Riot ML Platform. This crosses the 0.40 line the 2026-08 packet identified as the trigger to reconsider curriculum coverage.

**Ownership rule application.** Kept as `covered_at_peer_level`. Most 2026-09 agentic postings frame the theme with the **"build agents"** verb — Sentry ("Demonstrated expertise building production-grade agentic systems and tools"), Reddit Notifications ("experience building production Agentic AI frameworks"), Airbnb GenAI ("Experience with AI technologies in automating processes and developing agentic solutions and frameworks"). That verb is peer L40 [`senior-agentic-ai-engineer-learning`](https://github.com/ai-infra-curriculum/senior-agentic-ai-engineer-learning) scope; the L30 counterparts are [`agentic-systems-architect-learning`](https://github.com/ai-infra-curriculum/agentic-systems-architect-learning) and [`agentic-ai-engineer-learning`](https://github.com/ai-infra-curriculum/agentic-ai-engineer-learning). The distinct staff-scope "cross-team agent-eval program" framing this track owns (as opposed to "build agents") shows up in only ~2 postings — Cresta ("evaluation frameworks for LLM systems beyond single-turn prompts, including robustness testing and production monitoring") and Airbnb Growth Platform ("Experience building robust testing frameworks for agent behavior validation and continuous improvement"). That is below the ≥3-posting threshold needed to justify a new exercise. mod-406 chapter 3 ([`03-llm-as-judge-org-policy.md`](lessons/mod-406-cross-team-eval-and-experimentation/03-llm-as-judge-org-policy.md) — explicitly names "chat/agentic/generative-content systems" as the modal case) and chapter 4 ([`04-shadow-canary-progressive-rollout-contract.md`](lessons/mod-406-cross-team-eval-and-experimentation/04-shadow-canary-progressive-rollout-contract.md) — model-agnostic) already carry the org-scope framework these two postings ask for.

**Revised watch-list for 2026-10.** If ≥3 postings next cycle frame the **cross-team agent-eval program** as a distinct staff verb (rather than "build agents"), add exercise `agentic-eval-and-canary-program` in mod-406 with citations. The refined trigger — cross-team-agent-eval-program framing, not agentic-frequency alone — is the shape the ownership rule tolerates.

**Evidence (selected)**

- Cresta (Staff ML): *"Demonstrated leadership in architecting complex AI systems, particularly agentic or multi-step LLM workflows."*
- Cresta (Staff ML): *"Experience designing evaluation frameworks for LLM systems beyond single-turn prompts, including robustness testing and production monitoring."*
- Airbnb (Sr Staff ML, Growth Platform, preferred): *"Experience building robust testing frameworks for agent behavior validation and continuous improvement."*
- Sentry (Staff ML, AI): *"Demonstrated expertise building production-grade agentic systems and tools."*
- Reddit (Staff ML, Notifications, preferred): *"Big Plus: experience building production Agentic AI frameworks."*
- Airbnb (Sr Staff ML, GenAI, preferred): *"Experience with AI technologies in automating processes and developing agentic solutions and frameworks."*
- Snap (Staff ML, Search Ranking, preferred): *"Experience with LLMs, foundation models, semantic search, natural language understanding, or retrieval-augmented generation."*

### 16. RAG / retrieval-augmented generation systems

**Status:** covered_at_peer_level · **Frequency:** 0.24 (Δ ≈ 0.23, below 0.30 threshold) · **Owner:** `rag-engineer`

Retrieval pipelines, embedding models, vector search, hybrid retrieval, RAG evaluation. Depth link: [`rag-engineer-learning`](https://github.com/ai-infra-curriculum/rag-engineer-learning).

**Evidence (selected)**

- Cresta (Staff ML): *"Deep expertise in transformer-based models, embeddings, retrieval systems, and Retrieval-Augmented Generation (RAG) pipelines."*
- Databricks (preferred): *"Experience with LLM fine-tuning, prompt engineering, and retrieval-augmented generation (RAG)."*
- Snap (preferred): *"Experience with LLMs, foundation models, semantic search, natural language understanding, or retrieval-augmented generation."*
- Riot Games (preferred): *"Hands-on experience with optimizing ML & AI deployments (LLMs, diffusion models, etc.) for throughput, latency and reliability."*

### 17. Eval-platform build (LLM-judge harness, agent eval infra)

**Status:** covered_at_peer_level · **Frequency:** 0.21 (new metric this cycle) · **Owner:** `model-evaluation-engineer` / `ai-eval-engineer`

Cresta and Wayve postings surface eval-platform-build depth as a staff-level responsibility; that scope is owned by [`ai-eval-engineer-learning`](https://github.com/ai-infra-curriculum/ai-eval-engineer-learning) (LLM/agent eval platform) and [`model-evaluation-engineer-learning`](https://github.com/ml-engineering-curriculum/model-evaluation-engineer-learning) (classical + generative eval depth). Staff ML engineer's slice is the org-scope eval program (§6).

### 18. ML platform build (feature store, model registry, multi-tenant clusters)

**Status:** covered_at_peer_level · **Owner:** `ai-infra-ml-platform-learning` / `ai-infra-mlops-learning`

Riot ML Platform, Reddit MLSE, and Airbnb AI Enablement each *are* staff-level platform-build roles — they blur the line between staff ML engineer and staff ML-platform engineer. Where the primary verb is "own the platform build", the role belongs on the peer platform track. Where the primary verb is "set the org-scope consumer contract for the platform", it belongs here (§5). Depth links: [`ai-infra-ml-platform-learning`](https://github.com/ai-infra-curriculum/ai-infra-ml-platform-learning), [`ai-infra-mlops-learning`](https://github.com/ai-infra-curriculum/ai-infra-mlops-learning).

### 19. ML/AI security depth (poisoning, extraction, adversarial inputs, prompt injection)

**Status:** covered_at_higher_level · **Frequency:** 0.17 (surface is classical fraud / identity-threat detection, not prompt-injection) · **Owner:** `ai-infra-security-learning` (level 35)

Explicit ML/AI security language in the 2026-09 sample shows up as classical fraud (Uber Core Services, Ramp Identity Threat, Plaid), not as prompt-injection / poisoning / model-extraction language. Awareness and delegation contract in mod-409; depth linked to [`ai-infra-security-learning`](https://github.com/ai-infra-curriculum/ai-infra-security-learning).

### 20. Governance / compliance policy authoring, regulator engagement

**Status:** covered_at_peer_level · **Owner:** `ai-governance-analyst` / `head-of-ai-governance`

Depth links: [`ai-governance-analyst-learning`](https://github.com/ai-governance-curriculum/ai-governance-analyst-learning), [`head-of-ai-governance-learning`](https://github.com/ai-governance-curriculum/head-of-ai-governance-learning).

### 21. Direct people management (1:1s, performance reviews, headcount)

**Status:** covered_at_peer_level · **Frequency:** 0 (filtered out of sample) · **Owner:** `ai-infra-team-lead-learning` (peer level-40 EM path)

None of the 28 postings sampled this cycle included direct-report management as a required-skill bullet — the sample was filtered to IC-staff postings before coding. Mentorship, coaching, and "lead by influence" language appears widely (§10) and is covered by mod-410.

### 22. Org- and industry-wide technical strategy

**Status:** covered_at_higher_level · **Owner:** `principal-ml-engineer` (level 50)

Staff scope stops at multi-team + exec-audience communication; principal scope extends to industry-wide framing. Where a posting reads as principal-scoped, refer up rather than growing this track.

### 23. Product-integrated GenAI / LLM in production (new emerging theme)

**Status:** covered_at_lower_level · **Frequency:** 0.69 (new theme this cycle) · **Owner:** `ml-engineer` (L20) with peer specialists `llm-application-developer` and `applied-ai-engineer`

**Movement this cycle.** The dominant signal in the 2026-09 sample: 20 of 28 postings require the candidate to have shipped LLM-augmented product features in production. Reddit Notifications: *"Experience working with LLM in production and utilizing generative AI to augment recommendation systems."* Databricks: *"Shape the direction of our applied AI areas and intelligence features in our products."* Airbnb AI Enablement: *"Passionate about AI with a strong grasp of current trends in Generative AI, LLMs, and related technologies."* DoorDash: *"Define and lead DoorDash's cutting-edge AI vision for logistics: an LLM-inspired foundation model for intelligence across logistics."* Cohere Health, Zocdoc, Chewy, Riot, Ramp, Pinterest, Uber, Cresta, Sentry, Snap, Plaid, Workiva, Airbnb GenAI, Airbnb Post-Training, and Reddit ML Understanding echo it.

**Ownership rule application.** Kept as `covered_at_lower_level`. The verb is "you have shipped LLMs in production" — a résumé prerequisite, not a distinct staff-scope program-ownership verb. Staff-scope framing for LLM-in-product portfolios is already carried by mod-402 (multi-team ML architecture — the LLM-augmented recommender fits inside the portfolio), mod-403 (foundation-model programs — the DoorDash logistics FM is the archetype), mod-405 (platform strategy for LLM serving — Riot's KServe/Triton stack fits here), and mod-406 (cross-team eval for generative systems — the LLM-as-judge chapter already names generative/agentic systems as the modal case).

**Watch-list for 2026-10.** If this signal starts framing "own the org-wide GenAI-in-product platform" as a distinct staff verb (rather than "you have shipped LLM systems before"), consider one exercise in mod-405 titled `llm-in-product-paved-road-strategy`. Not proposed this cycle because the verb is prerequisite-shaped, not program-owner-shaped.

### 24. Graph Neural Networks / embedding retrieval / vector-DB systems (new emerging theme)

**Status:** covered_at_peer_level · **Frequency:** 0.21 (new theme this cycle, below 0.30 threshold) · **Owner:** `senior-ml-engineer` (ranking/embedding modelling) / `rag-engineer` (vector DB stack)

Six of 28 postings surface graph-based representation learning or vector-DB stack expertise as required or preferred: Reddit LS Embedding (primary — "Expertise in Graph Neural Networks (GNNs), graph-based representation learning, and transformer architectures"), Reddit MLSE (preferred — Neo4j / PyTorch Geometric / DGL), ZipRecruiter (preferred — "Two-Tower Neural Networks, Graph Neural Networks (GNNs)"), Snap Search Ranking, Riot ML Platform (Milvus vector DB), Pinterest Monetization. Portfolio-scope contract already carried by mod-402. Below threshold; watch for movement above 0.30 next cycle.

---

## Continuity summary

- All 10 requirement themes owned by this track pass the "covered" test.
- No requirement crosses the ≥3-posting AND ≥0.30-frequency bar for a new module or exercise that is not already covered by an existing module or by a peer track per the ownership rule.
- Agentic AI (§15) crossed the 0.40 line last cycle's packet flagged, but the postings' framing is majority "build agents" (peer L40 [`senior-agentic-ai-engineer-learning`](https://github.com/ai-infra-curriculum/senior-agentic-ai-engineer-learning) scope) rather than "cross-team agent-eval program" (this track's slice). Only ~2 postings frame the distinct staff-scope verb — below the ≥3 threshold. Trigger refined for 2026-10: watch for the cross-team-agent-eval-program framing specifically.
- Product-integrated GenAI (§23, new theme this cycle at 0.69) is the dominant signal but reads as inherited prerequisite from L20 / L25 peer tracks rather than a staff-scope-only gap; the staff-scope program framing is already carried by mod-402 / mod-403 / mod-405 / mod-406.
- Responsible-AI (§9) dropped from 0.20 to 0.14. Two consecutive cycles below 0.15 would trigger a proposal to demote mod-409 hours; one cycle is not enough.
- The [`.aicg/curriculum-plan-delta.json`](.aicg/curriculum-plan-delta.json) emitted this cycle contains empty `modules`, `exercises`, and `projects` arrays with a rationale that references this file.
