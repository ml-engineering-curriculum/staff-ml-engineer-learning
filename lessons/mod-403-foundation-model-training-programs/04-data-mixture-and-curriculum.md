# Data Mixture and Curriculum: Designing the D Side of the Bet

## Motivation

Chapter 2's scaling-law bet decided D — the number of training tokens the model should see. Chapter 3 defended the compute to reach D. This chapter answers the harder question: **which tokens?** Two programs with the same D and the same C but different data mixtures produce meaningfully different models. The mixture is the second-most-load-bearing decision in the program, after the (N, D) bet, and it is arguably the most-debated in program reviews because it is the decision leadership has the strongest intuitions about ("we should train on more code", "should we include patents?", "why don't we buy the whole Books3 corpus and be done").

This chapter walks the mixture-design decision: the source classes a program picks from, the quality-vs-tokens trade-off, the mixture-reweighting techniques the recent literature has produced, and the **curriculum** — the staged mixture across training that most modern programs use to squeeze out the last quality percent. Chapter 5 walks the curation pipeline that turns raw sources into usable tokens.

## The mixture-design decision

The first artifact of chapter 4 is a **mixture table** — a two-column artifact whose rows are source classes and whose columns are the fraction of training-token budget allocated to each class. A minimal template:

```
Source class                | Weight (%)   | Notes
----------------------------|--------------|--------------------------------
Curated web (high-quality)  |     35       | C4-style / RedPajama Common Crawl / DCLM
Broad web (Common Crawl)    |     25       | Deduped + quality-filtered
Code                        |     15       | GitHub + StackExchange + The Stack
Academic and books          |     10       | ArXiv, PubMed, licensed books
Math and reasoning          |      5       | OpenWebMath, Proof-Pile, curated CoT
Dialogue and forums         |      5       | Reddit / StackExchange / licensed dialogue
Multilingual                |      5       | Wikipedia + curated non-English web
```

That is one plausible mixture. Different programs pick differently — Meta's LLaMA-2 paper published its mixture (with weights the paper roughly implies rather than states directly), the Falcon RefinedWeb paper pushed hard on web-only-plus-filtering, and the Bloom mixture emphasised multilingual coverage. There is no single-right mixture; there is a family of defensible mixtures, and the program brief has to justify why *your* mixture is the right pick for *your* downstream use case.

The point of the mixture table is that it makes the debate concrete. A reviewer saying *"we should train on more code"* is asking to move the 15% code weight up. The program owner's answer is either *"yes, and we lose X percent from Y source to make room"* or *"no, because we tested it in ablation and saw Z"*. Neither answer is possible without the table.

## The source classes, in more detail

Each row of the mixture table represents a **source class** with its own quality, licensing, and volume profile. The commonly-recognised classes:

### Curated web

Web pages that pass a quality filter — most often a classifier trained to distinguish "Wikipedia-quality" content from generic web. The lineage runs from the C4 corpus (Raffel et al. 2020, T5) through the Common-Crawl-derived subsets in The Pile (Gao et al. 2020) through RedPajama-1T's Common Crawl slice (Together AI 2023) through DCLM (Li et al. 2024) which explicitly frames the quality-filter as a scaling axis.

Curated web is the volume workhorse — hundreds of billions to trillions of tokens available. The quality filter is what turns raw Common Crawl into usable tokens; the filter's design choices are what recent work has been contesting. See chapter 5 for the filter pipeline.

### Broad web (deduped Common Crawl)

The larger, less-heavily-filtered slice of Common Crawl that provides bulk tokens beyond the curated subset. Programs that want the maximum D within a fixed data-quality budget often include a broad-web slice at a lower weight than the curated slice — the theory being that mixing broad and curated buys diversity without paying too much for quality regression.

### Code

Source-code repositories from GitHub and equivalents, plus StackExchange, plus specialised corpora. The Stack (Kocetkov et al. 2022) and StarCoder (Li et al. 2023) established the licensing-aware collection pattern. Code is a scaling-law-multiplier for reasoning: including 10-20% code in the mixture improves downstream reasoning benchmarks measurably even when the model is not evaluated on code — a finding replicated across LLaMA, Mistral, and the DeepMind literature. The DeepSeek papers have argued for higher code weights than the field default; the ablation phase is where the program tests its own mixture.

### Academic and books

ArXiv, PubMed Central, licensed books corpora. High-quality-per-token but volume-limited: the entire English-language book corpus and arXiv sit in the tens of billions of tokens, so a 5-15% mixture weight is where most programs land. Licensing is the operational constraint (chapter 5) — the Books3 corpus that trained a generation of models is not licensed for commercial use, and modern programs that expect distribution licensing must source books from license-clean pipelines.

### Math and reasoning

Curated math (OpenWebMath, Proof-Pile), textbook-style problem-and-solution corpora, and increasingly, synthetic chain-of-thought data. Even smaller volume than academic-and-books; typically 3-8% weight. The bet is that a small percentage of high-quality reasoning tokens up-shifts the model's downstream reasoning performance materially. Ablation evidence supports this at modest scale; the exact ROI is program-specific.

### Dialogue and forums

Reddit, StackExchange, licensed dialogue corpora. The bet is that a model that has seen a lot of turn-taking discourse pretraining is stronger downstream on instruction-following. Not every program includes a dialogue slice at pretraining — some rely entirely on SFT (chapter 7) to inject conversational structure post-training. The pretraining-vs-post-training assignment is a program-level decision, and it is worth writing it down explicitly.

### Multilingual

Non-English web plus Wikipedia in multiple languages. The mixture weight here reflects the program's downstream use case: an English-primary consumer product weights this at 3-7%; a genuinely multilingual product weights it at 20-40%. Bloom (BigScience 2022) was the reference multilingual-heavy program; LLaMA-2 was near-English-only.

### Domain-specific (for large-scale fine-tune programs)

The distinguishing row for a domain-adapted fine-tune program: the domain corpus itself — legal filings for a legal model, medical notes for a healthcare model, ticket transcripts for a customer-support model. Weight this at **10-40%** of the continued-pretraining mixture depending on how much the base model already saw of the domain and how important domain-specific vocabulary is to the downstream task. This row does not appear in a from-scratch pretraining mixture; it is the reason a large-scale fine-tune program exists.

## The quality-vs-tokens trade-off

The single most important framing in modern mixture design is the trade-off between **mixture quality** and **total tokens available**. The Chinchilla-optimal D from chapter 2 assumes tokens are unbounded; in practice, high-quality tokens are bounded, and the program's D target may or may not be reachable using only high-quality sources.

Three points on the trade-off, in order of aggressiveness:

**1. Stay strictly within high-quality sources.** Cap D at whatever the high-quality corpus provides after dedup. If that D is less than the Chinchilla-optimal D from chapter 2, either (a) scale N down to match, or (b) accept a training-cost-dominated program that is off Chinchilla-optimal but on the same isoflop bowl. This is what smaller, quality-focused programs (Phi-1, Phi-2, Phi-3, and some domain-adapted fine-tune programs) do explicitly. Gunasekar et al. (2023) *"Textbooks Are All You Need"* is the canonical write-up.

**2. Extend D by broadening the mixture, accepting per-token quality regression.** Add broader-web, multilingual, and code slices at their natural quality distributions, filtered but not curated. The regression is bounded and per-token quality is well below curated web, but the additional D can be worth the trade at fixed C. This is the majority-of-programs choice and what LLaMA-2 through LLaMA-3 documented.

**3. Extend D by repeating high-quality tokens (multi-epoch training).** Muennighoff et al. (2023) *"Scaling Data-Constrained Language Models"* is the definitive study. The finding: repeating high-quality tokens up to ~4 epochs adds nearly the same benefit as fresh tokens; beyond ~4-8 epochs the diminishing returns are steep. For programs whose Chinchilla-optimal D exceeds their unique-high-quality corpus size, controlled epoching is a legitimate option and often preferable to option (2).

Modern programs almost always use a combination — a base mixture that is majority curated-web with code and academic mixed in (option 1 flavour), stretched by broader web (option 2) and mild epoching of the highest-quality sources (option 3). The program brief writes down which lever is being pulled and how much.

## Mixture reweighting — DoReMi and successors

Which weights? Programs used to pick weights by hand — the LLaMA-2 paper's mixture is essentially hand-tuned — but the last two years produced systematic reweighting methods.

**DoReMi** (Xie et al. 2023, "DoReMi: Optimizing Data Mixtures Speeds Up Language Model Pretraining"). Trains a proxy model on the uniform mixture, then optimises the mixture weights against domain-level loss using Group Distributionally Robust Optimisation. The mixture that comes out of DoReMi is not the mixture that minimises average loss; it is the mixture that maximises loss on the *worst-performing* domain given a fixed compute budget. The idea is that the mixture whose worst-case domain loss is lowest at fixed C is a more robust base model.

**RegMix** (Liu et al. 2024) and **subsequent work** push the same idea further — treating the mixture-weight decision as a hyperparameter optimisation problem where the objective is measured on small proxy runs and extrapolated. The mechanics vary; the framing is the same.

The staff-plus decision on reweighting:

- For a first-time program, **hand-tune the mixture based on published references and confirm with small proxy-run ablations** (chapter 6). Reweighting methods add real value but they add real complexity, and their extrapolation from proxy runs to main run is a research question the program has to test.
- For a program at scale with a stable data pipeline, **use DoReMi-style reweighting as an ablation-time decision** and treat its output as a proposal, not a fait accompli. The mixture the reweighting method produces sometimes ranks the "wrong" domain higher (upweighting a source class that has data-quality problems the loss does not see); the review is the check.
- **Do not use reweighting to escape a hard licensing constraint.** If a source class is off-limits for licensing, no amount of DoReMi-inference will make it useful.

## Curriculum — staged mixture across training

The mixture the program uses is often not one mixture but **several mixtures across training phases**. Two common patterns:

**1. Warmup → main → cool-down.** Most modern programs use a small warmup phase (0.5-2% of steps) with a lower learning rate on a broad mixture, transition to the main mixture and target learning rate, and end with a **cool-down phase** (5-15% of steps) in which the learning rate decays to near-zero *and* the mixture is shifted toward the highest-quality tokens. The cool-down is where the program "spends" its best data — the intuition is that late-training tokens have the highest impact per token on the final model, so use the best tokens then.

The MiniCPM paper (Hu et al. 2024) and the DataComp-LM paper (Li et al. 2024) both make the cool-down mixture shift explicit. LLaMA-3 mentioned a cool-down phase in its report. The Chinchilla-era intuition was uniform mixture across training; the modern intuition is unambiguously that the last 10% of training benefits from a higher-quality mixture.

**2. Domain-progressive curriculum.** For a large-scale fine-tune program continuing from a base, the standard shape is: (a) continue pretraining on a mixture that is broad-plus-domain (the domain data mixed in at 20-40%), then (b) instruction-tune with domain-specific SFT, then (c) preference-optimise with domain-specific preference data. The "curriculum" is across the three phases of the program rather than within a phase. Chapter 7 walks phases (b) and (c) as post-training decisions.

Some programs experiment with **within-phase curriculum** — starting with easier tokens and progressing to harder tokens — but the empirical evidence for this at pretraining scale is weaker than the cool-down evidence. Treat within-phase curriculum as a research choice to be validated in ablation, not as a default.

## Two mixture-design failure modes

**1. Ship the LLaMA mixture and stop thinking.** LLaMA-2's mixture is well-published and reproducible, and a first-time program adopting it as a baseline is legitimate. The failure is stopping there: not running any mixture ablation, not testing the code weight, not testing the multilingual weight, and not documenting *why* the LLaMA mixture is the right pick for a program whose downstream use case is materially different from Meta's. The review will nudge to run at least a two-cell mixture ablation before commit.

**2. Over-index on domain data in a large-scale fine-tune program.** A common overshoot in fine-tune programs: weight the domain corpus at 60-80% of continued pretraining and assume that maximises domain benefit. In practice, this over-shifts the base's distribution and the model regresses on general capability; the resulting model is "worse at everything except the domain" and often worse-on-domain too because the mixture destroyed the base's reasoning. The published fine-tune literature (BloombergGPT, Med-PaLM, HealthLLM, code-specific models) converges on **10-30%** domain-corpus weight during continued pretraining, not 60-80%. Test this in ablation.

## Where the mixture decision meets the D-of-chapter-2 decision

Chapter 2's D was a demand. Chapter 4's mixture design tests whether D is *supplied*. If the program's mixture at the required weights produces only 700B usable tokens after dedup and the D target is 1.1T, the program either (a) accepts option-3 mild epoching (~1.6× on the high-quality slice), (b) broadens the mixture to include lower-quality sources (option 2, at a per-token quality cost), or (c) revises the (N, D) bet toward smaller N.

The program brief should document the arithmetic: at the picked mixture weights, how many unique tokens per source class are available post-dedup, and what the effective epochs-per-source are at the target D. This is the calculation the review will demand.

## Summary

- The mixture table is the second artifact of the program, after the scaling-law bet. It allocates D across source classes: curated web, broad web, code, academic and books, math and reasoning, dialogue, multilingual, and (for fine-tune programs) domain corpus.
- The core trade-off is **mixture quality vs. total tokens available**. Programs land somewhere between strict-high-quality (bounded D, small N), broadened mixture (accepting per-token quality regression to hit D), and mild epoching of high-quality sources (Muennighoff et al. 2023 shape).
- Mixture reweighting methods — **DoReMi**, RegMix, and successors — automate the weight-tuning decision at ablation time. Use them as proposals, not diktats.
- Modern programs use a **curriculum**: warmup → main → cool-down with a higher-quality mixture in the cool-down. Domain-progressive curriculum is standard for fine-tune programs.
- Two failure modes: **ship the LLaMA mixture blindly**, and **over-index on domain data** in fine-tune programs. Test in ablation before commit.

The next chapter walks the curation pipeline that produces those tokens: sourcing, quality filtering, license and PII handling, dedup, and safety filters — the operational substrate the mixture design assumes.
