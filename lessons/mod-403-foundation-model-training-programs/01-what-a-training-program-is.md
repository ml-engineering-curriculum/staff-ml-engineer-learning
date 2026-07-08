# What "Training Program" Means as a Scope of Work

## Motivation

The Senior tech-lead who has trained ranking models, and has recently trained a first LoRA fine-tune on a 7B open-weights base, arrives at Staff and gets asked to *"scope the pretraining program"* or *"scope the large-scale fine-tune program the leadership team wants to fund next year"*. The reflex is to reach for the vocabulary of a single training run — model architecture, tokenizer, optimizer, batch size, hyperparameters — and to start writing a training script. The vocabulary is right; the scope is wrong. The staff-plus deliverable is not a training script; it is a **program** that will produce many training runs — small proxy runs, ablation runs, a main run, a re-run when the main run fails at 40% — and a post-training track that follows the main run for another one to three months. This chapter names the program as a scope-of-work.

The rest of the module depends on this framing. Chapter 2 puts the scaling-law bet inside the program envelope. Chapter 3 converts the bet to cluster-hours. Chapters 4-5 walk the data pipeline the program depends on. Chapter 6 sequences the runs. Chapter 7 decides what post-training work belongs in the program vs. what belongs to the peer fine-tuning-engineer track. Chapter 8 rolls the program into an executive one-pager.

## What "program" means here

A **foundation-model or large-scale fine-tune training program** in this module means:

> A multi-month, multi-team effort that spends **thousands to tens-of-thousands of accelerator-hours** to produce **one or a small number of base or fine-tuned models** intended to serve a portfolio of downstream systems. The program is scoped by a written **compute budget**, a written **data plan**, a written **ablation and eval plan**, and a written **post-training plan** — and is approved as a whole by leadership before the main run starts.

Two representative shapes:

- **A pretraining program.** The org has decided that off-the-shelf 7B / 13B open-weights bases are not enough — usually because domain vocabulary is very different, the license does not cover the use case, or the domain-specific benchmarks the org actually cares about show a floor open-weights bases cannot cross. The program's output is a **new base model** in the 3B — 70B parameter range trained on 100B — 10T tokens.
- **A large-scale fine-tune program.** The org has picked a strong open-weights base (Llama, Mistral, Gemma, Qwen, DeepSeek, or an equivalent) and needs to lift it materially on a domain — coding, medical, legal, customer-support dialogue — via **continued pretraining**, **instruction tuning**, and **preference optimisation**. The program's output is a **domain-specialised model** whose evaluation deltas justify the training cost.

The two shapes share more than they differ. The compute budget, the data-mixture design, the ablation cadence, the eval waterfall, and the post-training investment decisions have the same shape at pretraining scope and at large-scale fine-tune scope. Where they diverge — cold-start scaling laws vs. inheriting the base's scaling curve, license and PII rules for raw web crawl vs. domain corpora, the *starting point* on the loss curve — is called out chapter by chapter.

Below that scale, a single training-pipeline engineer or a fine-tuning-engineer specialist runs a single run and does not need the program artifact. Above that scale — a frontier-lab program with tens or hundreds of thousands of accelerators for months — the Staff engineer is inside a Principal-and-VP program envelope, and the program artifact is somebody else's document. Mod-403 targets the middle, which is where the vast majority of published staff-plus ML engineering job descriptions live.

## The program is not a training run

Three concrete illustrations of the scope difference. Each is a place where treating the program as a scaled-up training run wastes weeks and burns leadership trust.

**1. The single-run mindset picks (parameters, tokens) as if it were free.** A Senior tech-lead training a ranking model asks *"how many parameters do I need to hit 0.79 NDCG?"* and iterates for a week. A program-scope Staff engineer asks *"given the FLOP budget the exec is willing to fund, what is the compute-optimal (N, D) trade-off, and how confident am I in the scaling-law extrapolation from my ablation runs?"* This is the Chinchilla bet (chapter 2). Getting it wrong by a factor of two costs the program a factor of two in wall-clock.

**2. The single-run mindset treats data as an input, not as a program-scope dependency.** A single-run engineer downloads a dataset. A program engineer commissions a **data curation pipeline** — sourcing, licensing, PII redaction, near-duplicate removal, mixture reweighting — that runs continuously for the entire program duration and evolves between ablation runs. The pipeline is a dependency on par with the compute cluster. Chapters 4 and 5 walk this in depth. The single-run mindset that treats data as a static input is why so many first-time program owners are surprised, in month two, that their held-out perplexity got *worse* after adding "more data" — dedup was skipped and near-duplicates gamed the metric.

**3. The single-run mindset treats each run as terminal.** In a program, no single run is terminal. The proxy runs teach the extrapolation; the ablations decide which architecture and mixture change wins; the main run may crash at 40% and restart from checkpoint 200; the post-training track re-uses the main-run checkpoints for months afterwards. Chapter 6 walks the ablation cadence, the eval waterfall, and the checkpoint policy that make each run a learning artifact rather than a terminal deliverable. A program owner who does not budget for restarts *will* have a restart she cannot afford.

The framing that survives all three is: **the program produces knowledge as much as it produces model weights**. The scaling-law extrapolation, the mixture that beats the baseline, the ablation that ruled out an architecture tweak, the post-training recipe that lifted the domain benchmark — those learnings are the program's second output and are the reason the compute budget is defensible. A program that only produces weights and no reusable extrapolation is *materially harder to fund again next year*.

## The five artifacts a program produces

The program's outputs are:

1. **The scaling-law bet document** — the (N, D, C) target and the written argument for why it is the right point on the isoflop curve for this program (chapter 2).
2. **The cluster-hour budget** — the FLOP target converted into accelerator-hours, risk-adjusted for ablations and restarts, defended against alternate calls (chapter 3).
3. **The data plan** — mixture design, curation pipeline, license and PII constraints, dedup policy, curriculum (chapters 4-5).
4. **The ablation and eval plan** — the cadence of proxy and full-scale runs, the checkpoint policy, the eval waterfall from loss to red-team gates (chapter 6).
5. **The post-training plan** — SFT / RLHF / DPO / red-team investments, and the hand-off contract to the peer fine-tuning-engineer track (chapter 7).

Plus one integrating artifact:

6. **The executive one-pager** — the two-page summary that rolls all five above into a leadership-audience artifact (chapter 8).

Each of the six artifacts corresponds to one exercise in this module and one section of the project-402 capstone. A program owner who has drafted all six is scope-complete; a program owner who has drafted only two or three is asking leadership to approve on faith.

## What the Staff ML engineer owns vs. what the specialist tracks own

A common miscalibration is for the newly-Staff engineer to own too much of the training-run internals. Mod-401 chapter 4 covered the hand-off contracts to peer specialist tracks; this module operationalises them for a training program specifically.

**Staff owns.** The program-envelope decisions: the FLOP budget and the scaling-law bet defending it; the mixture design and the license / PII / dedup policy; the ablation cadence and the eval waterfall; the go/no-go criteria for the main run; the post-training investment split; the executive framing.

**Training-pipeline engineer (peer specialist track) owns.** The training-loop implementation: the parallelism recipe (mod-404's territory), the checkpointing and restart mechanics, the throughput optimisation, the failure-domain instrumentation, the actual kernels. The Staff engineer *reads* their MFU numbers to defend the cluster-hour budget; they do not write the training loop themselves.

**Fine-tuning-engineer (peer specialist track) owns.** The depth of SFT / RLHF / DPO recipes: the reward-model training, the KL-penalty tuning, the preference-data collection process, the constitutional-AI-style methods. The Staff engineer *sets the program-level investment* — how many post-training passes, what alignment goals, what evaluation gates — and *hands off* the recipe-level work to this track.

**Model-evaluation-engineer (peer specialist track) owns.** The depth of eval harness design and the safety / red-team benchmark curation. The Staff engineer *sets the waterfall* — which gates the program must pass before the main run and before ship — and *consumes their benchmarks*.

**Peer platform tracks (ai-infra-ml-platform, ai-infra-performance, ai-infra-mlops) own.** The training cluster itself, the data-plane throughput, the observability substrate, the checkpoint storage. The Staff engineer *contracts with them* for capacity and reliability commitments; they do not build these themselves.

If the Staff engineer finds themselves debugging a CUDA OOM at 3 AM during the main run, either (a) the training-pipeline engineer is out and the Staff engineer is holding pager as a backup, which is legitimate but rare, or (b) the hand-off contract broke and the program has taken on operational load it did not budget for. Case (b) is a program-management incident; chapter 3's risk-adjusted budget should have carved a slot for the on-call rotation.

## Pretraining vs. large-scale fine-tune — the substantive differences

Both are programs; both use the six artifacts above. The differences show up in three places:

**Scaling laws.** Pretraining programs commission their own scaling-law extrapolation from small proxy runs — the isoflop grid, the ablation on architecture at small N. Large-scale fine-tune programs inherit the base model's scaling curve (the base was trained to some point on it) and their proxy runs measure *how much a fine-tune moves the loss from that starting point per unit of additional compute*. That is a different-shape extrapolation and chapter 2 handles it distinctly.

**Data provenance and licensing.** Pretraining programs are almost always Common-Crawl-derived plus curated corpora — huge raw-web volumes with all their quality, licensing, PII, and toxicity pathology. Large-scale fine-tune programs work off a base that already digested Common Crawl and add domain-specific data whose provenance is far more traceable. The dedup and license rules (chapter 5) are stricter for pretraining programs but the *scarcity* of high-quality domain data is a harder constraint for fine-tune programs.

**Post-training scope.** Pretraining programs almost always plan the full SFT → preference-optimisation stack. Large-scale fine-tune programs sometimes ship immediately after the continued-pretraining phase (the base already has SFT and preference optimisation on it) and sometimes redo the post-training stack because the domain shifted the base's behaviour enough to require alignment refresh. Chapter 7 walks the decision.

Where the two programs share shape — compute budget, ablation cadence, eval waterfall, executive framing — the module treats them together. Where they diverge, the chapters call it out explicitly.

## The "am I doing program-scope work?" heuristic

A sibling to the mod-401 and mod-402 heuristics:

> **If my document proposes what runs to fund, in what order, against what evals, over multiple months, and defends the total compute cost, I am doing program-scope work. If it proposes hyperparameters for one run, I am doing training-run work.**

Hyperparameters for one run are a Senior tech-lead artifact. Program scope is Staff. The intermediate case — "I own the ablation cadence but not the main-run hyperparameters" — is legitimate program work; the point of the heuristic is to catch the drift toward "I am the person who picks the learning rate" that program owners fall into when they have not fully transitioned altitude.

## Summary

- A **training program** is a multi-month, multi-team effort that spends thousands to tens-of-thousands of accelerator-hours to produce one or a small number of base or fine-tuned models, scoped by six written artifacts.
- A program is *not* a scaled-up training run. Its compute, data, ablation-and-eval, and post-training plans are each independent staff-scope decisions.
- Two shapes: **pretraining** (cold-start scaling laws, Common-Crawl-scale data) and **large-scale fine-tune** (inherit the base's curve, domain data). Both use the same program envelope.
- The Staff ML engineer owns the program envelope. Peer specialist tracks — training-pipeline, fine-tuning, model-evaluation, and the platform tracks — own the depth. The hand-off contracts are load-bearing.
- The five substantive artifacts (scaling-law bet, cluster-hour budget, data plan, ablation and eval plan, post-training plan) plus one integrating artifact (executive one-pager) map one-to-one to the six chapters of this module and the five exercises.

The next chapter walks the scaling-law bet — Kaplan et al. through Hoffmann et al. (Chinchilla) — and the compute-optimal (N, D) trade-off that sits at the top of the program.
