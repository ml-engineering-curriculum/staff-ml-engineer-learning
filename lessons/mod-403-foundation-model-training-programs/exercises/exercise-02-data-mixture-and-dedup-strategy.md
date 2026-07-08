# exercise-02: Data Mixture and Dedup Strategy

**Estimated effort:** 3 hours

## Objective

Design the **data plan** for the program brief you scoped in exercise-01: the mixture table across source classes, the curation pipeline stages, the license and PII posture, the dedup configuration, and the corpus-versioning discipline. The document produced here is sections 3 and 4 of the program brief and the "what data does the program depend on" appendix of the executive one-pager.

The exercise is deliberately not about *finding* petabytes of tokens; assume you can source from Common Crawl, RedPajama, Dolma, or an equivalent open corpus for pretraining programs, and an internal domain corpus plus a base model for continued-pretraining programs. The exercise is about the *design decisions*.

## Prerequisites

- Exercise-01 completed. The (N, D) target sets the token-supply requirement.
- Chapter 04 — Data Mixture and Curriculum (mixture table, source classes, quality-vs-tokens trade-off, curriculum shape).
- Chapter 05 — Data Curation Pipeline (the eight stages, license and PII posture, MinHash-LSH dedup, dedup-as-scaling-axis).

## Steps

1. **Restate the D target from exercise-01.** State D and the confidence band. The mixture design has to supply this D.
2. **Draft the mixture table.** For your program (pretraining or continued-pretraining), pick source-class weights across curated web, broad web, code, academic/books, math/reasoning, dialogue/forums, multilingual, and (if continued-pretraining) the domain corpus. Aim for 5-8 rows. State a starting weight for each and one-sentence justification.
3. **Compute token supply per row.** For each row, estimate the unique post-dedup token count available from the sources you would use. Use published numbers where possible (e.g., RedPajama-v2 documented volumes, Dolma per-source counts, StarCoder / The Stack code counts). State each with a source citation.
4. **Compute effective epochs per row.** At your mixture weights and target D, how many times does each row's unique corpus get consumed? Read Muennighoff et al. 2023 first — flag any row consumed more than 4× as needing an explicit ablation.
5. **Decide the quality-vs-tokens posture.** Are you in the option-1 (strict-high-quality), option-2 (broadened mixture), or option-3 (repeat high-quality) posture, per chapter 4? State the posture and defend against the other two.
6. **Design the curation pipeline.** Draw the eight-stage pipeline from chapter 5 with your program's specific choices at each stage: source-snapshot pinning, heuristic vs. classifier quality filter, license and PII treatment, MinHash threshold and scope, substring-dedup scope, safety-filter categories, sampler, tokenizer.
7. **Write the license and PII posture.** For each source class: license status, opt-out signals honoured, PII treatment (redact / remove / annotate), audit-trail commitment. State which sections require legal sign-off before the pipeline runs.
8. **Set the dedup configuration and defend it as a scaling axis.** MinHash similarity threshold, number of permutations, scope (within-snapshot, across-snapshots, across-source-classes). Cite Lee et al. 2022 to defend the choice as material.
9. **Design the curriculum.** Warmup mixture, main mixture, cool-down mixture. State the phase boundaries as fractions of training steps. If your program is continued-pretraining, state the domain-progressive curriculum across continued-pretraining → SFT → preference-optimisation.
10. **Design corpus versioning.** How is each corpus version identified, stored, and referenced from run metadata? Who owns the corpus.

## Deliverable

A single document, 4-6 pages plus the mixture table and the pipeline diagram, containing:

- **Section 1** — Program brief overview and restated (N, D) target from exercise-01.
- **Section 2** — Mixture table (rows × source classes; columns for weight, source citation, unique tokens available, effective epochs).
- **Section 3** — Quality-vs-tokens posture with one paragraph defending the choice against the other two options.
- **Section 4** — Curation pipeline diagram with the eight stages and per-stage decisions.
- **Section 5** — License and PII posture, per source class, with the sections marked "requires legal sign-off".
- **Section 6** — Dedup configuration with citation of Lee et al. 2022.
- **Section 7** — Curriculum (warmup / main / cool-down; plus continued-pretraining phase progression if applicable).
- **Section 8** — Corpus-versioning scheme and named pipeline owner.
- **Section 9** — Three-sentence reflection on the biggest data-side trade-off the exercise made you write down explicitly.

## Starter guidance

- **Anchor on published mixtures first.** LLaMA-2 mixture, Dolma mixture, RedPajama-v2 mixture, DCLM mixture. Start from one and edit; do not build from scratch. State which reference mixture you started from.
- **The unique-token counts are the constraint you did not have when writing the mixture weights.** Compute them second; expect to move a mixture weight or two after seeing the numbers.
- **Do not skip step 4 (effective epochs).** This is the calculation the review will demand and the one first-time program owners consistently under-do. If the answer is "we epoch curated-web 6×", flag it explicitly and cite Muennighoff et al. 2023 on the diminishing returns.
- **Legal sign-off is real.** State which sections legal has to approve. If you are running this against your current employer's program, you can note "would need legal review here" rather than actually doing the review.
- **Dedup is not optional.** If the pipeline has no dedup stage, the exercise is not complete. The threshold and scope are decisions to defend, not "let's skip it for now."

## Acceptance criteria

- The mixture table has 5-8 rows, each with a numeric weight, a source citation, an estimated unique-token count, and an effective-epochs number.
- The quality-vs-tokens posture is stated and defended.
- The pipeline diagram covers all eight stages from chapter 5; no `TBD` cells.
- License and PII posture is stated per source class; sections requiring legal sign-off are explicitly marked.
- The dedup configuration is specific (numeric MinHash threshold, permutations, scope) and cites at least Lee et al. 2022.
- The curriculum is specified as a fraction of training steps; the cool-down mixture is different from the main mixture.
- Corpus-versioning scheme is described and a pipeline owner is named (or, for public-reference programs, the role of the named owner is described).
- The reflection is one specific observation, not a summary.

## Stretch goals

- **Run a small mixture ablation on paper.** For your top mixture uncertainty (usually code weight, math weight, or domain weight for a fine-tune program), design the 2-3 cell ablation you would run in chapter 6 phase 2 to resolve it. State the cells, the small-scale N, the small-scale D per cell, and the eval-waterfall gates you would decide on.
- **Compare quality-filter approaches.** Read the DCLM (Li et al. 2024) paper and pick either a heuristic-only, a classifier-only, or a hybrid quality-filter configuration for your pipeline. Defend the choice using DCLM's scaling-law analysis.
- **Data-plane cost model.** Estimate the CPU-hours and storage cost of your pipeline for one full run at your (N, D) target. Cross-check that the number is 5-15% of the accelerator-hour cost from exercise-01. If it is materially higher, revisit the pipeline design.
- **Continued-pretraining domain-corpus sourcing plan.** If your program is a fine-tune, propose a domain-corpus acquisition path: internal warehouse extraction, licensed vendor, human-curated collection. State the license terms, the volume, and the timing.
