# Post-Training: Capability Elicitation vs. Safety Investment

## Motivation

A pretraining or continued-pretraining base is not shippable. It knows a lot; it does not know how to be helpful, how to be honest about what it does not know, how to refuse harmful requests, or how to use tools. Post-training is the phase that turns the base into a shippable model — and it is where the program's capability-vs-safety trade-off gets made concrete. Two programs with identical base models but different post-training investments produce meaningfully different downstream products.

This chapter maps the modern post-training menu, gives a framework for the capability-vs-safety investment split, and defines the hand-off contract to the peer `fine-tuning-engineer` specialist track. The staff-plus deliverable is a **post-training plan** — one to two pages — that names the passes the program will run, the eval gates each pass has to clear, and the boundary with the specialist track.

The chapter is deliberately not a recipe deep-dive. The recipe belongs to the fine-tuning-engineer specialist track. The *program-level investment decision* belongs here.

## The post-training menu

Modern post-training assembles from a set of well-established passes, applied in sequence. The reference sequence, from Ouyang et al. (2022, "InstructGPT") through Bai et al. (2022, "Constitutional AI") through Rafailov et al. (2023, "DPO") through Ethayarajh et al. (2024, "KTO") and the 2024-2025 mixture-of-methods literature, is:

```
[pretrained base]
    → [supervised fine-tuning (SFT)]
        → [reward-model training] → [RLHF (PPO)]              ← historical default
        →                          → [DPO / IPO / KTO]        ← modern default
        →                          → [rejection sampling / best-of-N distillation]
            → [red-team hardening pass]
                → [tool-use fine-tuning]
                    → [final evaluation and iteration]
```

Not every program runs every pass. The program's decision is *which passes, in what order, with how much investment*.

### Supervised fine-tuning (SFT)

**What.** Fine-tune the base on curated (prompt, response) pairs demonstrating the desired behaviour. Modern SFT uses tens-of-thousands to hundreds-of-thousands of curated pairs, spanning instructions, dialogue, reasoning, tool use, and refusal patterns.

**Investment.** Compute cost is small (a few percent of the pretraining compute or less). The dominant cost is **the SFT dataset itself** — curated high-quality demonstrations are expensive per token. Program-level decisions:

- **Data source split**: human-written vs. synthesised from a stronger model (distillation) vs. hybrid. Distillation-heavy SFT (Alpaca, Vicuna, Nectar shapes) is cheap but caps the model at the teacher's ceiling and inherits its biases. Human-written SFT (Anthropic's HH, OpenAI's InstructGPT SFT) is expensive but higher-ceiling.
- **Domain balance**: how much general instruction data vs. domain-specific SFT data for a domain-adapted fine-tune program.

**When it's enough on its own.** For narrow domain-adapted programs — code assistants, customer support routers, structured-output extractors — SFT is often the *entire* post-training stack. The preference-optimisation step below only earns its keep when the desired behaviour is hard to specify by demonstration but easy to compare between two candidates.

### Reward model + RLHF (PPO)

**What.** Train a reward model on human-preference pairs (given prompt, response A preferred to response B). Then fine-tune the SFT-ed model using proximal policy optimisation with the reward model providing the reward signal and a KL penalty against the SFT model providing the anchor. Ouyang et al. (2022, "InstructGPT") is the canonical write-up.

**Investment.** Compute is moderate — often 1-5% of pretraining compute for the RL phase; reward-model training is small. The dominant costs are:

- **The preference-data collection process.** Tens-of-thousands to hundreds-of-thousands of ranked comparisons; expensive human labour.
- **The engineering complexity.** RLHF-PPO is famously finicky — reward hacking, KL-penalty divergence, training instability, mode collapse. Programs that run PPO in-house consistently report months of tuning.

**Historical default.** From InstructGPT through late 2023, RLHF-PPO was the standard. The Anthropic Claude papers, the OpenAI GPT-3.5 / GPT-4 alignment writeups, and the Meta Llama-2 paper all used variants of RLHF-PPO.

**Modern shift.** Most programs starting in 2024 or later replace or supplement PPO with DPO-family methods (below). The reward-model artifact is still valuable for offline scoring and best-of-N sampling.

### DPO / IPO / KTO / preference-optimisation methods

**What.** Direct preference optimisation (Rafailov et al. 2023, "Direct Preference Optimization: Your Language Model is Secretly a Reward Model") reformulates the RLHF objective so that it can be optimised directly on preference pairs — no reward model, no PPO. IPO (Azar et al. 2023) is a stability-improved variant. KTO (Ethayarajh et al. 2024, "KTO: Model Alignment as Prospect Theoretic Optimization") allows unpaired positive/negative signals (rather than paired comparisons), which changes the data-collection economics.

**Investment.** Compute is small — DPO training is essentially a supervised fine-tune with a per-example gradient scaling that depends on the reference model's log-probabilities. No RL loop, no reward-model training. Programs that switched from PPO to DPO uniformly report much faster iteration and much lower engineering complexity.

**Modern default.** Most 2024-2025 programs adopt DPO-family methods as the preference-optimisation step. The follow-on literature is refining the recipe (DPO with iterated preference collection, self-play preference data, mixture-of-methods training).

**When it's not enough.** DPO is excellent at aligning a model's preferences to a specific preference dataset. It is less effective at behaviours that are hard to elicit from the SFT model in the first place — the DPO gradient is a preference-shape signal on outputs the model already produces. Programs with capability gaps that need to be *elicited* (multi-step reasoning, tool-use planning) often still need a targeted SFT expansion or a rejection-sampling loop.

### Rejection sampling and best-of-N distillation

**What.** Sample many candidate responses per prompt from the current model; score each with a reward model or heuristic; keep the top-K; fine-tune on the kept samples. Meta's Llama-2 paper documented this at scale as an alternative or supplement to RLHF-PPO.

**Investment.** Compute is dominated by the sampling — running the current model to generate 10-100 candidates per prompt across a large dataset. Manageable, but not trivial at scale.

**When it earns its keep.** When the current model produces good outputs some fraction of the time but not consistently. Rejection sampling amplifies the good outputs into a fine-tuning signal. Especially useful when preferences are readily scored (correct math, passing code tests, structured-output validity) so no human labour is needed for the scoring.

### Red-team hardening

**What.** Systematically probe the model for harmful behaviour and fine-tune on the failure cases (or refusal cases) to reduce the harm rate. Two shapes:

- **Human red-team.** An internal or contracted red-team probes the model, collects failure cases, generates the (prompt, refusal) or (prompt, safe-response) fine-tuning pairs, and re-trains. Iterated across the post-training phase.
- **Automated red-team.** LLM-generated red-team payloads at higher volume than human red-team can produce; the AI can enumerate categories the human red-team missed. Perez et al. (2022, "Red Teaming Language Models with Language Models") is the reference.

Both are commonly used together. The safety benchmarks from chapter 6's tier-4 waterfall gate the red-team success.

**Investment.** Compute is small; the cost is the red-team labour (or the automation infra) and the iteration cycles. Programs consistently under-budget for the iterations — the first red-team pass turns up more categories than the plan, each category needs a fine-tuning cycle, and the next red-team pass turns up different categories.

### Tool-use fine-tuning

**What.** Fine-tune the model on structured tool-use traces — the model learns to emit a well-formed function call, receive a tool result, and integrate the result into a subsequent generation. The Toolformer paper (Schick et al. 2023) is the modern reference; the OpenAI function-calling API, Anthropic's tool-use spec, and the LLaMA-3 tool-use recipe all documented variants.

**When it belongs in-program vs. deferred.** If the program's downstream use case is agentic or heavily tool-using, tool-use fine-tuning is a first-class pass. If the program is a general chat model that will get tool-use via a downstream product surface, tool-use often gets deferred to a per-product fine-tune. State the choice explicitly.

### Constitutional-style methods

**What.** Bai et al. (2022, "Constitutional AI: Harmlessness from AI Feedback") — instead of humans providing preference signals on harmlessness, an AI trained on a constitution (a short prose statement of desired behaviour) provides them. Reduces human labour and produces preference data at higher volume.

**When to use.** For programs whose alignment goals are well-articulated in prose and whose scale of preference-data need is high. Requires a strong model to be the constitutional judge, which for a first-time program often means using an external model — which raises IP and contamination questions.

## The capability-vs-safety trade-off

The core program-level decision this chapter names: **how much of the post-training compute and human labour goes to elicitation vs. safety**.

The framing:

- **Capability elicitation** — SFT expansion, tool-use fine-tuning, rejection sampling, preference optimisation on capability-shaped preferences. Aim: lift the model's benchmark and downstream-task performance ceiling.
- **Safety investment** — red-team hardening, refusal-pattern SFT, preference optimisation on harm-shaped preferences, adversarial-robustness training. Aim: lower the model's harmful-output rate under adversarial and non-adversarial prompting.

The two are not directly opposed — improvements to safety often improve helpful behaviour, and vice versa — but they compete for **labour hours** (human red-team vs. capability-preference collection) and **iteration cycles** (each post-training pass takes wall-clock and blocks the next). The program budget makes the split explicit.

### The three postures

**Capability-heavy** (e.g., research previews, code models where safety concerns are narrow): 80/20 capability/safety. Minimal red-team, safety benchmarks used as a floor rather than a target.

**Balanced** (e.g., general-purpose chat models for internal or moderate-audience external release): 60/40 capability/safety. Multi-pass red-team, safety benchmarks as go/no-go gates.

**Safety-heavy** (e.g., consumer-facing chat model, regulated-industry deployment, high-scale API): 40/60 capability/safety, sometimes 30/70. Multiple red-team rounds, external red-team engagement, extensive refusal training.

The program's posture depends on **who ships this model to whom**. A frontier-lab consumer product is safety-heavy; an internal research model is capability-heavy. The exec one-pager (chapter 8) states the posture on page one — leadership needs to sign off on it before the post-training work starts.

### The Bai et al. inverted-U observation

Bai et al. (2022, in the Anthropic HH paper) reported a pattern that has held up across the literature: **at fixed compute, safety training up to a point is a strict improvement on both safety and helpfulness; past that point, additional safety training reduces helpfulness materially**. The specific point is program-dependent, but the U is a real phenomenon and the program's eval waterfall should watch for it — if the mid-training helpfulness benchmark starts regressing after a safety pass, the program has crossed the point and needs to re-balance.

This is the reason a program that plans "we'll just keep doing red-team passes until the harm rate is zero" ends up with a model people describe as *"annoying, unhelpful, refuses everything"*. The safety investment has a ceiling of usefulness; find it in ablation.

## The hand-off contract to fine-tuning-engineer

The Staff engineer's authored contract for post-training scope, per the mod-401 chapter 4 hand-off framing:

**Staff owns.**

- The post-training posture: capability-heavy / balanced / safety-heavy.
- The pass list: which passes run, in what order, with rough compute allocation.
- The eval gates: which tiers of the waterfall (chapter 6) each pass has to clear.
- The go/no-go on shipping the final post-trained model.
- The launch-gate red-team threshold.

**Fine-tuning-engineer specialist owns.**

- The SFT dataset curation and mixture.
- The reward-model architecture and training recipe (if used).
- The DPO / PPO / KTO recipe and hyperparameter selection.
- The rejection-sampling implementation and reward-model scoring.
- The tool-use fine-tuning implementation.
- The internal red-team payload authoring at the operational level.
- The training-loop instrumentation for the post-training phase.

**Both co-own.**

- The decision to skip or add a pass mid-program.
- The eval-waterfall interpretation when results are ambiguous.

**Neither owns.**

- External red-team engagement — that is the responsible-AI / policy team ([`mod-409-responsible-ai-at-portfolio-scope`](../mod-409-responsible-ai-at-portfolio-scope/)).
- The final launch decision from a product / legal / risk perspective — that is exec + policy.

The written hand-off contract goes in the post-training plan. Fine-tuning-engineer's specialist track material (in a mature org) provides the recipe depth; this module provides the program envelope.

## Where the model card meets the program

The **model card** (Mitchell et al. 2019, "Model Cards for Model Reporting") is the shipping artifact that documents the model's intended use, limitations, evaluation results, safety posture, and provenance. The post-training plan feeds directly into the model card:

- The posture, the pass list, the eval gates — model card sections on training methodology.
- The safety evaluation results — model card sections on limitations and safety.
- The red-team categories tested — model card section on adversarial evaluation.

`mod-409-responsible-ai-at-portfolio-scope` walks the model-card format at portfolio scope. This chapter's connection: the post-training plan is *how the model card gets populated*.

## The post-training plan document

Chapter 7's deliverable, one-to-two pages:

1. **Posture.** Capability-heavy / balanced / safety-heavy, with a one-paragraph rationale from the downstream use case.
2. **Pass list.** SFT → RLHF or DPO or KTO → optional rejection sampling → red-team hardening → optional tool-use fine-tuning. Each pass has a rough compute and human-labour budget.
3. **Data sources per pass.** SFT dataset composition, preference-data source, red-team payload source.
4. **Eval gates.** For each pass, which tiers of the eval waterfall it has to clear before proceeding.
5. **Iteration budget.** How many red-team-plus-remediation cycles the program budgets for.
6. **Launch gate.** The specific safety-benchmark threshold that must be crossed before the model is considered ship-ready.
7. **Hand-off contract.** The Staff / fine-tuning-engineer / responsible-AI split.

## Two common failure modes

**1. Ship the base model with just SFT.** Program-owner assumption: "we're internal-only, SFT is enough." The failure: SFT-only models have not been shaped by preference feedback and their calibration on hard requests (refusals, honesty about uncertainty, tool-use planning) is materially worse than a model with a light preference-optimisation pass. The remediation is small — a modest DPO pass on curated preference data lifts these behaviours substantially. Do not skip it.

**2. Iterate on red-team without a stop condition.** Program-owner assumption: "we'll keep red-teaming until harm rate is zero." The failure: past a program-specific point, additional safety fine-tuning regresses helpfulness (the Bai et al. inverted-U). The program ships an over-refusing model that users complain about, or fails to ship at all. Set the stop condition in the launch gate: *"harm rate below X and helpfulness benchmark above Y."*

## Summary

- Post-training turns a base model into a shippable model via a menu of passes: **SFT, reward model + RLHF or DPO-family preference optimisation, rejection sampling, red-team hardening, tool-use fine-tuning, constitutional-style methods**.
- The staff-plus decision is the **posture**: capability-heavy / balanced / safety-heavy. Determined by who the model ships to.
- The **inverted-U** (Bai et al. 2022) — additional safety training past a program-specific point reduces helpfulness. The launch gate has to name both a safety floor and a helpfulness floor.
- The **hand-off contract** to the fine-tuning-engineer specialist track: Staff owns posture, pass list, eval gates, and launch decision; specialist owns recipe, dataset, and implementation.
- The post-training plan is one-to-two pages, structured around **posture, pass list, data sources, eval gates, iteration budget, launch gate, hand-off contract**.

The final chapter walks the executive one-pager — the two-page synthesis that carries the program's scaling-law bet, cluster-hour budget, data plan, ablation plan, and post-training plan into leadership review.
