# Job Requirements — Staff Machine Learning Engineer

> **Cycle:** 2026-10 (previous: 2026-09 against 28 in-window postings)
> **Postings reviewed:** 41 distinct in-window postings — 28 from 2026-09 still in the current window (date_observed 2026-09-07) plus 13 observed on 2026-10-07 refreshing the sample across AI-native, big-tech, consumer, fintech, autonomy, health, auto, applied-industrial, and defense segments (last 90 days: 2026-07-09 → 2026-10-07).
> **Delta recommendation:** **no change** — the existing 10-module curriculum still covers every requirement that clears the ≥3-posting AND ≥0.30-frequency bar. Three signals moved this cycle: (a) the agentic-AI theme rose a second consecutive cycle (0.30 → 0.41 → 0.49) and the specific "cross-team agent-eval program as staff verb" framing (the 2026-09 refined exercise trigger) now clears ≥3 postings (7 postings: Robinhood, Scale AI Public Sector, Scale AI General Agents, Anthropic Multi-Agent, Pinterest Shopping, plus prior Cresta and Airbnb Growth Platform), but the packet-wide ≥0.30 frequency bar remains unmet (7/41 = 0.17), and the ownership rule continues to assign the "build agents" verb to peer L40 `senior-agentic-ai-engineer`; (b) a NEW emerging sub-theme — action-level guardrails for agentic systems (permission and tool-scoping, approval gates, blast-radius controls, sandboxing) — surfaced at Robinhood and Scale AI Public Sector (2/41 = 0.05, below the ≥3-posting bar); (c) explicit responsible-AI dropped a second consecutive cycle (0.20 → 0.14 → 0.12), activating the 2026-09 two-cycles-below-0.15 demotion-proposal trigger. Removals are out of scope for this packet. [`.aicg/curriculum-plan-delta.json`](.aicg/curriculum-plan-delta.json) is emitted with empty additions arrays.
> **Machine-readable index:** [`.aicg/job-requirements.json`](.aicg/job-requirements.json)

## How this file is organized

Requirements are grouped by ownership. For each requirement we list:

- **Status** — `covered`, `covered_at_lower_level`, `covered_at_peer_level`, or `covered_at_higher_level`.
- **Frequency** — share of the 41-posting sample that names the requirement (additive-change threshold is 0.30).
- **Δ vs 2026-09** — direction of change since the previous cycle (↑ / ↓ / — / *new*).
- **Owner** — role that primarily teaches it. When ownership sits at a lower or peer level, this file links out rather than duplicating.
- **Curriculum coverage** — module / project paths inside this repo, or a link to the sibling track that owns it.
- **Evidence** — verbatim quotes from postings. Full posting URLs and bullets are in the JSON index.

## Sampling notes

**Title filter applied.** The sample counts only postings with one of: "Staff Machine Learning Engineer", "Staff ML Engineer", "Staff Software Engineer, Machine Learning", "Staff Engineer, ML Systems / ML Platform / ML Infra", "Staff AI/ML Engineer", "Senior Staff Machine Learning Engineer", "Lead ML Engineer", "ML Tech Lead", "ML Research Engineer" (where the primary output is production ML, not publications), or "Staff MLOps Engineer" where the primary verb is ML-modeling or ML-platform (not people management). Excluded: plain "Machine Learning Engineer" (no seniority), "Senior Machine Learning Engineer" (L30), "Principal" (L50), any "Engineering Manager" title, "Research Scientist" / "Applied Scientist" where the primary output is publications, "Staff Data Scientist", and "Tech Lead Manager" (people-management path).

**Carry-over policy.** 28 postings observed on 2026-09-07 remain within the current 90-day window (2026-07-09 → 2026-10-07) and are retained in the sample without re-fetching. 13 new postings observed on 2026-10-07 were added: Anthropic ×2 (Code RL, Multi-Agent Scaling), Spotify Personalization, Robinhood AI Platform & Agentic Apps, Reddit Ads ML Efficiency, Scale AI ×2 (Public Sector, General Agents), Pinterest Shopping Ads, Babylist, Breeze Risk, LinkedIn Bengaluru, Hive, PlusAI RL Motion.

**Title-convention gap narrowed.** Pure AI-native labs (OpenAI, xAI, Perplexity, Cohere, Character.AI, ElevenLabs, Runway) use "Member of Technical Staff" (MoTS) as their L4/L5 title rather than "Staff …". This cycle, Anthropic's "Staff Software Engineer" and "Staff Research Engineer" titles (Code RL and Multi-Agent Scaling) qualify under the title filter and were included. OpenAI/xAI/Perplexity continued to use MoTS titles not matching the filter.

**Big-tech FAANG gap.** Apple / Meta / Google / Amazon ATS pages continued to 403/404 on WebFetch this cycle. LinkedIn (Bengaluru) was the only direct FAANG-tier posting captured. The sample continues to skew toward consumer marketplaces (Airbnb ×4, Reddit ×5, plus Uber, DoorDash, Pinterest ×2, Snap, Discord, Chewy, Riot, Hinge, Zocdoc, Spotify, Babylist) because those employers index highly on public boards. Segment mix (41 postings): AI-native (5), big-tech (1), consumer (18), fintech (3), autonomy (3), health (2), auto (1), applied-industrial (3), defense (1), with Reddit (5) and Airbnb (4) the only employers appearing more than once. Full posting list in [`.aicg/job-requirements.json`](.aicg/job-requirements.json).

## Seniority differentiation — what changes from level 30

The single most reliable signal in a Staff Machine Learning Engineer posting is the **shift in the object of the verb**. Cresta: *"Define and lead the technical vision for Cresta's next-generation Agentic AI systems."* Plaid: *"You will serve as the technical lead for the full machine learning lifecycle, overseeing everything from data curation and experimentation to production deployment."* GM: *"Demonstrated track record of leading technically ambiguous, cross-team infrastructure initiatives and driving them to measurable impact."* Reddit LS Embedding: *"You will own the technical direction for large-scale embedding models, guiding the development of state-of-the-art graph-based ML architectures."* 2026-10 additions: Spotify Personalization: *"Drive technical direction in ambiguous problem spaces and contribute to the long-term architecture of personalization systems."* Scale AI Public Sector: *"Own and evolve shared agentic infrastructure and core libraries, enabling reuse across teams, products, and Public Sector contracts."* Pinterest Shopping Ads: *"You will also serve as the technical lead for ML in this space — reporting to a Director and acting as the first ML Engineering hire in this org."*

A level-30 posting says *"own the ranking model"*, *"lead the fraud detection team"*, *"run the experimentation review"*, *"author the model card"*. A level-40 posting says *"own the ranking, retrieval, and personalization portfolio"*, *"lead the ML org's build-vs-buy on the eval platform"*, *"run the cross-team experiment review body"*, *"author the AI-risk register the CISO signs off on"*, *"shape the multi-quarter modeling roadmap that leadership commits to"*, *"calibrate the senior ML engineers we hire"*.

### Staff-plus IC vs. Engineering Manager at level 40

Two distinct level-40 role families coexist in ML orgs, both hired against the "staff-plus" bar but with different day-to-day scope:

- **Staff Machine Learning Engineer (this track)** — IC-staff path. Multi-team technical leadership without direct reports. Owns architecture, program, platform strategy, and the technical bar.
- **Engineering Manager, ML** — people-leadership path. Owned by the peer track [`ai-infra-team-lead-learning`](https://github.com/ai-infra-curriculum/ai-infra-team-lead-learning).

None of the 41 postings sampled this cycle included direct-report management as a required-skill bullet; the sample was filtered to IC-staff postings before coding.

---

## 1. Staff role scope, staff-plus archetypes, multi-team contracts

**Status:** covered · **Frequency:** 0.83 (Δ ↑ from 0.79) · **Owner:** `staff-ml-engineer`

The staff-plus IC archetypes (Tech Lead, Architect, Solver, Right Hand), the shift from single-team ownership to multi-team ownership, the hand-off contracts to the manager peer (`ai-infra-team-lead`) and to specialist tracks.

**Curriculum coverage**

- [`lessons/mod-401-staff-ml-role-scope/`](lessons/mod-401-staff-ml-role-scope)

**Evidence (selected)**

- Cresta (Staff ML): *"Proven ability to influence technical direction across teams as a senior individual contributor."*
- Wayve (Staff ML, AV Core): *"Staff-level technical leadership: research-literate and pragmatic, setting direction, raising the bar, and leading cross-functional work."*
- Plaid (Staff ML, DFAI): *"Prior technical leadership experience (tech lead, principal, or staff) with demonstrated cross-team influence and mentorship."*
- Scale AI (Staff ML, Public Sector): *"Demonstrated ability to operate at Staff-level scope: setting technical direction, owning ambiguous problems, and driving 0→1 initiatives to production."*
- Pinterest (Staff ML, Shopping Ads): *"8+ years of industry experience in ML engineering / applied ML / software engineering, including meaningful time operating as a Staff-level (or equivalent) IC delivering complex production systems."*
- Breeze (Staff ML, Risk): *"Comfortable being the senior technical voice on a small team, with cross-functional technical leadership, able to set direction and not just execute."*

## 2. Multi-team ML systems architecture

**Status:** covered · **Frequency:** 0.66 (Δ ↑ from 0.62) · **Owner:** `staff-ml-engineer`

Portfolio-scope blueprints — shared data planes, shared feature layer, shared model-registry contract, shared eval contract; identify duplication and drive consolidation; author multi-system RFCs and defend them in architecture reviews.

**Curriculum coverage**

- [`lessons/mod-402-multi-team-ml-architecture/`](lessons/mod-402-multi-team-ml-architecture)
- Capstone: [`projects/project-401-multi-team-ml-blueprint/`](projects/project-401-multi-team-ml-blueprint)

**Evidence (selected)**

- GM (Staff ML, ML Training Infra): *"Demonstrated track record of leading technically ambiguous, cross-team infrastructure initiatives and driving them to measurable impact."*
- Plaid (Staff ML, DFAI): *"Ability to drive technical alignment across teams: setting standards, defining integration patterns, and influencing beyond your immediate scope."*
- Airbnb (Staff ML, AI Enablement): *"Your insights and technical expertise will shape the future of ML infrastructure, enabling hundreds of engineers to build world-class AI experiences."*
- Scale AI (Staff ML, Public Sector): *"Own and evolve shared agentic infrastructure and core libraries, enabling reuse across teams, products, and Public Sector contracts."*
- Robinhood (Staff ML, AI Platform & Agentic Apps): *"Proven ability to build platforms, not just models: you've shipped eval, safety, or agent tooling that other engineering teams adopted."*
- Pinterest (Staff ML, Shopping Ads): *"Strong communication skills and the ability to influence technical direction across teams without directly owning every implementation detail."*

## 3. Foundation-model / large-scale fine-tune training programs

**Status:** covered · **Frequency:** 0.41 (Δ — ≈ 0.41) · **Owner:** `staff-ml-engineer` (program) → peer specialists (`training-pipeline-engineer`, `fine-tuning-engineer`) for implementation

Scope pretraining and large-scale fine-tune programs: compute-optimal parameter/data trade-offs, scaling-law-driven roadmap, data mixture and curriculum, ablation sequencing, capability vs. safety post-training investment, executive framing of the program's cost and risk.

**Curriculum coverage**

- [`lessons/mod-403-foundation-model-training-programs/`](lessons/mod-403-foundation-model-training-programs)
- Capstone: [`projects/project-402-foundation-model-training-program/`](projects/project-402-foundation-model-training-program)
- Peer specialist depth: `training-pipeline-engineer-learning`, `fine-tuning-engineer-learning`

**Evidence (selected)**

- Plaid (Staff ML, DFAI): *"Deep expertise in Transformers/LLMs/Foundation Models, including large-scale training or domain adaptation."*
- DoorDash (Staff ML, Fulfillment Planning): *"Define and lead DoorDash's cutting-edge AI vision for logistics: an LLM-inspired foundation model for intelligence across logistics."*
- Airbnb (Sr Staff ML, Post Training): *"You will be responsible for fine-tuning state-of-the-art LLMs for diverse use cases while optimizing models for high-performance deployment."*
- LinkedIn (Staff SWE, ML): *"Proven experience in pre-training and fine-tuning LLMs, including domain-specific adaptations."*
- Scale AI (Sr/Staff ML Research Engineer, General Agents): *"Experience fine-tuning or adapting foundation models using SFT, RLVR, and LoRA methods."*
- Spotify (Staff ML, Personalization): *"Experience with LLM training, fine-tuning, evaluation, and optimization (SFT, distillation, LoRA)."*

## 4. Distributed-training strategy at scale

**Status:** covered · **Frequency:** 0.34 (Δ — ≈ 0.34) · **Owner:** `staff-ml-engineer` (strategy) → peer specialists for implementation

Parallelism recipe (DDP, FSDP/ZeRO, tensor-parallel, pipeline-parallel, 3D-parallel), MFU estimation, failure-mode budgeting, cluster-hour defence to leadership.

**Curriculum coverage**

- [`lessons/mod-404-distributed-training-systems-at-scale/`](lessons/mod-404-distributed-training-systems-at-scale)
- Peer implementation depth: `training-pipeline-engineer-learning`, `ai-infra-performance-learning`

**Evidence (selected)**

- GM (Staff ML, ML Training Infra): *"Experience designing and developing training platforms that support FSDP, pipeline parallelism, and other scalable solutions for training large foundational models."*
- Spotify (Staff ML, Personalization): *"Experience with distributed ML workloads (Ray, FSDP, HSDP, or similar)."*
- Reddit (Staff ML, Ads ML Efficiency): *"Experience with distributed training frameworks such as PyTorch Distributed, Ray, Tensorflow, Spark."*
- Reddit (Staff MLSE): *"Deep experience working with distributed training frameworks, including Ray and Kubernetes."*
- Plaid (Staff ML, DFAI): *"Distributed training experience and strong Python + software engineering fundamentals at a staff level."*
- Anthropic (Staff Research Engineer, Multi-Agent Scaling): *"Experience building or operating large-scale distributed systems."*

**Frequency note.** Steady at 0.34 across cycles. GM Training Infra remains the clearest explicit archetype ("FSDP, pipeline parallelism, and other scalable solutions for training large foundational models"); Spotify Personalization added "Ray, FSDP, HSDP" in the 2026-10 refresh. The consumer-marketplace sample continues to use implicit "scalable ML architectures" phrasing rather than the FSDP/MFU vocabulary — strategy framing in mod-404 explicitly bridges both idioms.

## 5. ML platform strategy at org scope

**Status:** covered · **Frequency:** 0.49 (Δ — ≈ 0.48) · **Owner:** `staff-ml-engineer` (consumer strategy) → peer platform tracks for build

Build-vs-adopt on feature store, model registry, eval platform, training platform, inference gateway; paved-road roadmap negotiation; contribute-back and gap RFCs; multi-team consumer contract.

**Curriculum coverage**

- [`lessons/mod-405-ml-platform-strategy/`](lessons/mod-405-ml-platform-strategy)
- Peer platform build depth: `ai-infra-ml-platform-learning`, `ai-infra-mlops-learning`

**Evidence (selected)**

- Riot Games (Staff ML, ML Platform): *"Experience operating inference platforms such as KServe and production ML infrastructure including Feast, Milvus, or similar open-source systems."*
- Robinhood (Staff ML, AI Platform & Agentic Apps): *"Proven ability to build platforms, not just models: you've shipped eval, safety, or agent tooling that other engineering teams adopted, and you have the judgment to know when to build versus buy."*
- Reddit (Staff MLSE): *"Hands-on experience administering and integrating MLOps tools for experiment tracking, model serving, and model registries (e.g. MLflow or Wandb)."*
- Airbnb (Staff ML, AI Enablement): *"Your insights and technical expertise will shape the future of ML infrastructure, enabling hundreds of engineers to build world-class AI experiences."*
- Breeze (Staff ML, Risk): *"Experience establishing ML platform standards and operating models in a growing organization."*
- Reddit (Staff ML, Ads ML Efficiency): *"Contributions to internal platforms used by multiple ML teams."*

## 6. Cross-team evaluation & experimentation program

**Status:** covered · **Frequency:** 0.54 (Δ ↑ from 0.52) · **Owner:** `staff-ml-engineer`

Standardise the offline eval harness across ML teams; LLM-as-judge policy; shadow/canary/progressive-rollout contract; cross-team experiment review; evaluation of LLM / agent / VLM systems at program scope.

**Curriculum coverage**

- [`lessons/mod-406-cross-team-eval-and-experimentation/`](lessons/mod-406-cross-team-eval-and-experimentation)
- Peer platform build depth: [`ai-eval-engineer-learning`](https://github.com/ai-infra-curriculum/ai-eval-engineer-learning) (LLM/agent eval platform), [`model-evaluation-engineer-learning`](https://github.com/ml-engineering-curriculum/model-evaluation-engineer-learning) (classical + generative eval depth)

**Evidence (selected)**

- Robinhood (Staff ML, AI Platform & Agentic Apps): *"Deep expertise evaluating agents: you've built trajectory-level evals, tool-call scoring, and simulation environments, and you can articulate why final-answer accuracy is insufficient for systems that act. Rigor in evaluation methodology: golden datasets, rubric and LLM-as-judge grading and their failure modes, statistical significance with small N, offline-to-online metric correlation, and eval data versioning and contamination control."*
- Scale AI (Staff ML, Public Sector): *"Experience building evaluation infrastructure for non-deterministic systems."*
- Scale AI (Sr/Staff ML Research Engineer, General Agents): *"Familiarity with evaluation, monitoring, and observability for LLM-powered systems in production."*
- Anthropic (Staff Research Engineer, Multi-Agent Scaling): *"Built evaluations, benchmarks or harnesses for LLMs or agents."*
- Pinterest (Staff ML, Shopping Ads): *"Deep experience with evaluation and measurement: dataset strategy, labeling/review operations, metric design, regression testing, and connecting offline improvements to online outcomes."*
- Cresta (Staff ML): *"Experience designing evaluation frameworks for LLM systems beyond single-turn prompts, including robustness testing and production monitoring."*
- Snap (Staff ML, Search Ranking): *"Strong understanding of online experimentation, A/B testing, metric design, model debugging, and tradeoff analysis."*

**Frequency note.** The 2026-10 refresh brought the specific cross-team-agent-eval framing to 7 postings (Robinhood, Scale AI Public Sector, Scale AI General Agents, Anthropic Multi-Agent, Pinterest Shopping Ads, Cresta, Airbnb Growth Platform). The exercise-level ≥3 trigger set in the 2026-09 cycle is now MET for a potential `agentic-eval-and-canary-program` exercise in mod-406 — but the packet-wide ≥0.30 frequency bar is NOT (7/41 = 0.17). Continuity bias and existing coverage of chapters 3 (LLM-as-judge policy for chat/agentic/generative-content systems) and 4 (shadow/canary/progressive rollout contract) argue against proposing new content this cycle. See §15 for the refined 2026-11 trigger.

## 7. GPU capacity, cost, and TCO — portfolio management

**Status:** covered · **Frequency:** 0.20 (Δ — ≈ 0.21) · **Owner:** `staff-ml-engineer`

Portfolio-level demand forecast, capacity portfolio (reserved / on-demand / spot / multi-cloud / self-hosted), FinOps discipline, TCO model a CFO can read.

**Curriculum coverage**

- [`lessons/mod-407-capacity-cost-and-tco/`](lessons/mod-407-capacity-cost-and-tco)

**Evidence (selected)**

- Riot Games (Staff ML, ML Platform): *"Familiarity with GPU orchestration, performance tuning, and cost-aware scheduling."*
- Reddit (Staff ML, Ads ML Efficiency): *"Experience optimizing cloud infrastructure costs across large ML workloads."* + *"Familiarity with GPU architectures and performance analysis tools."*
- Spotify (Staff ML, Personalization): *"Large-scale inference systems experience with latency, reliability, and cost optimization knowledge."*
- Cresta (Staff ML): *"Strong systems thinking: ability to design for scalability, latency constraints, cost efficiency, security, and long-term maintainability."*
- Kodiak (Staff ML, Deployment): *"Quantization, pruning, converting to ONNX/TensorRT, custom GPU kernels and profiling."*

**Frequency note.** Steady around 0.20 across cycles. Reddit Ads ML Efficiency added explicit "cloud infrastructure cost optimization" language in the 2026-10 refresh. Explicit TCO / demand-forecast framing remains rare — most postings embed cost as a design constraint rather than a program ownership verb. Kept at planned scope; watch next cycle for the "own the TCO model" staff verb.

## 8. Portfolio-scope ML reliability, SLOs, multi-team incident command

**Status:** covered · **Frequency:** 0.29 (Δ ↑ from 0.28, approaching 0.30) · **Owner:** `staff-ml-engineer`

Standardise ML-specific SLI/SLO vocabulary; severity matrix for ML incidents; cross-team incident command adapted to ML failure classes; org-standard postmortem template; retraining-as-deploy hygiene. **2026-10 emerging sub-theme:** action-level guardrails for agentic systems (permission and tool-scoping, approval gates, blast-radius controls, sandboxing) as the operational reliability contract for agents operating in systems where mistakes have consequences.

**Curriculum coverage**

- [`lessons/mod-408-portfolio-reliability-and-incident-command/`](lessons/mod-408-portfolio-reliability-and-incident-command)

**Evidence (selected)**

- DoorDash (Staff ML, Fulfillment Planning): *"Designed, launched, and operated mission-critical ML models or systems in production, including monitoring, retraining, reliability, and governance."*
- Robinhood (Staff ML, AI Platform & Agentic Apps): *"Demonstrated expertise designing action-level guardrails — permission and tool-scoping models, approval gates, blast-radius controls, and sandboxing — for agents operating in systems where mistakes have consequences."*
- Scale AI (Staff ML, Public Sector): *"Experience deploying ML systems into air-gapped, classified, or otherwise disconnected environments."*
- Breeze (Staff ML, Risk): *"Experience with real-time payment-risk systems, including low-latency model inference, monitoring, and incident response."*
- Riot Games (Staff ML, ML Platform): *"You will architect systems for model deployment, observability, and lifecycle management."*
- Sentry (Staff ML, AI): *"You will be at the forefront of integrating AI and machine learning into our core products, from issue triage and resolution to predictive analytics."*

**Frequency note.** Nudged from 0.28 → 0.29 — one more posting breaching 0.30 would clear the threshold. The agentic-guardrails sub-theme (Robinhood + Scale AI Public Sector) is new this cycle; too thin (2 postings = 0.05) to justify its own exercise but noted as a 2026-11 watch-list candidate. If ≥3 postings next cycle, consider one exercise in mod-408 titled `action-level-guardrails-and-blast-radius-for-agentic-portfolio`.

## 9. Responsible AI at portfolio scope

**Status:** covered · **Frequency:** 0.12 (Δ ↓ from 0.14; **2nd consecutive cycle below 0.15 demotion-trigger line**) · **Owner:** `staff-ml-engineer` (ML-side program) → `ai-governance-analyst` / `head-of-ai-governance` / `ai-infra-security-learning` for depth

Portfolio-scope AI risk register (NIST AI RMF + Gen-AI profile); EU AI Act classification; org-standard model card and datasheet format; pre-launch review gate; red-team cadence and escalation path to head-of-ai-governance.

**Curriculum coverage**

- [`lessons/mod-409-responsible-ai-at-portfolio-scope/`](lessons/mod-409-responsible-ai-at-portfolio-scope)

**Evidence (selected)**

- Robinhood (Staff ML, AI Platform & Agentic Apps): *"Demonstrated expertise designing action-level guardrails — permission and tool-scoping models, approval gates, blast-radius controls, and sandboxing — for agents operating in systems where mistakes have consequences."* (safety-adjacent framing)
- Airbnb (Sr Staff ML, Post Training): *"Post-training experience in areas like data processing for fine-tuning; responsible LLMs; LLM alignment."*
- Reddit (Staff ML, ML Understanding): *"Passion for developing scalable, well-designed, and responsible AI solutions that positively impact society."*
- Cresta (Staff ML): *"Experience designing evaluation frameworks for LLM systems beyond single-turn prompts, including robustness testing and production monitoring."*
- Wayve (Staff ML, AV Core): *"Experience with redundant or fallback architectures, safety-critical systems."*

**Frequency note.** Explicit responsible-AI language dropped a second consecutive cycle: 0.20 (2026-08) → 0.14 (2026-09) → 0.12 (2026-10). This activates the two-consecutive-cycles-below-0.15 demotion-proposal trigger the 2026-09 cycle set. **Removals are explicitly out of scope for this packet** (packet guidance: "Removals are out of scope for this packet — open a separate proposal if you believe a requirement is no longer relevant"). **Recommendation:** open a separate removal/demotion proposal cycle to either demote mod-409 from 10h or re-anchor it around the agentic-operational-safety framing surfacing at Robinhood (action-level guardrails) and Scale AI Public Sector (air-gapped deployment). No NIST AI RMF / EU AI Act / model-card language has surfaced explicitly in two consecutive cycles' samples; mod-409 may need to re-anchor on the agentic-operational-safety idiom rather than the NIST/EU AI Act idiom to track where staff postings actually gather language.

## 10. Staff-plus technical leadership — roadmaps, exec comms, hiring calibration, coaching seniors

**Status:** covered · **Frequency:** 0.78 (Δ ↑ from 0.76) · **Owner:** `staff-ml-engineer`

Multi-quarter ML program roadmap that leadership commits to; executive-audience one-pagers and RFCs; staff-plus hiring loops; coach senior tech leads and partner with the EM peer; vendor and partnership judgement calls.

**Curriculum coverage**

- [`lessons/mod-410-staff-plus-technical-leadership/`](lessons/mod-410-staff-plus-technical-leadership)
- Capstone: [`projects/project-403-staff-plus-tech-leadership-simulation/`](projects/project-403-staff-plus-tech-leadership-simulation)

**Evidence (selected)**

- LinkedIn (Staff SWE, ML): *"You will set the technical direction and lead initiatives that push the boundaries of what's possible."*
- Pinterest (Staff ML, Shopping Ads): *"You will also serve as the technical lead for ML in this space — reporting to a Director and acting as the first ML Engineering hire in this org."*
- Babylist (Staff ML): *"You set where our models go over the next year or two, sequence the bets that get there, and stay deep in building."*
- Spotify (Staff ML, Personalization): *"Drive technical direction in ambiguous problem spaces and contribute to the long-term architecture of personalization systems."*
- GM (Staff ML, ML Training Infra): *"Excellent communication skills, with the ability to build consensus, navigate controversial decisions, communicate risks clearly, and provide constructive technical feedback."*
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

**Status:** covered_at_peer_level · **Frequency:** 0.22 (Δ ↑ from 0.17, below 0.30 threshold) · **Owner:** `fine-tuning-engineer`

Rising from 0.17 → 0.22 as fine-tuning methods move from specialist-only to generalist Staff ML Engineer expectation. Program-level trade-off framing remains in mod-403 exercise-04 (`capability-vs-safety-post-training-plan`); depth link: [`fine-tuning-engineer-learning`](https://github.com/ml-engineering-curriculum/fine-tuning-engineer-learning).

**Evidence (selected)**

- Airbnb (Sr Staff ML, Post Training): *"Post-training experience in areas like data processing for fine-tuning; responsible LLMs; LLM alignment; reinforcement learning; efficient training and inference; language model evaluation."*
- Spotify (Staff ML, Personalization): *"Experience with LLM training, fine-tuning, evaluation, and optimization (SFT, distillation, LoRA)."*
- Scale AI (Sr/Staff ML Research Engineer, General Agents): *"Experience fine-tuning or adapting foundation models using SFT, RLVR, and LoRA methods."*
- Scale AI (Staff ML, Public Sector): *"Depth in model adaptation (training/fine-tuning embedding models, instruction tuning, LoRA/PEFT, RLHF)."*
- LinkedIn (Staff SWE, ML): *"Proven experience in pre-training and fine-tuning LLMs, including domain-specific adaptations."*
- Cresta (Staff ML): *"Deep hands-on experience with LLMs at scale."*

**Watch for 2026-11.** If frequency crosses 0.30, consider one exercise in mod-403 that names the SFT/LoRA/RLHF/RLVR vocabulary at program-selection altitude (not implementation).

### 15. Agentic AI systems (multi-step LLM workflows, tool use, agent frameworks)

**Status:** covered_at_peer_level · **Frequency:** 0.49 (Δ ↑ from 0.41; **2nd consecutive cycle above the 0.40 watch-list line**) · **Owner:** `senior-agentic-ai-engineer` (peer level-40 track) with depth links out to `agentic-ai-engineer` (L30) and `agentic-systems-architect` (L30)

**Movement this cycle.** 20 of 41 postings name agentic-systems experience as a staff-level responsibility — now ~half the sample. 2026-10 additions: Anthropic ×2 (Code RL preferred, Multi-Agent Scaling core), Robinhood (core), Scale AI Public Sector (core), Scale AI General Agents (core), Pinterest Shopping Ads ("LLM-powered applications in production (or adjacent GenAI systems)"), Reddit Ads ML Efficiency (preferred foundation-model systems), LinkedIn (preferred generative AI).

**Ownership rule application — refined trigger partially MET.** Kept as `covered_at_peer_level`. Most 2026-10 agentic postings continue to frame the theme with the **"build agents"** verb (Anthropic Multi-Agent "experience building complex agentic systems using LLMs"; Scale AI General Agents "modern LLMs, prompt-, context-, and system-level optimization, and agentic system design"; Sentry "Demonstrated expertise building production-grade agentic systems and tools"). That verb is peer L40 [`senior-agentic-ai-engineer-learning`](https://github.com/ai-infra-curriculum/senior-agentic-ai-engineer-learning) scope; the L30 counterparts are [`agentic-systems-architect-learning`](https://github.com/ai-infra-curriculum/agentic-systems-architect-learning) and [`agentic-ai-engineer-learning`](https://github.com/ai-infra-curriculum/agentic-ai-engineer-learning).

The distinct staff-scope **"cross-team agent-eval program"** framing this track owns now appears in **7 postings** — Robinhood (trajectory-level evals, tool-call scoring, simulation environments), Scale AI Public Sector (evaluation infrastructure for non-deterministic systems), Scale AI General Agents (evaluation, monitoring, and observability for LLM-powered systems in production), Anthropic Multi-Agent (evaluations, benchmarks or harnesses for LLMs or agents), Pinterest Shopping Ads (evaluation and measurement for LLM-powered applications), plus prior Cresta and Airbnb Growth Platform. This clears the 2026-09 ≥3 exercise-level trigger. **However**, the packet-wide ≥0.30 frequency bar is NOT met (7/41 = 0.17), and the packet's blanket rule requires BOTH thresholds for new content. Combined with the continuity bias ("Prefer citing existing content over proposing new content"), no new exercise is proposed; mod-406 chapter 3 ([`03-llm-as-judge-org-policy.md`](lessons/mod-406-cross-team-eval-and-experimentation/03-llm-as-judge-org-policy.md) — explicitly names "chat/agentic/generative-content systems") and chapter 4 ([`04-shadow-canary-progressive-rollout-contract.md`](lessons/mod-406-cross-team-eval-and-experimentation/04-shadow-canary-progressive-rollout-contract.md) — model-agnostic) continue to carry the org-scope framework.

**Refined watch-list for 2026-11.** If the cross-team-agent-eval-program framing clears BOTH ≥3 postings AND ≥0.30 frequency next cycle, add exercise `agentic-eval-and-canary-program` in mod-406 with citations.

**NEW emerging sub-theme for 2026-10 — action-level agent guardrails.** Robinhood explicitly names "action-level guardrails — permission and tool-scoping models, approval gates, blast-radius controls, and sandboxing" as a required skill; Scale AI Public Sector echoes it via "air-gapped, classified, or otherwise disconnected environments." Only 2/41 postings (0.05); below the ≥3 bar. Noted as 2026-11 watch-list candidate — if it clears ≥3 postings next cycle, consider one exercise in mod-408 (`action-level-guardrails-and-blast-radius-for-agentic-portfolio`) OR a re-anchoring of mod-409 around operational agentic safety (see §9).

**Evidence (selected)**

- Robinhood (Staff ML, AI Platform & Agentic Apps): *"Hands-on experience building agentic systems end to end — tool use, orchestration, context management, multi-step planning — on top of frontier models, in production."* + *"Deep expertise evaluating agents: you've built trajectory-level evals, tool-call scoring, and simulation environments, and you can articulate why final-answer accuracy is insufficient for systems that act."*
- Anthropic (Staff Research Engineer, Multi-Agent Scaling): *"Design, run and interpret large-scale experiments on agent teams."* + *"Experience building complex agentic systems using LLMs."* (preferred)
- Scale AI (Staff ML, Public Sector): *"Deep experience with agentic systems, autonomous workflows, or ML systems that reason and act over multiple steps."*
- Scale AI (Sr/Staff ML Research Engineer, General Agents): *"Deep understanding of modern LLMs, prompt-, context-, and system-level optimization, and agentic system design."*
- Cresta (Staff ML): *"Demonstrated leadership in architecting complex AI systems, particularly agentic or multi-step LLM workflows."*
- Pinterest (Staff ML, Shopping Ads): *"Hands-on experience building LLM-powered applications in production (or adjacent GenAI systems), with strong judgment on reliability, failure modes, rollout safety, and practical tradeoffs."*
- Sentry (Staff ML, AI): *"Demonstrated expertise building production-grade agentic systems and tools."*
- Airbnb (Sr Staff ML, Growth Platform, preferred): *"Experience building robust testing frameworks for agent behavior validation and continuous improvement."*

### 16. RAG / retrieval-augmented generation systems

**Status:** covered_at_peer_level · **Frequency:** 0.20 (Δ ↓ from 0.24, below 0.30 threshold) · **Owner:** `rag-engineer`

Retrieval pipelines, embedding models, vector search, hybrid retrieval, RAG evaluation. Depth link: [`rag-engineer-learning`](https://github.com/ai-infra-curriculum/rag-engineer-learning).

**Evidence (selected)**

- Cresta (Staff ML): *"Deep expertise in transformer-based models, embeddings, retrieval systems, and Retrieval-Augmented Generation (RAG) pipelines."*
- Scale AI (Staff ML, Public Sector): *"Hands-on experience with retrieval systems, embeddings, or representation learning."*
- Databricks (preferred): *"Experience with LLM fine-tuning, prompt engineering, and retrieval-augmented generation (RAG)."*
- Snap (preferred): *"Experience with LLMs, foundation models, semantic search, natural language understanding, or retrieval-augmented generation."*

### 17. Eval-platform build (LLM-judge harness, agent eval infra)

**Status:** covered_at_peer_level · **Frequency:** 0.27 (Δ ↑ from 0.21, approaching 0.30) · **Owner:** `model-evaluation-engineer` / `ai-eval-engineer`

Climbed 0.21 → 0.27 with 2026-10 refresh, driven primarily by agentic-system eval infra at Robinhood (trajectory-level evals), Scale AI Public Sector (eval infrastructure for non-deterministic systems), Scale AI General Agents (eval + monitoring + observability for LLM-powered systems in production), Anthropic Multi-Agent (benchmarks/harnesses for LLMs or agents), and Pinterest Shopping Ads (eval + measurement). Still owned by peer specialists ([`ai-eval-engineer-learning`](https://github.com/ai-infra-curriculum/ai-eval-engineer-learning) owns LLM/agent eval platform; [`model-evaluation-engineer-learning`](https://github.com/ml-engineering-curriculum/model-evaluation-engineer-learning) owns classical + generative eval depth). Staff ML engineer's slice remains the org-scope eval program (§6).

If this breaches 0.30 in 2026-11, the next action is NOT to pull platform-build depth into this track but to tighten the delegation contract in mod-406 and link out more explicitly.

### 18. ML platform build (feature store, model registry, multi-tenant clusters)

**Status:** covered_at_peer_level · **Owner:** `ai-infra-ml-platform-learning` / `ai-infra-mlops-learning`

Riot ML Platform, Reddit MLSE, and Airbnb AI Enablement each *are* staff-level platform-build roles — they blur the line between staff ML engineer and staff ML-platform engineer. Where the primary verb is "own the platform build", the role belongs on the peer platform track. Where the primary verb is "set the org-scope consumer contract for the platform", it belongs here (§5). Depth links: [`ai-infra-ml-platform-learning`](https://github.com/ai-infra-curriculum/ai-infra-ml-platform-learning), [`ai-infra-mlops-learning`](https://github.com/ai-infra-curriculum/ai-infra-mlops-learning).

### 19. ML/AI security depth (poisoning, extraction, adversarial inputs, prompt injection)

**Status:** covered_at_higher_level · **Frequency:** 0.20 (Δ ↑ from 0.17; surface mainly classical fraud / identity-threat + emerging agentic guardrails) · **Owner:** `ai-infra-security-learning` (level 35)

Nudged up from 0.17 → 0.20 with 2026-10 agentic-security additions: Robinhood ("action-level guardrails — permission and tool-scoping models, approval gates, blast-radius controls, and sandboxing") and Scale AI Public Sector ("air-gapped, classified, or otherwise disconnected environments"). Breeze adds payments-fraud-ML framing. Still mainly classical fraud/identity-threat detection (Ramp, Uber, Plaid) rather than prompt-injection / poisoning / model-extraction language. Awareness and delegation contract in mod-409; depth linked to [`ai-infra-security-learning`](https://github.com/ai-infra-curriculum/ai-infra-security-learning).

Watch the agentic-guardrails sub-theme — if it crosses ≥3 postings as a standalone staff responsibility (currently 2), see §15 and §8 for the relocation question (mod-408 operational-reliability framing vs. mod-409 responsible-AI-risk framing).

### 20. Governance / compliance policy authoring, regulator engagement

**Status:** covered_at_peer_level · **Owner:** `ai-governance-analyst` / `head-of-ai-governance`

Depth links: [`ai-governance-analyst-learning`](https://github.com/ai-governance-curriculum/ai-governance-analyst-learning), [`head-of-ai-governance-learning`](https://github.com/ai-governance-curriculum/head-of-ai-governance-learning).

### 21. Direct people management (1:1s, performance reviews, headcount)

**Status:** covered_at_peer_level · **Frequency:** 0 (filtered out of sample) · **Owner:** `ai-infra-team-lead-learning` (peer level-40 EM path)

None of the 41 postings sampled this cycle included direct-report management as a required-skill bullet — the sample was filtered to IC-staff postings before coding. Mentorship, coaching, and "lead by influence" language appears widely (§10 at 0.78) and is covered by mod-410.

### 22. Org- and industry-wide technical strategy

**Status:** covered_at_higher_level · **Owner:** `principal-ml-engineer` (level 50)

Staff scope stops at multi-team + exec-audience communication; principal scope extends to industry-wide framing. Where a posting reads as principal-scoped, refer up rather than growing this track.

### 23. Product-integrated GenAI / LLM in production (dominant theme)

**Status:** covered_at_lower_level · **Frequency:** 0.73 (Δ ↑ from 0.69; now dominant signal) · **Owner:** `ml-engineer` (L20) with peer specialists `llm-application-developer` and `applied-ai-engineer`

**Movement this cycle.** Continues as the dominant signal: 30 of 41 postings require the candidate to have shipped LLM-augmented product features in production. 2026-10 additions include every new Anthropic, Spotify, Robinhood, Scale AI, Pinterest, Reddit, LinkedIn, and Babylist posting. Robinhood: *"Shipping LLM-powered systems to production at scale."* Spotify: *"Experience with LLM training, fine-tuning, evaluation, and optimization."* Scale AI General Agents: *"Deep understanding of modern LLMs, prompt-, context-, and system-level optimization, and agentic system design."* Pinterest Shopping Ads: *"Hands-on experience building LLM-powered applications in production."* Reddit Notifications: *"Experience working with LLM in production and utilizing generative AI to augment recommendation systems."* DoorDash: *"Define and lead DoorDash's cutting-edge AI vision for logistics: an LLM-inspired foundation model for intelligence across logistics."*

**Ownership rule application.** Kept as `covered_at_lower_level`. The verb remains "you have shipped LLMs in production" — a résumé prerequisite, not a distinct staff-scope program-ownership verb. Staff-scope framing for LLM-in-product portfolios is already carried by mod-402 (multi-team ML architecture — the LLM-augmented recommender fits inside the portfolio), mod-403 (foundation-model programs — the DoorDash logistics FM is the archetype), mod-405 (platform strategy for LLM serving — Riot's KServe/Triton stack fits here), and mod-406 (cross-team eval for generative systems — the LLM-as-judge chapter already names generative/agentic systems as the modal case).

**Watch-list for 2026-11.** If this signal starts framing "own the org-wide GenAI-in-product platform" as a distinct staff verb (rather than "you have shipped LLM systems before"), consider one exercise in mod-405 titled `llm-in-product-paved-road-strategy`. Not proposed this cycle because the verb is prerequisite-shaped, not program-owner-shaped.

### 24. Graph Neural Networks / embedding retrieval / vector-DB systems

**Status:** covered_at_peer_level · **Frequency:** 0.17 (Δ ↓ from 0.21, below 0.30 threshold) · **Owner:** `senior-ml-engineer` (ranking/embedding modelling) / `rag-engineer` (vector DB stack)

Dropped slightly as the sample grew (0.21 → 0.17) — no new GNN-specific postings this cycle; only one adjacent "embeddings or representation learning" at Scale AI Public Sector. Reddit LS Embedding remains the primary archetype ("Expertise in Graph Neural Networks (GNNs), graph-based representation learning, and transformer architectures"); Reddit MLSE (preferred — Neo4j / PyTorch Geometric / DGL), ZipRecruiter (preferred — "Two-Tower Neural Networks, Graph Neural Networks (GNNs)"), Snap Search Ranking, Riot ML Platform (Milvus vector DB), Pinterest Monetization remain. Portfolio-scope contract already carried by mod-402. Below threshold; keep peer-track ownership.

---

## Continuity summary

- All 10 requirement themes owned by this track pass the "covered" test.
- No requirement crosses BOTH the ≥3-posting AND ≥0.30-frequency bars for a new module or exercise that is not already covered by an existing module or by a peer track per the ownership rule.
- **Agentic AI (§15)** rose a second consecutive cycle (0.30 → 0.41 → 0.49). The distinct **"cross-team agent-eval program as staff verb"** framing now clears the 2026-09 refined exercise-level ≥3 trigger (7 postings: Robinhood, Scale AI Public Sector, Scale AI General Agents, Anthropic Multi-Agent, Pinterest Shopping Ads, plus prior Cresta and Airbnb Growth Platform) — but the packet-wide ≥0.30 frequency bar remains unmet (7/41 = 0.17). Continuity bias and existing coverage of mod-406 chapters 3 (LLM-as-judge policy for chat/agentic/generative-content systems) and 4 (shadow/canary/progressive rollout) argue against proposing new content this cycle. Refined 2026-11 trigger: breach BOTH ≥3 postings AND ≥0.30 frequency with the distinct cross-team-agent-eval-program staff verb.
- **NEW emerging sub-theme — action-level agent guardrails (§15, §8).** Robinhood ("permission and tool-scoping models, approval gates, blast-radius controls, and sandboxing") and Scale AI Public Sector ("air-gapped, classified, or otherwise disconnected environments") introduce this sub-theme. 2/41 (0.05); below ≥3 bar. Watch 2026-11.
- **Portfolio reliability (§8)** crept to 0.29 (from 0.28) — one more posting breaching 0.30 would clear the threshold. Robinhood (action-level guardrails) and Breeze (low-latency inference monitoring + incident response) are the 2026-10 archetypes pulling this up.
- **Product-integrated GenAI (§23)** stayed at 0.73 (from 0.69) and remains the dominant signal, but reads as inherited prerequisite from L20 / L25 peer tracks rather than a staff-scope-only gap; the staff-scope program framing is already carried by mod-402 / mod-403 / mod-405 / mod-406.
- **Responsible-AI (§9) dropped a second consecutive cycle (0.20 → 0.14 → 0.12)** — activating the two-consecutive-cycles-below-0.15 demotion-proposal trigger set in 2026-09. **Removals are explicitly out of scope for this packet.** Recommendation: open a separate removal/demotion proposal cycle, OR re-anchor mod-409 around the agentic-operational-safety idiom (action-level guardrails, blast-radius controls) rather than NIST AI RMF / EU AI Act — the postings have shifted idiom even though the underlying concern (how do we gate agent actions responsibly in production?) remains at staff scope.
- **Fine-tuning depth (§14)** crept to 0.22 (from 0.17) — moving from specialist-only to generalist Staff ML Engineer expectation but still below threshold. Watch 2026-11 for 0.30 breach; if so, consider one exercise in mod-403 at program-selection altitude.
- **Eval-platform build (§17)** climbed to 0.27 (from 0.21) — approaching 0.30. If breached next cycle, tighten the delegation contract in mod-406 rather than pulling platform-build depth into this track.
- The [`.aicg/curriculum-plan-delta.json`](.aicg/curriculum-plan-delta.json) emitted this cycle contains empty `modules`, `exercises`, and `projects` arrays with a rationale that references this file.
