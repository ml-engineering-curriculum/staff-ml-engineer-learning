# exercise-04: Capability vs. Safety Post-Training Plan

**Estimated effort:** 3 hours

## Objective

Author the **post-training plan** for the program brief from exercises 01-03: pick the capability-vs-safety posture, sequence the passes (SFT, preference-optimisation, red-team hardening, tool-use, optional constitutional-style), set the eval gates each pass must clear, budget the iteration cycles, and write the hand-off contract to the peer `fine-tuning-engineer` specialist track. The document produced here is section 6 of the program brief and the "post-training investment" section of the executive one-pager.

The exercise is about the *program-level investment decision*, not the recipe. The recipe belongs to the fine-tuning-engineer specialist track; this exercise sets the envelope inside which that track operates.

## Prerequisites

- Exercises 01-03 completed. The base model shape and the eval waterfall are the inputs to the post-training decisions.
- Chapter 07 — Post-Training: Capability Elicitation vs. Safety Investment (the pass menu, the three postures, the Bai et al. inverted-U, the hand-off contract).
- Chapter 06 skim on the tier-4 (safety/red-team) and tier-5 (human/preference) evals; the post-training gates lean on those tiers.

## Steps

1. **State the posture.** Capability-heavy, balanced, or safety-heavy. Defend it in one paragraph from the downstream use case: who ships this model to whom, at what scale, under what regulatory framework.
2. **Draft the pass list.** In order: SFT → (RLHF-PPO or DPO-family or KTO) → optional rejection-sampling / best-of-N → red-team hardening → optional tool-use fine-tuning → optional constitutional-style method. State which passes you include and which you skip; state why for each skip.
3. **Budget per pass.** For each pass: compute cost as a fraction of pretraining compute (typically 0.1-5% each), human-labour estimate in person-weeks, wall-clock estimate.
4. **Data source per pass.** SFT: human-written vs. distilled vs. hybrid, with the specific sources named. Preference-optimisation: preference-data source (in-house collection, published dataset like UltraFeedback or Nectar or HH-RLHF, or synthetic). Red-team: internal red-team, contracted red-team, automated red-team. Cite specific published datasets where you use them.
5. **Set the eval gates.** For each pass, which tiers of the chapter-6 eval waterfall it has to clear before the pass is considered complete and the next pass starts. State specific benchmark thresholds. Add instruction-following benchmarks (MT-Bench, AlpacaEval 2.0, IFEval) to the post-training waterfall.
6. **Budget the iteration cycles.** How many red-team-plus-remediation cycles the program budgets for. State the ceiling — after N cycles without a clean pass, the program committee reviews whether to continue.
7. **Set the launch gate.** The specific safety-benchmark threshold AND the specific helpfulness-benchmark threshold that the final post-trained model must cross to ship. Both floors are required — the Bai et al. inverted-U is why.
8. **Write the hand-off contract to fine-tuning-engineer.** The three-way split: what Staff owns, what the specialist owns, what is co-owned. Follow the chapter-7 template; adjust for your org's actual role split.
9. **Write the responsible-AI hand-off.** What the [`mod-409-responsible-ai-at-portfolio-scope`](../../mod-409-responsible-ai-at-portfolio-scope/) team owns (external red-team, risk-register updates, model-card population). What this program contributes to their inputs.
10. **Draft the model card seeds.** For the sections of the model card that this exercise populates — training methodology, intended use, limitations, safety evaluation — write the paragraph you would seed into the eventual card.

## Deliverable

A single document, 3-5 pages containing:

- **Section 1** — Posture (capability-heavy / balanced / safety-heavy) with rationale.
- **Section 2** — Pass list with per-pass compute, human-labour, and wall-clock budgets.
- **Section 3** — Data sources per pass, with specific citations for published datasets used.
- **Section 4** — Eval gates per pass, from the chapter-6 waterfall, including instruction-following benchmarks.
- **Section 5** — Iteration-cycle budget and the review-trigger ceiling.
- **Section 6** — Launch gate: safety floor AND helpfulness floor.
- **Section 7** — Hand-off contract to fine-tuning-engineer specialist track.
- **Section 8** — Hand-off to responsible-AI (mod-409).
- **Section 9** — Model-card seed paragraphs for training methodology / intended use / limitations / safety.
- **Section 10** — Three-sentence reflection on the hardest decision you had to write down explicitly.

## Starter guidance

- **Do not skip preference-optimisation on the theory "SFT is enough."** Read Rafailov et al. 2023 (DPO) and skim Ouyang et al. 2022 (InstructGPT) first. The delta from a light DPO pass on calibration, refusal quality, and instruction-following is well-established. If you decide to skip, defend explicitly against the published evidence.
- **The Bai et al. inverted-U is real.** If your launch gate has only a safety floor and no helpfulness floor, you have set up the "annoying, refuses everything" failure mode.
- **The specific red-team categories are what legal will ask about.** State them. If your org has a policy list, use that. If not, start from OWASP LLM Top 10, the NIST AI Risk Management Framework's Generative AI Profile, or an equivalent published category list.
- **The hand-off contract is often written vaguely.** Force yourself to name a specific role or team for each item in the split. "The specialist team" is not specific enough.
- **Do not mirror OpenAI's / Anthropic's public alignment stack unless it fits your program.** Their programs are safety-heavy consumer products with regulatory scrutiny; a large-scale-fine-tune for an internal customer-support agent has a different posture.

## Acceptance criteria

- The posture (capability-heavy / balanced / safety-heavy) is stated with a specific downstream-use-case rationale.
- The pass list includes at minimum SFT and a preference-optimisation pass, or defends the omission of the latter against the published evidence.
- Each included pass has a compute-cost fraction, a human-labour estimate, and a wall-clock estimate.
- Each pass names a specific data source with citation for published datasets (UltraFeedback, HH-RLHF, Nectar, ShareGPT, WildChat, or equivalent).
- Each pass has an eval-gate tied to a specific chapter-6 waterfall tier and benchmarks.
- Iteration-cycle budget is stated with a numeric ceiling and a review-trigger condition.
- Launch gate has BOTH a safety floor and a helpfulness floor with specific benchmark thresholds.
- Hand-off contract to fine-tuning-engineer is a three-way split (Staff / specialist / co-owned) with specific responsibilities per item.
- Hand-off to responsible-AI (mod-409) names what the program contributes and what mod-409 owns.
- Model-card seed paragraphs cover training methodology, intended use, limitations, and safety evaluation.

## Stretch goals

- **Preference-data collection plan.** For your preference-optimisation pass, propose a data-collection pipeline: prompt sources, response generation setup, judge protocol (human or AI-assisted), quality-control gates, sample size. State the expected collection cost in person-weeks and dollars.
- **Compare DPO vs. RLHF-PPO for your program.** Read Rafailov et al. 2023 and one of the recent DPO-vs-PPO comparison papers (e.g., Xu et al. 2024). Write a one-paragraph defense of your choice, with citation.
- **Red-team category tree.** Design a two-level red-team category tree (10-20 top-level categories, 3-5 sub-categories each) mapped to the specific safety benchmarks in your chapter-6 tier-4 waterfall. Note the categories your benchmarks under-cover — this is where your program is exposed at ship.
- **External red-team engagement scope.** If your posture is safety-heavy and the program plans an external red-team engagement, scope it: which vendor or academic partner, what payload categories, what deliverables, what timing. This is often the first artifact mod-409 asks for.
- **Constitutional-AI cost model.** For a program considering a Constitutional-AI-style method, estimate the cost of the constitutional-judge inference at scale vs. the human-labour cost saved. Cite Bai et al. 2022 for the framing.
