# mod-403-foundation-model-training-programs: Foundation-Model & Large-Scale Fine-Tune Training Programs

**Estimated effort:** 16 hours

The Staff Machine Learning Engineer's second archetype-centre after multi-team architecture: **scoping and defending a training program**. Names what a "program" is as scope-of-work above a single training run, walks the Chinchilla-style scaling-law bet that picks (parameters, tokens, FLOPs), converts the bet into a cluster-hour budget leadership can approve, walks the data mixture and curation pipeline the program depends on, sequences the ablation cadence and the eval waterfall so each run teaches the next, decides where post-training effort should sit on the capability-vs-safety trade-off, and finishes on the executive one-pager that translates the scaling-law bet into a business framing.

Builds on [`mod-401-staff-ml-role-scope`](../mod-401-staff-ml-role-scope/) (staff-plus altitude, the "am I doing staff work?" heuristic, hand-off contracts to peer specialist tracks) and [`mod-402-multi-team-ml-architecture`](../mod-402-multi-team-ml-architecture/) (the multi-team RFC and review shape that this module's exec one-pager sits inside). Feeds forward into [`mod-404-distributed-training-systems-at-scale`](../mod-404-distributed-training-systems-at-scale/) (the parallelism recipe that spends the cluster-hour budget this module defends), [`mod-407-capacity-cost-and-tco`](../mod-407-capacity-cost-and-tco/) (the org-scope capacity model this module's budget rolls into), and the capstone [`project-402-foundation-model-training-program`](../../projects/project-402-foundation-model-training-program/).

## Learning objectives

- Scope a pretraining or large-scale fine-tune program: pick the compute-optimal parameter/data trade-off (Chinchilla-style), lay out the scaling-law-driven roadmap, defend the total cluster-hour budget to leadership.
- Design the data mixture, data-curation pipeline, and curriculum — including licensing, PII, and dedup constraints — and articulate the trade-off between mixture quality and total tokens.
- Sequence architecture and data ablations so the program learns from each run — set the ablation cadence, checkpoint policy, and eval waterfall.
- Reason about capability elicitation vs. safety trade-offs — where to invest post-training effort (SFT / RLHF / DPO / red-team fixes) and what to defer to the fine-tuning specialist track.
- Write the executive one-pager that translates the program's scaling-law bet into a business framing.

## Lecture chapters

1. [`01-what-a-training-program-is.md`](01-what-a-training-program-is.md) — The training program as scope-of-work above a single training run. Pretraining vs. large-scale fine-tune. What the Staff ML engineer owns vs. what belongs to the training-pipeline / fine-tuning specialist tracks. The five artifacts the program produces.
2. [`02-scaling-laws-and-the-chinchilla-bet.md`](02-scaling-laws-and-the-chinchilla-bet.md) — Kaplan et al. through Hoffmann et al. (Chinchilla). The compute-optimal (N, D) trade-off, the ~20-tokens-per-parameter heuristic, the 6ND FLOP estimator, isoflop curves. Where Chinchilla-optimal is the wrong answer — over-training for inference cost. The scaling-law bet as a written artifact.
3. [`03-cluster-hour-budget-defense.md`](03-cluster-hour-budget-defense.md) — Translating a FLOP target into cluster-hours via Model FLOPs Utilisation (MFU) and Hardware FLOPs Utilisation (HFU). The risk-adjusted budget — ablations, restarts, hyperparameter sweeps. Defending the budget against three alternate calls (smaller, bigger, buy-instead-of-train).
4. [`04-data-mixture-and-curriculum.md`](04-data-mixture-and-curriculum.md) — The mixture design. Source classes (web crawl, code, books, math, curated web, dialogue). The quality-vs-tokens trade-off. Mixture reweighting (DoReMi and successors). Curriculum: staged mixture across training, mid-training data upweighting, cool-down phases.
5. [`05-data-curation-pipeline-license-pii-dedup.md`](05-data-curation-pipeline-license-pii-dedup.md) — The curation pipeline stages: source acquisition, quality filtering, license and PII handling, near-duplicate removal (MinHash / SimHash / LSH), substring dedup, safety filters. Dedup as a scaling axis. Common Crawl and the reality of raw web data.
6. [`06-ablation-cadence-and-eval-waterfall.md`](06-ablation-cadence-and-eval-waterfall.md) — Small-scale proxy runs, mu-transfer for hyperparameter transfer, the ablation types worth running (data, architecture, optimizer, tokenizer). The eval waterfall: loss → held-out perplexity → downstream benchmarks → red-team gates. Checkpoint policy. Cadence and stop-conditions.
7. [`07-post-training-capability-vs-safety.md`](07-post-training-capability-vs-safety.md) — The post-training menu: SFT, RLHF, DPO / KTO, tool-use elicitation, red-team hardening, constitutional-style methods. The capability-elicitation-vs-safety trade-off. What the Staff engineer owns as a program decision and what belongs to the fine-tuning-engineer specialist track.
8. [`08-executive-one-pager.md`](08-executive-one-pager.md) — The exec one-pager template that translates the scaling-law bet into business framing. What the CFO reads first. The risk table. The timeline. How the one-pager pairs with the mod-402 RFC template for the program-approval review.

## Exercises

- [`exercises/exercise-01-compute-optimal-scoping-with-chinchilla.md`](exercises/exercise-01-compute-optimal-scoping-with-chinchilla.md) — Compute the compute-optimal (N, D, C) for a concrete program brief and defend the choice. 3 hours.
- [`exercises/exercise-02-data-mixture-and-dedup-strategy.md`](exercises/exercise-02-data-mixture-and-dedup-strategy.md) — Design the data mixture and the curation pipeline for the same program. 3 hours.
- [`exercises/exercise-03-ablation-sequencing-plan.md`](exercises/exercise-03-ablation-sequencing-plan.md) — Sequence the ablation cadence and eval waterfall for the program. 3 hours.
- [`exercises/exercise-04-capability-vs-safety-post-training-plan.md`](exercises/exercise-04-capability-vs-safety-post-training-plan.md) — Decide the post-training investment split between capability and safety. 3 hours.
- [`exercises/exercise-05-executive-one-pager-for-training-program.md`](exercises/exercise-05-executive-one-pager-for-training-program.md) — Roll the previous four exercises into the executive one-pager. 3 hours.

## Structure

```
mod-403-foundation-model-training-programs/
├── 01-…md … 08-…md    lecture chapters
├── exercises/          per-exercise prompts (solutions live in the paired solutions repo)
├── labs/               long-form hands-on labs (planned)
├── quizzes/            knowledge checks (planned)
├── resources.md        curated external references
└── README.md           this file
```

## Suggested sequencing

Read chapters 1-3 as one block: they define the artifact and the compute bet, and they build on each other with almost no drift room. Take a break, then read chapters 4-5 as the data-side block. Chapter 6 (ablations and evals) is the joint of the module — do not skim it; the ablation cadence is what turns the program from a bet into a series of learnings. Chapter 7 (post-training) is where the module deliberately hands scope to the peer fine-tuning-engineer track; read it with the mod-401 hand-off-contract chapter open in a second tab. Chapter 8 is the synthesis.

The five exercises assemble into project-402's capstone deliverable. Take exercise-01 immediately after chapter 3, exercise-02 after chapter 5, exercise-03 after chapter 6, exercise-04 after chapter 7, and exercise-05 after chapter 8. Learners planning to take project-402 should keep the program-brief constant across all five exercises — the exercises are designed to compose.

## What comes next

- **[`mod-404-distributed-training-systems-at-scale`](../mod-404-distributed-training-systems-at-scale/)** — the parallelism recipe (DDP, FSDP/ZeRO, tensor-parallel, pipeline-parallel, 3D-parallel) that spends the cluster-hour budget this module defends; MFU depth beyond chapter 3.
- **[`mod-407-capacity-cost-and-tco`](../mod-407-capacity-cost-and-tco/)** — the org-scope capacity model this module's per-program budget rolls into; the reserved/on-demand/spot portfolio conversation.
- **[`mod-409-responsible-ai-at-portfolio-scope`](../mod-409-responsible-ai-at-portfolio-scope/)** — the portfolio-scope risk register and pre-launch gate that chapter 7's safety decisions feed into.
- **[`project-402-foundation-model-training-program`](../../projects/project-402-foundation-model-training-program/)** — the capstone that combines this module's five exercises into a full end-to-end program artifact leadership can approve.

## Related tracks

- [`senior-ml-engineer-learning`](https://github.com/ml-engineering-curriculum/senior-ml-engineer-learning) (L30) — the prerequisite tech-lead track; single-run training scope. See [`PREREQUISITES.md`](../../PREREQUISITES.md).
- `training-pipeline-engineer` — the peer specialist track that owns the *implementation* of the training loop this module scopes the program around; see mod-401 chapter 4 for the hand-off contract.
- `fine-tuning-engineer` — the peer specialist track that owns the *depth* of the SFT / RLHF / DPO work this module's chapter 7 sets the program-level budget for.
- [`ai-infra-performance-learning`](https://github.com/ai-infra-curriculum/ai-infra-performance-learning) — the peer platform track that owns MFU / kernel-level performance; this module uses their numbers to defend the cluster-hour budget.
