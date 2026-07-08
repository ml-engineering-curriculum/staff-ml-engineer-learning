# Job Requirements — Staff Machine Learning Engineer

**Role level:** 40 (staff-plus IC altitude, ML engineering ladder)
**Track:** `staff-ml-engineer-learning`
**Research window:** 2026-04-08 → 2026-07-07 (last 90 days)
**Today:** 2026-07-07

This file documents the requirements catalogue used to seed the Staff Machine Learning Engineer curriculum. Raw normalized data lives in [`.aicg/job-requirements.json`](.aicg/job-requirements.json); the planned curriculum lives in [`.aicg/curriculum-plan.json`](.aicg/curriculum-plan.json).

## Status — bootstrap session, postings deferred

<!-- needs-research: collect ≥25 distinct in-window postings titled "Staff Machine Learning Engineer" / "Staff ML Engineer" / "Staff Software Engineer, Machine Learning" / "Staff Engineer, ML Systems" (NOT plain "Machine Learning Engineer", NOT "Senior", NOT "Principal", NOT "Engineering Manager", NOT "Research Scientist", NOT "Applied Scientist", NOT "Staff Data Scientist") and re-validate every requirement against the live evidence. -->

This packet was authored in a bootstrap session **without WebSearch / WebFetch permissions**. The parent session and three delegated general-purpose research subagents (AI-native, big-tech / consumer, and fintech / retail / health / autonomy segments) were each denied web-tool access on every attempt. Per the project rules ("Do not invent facts, incidents, or salary figures. Cite sources."), the `postings` array in `.aicg/job-requirements.json` is intentionally empty — the curriculum cannot honestly claim to have analysed 25 live postings when none were fetched. This mirrors the pattern the sibling [`senior-ml-engineer-learning`](https://github.com/ml-engineering-curriculum/senior-ml-engineer-learning) packet used under the same constraint.

The autonomous research loop is expected to fill that gap on its next cycle. To make that loop deterministic, this document grounds the requirements catalogue in **authoritative public references** that publish what the role is hired against — canonical books (Chip Huyen's *Designing Machine Learning Systems*, Will Larson's *Staff Engineer*), Google/DeepMind/NVIDIA/Microsoft primary literature (Chinchilla, Megatron, ZeRO, scaling laws), Google SRE and MLOps writing, NIST AI RMF and EU AI Act primary text, and public engineering career frameworks (Engineering Ladders, progression.fyi, StaffEng.com). Every requirement below cites at least one such reference, and every requirement is shaped so posting-frequency evidence can be added underneath it without restructure.

## Methodology

1. Sourced the canonical task domains for a level-40 staff-plus IC Machine Learning Engineer from public references — see `authoritative_references` in `.aicg/job-requirements.json`.
2. Applied the **staff-differentiation heuristic**: for every requirement, articulated **what a level-40 staff engineer owns that a level-30 senior tech-lead does not** — multi-team blueprint, cross-team eval program, org-scope build-vs-adopt call, portfolio-scope AI-risk register, multi-quarter roadmap that leadership commits to, staff-plus hiring calibration. This is captured in the `seniority_signal` field of every requirement.
3. Applied the **ownership rule**: assign coverage to the lowest-level role that genuinely requires the skill; higher levels add depth, architecture, or program-leadership framing. Peer specialist tracks own individual-contributor depth in their domain; the staff track owns the program that coordinates them. Peer platform tracks own the paved-road build; the staff track owns the org-scope consumer contract. The peer level-40 EM track (`ai-infra-team-lead-learning`) owns direct people management; this IC-staff track owns technical leadership without direct reports.
4. Flagged everything that has not yet been validated against in-window postings so the next cycle can demote any requirement whose evidence stays empty.

## Requirement themes → curriculum ownership

The table below lists each requirement theme, its planned owner per the level hierarchy, and the curriculum coverage path. **Freq** is intentionally blank for this cycle — it will be backfilled by the next research pass.

| # | Theme | Freq | Owner role | Coverage |
|---|---|---|---|---|
| 1 | Staff role scope, staff-plus archetypes, multi-team contracts | <!-- needs-research --> | `staff-ml-engineer` (this) | [`mod-401-staff-ml-role-scope`](lessons/mod-401-staff-ml-role-scope) |
| 2 | Multi-team ML systems architecture (portfolio-scope blueprints, shared substrates) | <!-- needs-research --> | `staff-ml-engineer` | [`mod-402-multi-team-ml-architecture`](lessons/mod-402-multi-team-ml-architecture); capstone in [`project-401`](projects/project-401-multi-team-ml-blueprint) |
| 3 | Foundation-model / large-scale fine-tune training programs (scaling laws, data mixture, ablations) | <!-- needs-research --> | `staff-ml-engineer` (program) → peer specialists for implementation | [`mod-403-foundation-model-training-programs`](lessons/mod-403-foundation-model-training-programs); capstone in [`project-402`](projects/project-402-foundation-model-training-program); depth at `training-pipeline-engineer`, `fine-tuning-engineer` |
| 4 | Distributed-training strategy (parallelism recipe, MFU, failure-mode budgeting, cluster-hour defence) | <!-- needs-research --> | `staff-ml-engineer` (strategy) → peer specialists for implementation | [`mod-404-distributed-training-systems-at-scale`](lessons/mod-404-distributed-training-systems-at-scale); depth at `training-pipeline-engineer`, `ai-infra-performance-learning` |
| 5 | ML platform strategy at org scope (build-vs-adopt, contribute-back RFCs, consumer contract) | <!-- needs-research --> | `staff-ml-engineer` (consumer strategy) → peer platform tracks for build | [`mod-405-ml-platform-strategy`](lessons/mod-405-ml-platform-strategy); build depth at `ai-infra-ml-platform-learning`, `ai-infra-mlops-learning` |
| 6 | Cross-team evaluation & experimentation program | <!-- needs-research --> | `staff-ml-engineer` | [`mod-406-cross-team-eval-and-experimentation`](lessons/mod-406-cross-team-eval-and-experimentation); platform depth at `model-evaluation-engineer`, `ai-eval-engineer` |
| 7 | GPU / cloud-ML capacity, cost, TCO at org scope | <!-- needs-research --> | `staff-ml-engineer` | [`mod-407-capacity-cost-and-tco`](lessons/mod-407-capacity-cost-and-tco) |
| 8 | Portfolio-scope ML reliability, SLOs, multi-team incident command | <!-- needs-research --> | `staff-ml-engineer` | [`mod-408-portfolio-reliability-and-incident-command`](lessons/mod-408-portfolio-reliability-and-incident-command) |
| 9 | Responsible AI at portfolio scope — NIST AI RMF, EU AI Act, model-card / datasheet standard | <!-- needs-research --> | `staff-ml-engineer` (ML-side program) → `ai-governance-analyst`, `head-of-ai-governance`, `ai-infra-security-learning` for depth | [`mod-409-responsible-ai-at-portfolio-scope`](lessons/mod-409-responsible-ai-at-portfolio-scope) |
| 10 | Staff-plus technical leadership — multi-quarter roadmap, exec comms, hiring calibration, coaching seniors | <!-- needs-research --> | `staff-ml-engineer` | [`mod-410-staff-plus-technical-leadership`](lessons/mod-410-staff-plus-technical-leadership); capstone in [`project-403`](projects/project-403-staff-plus-tech-leadership-simulation) |
| 11 | ML practitioner workflow foundations (data → model → deploy → monitor) | n/a — prerequisite | `ml-engineer` (level 20) | Listed in [`PREREQUISITES.md`](PREREQUISITES.md); not re-taught. Depth link: [`ml-engineer-learning`](https://github.com/ml-engineering-curriculum/ml-engineer-learning) |
| 12 | Single-team ML tech-lead scope (single-system architecture, eval harness, one-team roadmap, model-card authoring) | n/a — prerequisite | `senior-ml-engineer` (level 30) | Listed in [`PREREQUISITES.md`](PREREQUISITES.md); not re-taught. Depth link: [`senior-ml-engineer-learning`](https://github.com/ml-engineering-curriculum/senior-ml-engineer-learning) |
| 13 | Distributed training implementation depth (DDP/FSDP/NCCL, checkpoint sharding, kernel tuning) | n/a — peer specialist | `training-pipeline-engineer`, `ai-infra-performance-learning` | Strategy in mod-404; implementation depth linked out |
| 14 | LLM fine-tuning depth (LoRA/QLoRA, DPO/RLHF, dataset engineering) | n/a — peer specialist | `fine-tuning-engineer` | Program authorship in mod-403; depth linked out |
| 15 | Eval-platform build (LLM-judge harness, agent eval infra) | n/a — peer specialist | `model-evaluation-engineer`, `ai-eval-engineer` | Program authorship in mod-406; depth linked out |
| 16 | ML platform build (feature store, model registry, multi-tenant clusters) | n/a — peer platform | `ai-infra-ml-platform-learning`, `ai-infra-mlops-learning` | Consumer contract in mod-405; depth linked out |
| 17 | ML/AI security depth (data poisoning, model extraction, adversarial inputs, red-team ops) | n/a — higher level | `ai-infra-security-learning` (level 35) | ML-side program authorship in mod-409; depth linked out |
| 18 | Governance / compliance policy authoring, regulator engagement | n/a — peer / higher | `ai-governance-analyst`, `head-of-ai-governance` | ML-side program authorship in mod-409; depth linked out |
| 19 | Direct people management (1:1s, performance reviews, headcount) | n/a — peer level-40 EM | `ai-infra-team-lead-learning` | Not in scope; this track is the IC-staff counterpart. Manager-partnership dynamics covered in mod-410 |
| 20 | Org- and industry-wide strategy, keynote-grade external influence | n/a — higher level | `principal-ml-engineer` (level 50) | Linked forward; staff scope stops at multi-team + exec-audience communication |

## Seniority differentiation — what changes from level 30

The single most important signal in a Staff Machine Learning Engineer posting is the **shift in the object of the verb**. A level-30 posting says the engineer will "own the ranking model", "lead the fraud detection team", "run the experimentation review", "author the model card". A level-40 posting says the engineer will "own the ranking, retrieval, and personalization portfolio", "lead the ML org's build-vs-buy on the eval platform", "run the cross-team experiment review body", "author the AI-risk register the CISO signs off on", "shape the multi-quarter modeling roadmap that leadership commits to", "calibrate the senior ML engineers we hire".

The `seniority_signal` field on every requirement in `.aicg/job-requirements.json` captures that shift. When the autonomous research loop backfills the postings array, it should verify that each requirement's phrasing on the live evidence matches this staff shift — a "familiarity with" bullet or a single-system framing does not count as level-40 evidence for a requirement.

### Staff-plus IC vs. Engineering Manager at level 40

Two distinct level-40 role families coexist in ML orgs, both hired against the "staff-plus" bar but with different day-to-day scope:

- **Staff Machine Learning Engineer (this track)** — IC-staff path. Multi-team technical leadership without direct reports. Owns architecture, program, platform strategy, and the technical bar.
- **Engineering Manager, ML** — people-leadership path. Owned by the peer track [`ai-infra-team-lead-learning`](https://github.com/ai-infra-curriculum/ai-infra-team-lead-learning). Direct reports, hiring, performance management, cross-functional program partnership.

Postings whose primary verbs are people-management verbs ("hire", "grow the team", "1:1s", "performance reviews", "headcount") belong to the EM track and should be excluded from this catalogue on the next research pass. Postings whose primary verbs are architecture, program, and technical-bar verbs — even when they include a mentorship dimension — belong here.

Framework references used to cross-check the staff scope:

- Will Larson, *[Staff Engineer](https://staffeng.com/book)* (Stripe Press, 2021) — the canonical staff-plus IC ladder text.
- [StaffEng.com](https://staffeng.com/stories/) — corpus of real staff-plus engineer interviews.
- [Engineering Ladders](https://www.engineeringladders.com/) — public five-axis framework.
- [progression.fyi](https://www.progression.fyi/) — collected public engineering-career frameworks.

## Posting evidence

<!-- needs-research: populate with the ≥25 in-window postings sampled next cycle. Use the same table shape as ai-infra-team-lead-learning/JOB_REQUIREMENTS.md. -->

No postings were sampled this cycle. See the **Status** section above for the reason. The next autonomous research cycle should fan out across `job-boards.greenhouse.io`, `jobs.lever.co`, `jobs.ashbyhq.com`, and major employer ATS roots. A recommended employer coverage set:

- **AI-native / research labs** (weight heavily — the description-seed maps most cleanly here): Anthropic, OpenAI, xAI, Google DeepMind, Cohere, Scale AI, Databricks, Character.AI, Perplexity, Anysphere/Cursor, Together AI, Mistral, Hugging Face, Runway, ElevenLabs, Nvidia (Applied / Research), Fireworks AI, Groq, SambaNova, Weights & Biases, Cresta.
- **Consumer tech** (applied ML at scale — recsys, ranking, search, personalization): Airbnb, DoorDash, Instacart, Pinterest, Reddit, Snap, TikTok/ByteDance, Netflix, Spotify, Roblox, Duolingo, Discord.
- **Big tech** (dual scope — applied ML plus foundation-model programs): Google, Meta, Apple, Amazon, Microsoft, LinkedIn, Salesforce, ServiceNow, Uber, Cloudflare, Shopify.
- **Fintech** (fraud, credit, forecasting at scale): Stripe, Ramp, Brex, Plaid, Robinhood, SoFi, Chime, Affirm, Klarna, Block/Square, Coinbase.
- **Retail / adtech**: Wayfair, Walmart Global Tech, Target, The Trade Desk, Teads, Criteo, Etsy, Zillow.
- **Health / bio**: Tempus, Recursion, Flatiron, Verily, Included Health, PathAI, Grail.
- **Robotics / autonomy** (perception at fleet scope): Waymo, Cruise, Zoox, Nuro, Applied Intuition, Wayve, Figure, Skydio, Anduril.
- **Traditional**: JPMorgan Chase, Capital One, American Express, GM, Ford, Comcast.

For each posting, capture employer, exact title, URL, `date_observed`, `date_posted_estimate` (or `estimated:2026-MM`), location, 5–10 verbatim required-skill bullets, 2–6 preferred-qualification bullets, salary range when published, and one short representative quote. **Filter out**: mid-level (plain "Machine Learning Engineer"), Senior (L30), Principal (L50), Engineering Manager (peer L40 EM), Research Scientist / Applied Scientist (research track), and "Staff Data Scientist" titles — those belong to other tracks. **When in doubt on scope**, use the "verb test" described above: if the primary verbs are architecture, program, and technical-bar verbs, it belongs here; if they are people-management verbs, it belongs to the EM track.

## Ownership map — quick reference for next cycle

When backfilling postings, use this ownership decision to keep the curriculum from drifting into peer territory:

- **Staff ML Engineer (this track, level 40)** owns *multi-team architecture, cross-team programs (eval, experimentation, reliability), org-scope platform strategy, portfolio-scope responsible-AI, capacity/TCO framing, and staff-plus technical leadership*. Build-altitude fundamentals (L20) and single-team tech-lead scope (L30) are inherited, not re-taught.
- **Senior ML Engineer (level 30)** owns single-team tech-lead ML engineering — this track links to it rather than re-teaching.
- **Principal ML Engineer (level 50)** owns org- and industry-wide technical strategy — refer up when a posting reads that scope.
- **AI Infra Team Lead / EM (peer level 40)** owns the people-management path — this track is the IC-staff counterpart.
- **AI Infra ML Platform / MLOps** peer tracks own the paved-road platforms this staff role sets the org contract for.
- **Training Pipeline / AI Infra Performance** peer tracks own distributed-training infrastructure implementation and cluster tuning.
- **Model Evaluation / AI Evaluation Engineer** own evaluation-platform depth; the staff track owns the cross-team eval program.
- **NLP / RAG / LLM Application / Fine-Tuning / Applied AI** engineers own LLM specialisations; the staff track owns the delegation contract and the program.
- **AI Infra Security** (level 35) owns ML/AI security depth; the staff ML engineer is the ML-side program-owning counterpart.
- **AI Governance Analyst / Head of AI Governance** own governance depth; the staff ML engineer authors the ML-side risk register.

## Conclusion

<!-- needs-research: re-run on the next autonomous cycle with web tools enabled, populate `postings` in .aicg/job-requirements.json, backfill the Freq column above, and demote any requirement whose evidence_post_ids stays empty. -->

The curriculum plan in [`.aicg/curriculum-plan.json`](.aicg/curriculum-plan.json) is structured so that requirement frequencies can be added underneath each module without restructure. The themes are grounded in cited public references, not invented, and every requirement carries an explicit `seniority_signal` statement so the next research cycle can validate that live postings actually differentiate this level-40 role from a level-30 Senior ML Engineer and from a level-40 Engineering Manager.
