# exercise-01: Parallelism Recipe Decision Doc

**Estimated effort:** 3 hours

## Objective

Author the **parallelism-recipe decision doc** for a concrete (model, cluster, dataset) point: name the specific `(TP, PP, DP, SP, EP, precision, recompute, batch, seq)` tuple, walk chapter 3's decision tree to defend how each dimension was chosen, argue against one-degree-more and one-degree-less-parallelism alternates, map the mesh to the cluster's physical topology, and set a numeric re-plan trigger. This document is section 1 of the mod-404 five-artifact stack; it feeds forward into exercises 02-05 and is the top of the one-page recipe summary chapter 7 rolls up.

The exercise is deliberately about a *written commitment* — a specific tuple with a decision-tree argument — not about running a training job. Measurement lands during the ablation phase; the re-plan trigger is what handles disagreement.

## Prerequisites

- Chapter 01 — Scope and hand-off (five-artifact stack, Staff vs. specialist ownership, re-plan trigger shape).
- Chapter 02 — Parallelism primitives (DDP, FSDP/ZeRO, TP, PP, SP, EP vocabulary; the trade-off triangle; the recipe-tuple form).
- Chapter 03 — Choosing the parallelism recipe (the five-step decision tree, the mesh-to-topology mapping, the four worked recipes).
- A concrete program brief, from either your current employer, a prior employer (< 18 months), or a public reference program. Do not fabricate.

## Choose your program brief

Pick one, in preference order. You will carry the same brief through exercises 02, 03, 04, and 05 — do not swap it.

1. **A real program at your current employer that you would plausibly own or co-own.** Anonymise team names; keep the actual (N, cluster shape, dataset shape, deadline).
2. **A real program at a prior employer less than 18 months ago.** Same anonymisation.
3. **A publicly-scoped program you can reverse-engineer from a paper.** Good picks: LLaMA-3 8B or 70B (Meta 2024), Mixtral 8×7B (Mistral 2024), DeepSeek-V2 (DeepSeek 2024), MPT-7B (MosaicML 2023). Pretend you are the recipe author and re-derive the recipe from scratch; do not copy the paper's answer, arrive at it.
4. **A domain-adapted continued-pretraining program.** Assume you are continuing pretraining from an existing 7B or 13B open-weights base on 32-128 H100s.

If you carried a brief across exercises in mod-403, use the same one here — the mod-403 outputs (compute budget, data plan) are exactly the inputs mod-404 consumes.

## Steps

1. **Restate the five inputs.** From chapter 3 — model shape (N, architecture family, hidden × heads × MLP, num_layers, seq, vocab), cluster shape (accelerator count and model, HBM, NVLink domain size, IB generation), batch shape (target global batch in tokens, target microbatch, throughput target), precision plan, recompute plan. One paragraph each. Pull the numbers from your program brief; do not fabricate.
2. **Walk the decision tree.** For each of the five steps in chapter 3, show your arithmetic and record which exit you took.
   - Step 1 (does model + Adam + activation fit on one accelerator?) — compute peak memory with the chapter 3 formulas; state yes/no.
   - Step 2 (does FSDP-3 alone fit?) — compute per-rank memory with `(14·N)/DP + gathered_block + activations`; state yes/no.
   - Step 3 (does one transformer block fit?) — compute `2 · block_params + block-activation`; state whether TP is required.
   - Step 4 (add PP?) — if 3D-parallel, pick T, P, and DP with the reasoning; verify `T · P · DP · EP = K`.
   - Step 5 (MoE?) — if dense, exit; if MoE, pick EP and defend the intra-node placement.
   - Step 6 (long sequences?) — verify SP if TP present; consider CP if seq > ~32K.
3. **Write the recipe tuple.** `(TP=?, PP=?, DP=?, SP=?, EP=?, precision=?, recompute=?, batch=?, seq=?)`. This is the top-line commitment.
4. **Map the mesh to physical topology.** Which nodes host which pipeline stage. Which GPUs inside a node host which TP rank. How DP replicates. A small ASCII diagram (chapter 3 has one) or a two-column table is enough.
5. **Defend against alternates.** Name two alternate recipes — typically "one degree less parallelism" (e.g., drop PP by one; use pure FSDP instead of 3D-parallel) and "one degree more parallelism" (e.g., increase TP or PP; add EP where not needed). For each, state why it is worse: what constraint from chapter 2's trade-off triangle or chapter 3's mesh-nesting rule it violates.
6. **Run the five sanity checks.**
   - `TP · PP · DP · EP == K`?
   - `TP ≤ NVLink_domain_size`?
   - Pipeline bubble `≤ 10%` at your V, M, P?
   - Microbatch × PP-stages fits activation memory?
   - Global batch inside the critical batch band (McCandlish et al. 2018 for the classical form)?
7. **State the numeric re-plan trigger.** Concrete. Examples from chapter 3: "MFU comes in > 20% below plan → re-plan"; "pipeline bubble > 15% at measurement → adjust V, M, or P"; "cluster shape changes by more than one node → full re-plan."

## Deliverable

A single document, 2-4 pages, containing:

- **Section 1** — Program brief overview (one paragraph): what you would train, on what cluster, for what downstream product.
- **Section 2** — The five inputs, one paragraph each.
- **Section 3** — The recipe tuple, on one line, followed by the mesh-to-topology mapping.
- **Section 4** — The decision-tree walk, one paragraph per step, with the arithmetic that supports each answer.
- **Section 5** — The defense against two alternate recipes, one paragraph each, naming the constraint violated.
- **Section 6** — The sanity-check table, five rows, pass/fail with the arithmetic.
- **Section 7** — The re-plan trigger, one paragraph, numeric.
- **Section 8** — Three-sentence personal reflection: what surprised you between running the arithmetic and the recipe you had in your head before?

## Starter guidance

- **Do not skip the arithmetic.** The exercise's value is that you compute per-rank memory, block-gather cost, and pipeline bubble longhand at least once. LLM-assisted arithmetic is fine to check your work; do not use it to skip the derivation.
- **State your NVLink domain size.** For a stock HGX H100 8-GPU node, the domain is 8. For NVSwitch-enabled configurations (GH200 NVL32, GB200 NVL72), the domain can be much larger. Do not guess — cite the hardware spec.
- **Alternates are not straw men.** Pick alternates a reviewer would actually raise. "Why not TP=1?" for a 70B on 32 H100s is a legitimate question you have to answer. Chapter 3's four worked examples show what defensible alternates look like.
- **The re-plan trigger is uncomfortable to write.** ±20% MFU disagreement feels wide when you write it. It is the honest band because the primary source of MFU disagreement (network overhead you did not model) can easily move the needle 15-25%. Cite chapter 4's published bands if you need the calibration.
- **Do not exceed four pages.** A recipe doc that runs six pages is a recipe doc reviewers skim. If your decision-tree walk needs more space, factor the model-shape arithmetic into an appendix.
- **Do not write the training-loop code.** Chapter 1's heuristic — *am I doing recipe-scope work?* — is the check. If a paragraph starts *"the FSDP wrapping policy will use..."*, that is the training-pipeline engineer's paragraph.

## Acceptance criteria

- The five inputs are stated with numeric values pulled from a real program brief (not fabricated).
- The recipe tuple is complete: `(TP, PP, DP, SP, EP, precision, recompute, batch, seq)` with no dimension left blank.
- `TP · PP · DP · EP` equals the target accelerator count exactly. (Common failure mode: off-by-one.)
- `TP ≤ NVLink_domain_size` — stated and cited.
- The decision-tree walk shows arithmetic for at least steps 1, 2, and 3 (whichever apply until the tree exits).
- The mesh-to-topology mapping is stated: which nodes host which pipeline stage, which GPUs inside a node host which TP rank.
- Two alternate recipes are named and defended against — each with a specific chapter-2 or chapter-3 constraint the alternate would violate.
- The sanity-check table has all five rows filled with pass/fail arithmetic.
- The re-plan trigger is stated with a specific numeric threshold and a specific action ("if X measured > Y, re-plan Z").
- The reflection paragraph makes at least one specific observation about what surprised you.

## Stretch goals

- **Multi-scenario recipe.** Redo steps 2-6 for the same model at 0.5× and 2× the accelerator count. Which of the three scenarios' recipes are Pareto-defensible if platform tells you a bigger or smaller cluster is available than planned? Chapter 3's decision tree should exit at different points; note where.
- **Compare with a published recipe.** Take a program whose recipe is publicly documented (LLaMA-2 or LLaMA-3 tech reports, MegaScale paper, DeepSeek-V2 report) and apply the same decision tree to their `(model, cluster)`. Report where your derivation lands vs. what the paper documents. If you land more than one dimension different, investigate; usually the paper has an additional constraint (interconnect specifics, MFU history on the hardware generation) you did not model.
- **Hardware-generation sensitivity.** Redo step 3-4 assuming the same program on the *next* hardware generation (H100 → H200, or A100 → H100). Which dimensions of the tuple change and why? Chapter 4's roofline notes anchor the answer.
- **FP8 recipe variant.** Redo the recipe assuming Transformer Engine FP8 for the GEMMs. Which memory constraint of the decision tree relaxes? What does the recipe tuple change to?
