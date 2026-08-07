# Resources for mod-403-foundation-model-training-programs (Foundation-Model & Large-Scale Fine-Tune Training Programs)

Real, citable references for the topics this module covers. Read the scaling-law papers (Kaplan → Hoffmann → Muennighoff) end-to-end before exercise-01; treat the pipeline and pretraining reports as reference implementations whose *shape* your program brief adopts; treat the alignment papers as the menu chapter 7 selects from.

## Scaling laws — the chapter 2 core reading list

- **Kaplan, Jared et al.** *Scaling Laws for Neural Language Models.* arXiv 2020. [arXiv:2001.08361](https://arxiv.org/abs/2001.08361). The origin text for the 6ND FLOP accounting and the power-law form. Chapter 2's Kaplan-era over-N framing is derived from — and its limitations noted by — this paper's fit ranges.
- **Hoffmann, Jordan et al.** *Training Compute-Optimal Large Language Models.* NeurIPS 2022 / DeepMind ("Chinchilla"). [arXiv:2203.15556](https://arxiv.org/abs/2203.15556). The compute-optimal (N, D) result and the ~20-tokens-per-parameter heuristic that reset the field. Read all three fit methodologies (isoflop, isoloss, parametric) — the paper's caveats matter as much as the headline.
- **Besiroglu, Tamay et al.** *Chinchilla Scaling: A replication attempt.* arXiv 2024. [arXiv:2404.10102](https://arxiv.org/abs/2404.10102). Re-fits the Chinchilla parametric form and widens the confidence intervals; the ±30% band chapter 2 recommends comes from replications like this one.
- **Muennighoff, Niklas et al.** *Scaling Data-Constrained Language Models.* NeurIPS 2023. [arXiv:2305.16264](https://arxiv.org/abs/2305.16264). The definitive study of controlled epoching. Chapter 4's option-3 (repeat high-quality tokens) posture cites the finding that repeats up to ~4 epochs are nearly as good as fresh tokens, with steep diminishing returns beyond.
- **Sardana, Nikhil; Frankle, Jonathan.** *Beyond Chinchilla-Optimal: Accounting for Inference in Language Model Scaling Laws.* arXiv 2023. [arXiv:2401.00448](https://arxiv.org/abs/2401.00448). The formal treatment of the deployment-cost-dominated over-training call that chapter 2 walks informally.

## Reference training programs — the "read the report" list

The chapters cite specific programs' publications for specific decisions. Read at least one pretraining report and one fine-tune report before exercise-01.

- **Touvron, Hugo et al.** *LLaMA: Open and Efficient Foundation Language Models.* Meta AI 2023. [arXiv:2302.13971](https://arxiv.org/abs/2302.13971). LLaMA-1. The origin of the deliberate over-training call at 7B and 13B; ~140:1 tokens-per-parameter for 7B. Chapter 2's deployment-cost-dominated framing anchors here.
- **Touvron, Hugo et al.** *Llama 2: Open Foundation and Fine-Tuned Chat Models.* Meta AI 2023. [arXiv:2307.09288](https://arxiv.org/abs/2307.09288). LLaMA-2. The chat-model post-training stack (SFT + rejection-sampling + PPO) chapter 7 uses as a reference sequence, plus mixture documentation.
- **Llama Team, Meta AI.** *The Llama 3 Herd of Models.* Meta 2024. [arXiv:2407.21783](https://arxiv.org/abs/2407.21783). LLaMA-3. The 15T-token pretraining, the cool-down mixture shift, the tool-use post-training, and the extensive scaling-law and mixture discussion make this the single most useful contemporary training report for the module.
- **DeepSeek-AI.** *DeepSeek LLM: Scaling Open-Source Language Models with Longtermism.* arXiv 2024. [arXiv:2401.02954](https://arxiv.org/abs/2401.02954). The scaling-law re-derivation with an emphasis on hyperparameter transfer and mixture; the paper's discussion of code-weight is one of the strongest published treatments.
- **DeepSeek-AI.** *DeepSeek-V2: A Strong, Economical, and Efficient Mixture-of-Experts Language Model.* arXiv 2024. [arXiv:2405.04434](https://arxiv.org/abs/2405.04434). MoE-scale reference for programs considering non-dense architectures.
- **Almazrouei, Ebtesam et al.** *The Falcon Series of Open Language Models.* TII 2023. [arXiv:2311.16867](https://arxiv.org/abs/2311.16867). The RefinedWeb pretraining pipeline; a strong counterexample to the "add curated corpora" mixture default.
- **MosaicML.** *Introducing MPT-7B: A New Standard for Open-Source, Commercially Usable LLMs.* MosaicML blog 2023. [databricks.com/blog/mpt-7b](https://www.databricks.com/blog/mpt-7b). One of the most-cited mid-sized-open-training reports; useful as a program-shape reference for a 7B pretraining program.
- **BigScience Workshop.** *BLOOM: A 176B-Parameter Open-Access Multilingual Language Model.* arXiv 2022. [arXiv:2211.05100](https://arxiv.org/abs/2211.05100). The reference multilingual-heavy mixture and the reference open-consortium program-management writeup.
- **Zhang, Susan et al.** *OPT: Open Pre-trained Transformer Language Models.* Meta AI 2022. [arXiv:2205.01068](https://arxiv.org/abs/2205.01068). Companion to the OPT-175B *logbook* linked below; the paper is the write-up, the logbook is the reality.
- **BigScience.** *The BLOOM training chronicles.* [huggingface.co/bigscience/bloom (model card)](https://huggingface.co/bigscience/bloom) and the community `chronicles.md`. The reference "what an actual training program's operational shape looks like" document for chapter 3's restart budgeting.
- **Meta / OPT-175B logbook.** [`facebookresearch/metaseq` `logbook.pdf`](https://github.com/facebookresearch/metaseq/blob/main/projects/OPT/chronicles/OPT175B_Logbook.pdf). The most-cited "what really happens during a main run" primary source; the restart, loss-spike, and hardware-failure story chapter 3's risk-adjustment table is calibrated against.
- **Rae, Jack W. et al.** *Scaling Language Models: Methods, Analysis & Insights from Training Gopher.* DeepMind 2021. [arXiv:2112.11446](https://arxiv.org/abs/2112.11446). Reference for the MassiveWeb heuristic-filter cascade chapter 5 uses.
- **Yang, Aiyuan et al.** *Baichuan 2: Open Large-scale Language Models.* Baichuan Inc. 2023. [arXiv:2309.10305](https://arxiv.org/abs/2309.10305). Reference for a domain-heavy Chinese-primary program; useful triangulation on the multilingual mixture-weight decision.

## Data pipelines and datasets — the chapter 4-5 substrate

- **Soldaini, Luca et al.** *Dolma: an Open Corpus of Three Trillion Tokens for Language Model Pretraining Research.* AI2 2024. [arXiv:2402.00159](https://arxiv.org/abs/2402.00159). The single most thoroughly-documented open pretraining corpus and pipeline; the paper is the reference for a chapter 5 pipeline design.
- **Li, Jeffrey et al.** *DataComp-LM: In search of the next generation of training sets for language models.* arXiv 2024. [arXiv:2406.11794](https://arxiv.org/abs/2406.11794). The DCLM benchmark and the framing of the quality-classifier filter as a scaling axis; chapter 5's classifier-filter discussion is DCLM-shaped.
- **Together AI.** *RedPajama-Data-v2: An open dataset with 30 trillion tokens for training large language models.* Together blog 2023 + [`togethercomputer/RedPajama-Data`](https://github.com/togethercomputer/RedPajama-Data). Reference open pretraining dataset at trillion-token scale with per-slice provenance.
- **Penedo, Guilherme et al.** *The RefinedWeb Dataset for Falcon LLM.* NeurIPS 2023. [arXiv:2306.01116](https://arxiv.org/abs/2306.01116). The web-only filter-heavy pretraining-data case study.
- **Gao, Leo et al.** *The Pile: An 800GB Dataset of Diverse Text for Language Modeling.* EleutherAI 2020. [arXiv:2101.00027](https://arxiv.org/abs/2101.00027). The reference multi-source open corpus that documented per-slice weighting before the current generation.
- **Raffel, Colin et al.** *Exploring the Limits of Transfer Learning with a Unified Text-to-Text Transformer.* JMLR 2020 (T5 / C4). [arXiv:1910.10683](https://arxiv.org/abs/1910.10683). The origin of the C4 corpus and its heuristic quality filters.
- **Common Crawl Foundation.** [commoncrawl.org](https://commoncrawl.org/). The upstream source. The [snapshot list](https://commoncrawl.org/get-started) is what chapter 5's "pin the snapshot ID" recommendation refers to.
- **Kocetkov, Denis et al.** *The Stack: 3 TB of permissively licensed source code.* arXiv 2022. [arXiv:2211.15533](https://arxiv.org/abs/2211.15533). Reference for licensing-aware code corpus collection.
- **Li, Raymond et al.** *StarCoder: may the source be with you!* arXiv 2023 / BigCode. [arXiv:2305.06161](https://arxiv.org/abs/2305.06161). The code-pretraining pipeline that adopted the licensing-and-opt-out discipline chapter 5 recommends.
- **Xie, Sang Michael et al.** *DoReMi: Optimizing Data Mixtures Speeds Up Language Model Pretraining.* NeurIPS 2023. [arXiv:2305.10429](https://arxiv.org/abs/2305.10429). The domain-reweighting-via-Group-DRO method chapter 4 discusses.
- **Liu, Qian et al.** *RegMix: Data Mixture as Regression for Language Model Pre-training.* arXiv 2024. [arXiv:2407.01492](https://arxiv.org/abs/2407.01492). The follow-on mixture-optimisation method.
- **Gunasekar, Suriya et al.** *Textbooks Are All You Need.* Microsoft Research 2023 (Phi-1). [arXiv:2306.11644](https://arxiv.org/abs/2306.11644). The reference for chapter 4's option-1 strict-high-quality posture.
- **Hu, Shengding et al.** *MiniCPM: Unveiling the Potential of Small Language Models with Scalable Training Strategies.* arXiv 2024. [arXiv:2404.06395](https://arxiv.org/abs/2404.06395). One of the clearest published treatments of the warmup / stable / decay learning-rate schedule with cool-down mixture shift.

## Dedup, decontamination, and memorisation

- **Lee, Katherine et al.** *Deduplicating Training Data Makes Language Models Better.* ACL 2022. [arXiv:2107.06499](https://arxiv.org/abs/2107.06499). Chapter 5's "dedup is a scaling axis" claim rests on this paper. Read it before drafting exercise-02's dedup section.
- **Carlini, Nicholas et al.** *Quantifying Memorization Across Neural Language Models.* ICLR 2023. [arXiv:2202.07646](https://arxiv.org/abs/2202.07646). The memorisation-scaling result and the reason chapter 5 treats dedup as a legal and safety concern, not just a quality one.
- **Broder, Andrei Z.** *On the resemblance and containment of documents.* IEEE Compression and Complexity of Sequences 1997. The original MinHash paper. Foundational reading if the exercise-02 dedup section needs to defend the choice at the review meeting.

## Distributed training and MFU (chapter 3 depth)

- **Chowdhery, Aakanksha et al.** *PaLM: Scaling Language Modeling with Pathways.* Google Research 2022. [arXiv:2204.02311](https://arxiv.org/abs/2204.02311). The reference PaLM 540B training and one of the first papers to publish MFU as a first-class metric (~46% on TPUv4).
- **Jiang, Ziheng et al.** *MegaScale: Scaling Large Language Model Training to More Than 10,000 GPUs.* ByteDance / NSDI 2024. [arXiv:2402.15627](https://arxiv.org/abs/2402.15627). Reference for very-large-scale MFU numbers (~55%) and the operational engineering that gets there.
- **Rajbhandari, Samyam et al.** *ZeRO: Memory Optimizations Toward Training Trillion Parameter Models.* SC 2020. [arXiv:1910.02054](https://arxiv.org/abs/1910.02054). ZeRO-1/2/3 and the foundation for FSDP. The peer mod-404 goes deep here.
- **Shoeybi, Mohammad et al.** *Megatron-LM: Training Multi-Billion Parameter Language Models Using Model Parallelism.* arXiv 2019. [arXiv:1909.08053](https://arxiv.org/abs/1909.08053). Tensor-parallel origin text.
- **Narayanan, Deepak et al.** *Efficient Large-Scale Language Model Training on GPU Clusters Using Megatron-LM.* SC 2021. [arXiv:2104.04473](https://arxiv.org/abs/2104.04473). The 3D-parallelism paper; chapter 3's MFU planning bands are anchored on this and later work.

## Hyperparameter transfer (chapter 6 depth)

- **Yang, Greg et al.** *Tensor Programs V: Tuning Large Neural Networks via Zero-Shot Hyperparameter Transfer (µTransfer).* NeurIPS 2022. [arXiv:2203.03466](https://arxiv.org/abs/2203.03466). µP and µTransfer; chapter 6 phase 1 is µP-centred.
- **Yang, Greg et al.** *Tensor Programs VI: Feature Learning in Infinite-Depth Neural Networks.* arXiv 2023. [arXiv:2310.02244](https://arxiv.org/abs/2310.02244). The depth-transfer extension of µP that some 2024+ programs use.

## Evaluation and benchmarks (chapter 6 tier 3-5)

- **Hendrycks, Dan et al.** *Measuring Massive Multitask Language Understanding (MMLU).* ICLR 2021. [arXiv:2009.03300](https://arxiv.org/abs/2009.03300).
- **Suzgun, Mirac et al.** *Challenging BIG-Bench Tasks and Whether Chain-of-Thought Can Solve Them (BBH).* ACL 2023. [arXiv:2210.09261](https://arxiv.org/abs/2210.09261).
- **Clark, Peter et al.** *Think you have Solved Question Answering? Try ARC.* arXiv 2018. [arXiv:1803.05457](https://arxiv.org/abs/1803.05457).
- **Zellers, Rowan et al.** *HellaSwag: Can a Machine Really Finish Your Sentence?* ACL 2019. [arXiv:1905.07830](https://arxiv.org/abs/1905.07830).
- **Chen, Mark et al.** *Evaluating Large Language Models Trained on Code (HumanEval).* arXiv 2021. [arXiv:2107.03374](https://arxiv.org/abs/2107.03374).
- **Austin, Jacob et al.** *Program Synthesis with Large Language Models (MBPP).* arXiv 2021. [arXiv:2108.07732](https://arxiv.org/abs/2108.07732).
- **Cobbe, Karl et al.** *Training Verifiers to Solve Math Word Problems (GSM8K).* arXiv 2021. [arXiv:2110.14168](https://arxiv.org/abs/2110.14168).
- **Hendrycks, Dan et al.** *Measuring Mathematical Problem Solving With the MATH Dataset.* NeurIPS 2021. [arXiv:2103.03874](https://arxiv.org/abs/2103.03874).
- **Lin, Stephanie; Hilton, Jacob; Evans, Owain.** *TruthfulQA: Measuring How Models Mimic Human Falsehoods.* ACL 2022. [arXiv:2109.07958](https://arxiv.org/abs/2109.07958).
- **Parrish, Alicia et al.** *BBQ: A Hand-Built Bias Benchmark for Question Answering.* ACL 2022. [arXiv:2110.08193](https://arxiv.org/abs/2110.08193).
- **Hartvigsen, Thomas et al.** *ToxiGen: A Large-Scale Machine-Generated Dataset for Adversarial and Implicit Hate Speech Detection.* ACL 2022. [arXiv:2203.09509](https://arxiv.org/abs/2203.09509).
- **Gehman, Samuel et al.** *RealToxicityPrompts: Evaluating Neural Toxic Degeneration in Language Models.* EMNLP 2020. [arXiv:2009.11462](https://arxiv.org/abs/2009.11462).
- **Zheng, Lianmin et al.** *Judging LLM-as-a-Judge with MT-Bench and Chatbot Arena.* NeurIPS 2023. [arXiv:2306.05685](https://arxiv.org/abs/2306.05685). The instruction-following judge-model reference.
- **Dubois, Yann et al.** *Length-Controlled AlpacaEval: A Simple Way to Debias Automatic Evaluators (AlpacaEval 2.0).* arXiv 2024. [arXiv:2404.04475](https://arxiv.org/abs/2404.04475).
- **Zhou, Jeffrey et al.** *Instruction-Following Evaluation for Large Language Models (IFEval).* arXiv 2023. [arXiv:2311.07911](https://arxiv.org/abs/2311.07911).
- **Gao, Leo et al.** *lm-evaluation-harness.* EleutherAI. [`EleutherAI/lm-evaluation-harness`](https://github.com/EleutherAI/lm-evaluation-harness). The open-source harness that most programs' tier-3 waterfall runs against. Peer mod-406 goes deep here.

## Post-training — the chapter 7 menu

- **Ouyang, Long et al.** *Training language models to follow instructions with human feedback (InstructGPT).* NeurIPS 2022. [arXiv:2203.02155](https://arxiv.org/abs/2203.02155). The reference SFT + RLHF-PPO sequence.
- **Bai, Yuntao et al.** *Training a Helpful and Harmless Assistant with Reinforcement Learning from Human Feedback (Anthropic HH).* arXiv 2022. [arXiv:2204.05862](https://arxiv.org/abs/2204.05862). The source of the inverted-U observation between safety training and helpfulness that chapter 7 anchors on.
- **Bai, Yuntao et al.** *Constitutional AI: Harmlessness from AI Feedback.* Anthropic 2022. [arXiv:2212.08073](https://arxiv.org/abs/2212.08073). The reference for the constitutional-style methods.
- **Rafailov, Rafael et al.** *Direct Preference Optimization: Your Language Model is Secretly a Reward Model.* NeurIPS 2023. [arXiv:2305.18290](https://arxiv.org/abs/2305.18290). The DPO paper; the modern default preference-optimisation method.
- **Azar, Mohammad Gheshlaghi et al.** *A General Theoretical Paradigm to Understand Learning from Human Preferences (IPO).* arXiv 2023. [arXiv:2310.12036](https://arxiv.org/abs/2310.12036). The IPO stability-improved variant.
- **Ethayarajh, Kawin et al.** *KTO: Model Alignment as Prospect Theoretic Optimization.* arXiv 2024. [arXiv:2402.01306](https://arxiv.org/abs/2402.01306). Preference optimisation from unpaired positive/negative signals.
- **Schulman, John et al.** *Proximal Policy Optimization Algorithms.* arXiv 2017. [arXiv:1707.06347](https://arxiv.org/abs/1707.06347). The PPO reference for programs that still run RLHF-PPO.
- **Perez, Ethan et al.** *Red Teaming Language Models with Language Models.* EMNLP 2022. [arXiv:2202.03286](https://arxiv.org/abs/2202.03286). The reference for automated red-teaming that chapter 7 cites.
- **Schick, Timo et al.** *Toolformer: Language Models Can Teach Themselves to Use Tools.* NeurIPS 2023. [arXiv:2302.04761](https://arxiv.org/abs/2302.04761). The tool-use fine-tuning reference.
- **Xu, Shusheng et al.** *Is DPO Superior to PPO for LLM Alignment? A Comprehensive Study.* ICML 2024. [arXiv:2404.10719](https://arxiv.org/abs/2404.10719). The published comparison the stretch goal in exercise-04 references.
- **Cui, Ganqu et al.** *UltraFeedback: Boosting Language Models with High-quality Feedback.* arXiv 2023. [arXiv:2310.01377](https://arxiv.org/abs/2310.01377). The reference open preference dataset.
- **Anthropic.** *HH-RLHF dataset.* [`anthropics/hh-rlhf`](https://github.com/anthropics/hh-rlhf). The reference helpful-and-harmless preference dataset.

## Regulatory, licensing, safety, and model documentation

- **European Parliament and Council.** *Regulation (EU) 2024/1689 on Artificial Intelligence (EU AI Act).* Official Journal L Series, 12 July 2024. [eur-lex.europa.eu](https://eur-lex.europa.eu/eli/reg/2024/1689/oj). Article 53 obligations on providers of general-purpose AI models — including the training-data-summary requirement — are what chapter 5's pipeline audit trail feeds.
- **NIST.** *AI Risk Management Framework (AI RMF 1.0).* NIST AI 100-1, January 2023. [nist.gov/itl/ai-risk-management-framework](https://www.nist.gov/itl/ai-risk-management-framework). Plus the *Generative AI Profile* (NIST AI 600-1, 2024) covering GenAI-specific risks. Chapter 7's red-team category work triangulates against the RMF's risk framings.
- **OWASP.** *OWASP Top 10 for Large Language Model Applications.* [owasp.org/www-project-top-10-for-large-language-model-applications](https://owasp.org/www-project-top-10-for-large-language-model-applications/). The reference red-team category taxonomy chapter 7 and exercise-04 reference.
- **Mitchell, Margaret et al.** *Model Cards for Model Reporting.* FAccT 2019. [arXiv:1810.03993](https://arxiv.org/abs/1810.03993). The model-card format chapter 7 hands off into mod-409.
- **Gebru, Timnit et al.** *Datasheets for Datasets.* Communications of the ACM 2021. [arXiv:1803.09010](https://arxiv.org/abs/1803.09010). The corpus-documentation format the chapter 5 pipeline metadata is closest to.
- **General Data Protection Regulation.** [gdpr.eu](https://gdpr.eu). And **California Consumer Privacy Act.** [oag.ca.gov/privacy/ccpa](https://oag.ca.gov/privacy/ccpa). The regional PII regimes chapter 5's PII posture cites.

## Program-management and executive-communication references

- **Larson, Will.** *Staff Engineer: Leadership beyond the management track.* Stripe Press, 2021. [staffeng.com/book](https://staffeng.com/book). The programme-scope archetype the chapters position the mod-403 owner against.
- **Fournier, Camille.** *The Manager's Path.* O'Reilly, 2017. Cross-reference for the executive-review meeting shape chapter 8 walks.
- **Bezos, Jeff.** *2004 letter to shareholders* and the Amazon "six-page narrative memo" tradition. The prose-first executive-review culture chapter 8's one-pager sits inside. See [aboutamazon.com/news/company-news/2004-shareholder-letter](https://www.aboutamazon.com/news/company-news/2004-letter-to-shareholders) for the shareholder-letter origin.

## In-track references

- [`../../CURRICULUM.md`](../../CURRICULUM.md) — the module and project plan mod-403 sits inside.
- [`../../PREREQUISITES.md`](../../PREREQUISITES.md) — the assumed L20 / L30 foundation and the full list of peer specialist, peer platform, and peer L40 tracks the chapter 1 hand-off contracts route to.
- [`../mod-401-staff-ml-role-scope/`](../mod-401-staff-ml-role-scope/) — the altitude vocabulary, the four staff archetypes, and the hand-off-contract shape chapter 1 and chapter 7 operationalise for training-program scope.
- [`../mod-402-multi-team-ml-architecture/`](../mod-402-multi-team-ml-architecture/) — the RFC template chapter 8's one-pager pairs with when the program lands inside a portfolio-scope architectural change.
- [`../mod-404-distributed-training-systems-at-scale/`](../mod-404-distributed-training-systems-at-scale/) — the parallelism-recipe depth chapter 3's MFU planning bands cite forward to.
- [`../mod-406-cross-team-eval-and-experimentation/`](../mod-406-cross-team-eval-and-experimentation/) — the cross-team eval-contract depth chapter 6's tier-5 human-preference-eval section cites forward to.
- [`../mod-407-capacity-cost-and-tco/`](../mod-407-capacity-cost-and-tco/) — the org-scope capacity portfolio chapter 3's cluster-hour budget rolls up into.
- [`../mod-409-responsible-ai-at-portfolio-scope/`](../mod-409-responsible-ai-at-portfolio-scope/) — the portfolio-scope risk register and pre-launch gate that chapter 7's safety posture and exercise-04's launch-gate feed into.
- [`../../projects/project-402-foundation-model-training-program/`](../../projects/project-402-foundation-model-training-program/) — the capstone that combines this module's five exercises into an end-to-end program artifact.
