# Job Requirements — Staff Machine Learning Engineer

> **Cycle:** 2026-08 (previous: 2026-07 bootstrap, no live postings)
> **Postings reviewed:** 30 distinct in-window postings from three segments — AI-native labs, big tech / consumer, and fintech / health / autonomy / auto (last 90 days: 2026-05-09 → 2026-08-07).
> **Delta recommendation:** **no change** — the existing 10-module curriculum covers every requirement that clears the ≥3-posting AND ≥0.30-frequency bar. Every emerging theme (agentic AI depth, RAG systems, LLM/agent eval-harness engineering, RLHF/DPO/GRPO post-training) either belongs to a peer specialist / peer L40 track per the ownership rule, or falls below the frequency threshold. [`.aicg/curriculum-plan-delta.json`](.aicg/curriculum-plan-delta.json) is emitted with empty additions arrays.
> **Machine-readable index:** [`.aicg/job-requirements.json`](.aicg/job-requirements.json)

## How this file is organized

Requirements are grouped by ownership. For each requirement we list:

- **Status** — `covered`, `covered_at_lower_level`, `covered_at_peer_level`, or `covered_at_higher_level`.
- **Frequency** — share of the 30-posting sample that names the requirement (additive-change threshold is 0.30).
- **Δ vs 2026-07** — direction of change since the July cycle (↑ / ↓ / — / *new*). Every requirement is marked *new* this cycle because the prior packet ran under a WebSearch/WebFetch denial and had no live evidence.
- **Owner** — role that primarily teaches it. When ownership sits at a lower or peer level, this file links out rather than duplicating.
- **Curriculum coverage** — module / project paths inside this repo, or a link to the sibling track that owns it.
- **Evidence** — verbatim quotes from postings. Full posting URLs and bullets are in the JSON index.

## Sampling notes

**Title filter applied.** The sample counts only postings with one of: "Staff Machine Learning Engineer", "Staff ML Engineer", "Staff Software Engineer, Machine Learning", "Staff Engineer, ML Systems", "Staff AI/ML Engineer", "Senior Staff Machine Learning Engineer", "Senior / Staff Machine Learning Engineer / Research Engineer", "Lead ML Engineer", "ML Tech Lead", or "Staff MLOps Engineer" where the primary verb is ML-modeling or ML-platform (not people management). Excluded: plain "Machine Learning Engineer" (no seniority), "Senior Machine Learning Engineer" (L30), "Principal" (L50), any "Engineering Manager" title, "Research Scientist" / "Applied Scientist" where the primary output is publications, and "Staff Data Scientist".

**Title-convention gap.** Pure AI-native labs (OpenAI, xAI, Perplexity, Cohere, Character.AI, ElevenLabs, Runway, and much of Anthropic) use "Member of Technical Staff" (MoTS) as their L4/L5 title rather than "Staff …", so the AI-native segment leans on Databricks, Scale AI, Anthropic's Labs Applied AI SWE opening, Cresta, Sentry, Primer, Lila Sciences, Xometry, and Kodiak. This is not a coverage bug — the requirements gathered from these employers still describe the level-40 IC-staff scope this track owns; the vocabulary of the postings is just different.

**Employer coverage by segment (30 postings):** AI-native (10), consumer / big tech (10), fintech / health / autonomy / auto (10). Employers appearing more than once: Uber (3), Reddit (3), Airbnb (2), Scale AI (2), GM (2), Plaid (2). Full posting list in [`.aicg/job-requirements.json`](.aicg/job-requirements.json).

## Seniority differentiation — what changes from level 30

The single most reliable signal in a Staff Machine Learning Engineer posting is the **shift in the object of the verb**. Uber's Applied-AI posting states this explicitly: *"At this level, you will not just build models — you will shape technical direction across teams, influence product strategy, and deliver measurable impact."* Cresta: *"Proven ability to influence technical direction across teams as a senior individual contributor."* Apptronik: *"lead by influence across teams — setting standards that other engineers adopt voluntarily."* GM: *"leading technically ambiguous, cross-team infrastructure initiatives."*

A level-30 posting says *"own the ranking model"*, *"lead the fraud detection team"*, *"run the experimentation review"*, *"author the model card"*. A level-40 posting says *"own the ranking, retrieval, and personalization portfolio"*, *"lead the ML org's build-vs-buy on the eval platform"*, *"run the cross-team experiment review body"*, *"author the AI-risk register the CISO signs off on"*, *"shape the multi-quarter modeling roadmap that leadership commits to"*, *"calibrate the senior ML engineers we hire"*.

### Staff-plus IC vs. Engineering Manager at level 40

Two distinct level-40 role families coexist in ML orgs, both hired against the "staff-plus" bar but with different day-to-day scope:

- **Staff Machine Learning Engineer (this track)** — IC-staff path. Multi-team technical leadership without direct reports. Owns architecture, program, platform strategy, and the technical bar.
- **Engineering Manager, ML** — people-leadership path. Owned by the peer track [`ai-infra-team-lead-learning`](https://github.com/ai-infra-curriculum/ai-infra-team-lead-learning).

None of the 30 postings sampled this cycle included direct-report management as a required-skill bullet; the sample was filtered to IC-staff postings before coding.

---

## 1. Staff role scope, staff-plus archetypes, multi-team contracts

**Status:** covered · **Frequency:** 0.87 (Δ *new*) · **Owner:** `staff-ml-engineer`

The staff-plus IC archetypes (Tech Lead, Architect, Solver, Right Hand), the shift from single-team ownership to multi-team ownership, the hand-off contracts to the manager peer (`ai-infra-team-lead`) and to specialist tracks.

**Curriculum coverage**

- [`lessons/mod-401-staff-ml-role-scope/`](lessons/mod-401-staff-ml-role-scope)

**Evidence (selected)**

- Uber (Staff ML, Applied AI): *"At this level, you will not just build models — you will shape technical direction across teams, influence product strategy, and deliver measurable impact."*
- Cresta (Staff ML): *"Proven ability to influence technical direction across teams as a senior individual contributor."*
- Wayve (Staff ML, AV Core): *"Staff-level technical leadership: research-literate and pragmatic, setting direction, raising the bar, and leading cross-functional work without formal line management."*
- Airbnb (Staff ML, AI Experience): *"9+ years of professional machine learning engineering experience, including proven leadership on AI/ML projects at scale, ideally at the Staff Engineer level or above."*
- Apptronik (Staff MLOps): *"Demonstrated ability to lead by influence across teams — setting standards that other engineers adopt voluntarily."*

## 2. Multi-team ML systems architecture

**Status:** covered · **Frequency:** 0.47 (Δ *new*) · **Owner:** `staff-ml-engineer`

Portfolio-scope blueprints — shared data planes, shared feature layer, shared model-registry contract, shared eval contract; identify duplication and drive consolidation; author multi-system RFCs and defend them in architecture reviews.

**Curriculum coverage**

- [`lessons/mod-402-multi-team-ml-architecture/`](lessons/mod-402-multi-team-ml-architecture)
- Capstone: [`projects/project-401-multi-team-ml-blueprint/`](projects/project-401-multi-team-ml-blueprint)

**Evidence (selected)**

- GM (Staff ML, ML Training Infra): *"Demonstrated track record of leading technically ambiguous, cross-team infrastructure initiatives and driving them to measurable impact."*
- Plaid (Staff ML, DFAI): *"Ability to drive technical alignment across teams: setting standards, defining integration patterns, and influencing beyond your immediate scope."*
- Cresta: *"Demonstrated leadership in architecting complex AI systems, particularly agentic or multi-step LLM workflows."*
- Reddit (Staff ML, ML Efficiency): *"Contributions to internal platforms used by multiple ML teams."*
- Apptronik: *"Own the technical direction for the MLOps platform — define subsystem interfaces, drive architecture decisions, and establish engineering standards."*

## 3. Foundation-model / large-scale fine-tune training programs

**Status:** covered · **Frequency:** 0.40 (Δ *new*) · **Owner:** `staff-ml-engineer` (program) → peer specialists (`training-pipeline-engineer`, `fine-tuning-engineer`) for implementation

Scope pretraining and large-scale fine-tune programs: compute-optimal parameter/data trade-offs, scaling-law-driven roadmap, data mixture and curriculum, ablation sequencing, capability vs. safety post-training investment, executive framing of the program's cost and risk.

**Curriculum coverage**

- [`lessons/mod-403-foundation-model-training-programs/`](lessons/mod-403-foundation-model-training-programs)
- Capstone: [`projects/project-402-foundation-model-training-program/`](projects/project-402-foundation-model-training-program)
- Peer specialist depth: `training-pipeline-engineer-learning`, `fine-tuning-engineer-learning`

**Evidence (selected)**

- Plaid (Staff ML, DFAI): *"You will lead the technical strategy and development of Plaid's foundation models, driving key decisions."*
- Waymo (Staff ML, VLM/LLM Evaluation): *"Lead the development of end-to-end evaluation systems and benchmarks for Waymo Foundation models."*
- Tempus AI (Staff ML, Oncology Foundation Model): *"Your work will directly enable the training and deployment of robust, production-ready multimodal systems."*
- Databricks: *"Strong track record of working with language modeling technologies (generative and embedding techniques, modern model architectures, fine tuning/pre-training datasets, evaluation benchmarks)."*
- Wayve: *"Experience across foundations/pretraining and applied engineering teams; large-scale training infrastructure and/or agentic workflows."*

## 4. Distributed-training strategy at scale

**Status:** covered · **Frequency:** 0.50 (Δ *new*) · **Owner:** `staff-ml-engineer` (strategy) → peer specialists for implementation

Parallelism recipe (DDP, FSDP/ZeRO, tensor-parallel, pipeline-parallel, 3D-parallel), MFU estimation, failure-mode budgeting, cluster-hour defence to leadership.

**Curriculum coverage**

- [`lessons/mod-404-distributed-training-systems-at-scale/`](lessons/mod-404-distributed-training-systems-at-scale)
- Peer implementation depth: `training-pipeline-engineer-learning`, `ai-infra-performance-learning`

**Evidence (selected)**

- GM (Staff ML, ML Training Infra): *"Experience designing and developing training platforms that support FSDP, pipeline parallelism, and other scalable solutions for training large foundational models."*
- Lila Sciences: *"Experience with distributed ML training frameworks (Megatron-LM, TorchTitan, DeepSpeed, Ray)."*
- Reddit (Staff ML, ML Systems): *"Deep experience working with distributed training frameworks, including Ray and Kubernetes."*
- Stripe: *"Experience with distributed ML training systems, accelerator-backed compute, training data pipelines, experiment tracking, and model evaluation."*
- Kodiak: *"Experience working with large-scale distributed training systems."*

## 5. ML platform strategy at org scope

**Status:** covered · **Frequency:** 0.40 (Δ *new*) · **Owner:** `staff-ml-engineer` (consumer strategy) → peer platform tracks for build

Build-vs-adopt on feature store, model registry, eval platform, training platform, inference gateway; paved-road roadmap negotiation; contribute-back and gap RFCs; multi-team consumer contract.

**Curriculum coverage**

- [`lessons/mod-405-ml-platform-strategy/`](lessons/mod-405-ml-platform-strategy)
- Peer platform build depth: `ai-infra-ml-platform-learning`, `ai-infra-mlops-learning`

**Evidence (selected)**

- Stripe (Staff SWE, ML Platform): *"Experience building and operating production ML platform in one or more areas such as model training, model serving, orchestration, or ML data systems, with requirements for performance, reliability, scalability, and cost efficiency."*
- Apptronik (Staff MLOps): *"Proven experience owning and delivering an MLOps platform end-to-end — dataset lifecycle, experiment tracking, model registry, evaluation, and serving — at a company that ships models to production."*
- Reddit (Staff ML, ML Systems): *"Hands-on experience administering and integrating MLOps tools for experiment tracking, model serving, and model registries (e.g. MLflow or Wandb)."*
- Plaid (Staff ML, DFAI): *"Experience defining ML platform capabilities (serving infra, feature stores) used across multiple teams."*
- Tempus AI: *"Experience with MLOps tools and platforms (e.g., MLflow, Kubeflow, SageMaker Pipelines)."*

## 6. Cross-team evaluation & experimentation program

**Status:** covered · **Frequency:** 0.33 (Δ *new*) · **Owner:** `staff-ml-engineer`

Standardise the offline eval harness across ML teams; LLM-as-judge policy; shadow/canary/progressive-rollout contract; cross-team experiment review; evaluation of LLM / agent / VLM systems at program scope.

**Curriculum coverage**

- [`lessons/mod-406-cross-team-eval-and-experimentation/`](lessons/mod-406-cross-team-eval-and-experimentation)
- Peer platform build depth: [`ai-eval-engineer-learning`](https://github.com/ai-infra-curriculum/ai-eval-engineer-learning) (LLM/agent eval platform), [`model-evaluation-engineer-learning`](https://github.com/ml-engineering-curriculum/model-evaluation-engineer-learning) (classical + generative eval depth)

**Evidence (selected)**

- Waymo (Staff ML, VLM/LLM Evaluation): *"Lead the development of end-to-end evaluation systems and benchmarks for Waymo Foundation models."*
- Cresta (Staff ML): *"Experience designing evaluation frameworks for LLM systems beyond single-turn prompts, including robustness testing and production monitoring."*
- Airbnb (Staff ML, AI Experience): *"Demonstrated success improving AI data quality through synthetic data techniques, adversarial testing, or advanced data annotation pipelines."*
- Lila Sciences: *"Experience building evaluation harnesses, model monitoring, or quality dashboards."*
- Apptronik: *"Experience defining evaluation and qualification frameworks for ML models where the cost of a regression is high (robotics, safety-critical, or production-customer-facing)."*

## 7. GPU capacity, cost, and TCO — portfolio management

**Status:** covered · **Frequency:** 0.17 (Δ *new*) · **Owner:** `staff-ml-engineer`

Portfolio-level demand forecast, capacity portfolio (reserved / on-demand / spot / multi-cloud / self-hosted), FinOps discipline, TCO model a CFO can read.

**Curriculum coverage**

- [`lessons/mod-407-capacity-cost-and-tco/`](lessons/mod-407-capacity-cost-and-tco)

**Evidence (selected)**

- Reddit (Staff ML, ML Efficiency): *"Experience optimizing cloud infrastructure costs across large ML workloads."*
- Cresta: *"Strong systems thinking: ability to design for scalability, latency constraints, cost efficiency, security, and long-term maintainability."*
- Stripe: *"…with requirements for performance, reliability, scalability, and cost efficiency."*

**Frequency note.** Explicit cost/TCO language surfaces mainly in postings whose primary responsibility is platform ownership. Implicit in the "own the program" language of nearly every staff posting. Kept at planned scope; watch on the next cycle in case explicit TCO language becomes a broader requirement.

## 8. Portfolio-scope ML reliability, SLOs, multi-team incident command

**Status:** covered · **Frequency:** 0.23 (Δ *new*) · **Owner:** `staff-ml-engineer`

Standardise ML-specific SLI/SLO vocabulary; severity matrix for ML incidents; cross-team incident command adapted to ML failure classes; org-standard postmortem template; retraining-as-deploy hygiene.

**Curriculum coverage**

- [`lessons/mod-408-portfolio-reliability-and-incident-command/`](lessons/mod-408-portfolio-reliability-and-incident-command)

**Evidence (selected)**

- Sentry (Staff ML, AI): *"Build state-of-the-art agentic AI systems to triage, debug, and solve real production issues."*
- Uber (Senior Staff ML, Core Services): *"We proactively safeguard the platform against the evolving landscape of AI-driven fraud, ensuring safety and trust remain at the core."*
- Wayve: *"Experience with redundant or fallback architectures, safety-critical systems."*
- Apptronik: *"Familiarity with policy gating, shadow deployments, or staged rollout strategies for autonomy."*
- Kodiak: *"Design and deploy machine learning systems that improve our vehicles' ability to understand the world, predict behavior of other road users, and make safe driving decisions."*

**Frequency note.** Explicit incident-command language clusters in safety-critical domains (autonomy) and production-AI-quality domains. The severity-matrix and postmortem artifacts remain differentiated at staff scope.

## 9. Responsible AI at portfolio scope

**Status:** covered · **Frequency:** 0.20 (Δ *new*) · **Owner:** `staff-ml-engineer` (ML-side program) → `ai-governance-analyst` / `head-of-ai-governance` / `ai-infra-security-learning` for depth

Portfolio-scope AI risk register (NIST AI RMF + Gen-AI profile); EU AI Act classification; org-standard model card and datasheet format; pre-launch review gate; red-team cadence and escalation path to head-of-ai-governance.

**Curriculum coverage**

- [`lessons/mod-409-responsible-ai-at-portfolio-scope/`](lessons/mod-409-responsible-ai-at-portfolio-scope)

**Evidence (selected)**

- Airbnb (Staff ML, AI Experience): *"Passion for responsible and ethical AI, designing solutions with guardrails that prioritize accuracy, user trust, and inclusiveness."*
- Wayve: *"you will help shape what our end-to-end driving model must understand to be safe and reliable."*
- Kodiak: *"…make safe driving decisions."*
- Xometry: *"Must be a U.S. Citizen or Green Card holder (ITAR compliance)."*
- Anthropic (Staff SWE, Labs Applied AI): *"Communicate effectively and can make complex AI capabilities feel intuitive to people who don't think in software."*

**Frequency note.** Explicit responsible-AI language surfaces in three flavours: (a) consumer-scale ML with public trust surface (Airbnb), (b) safety-critical autonomy (Wayve, Kodiak, Apptronik), (c) export-controlled applied ML (Xometry ITAR). Below the 0.30 additive-change threshold but non-negotiable at staff scope.

## 10. Staff-plus technical leadership — roadmaps, exec comms, hiring calibration, coaching seniors

**Status:** covered · **Frequency:** 0.67 (Δ *new*) · **Owner:** `staff-ml-engineer`

Multi-quarter ML program roadmap that leadership commits to; executive-audience one-pagers and RFCs; staff-plus hiring loops; coach senior tech leads and partner with the EM peer; vendor and partnership judgement calls.

**Curriculum coverage**

- [`lessons/mod-410-staff-plus-technical-leadership/`](lessons/mod-410-staff-plus-technical-leadership)
- Capstone: [`projects/project-403-staff-plus-tech-leadership-simulation/`](projects/project-403-staff-plus-tech-leadership-simulation)

**Evidence (selected)**

- Uber (Staff ML, Applied AI): *"Defined long-term technical roadmaps adopted across orgs. Elevated engineering standards through mentorship and technical leadership."*
- GM (Staff ML, ML Training Infra): *"Excellent communication skills, with the ability to build consensus, navigate controversial decisions, communicate risks clearly, and provide constructive technical feedback."*
- Xometry: *"A proven ability to communicate effectively with all levels of the organization, from executives to product managers and various stakeholders."*
- Stripe: *"Track record of serving as a technical lead, with the ability to provide technical direction, lead multi-team initiatives, and mentor team members."*
- Reddit (Senior Staff ML, Notifications): *"Proven ability to identify key opportunities, define roadmaps and drive scalable improvement in notifications relevance."*
- Tempus AI: *"Leadership and collaboration skills including proven ability to bring thought leadership, mentor junior engineers, collaborate cross-functionally, communicate complex concepts."*

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

**Status:** covered_at_peer_level · **Frequency:** 0.13 (below 0.30 threshold) · **Owner:** `fine-tuning-engineer`

Explicit RLHF/RLVR/GRPO/PPO/MoE-training language appears in Scale AI (post-training + agents), Lila, and Primer. Program-level trade-off framing is captured in mod-403 exercise-04 (`capability-vs-safety-post-training-plan`). Depth link: [`fine-tuning-engineer-learning`](https://github.com/ml-engineering-curriculum/fine-tuning-engineer-learning).

**Evidence (selected)**

- Scale AI (Staff MLRE, Agent Post-training): *"Experience with post-training methods like RLHF/RLVR and related algorithms like PPO/GRPO etc."*
- Lila Sciences: *"Experience with RL post-training, such as RLHF, GRPO, or tool-augmented RL. Experience training MoE architectures."*

### 15. Agentic AI systems (multi-step LLM workflows, tool use, agent frameworks)

**Status:** covered_at_peer_level · **Frequency:** 0.30 (at threshold) · **Owner:** `senior-agentic-ai-engineer` (peer level-40 track) with depth links out to `agentic-ai-engineer` (L30) and `agentic-systems-architect`

This is the top rising theme this cycle. Nine of the 30 postings name agentic-systems experience as a staff-level responsibility — Scale AI (Agents), Sentry, Cresta, Primer, Stripe (as an "agentic AI patterns" preferred), Lila, Reddit (Notifications), Airbnb (AI Experience), Apptronik (RL for embodied agents).

**Ownership rule application.** The peer L40 track [`senior-agentic-ai-engineer-learning`](https://github.com/ai-infra-curriculum/senior-agentic-ai-engineer-learning) owns the multi-team agentic program depth; [`agentic-systems-architect-learning`](https://github.com/ai-infra-curriculum/agentic-systems-architect-learning) owns single-org agentic architecture; [`agentic-ai-engineer-learning`](https://github.com/ai-infra-curriculum/agentic-ai-engineer-learning) owns single-team agentic build. The Staff Machine Learning Engineer's contribution is the delegation-contract framing already covered by mod-406 (cross-team eval — an agent eval program is a special case) and mod-405 (platform strategy for agent frameworks), plus the program-scope trade-off framing in mod-403.

**Watch-list next cycle.** If this frequency moves above 0.40 with staff-scope framing that the peer track does not cover (e.g., a distinct "own the cross-team agentic program" verb rather than a "build agents" verb), consider adding one exercise in mod-406 titled `agentic-eval-and-canary-program`. Not proposed this cycle because the threshold is only just met and the peer track already covers the multi-team framing.

**Evidence (selected)**

- Sentry (Staff ML, AI): *"Production-grade agentic systems experience."*
- Cresta: *"Demonstrated leadership in architecting complex AI systems, particularly agentic or multi-step LLM workflows."*
- Primer.ai: *"Hands-on depth with LLMs and agentic systems (prompt and context engineering, tool use, retrieval and RAG) and the broader ML toolkit."*
- Stripe (preferred): *"Familiarity with LLMs, LLM application frameworks, and agentic AI patterns (e.g., tool use, multi-agent orchestration, retrieval-augmented generation)."*
- Reddit (Senior Staff, Notifications, preferred): *"Big Plus: experience building production Agentic AI frameworks."*

### 16. RAG / retrieval-augmented generation systems

**Status:** covered_at_peer_level · **Frequency:** 0.23 (below 0.30 threshold) · **Owner:** `rag-engineer`

Retrieval pipelines, embedding models, vector search, hybrid retrieval, RAG evaluation. Depth link: [`rag-engineer-learning`](https://github.com/ai-infra-curriculum/rag-engineer-learning).

**Evidence (selected)**

- Databricks (preferred): *"Experience with LLM fine-tuning, prompt engineering, and retrieval-augmented generation (RAG)."*
- Cresta: *"Deep expertise in transformer-based models, embeddings, retrieval systems, and Retrieval-Augmented Generation (RAG) pipelines."*
- Snap (preferred): *"Experience with candidate generation, retrieval models, ANN search, embeddings, vector search, or two-stage ranking architectures."*
- Airbnb: *"Hands-on experience building and shipping generative AI solutions, emphasizing Retrieval-Augmented Generation (RAG), agentic workflows, and large language models."*

### 17. Eval-platform build (LLM-judge harness, agent eval infra)

**Status:** covered_at_peer_level · **Owner:** `model-evaluation-engineer` / `ai-eval-engineer`

Waymo VLM/LLM Eval and Cresta agent-eval postings surface eval-platform-build depth as a staff-level responsibility; that scope is owned by [`ai-eval-engineer-learning`](https://github.com/ai-infra-curriculum/ai-eval-engineer-learning) (LLM/agent eval platform) and [`model-evaluation-engineer-learning`](https://github.com/ml-engineering-curriculum/model-evaluation-engineer-learning) (classical + generative eval depth). Staff ML engineer's slice is the org-scope eval program (§6).

### 18. ML platform build (feature store, model registry, multi-tenant clusters)

**Status:** covered_at_peer_level · **Owner:** `ai-infra-ml-platform-learning` / `ai-infra-mlops-learning`

Apptronik, Stripe, and Reddit ML Systems each *are* staff-level platform-build roles — they blur the line between staff ML engineer and staff ML-platform engineer. Where the primary verb is "own the platform build", the role belongs on the peer platform track. Where the primary verb is "set the org-scope consumer contract for the platform", it belongs here (§5). Depth links: [`ai-infra-ml-platform-learning`](https://github.com/ai-infra-curriculum/ai-infra-ml-platform-learning), [`ai-infra-mlops-learning`](https://github.com/ai-infra-curriculum/ai-infra-mlops-learning).

### 19. ML/AI security depth (poisoning, extraction, adversarial inputs, prompt injection)

**Status:** covered_at_higher_level · **Owner:** `ai-infra-security-learning` (level 35)

Awareness and delegation contract in mod-409; depth linked to [`ai-infra-security-learning`](https://github.com/ai-infra-curriculum/ai-infra-security-learning).

### 20. Governance / compliance policy authoring, regulator engagement

**Status:** covered_at_peer_level · **Owner:** `ai-governance-analyst` / `head-of-ai-governance`

Depth links: [`ai-governance-analyst-learning`](https://github.com/ai-governance-curriculum/ai-governance-analyst-learning), [`head-of-ai-governance-learning`](https://github.com/ai-governance-curriculum/head-of-ai-governance-learning).

### 21. Direct people management (1:1s, performance reviews, headcount)

**Status:** covered_at_peer_level · **Frequency:** 0 (filtered out of sample) · **Owner:** `ai-infra-team-lead-learning` (peer level-40 EM path)

None of the 30 postings sampled this cycle included direct-report management as a required-skill bullet — the sample was filtered to IC-staff postings before coding. Mentorship, coaching, and "lead by influence" language appears widely (§10) and is covered by mod-410.

### 22. Org- and industry-wide technical strategy

**Status:** covered_at_higher_level · **Owner:** `principal-ml-engineer` (level 50)

Staff scope stops at multi-team + exec-audience communication; principal scope extends to industry-wide framing. Where a posting reads as principal-scoped, refer up rather than growing this track.

---

## Continuity summary

- All 10 requirement themes owned by this track pass the "covered" test.
- No requirement crosses the ≥3-posting AND ≥0.30-frequency bar for a new module or exercise that is not already covered by an existing module or by a peer track per the ownership rule.
- Agentic AI (§15) sits exactly on the 0.30 threshold but is owned by the peer L40 [`senior-agentic-ai-engineer-learning`](https://github.com/ai-infra-curriculum/senior-agentic-ai-engineer-learning) track and by the L30 [`agentic-systems-architect-learning`](https://github.com/ai-infra-curriculum/agentic-systems-architect-learning) / [`agentic-ai-engineer-learning`](https://github.com/ai-infra-curriculum/agentic-ai-engineer-learning) tracks. Watch-list for next cycle if the frequency rises above 0.40 with staff-scope framing.
- The [`.aicg/curriculum-plan-delta.json`](.aicg/curriculum-plan-delta.json) emitted this cycle contains empty `modules`, `exercises`, and `projects` arrays with a rationale that references this file.
