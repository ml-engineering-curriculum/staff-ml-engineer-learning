# Ablation Cadence and the Eval Waterfall

## Motivation

The program's compute budget from chapter 3 does *not* get spent all in the main run. Twenty to fifty percent gets spent on **ablations** — deliberately smaller-than-main runs whose only purpose is to inform decisions about the main run. And the outputs of every run — proxy, ablation, or main — flow through an **eval waterfall** that decides what the next decision is: continue as planned, re-parameterise, halt.

A program without an ablation cadence and an eval waterfall is a bet, not a program. The main run either wins or it does not; there is no way to *learn* mid-program, and there is no way to defend the launch decision beyond "the loss curve looked good." A program *with* a cadence and a waterfall spends its ablation budget as an information-gathering exercise, and the resulting learnings are the second output (chapter 1) that make the program's compute defensible.

This chapter walks the cadence — proxy runs, small-scale ablations, full-scale ablations, main run, post-training — and the waterfall — loss, held-out perplexity, downstream benchmarks, red-team gates. It sets the checkpoint policy, the promotion criteria between phases, and the stop-conditions the review agreed on up front.

## The four phases of runs

Structure the program's runs into four phases in wall-clock order:

### Phase 1 — proxy runs

**Purpose.** Extrapolation. The scaling-law bet in chapter 2 is grounded in small-scale isoflop grids the program runs itself; phase 1 is where that grid gets built.

**Shape.** Three to five model sizes (typically 40M / 100M / 300M / 800M parameters) at three to five token counts (~5-50B tokens each), giving a 9-25 point grid across the loss-vs-(N, C) surface. Each proxy run is minutes-to-hours on a small handful of accelerators; the aggregate compute is small (typically <5% of main-run compute).

**Key techniques.**

- **µ-Transfer / µP** (Yang et al. 2022, *"Tensor Programs V: Tuning Large Neural Networks via Zero-Shot Hyperparameter Transfer"*). Under maximal-update parameterisation, optimal learning rate transfers from small to large models. µP proxy runs at 100M parameters find a learning rate that transfers to a 70B main run. This is the single most compute-efficient result in modern pretraining and every serious program uses it or an equivalent.
- **Learning-rate range test.** Aggressive lr sweep at each proxy size to confirm the µP transfer prediction.
- **Simplified architecture proxies.** Use the same architecture as the main run in miniature; do not change the block structure between phase 1 and phase 3. Cross-architecture transfer of scaling-law fits is not reliable.

**Deliverables.**

- **Isoflop grid.** Loss at each (N, C) grid point, fitted to a parametric form (Hoffmann's L(N, D) = E + A / N^α + B / D^β).
- **Learning-rate transfer table.** The µP-derived learning rate as a function of N.
- **First-pass eval numbers** on the proxy models, primarily to sanity-check that the harness works and the metrics are what the ablation phase will read.

### Phase 2 — small-scale ablations

**Purpose.** Decide the between-candidate calls that go into the main run: architecture variant, mixture weights, dedup aggressiveness, tokenizer choice, optimizer setting, activation function.

**Shape.** A comparison design — pick one variable, hold everything else constant, run 2-4 variants at a scale where the difference is measurable but the cost is small (typically 300M-3B parameters, 10-100B tokens). Each ablation is one or a few days on a modest slice of the cluster.

**Ablations worth budgeting for.**

- **Data mixture ablations.** LLaMA-2-shape vs. domain-heavier vs. code-heavier. 3-5 mixture cells is typical.
- **Filter aggression ablation.** Higher-aggression classifier filter vs. baseline. Tests whether the DCLM finding replicates on your corpus.
- **Dedup aggressiveness ablation.** MinHash threshold 0.7 vs. 0.85. Tests chapter 5's dedup-as-scaling-axis finding on your corpus.
- **Architecture ablations.** Rotary embeddings vs. ALiBi, SwiGLU vs. GELU, grouped-query-attention vs. multi-head — one at a time. Usually not the highest-ROI ablation; modern architectures cluster tightly.
- **Optimizer ablations.** AdamW vs. Lion vs. Sophia. Modest deltas at scale; often worth one or two cells to confirm.

**How many ablations.** More cells is more information but also more compute. A well-scoped ablation phase has **10-25 cells** in total. Cells that are unlikely to change the main-run design (running an ablation just to see) are cut before the review sign-off.

**Deliverables.**

- **Ablation matrix.** Each cell: the changed variable, the fixed config, the eval waterfall output, the effect size, the decision.
- **Statistical significance analysis.** With single seeds at modest N, effect sizes have real noise. A cell with delta smaller than the seed-noise band is a null result, not a positive.

**Failure mode.** The ablation phase that becomes exploration — running cells because they are interesting rather than because they inform the main-run decision — burns budget and delays the main run. Stay strict.

### Phase 3 — full-scale ablations

**Purpose.** Confirm that the phase-2 decisions hold at the target N. The transfer from 3B ablation to 15B main run is not free; some phase-2 winners regress at scale.

**Shape.** 1-3 full-scale runs at the target N and a fraction (typically 20-40%) of the target D. The compute per full-scale ablation is 5-10% of the main run.

**When to run these.**

- The phase-2 winner is by a narrow margin and the review is not confident it holds at scale.
- The mixture change is large enough that scaling-law fits from phase 2 do not confidently predict main-run behaviour.
- A candidate architecture change (tensor-parallel-friendly attention variant, MoE vs. dense) has real transfer risk.

**When to skip.** If phase-2 winners are decisive and low-risk, skip phase 3 and go direct to main. The program brief should document the decision either way — the review will ask.

### Phase 4 — main run

**Purpose.** The C = 6ND run. The one leadership funded the program for.

**Shape.** One run at the target (N, D), consuming the bulk of the compute budget (roughly 50% of program compute — see chapter 3's risk-adjustment table).

**Operational shape.**

- **Warmup phase** (0.5-2% of steps) with the mixture and learning rate schedule from chapter 4.
- **Main phase** (85-95% of steps) at the target learning rate on the main mixture.
- **Cool-down phase** (5-15% of steps) with decayed learning rate and higher-quality mixture cut-in.
- **Checkpoints at fixed intervals** and additional checkpoints at planned eval waypoints.
- **Continuous eval-waterfall sampling** running against fresh checkpoints (see below).

### Phase 5 — post-training

Not strictly an ablation phase; chapter 7 walks it. But the eval waterfall developed in phase 1-4 is what post-training gates against, so it belongs mentioned here.

## The eval waterfall

Every run's output flows through a **waterfall** of evaluations, each with a promotion or halt decision. Structured this way, the program has a single vocabulary for talking about "how the model is doing" across ablations, main-run checkpoints, and post-training.

The waterfall, in order of latency and cost:

### Tier 1 — training loss and gradient norms

**Latency.** Every step.

**What it tells you.** Whether the training loop is running. A spike in loss, a NaN, a growing gradient norm — these are training-instability signals and the training-pipeline engineer's on-call handles them. Program-level gate: none; this is a "the run is broken" signal.

### Tier 2 — held-out validation perplexity

**Latency.** Every 100-1000 steps, sub-sampled.

**What it tells you.** Whether the model is generalising to unseen text with the same distribution as training. The primary signal during phase 1-3 ablations for whether one cell beats another. Loss curves cross; perplexity curves rarely lie.

**Held-out sets.** Multiple: a held-out slice of the training mixture (in-distribution), plus several distribution-shifted held-outs (Wikipedia-only, code-only, math-only, target-domain-only). The ratio of in-distribution to distribution-shifted perplexities is a signal for whether the mixture is overfitting to one source class.

### Tier 3 — downstream capability benchmarks

**Latency.** Every few thousand steps, or at planned checkpoint waypoints (10%, 25%, 50%, 75%, 100% of training).

**What it tells you.** Whether the model is acquiring downstream capabilities in the direction the program is committed to. The benchmark set typically includes:

- **Reasoning and knowledge**: MMLU (Hendrycks et al. 2021), Big-Bench-Hard (Suzgun et al. 2022), ARC (Clark et al. 2018).
- **Reading comprehension**: HellaSwag (Zellers et al. 2019), TriviaQA (Joshi et al. 2017).
- **Code**: HumanEval (Chen et al. 2021), MBPP (Austin et al. 2021).
- **Math**: GSM8K (Cobbe et al. 2021), MATH (Hendrycks et al. 2021).
- **Multilingual and instruction-following**, weighted by program target.
- **Domain benchmarks** (for fine-tune programs): whichever domain benchmarks the program is optimising for.

**Sampling caveats.** Benchmark evaluation on a pretraining base model happens in a few-shot / zero-shot regime. Post-training checkpoints get evaluated on the same benchmarks *plus* instruction-following benchmarks (MT-Bench, AlpacaEval, IFEval) that only make sense for post-trained models.

**Contamination.** Benchmark-into-training contamination is a real risk — if a benchmark's test-set text is in the training corpus, the model's benchmark score is inflated in a way the program cannot detect from the score. The pipeline (chapter 5) should include a **decontamination pass** against every benchmark's test set at ingest, and the corpus manifest should record which benchmarks were decontaminated against. Programs that skip this get accused of contamination when they publish and the accusations often stick.

### Tier 4 — safety and red-team evaluations

**Latency.** At planned checkpoint waypoints — typically 50%, 75%, 100% of training and after each post-training pass.

**What it tells you.** Whether the model produces harmful content under adversarial prompting; whether it complies with policy or refuses; whether it leaks training data verbatim.

**The eval set.**

- **Safety benchmarks**: ToxiGen, RealToxicityPrompts, TruthfulQA, BBQ (Bias Benchmark for QA), and equivalents.
- **Red-team suites** (mostly internal, sometimes shared): categories of policy-violating prompts, jailbreak attempts, prompt-injection tests, tool-misuse tests.
- **Memorisation tests**: prompt with a training-corpus prefix and check whether the model completes verbatim.

**Program-scope decision.** How much red-team work runs during pretraining (limited utility on a raw base) vs. post-training (much higher yield). Chapter 7 walks this.

**Gate**. The safety gate is often the **launch gate** for the program: if the model does not clear the internal red-team threshold, the program does not ship regardless of the capability benchmarks.

### Tier 5 — human and preference evaluation

**Latency.** Once or twice during the program — usually after phase 3 for a preview and after phase 4 + post-training for launch decision.

**What it tells you.** Whether human evaluators prefer the model's outputs to a comparable baseline (previous internal model, competitor model, no-model human baseline).

**Sampled with care.** Human evaluation is expensive and noisy. Get it right — mod-406 covers cross-team human-eval discipline in depth. For chapter 6's purposes: budget for two human-eval passes, each with N=200-500 sampled prompts.

## Checkpoint policy

The main run generates checkpoints. Which get kept?

**Recommended checkpoint policy.**

- **Every N steps** (roughly every 2-4 hours of wall-clock at scale) for restart / rollback. Keep the last 3-5 rolling.
- **Every 10% of training** for the eval waterfall to sample from. Keep permanently.
- **Cool-down phase checkpoints** at every 2% during the cool-down. Keep permanently. This is where post-training will start from and the last few percent of training substantially changes the model.
- **The final checkpoint.** Keep permanently. Obviously.

**Storage.** Full-precision checkpoints of a 15B model in BF16 optimizer-state are ~200GB each; sharded checkpoints across the cluster's parallel processes are the same size in aggregate. Ten permanent checkpoints is 2TB. Budget storage.

**Post-training re-uses these.** The post-training team (chapter 7) needs the pretraining checkpoints. Do not delete them.

**Failure mode.** Programs that keep only "the final checkpoint" and one restart checkpoint have no way to answer *"what did the model look like at 60% of training"* six months later. That is a question that shows up in almost every post-mortem and in almost every follow-on paper. Keep the waypoints.

## The stop-conditions and re-plan triggers

The program brief committed to a scaling-law bet with a confidence band (chapter 2). The ablation and eval waterfall is what tests the bet. When the waterfall shows the bet is wrong, the program has to decide **stop, re-plan, or continue with revised expectations**. Write these decisions down before the run starts, not after.

**Stop conditions the review should agree on.**

- **Phase 1 exit criterion.** The proxy grid must produce a scaling-law fit within a specific confidence band; if the fit is materially off from the Chinchilla / Hoffmann parametric form, the (N, D) target may be wrong.
- **Phase 2 exit criterion.** The winning ablation cell must produce a specific effect size over the baseline; if no cell wins, the main run runs on the baseline mixture and a lessons-learned artifact captures why nothing beat it.
- **Phase 3 exit criterion.** Full-scale ablation loss curves must track the phase-1 extrapolation within a specific confidence band. Divergence at this stage is a mandatory re-plan.
- **Phase 4 mid-run halt.** Specific loss-curve or eval-waterfall triggers that mandate halting the main run before completion — for example, if MMLU at 40% of training has not crossed the previous-generation checkpoint's 40%-of-training score, the program committee reviews whether to continue.

The mid-run halt trigger is the hardest to write and the most important. Programs without a written halt trigger tend to run to completion because "we already spent so much" — the sunk-cost fallacy at accelerator-hour scale. Write it up front and put the trigger threshold in the exec one-pager (chapter 8).

## The written artifact

Chapter 6's deliverable is the **ablation and eval plan** — a 3-5 page section of the program brief containing:

1. **Phase table.** The four (or five) phases, their timing, and their compute cost.
2. **Ablation matrix.** Every planned cell, its variable, its config, its expected compute.
3. **Eval waterfall.** The five tiers, their evaluation sets, their frequency.
4. **Decontamination policy.** Which benchmarks are decontaminated against and how it is verified.
5. **Checkpoint policy.** Which checkpoints are kept, at what cadence, at what storage cost.
6. **Stop and re-plan triggers.** Every phase's exit criterion; the mid-run halt trigger.
7. **Owner and cadence.** Who runs the eval waterfall between checkpoints. When the program committee reviews the results.

## The role of the peer eval-engineer track

Chapter 6 is where the program's boundary with the peer `model-evaluation-engineer` and `ai-eval-engineer` specialist tracks is most concrete. The waterfall's *design* — five tiers, promotion criteria, decontamination policy — is Staff scope. The *implementation* — the eval harness, the benchmark curation, the human-eval logistics, the red-team payload authoring — belongs to the peer specialist tracks in a mature org.

The staff-plus discipline is to **contract** with the peer track for the harness and the payloads, and to specify the SLO on eval turnaround. If the eval waterfall's tier-4 red-team gate takes six weeks from checkpoint to result, the program cannot use it as a mid-run halt trigger; the trigger has to be a tier-2 or tier-3 signal with faster turnaround. That constraint is a program-planning constraint, not a bug.

## Summary

- The program runs in four (or five) phases: **proxy runs** for extrapolation, **small-scale ablations** for between-candidate decisions, **full-scale ablations** for scale-transfer confirmation, the **main run**, and post-training.
- Use **µ-Transfer / µP** for hyperparameter transfer from proxy to main; it is the single most compute-efficient technique in modern pretraining.
- The **eval waterfall** has five tiers: training loss, held-out perplexity, downstream benchmarks, safety and red-team, human evaluation. Each has a latency, a cost, and a promotion decision.
- **Decontamination** and **checkpoint policy** are program-scope decisions that need to be written down before the main run, not after.
- **Stop conditions and re-plan triggers** are the artifact that turns the program from a bet into a program. The mid-run halt trigger is the hardest to write and the most important.
- The peer eval-engineer track owns the implementation; the Staff engineer owns the waterfall design and the promotion criteria.

The next chapter walks post-training — SFT, RLHF, DPO, KTO, red-team hardening — and the capability-vs-safety trade-off the program has to make on the investment split.
