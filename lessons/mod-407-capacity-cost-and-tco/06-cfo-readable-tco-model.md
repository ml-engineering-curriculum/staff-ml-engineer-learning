# The CFO-Readable TCO Model — Amortisation, Marginal Cost, Sensitivity, One-Pager

## Motivation

Everything this module has built so far — the demand forecast, the capacity portfolio, the utilisation review, the FinOps discipline — feeds into one artifact: the **TCO model the CFO can actually read**. That artifact is what the ML org's headcount and roadmap get signed off against for the next fiscal year, and it is the single Staff-scope document most often written badly by first-time authors. The classic failure mode is a spreadsheet dense with MFU arithmetic, program-brief line items, and inference-fleet detail — technically correct, executively unreadable, and rejected in review because the CFO cannot answer the three questions from chapter 1 against it.

This chapter walks the section shape of the CFO-readable TCO model, the four arithmetic patterns the model must handle correctly (training amortisation, inference marginal cost, per-request cost target, sensitivity analysis), and the one-page executive summary that sits on top. It pairs with [`exercise-04`](exercises/exercise-04-cfo-readable-tco-model.md), which walks a full model authored end-to-end.

## What the model is as an artifact

The **CFO-readable TCO model** is a workbook (spreadsheet plus 5-8 page text companion, plus a single-page executive summary) that answers the three questions from chapter 1:

1. *"What is the ML org going to spend next fiscal year, and on what?"* — annualised total, broken down by workload class (training / batch inference / online inference / hosted-API), by capacity mode (chapter 3's five buckets), and by team.
2. *"What levers do we have if the number needs to come down 20%?"* — the top three-to-five cost-reduction moves ranked by dollar impact, each with the trade-off named.
3. *"How sensitive is the number to things we cannot control?"* — a sensitivity table across the four axes that matter (demand shift, hardware generation, hosted-API price change, MFU miss).

The workbook is read by the CFO and the finance partner. The text companion is read by the VP of Engineering and by directors. The one-page executive summary is read by everyone else — the leadership team, the board, and the ML org itself.

## The four arithmetic patterns

### 1. Training amortisation over model-family lifetime

A training program's cost is not "the cost of one main run." It is the cost of the main run *plus* the ablation and restart budget from mod-403 chapter 3, *plus* the operational overhead of the program team, *plus* the fractional cost of the shared infrastructure the program consumed. The TCO model amortises that total cost over the **lifetime of the model family** the program produces — usually one to three fiscal years — because that is how long the model will serve inference and generate business value.

Concretely: a training program that cost $2M and produces a base model served for 18 months should be amortised at $1.33M per year against inference over that period. If the same base model spawns three derivative fine-tunes, the training cost is shared across the four models proportionally to serving volume — not equally. This shape mirrors the way finance amortises capex over the useful life of the asset; the Staff engineer's job is to *name the useful life* and *defend the assumption*.

**What the CFO wants to see.** A single line item per training program: "Base model X, trained Q1 for $2M, amortised over 18 months against $Y of inference revenue attribution." Do not surface the MFU arithmetic. Do surface the amortisation policy.

**What the model must handle correctly.** Do not double-count the training cost by charging it against both the program budget and the inference-marginal-cost line — pick one attribution shape and stick to it. Do handle model deprecation: if a base model is decommissioned early, un-amortised training cost hits that quarter's line. Name the decommissioning trigger.

### 2. Inference marginal cost

For every production inference workload, the model quotes a **marginal cost per unit** — dollars per 1000 requests, dollars per 1000 tokens, dollars per document, dollars per prediction, whichever unit the business uses. The marginal cost is the number that lets product managers reason about "does this feature pay for itself?"

The marginal cost is *not* the average cost of running the fleet divided by requests served — that includes fixed costs (reserved commits, on-call, platform overhead) that do not scale with the next request. The marginal cost is **the cost of serving the next request assuming the fleet is already provisioned at current traffic** — usually dominated by the GPU-seconds per request and the energy consumed, with a small allocation for storage and networking.

Concretely: an online inference workload running on H100s at 40% utilisation, with a measured 20ms P50 GPU time per request, has a marginal cost roughly = (GPU-seconds per request) × (dollars per GPU-hour / 3600). At $3/hour on-demand and 0.02 GPU-seconds, that is ~$1.67 × 10^-5 per request, or ~$0.017 per 1000 requests. The number matters less than the *derivation being visible* — the CFO will not check the arithmetic but the finance partner will, and the finance partner needs to be able to see the pipeline of numbers.

**What the CFO wants to see.** Marginal cost per 1000 requests (or per 1000 tokens for LLM workloads) per workload, with a year-over-year trend and a comparison to the hosted-API alternative price. This is the number leadership uses to reason about pricing, feature-cost trade-offs, and whether the org's inference is competitive.

**What the model must handle correctly.** Distinguish **peak-load marginal cost** (relevant to product managers thinking about surge) from **average marginal cost** (relevant to unit economics). LLM workloads: distinguish per-input-token cost from per-output-token cost — output tokens dominate serving cost because of KV cache. See PagedAttention / vLLM (Kwon et al. 2023) for the KV-cache mechanics that make this so.

### 3. Per-request cost target

The **per-request cost target** is a forward-looking number: what does the org *want* the marginal cost to be in twelve months, and what is the plan to get there? The target is set by product / business needs — if the product needs to serve at $X per 1000 requests to be profitable at target margin, that is the input — and translated into an engineering target the ML org can commit against.

Setting the target is a joint call: product / finance name the business number, the Staff engineer names the achievable engineering number, and the target is the reconciliation. If the two numbers disagree by more than 2×, the workload has a fundamental economics problem that the TCO model must surface, not paper over.

**What the CFO wants to see.** For each material workload, current marginal cost, target marginal cost twelve months out, and the top two engineering moves that close the gap (typically: hardware-generation migration, model quantisation / smaller model, better cache utilisation, consolidation onto shared serving substrate). Ties into chapter 4's migration docs.

### 4. Sensitivity analysis

The model publishes a sensitivity table showing what the annual total changes to under each of four scenarios:

- **Demand shift.** Inference traffic +30% / -30% vs. forecast.
- **Hardware generation.** Next-generation hardware ships two quarters early / two quarters late; performance uplift is at the low / high end of the vendor claim.
- **Hosted-API pricing.** Hosted-API rates drop 30% (competitive pressure) / rise 30% (supply constraints). Relevant if managed-API is a material line item.
- **MFU / throughput miss.** Every training program's MFU misses by 10 points; every inference workload's throughput misses by 20%. Aggregate cost impact.

Each scenario is quoted as a delta to the P50 total and as a percent of the total. Two things this section must do: (a) show that the largest sensitivity is what leadership expects it to be (usually demand shift for inference-heavy orgs, hardware for training-heavy orgs) — if it is not, the model has an assumption error and the section forces its discovery; (b) name **the pre-agreed re-plan trigger** for each axis. "If inference traffic exceeds forecast by 30%, re-run the demand forecast and the capacity portfolio outside the annual cadence."

The pre-agreed re-plan trigger is the equivalent of chapter 3's sensitivity-and-trigger discipline from [`mod-403`](../mod-403-foundation-model-training-programs/03-cluster-hour-budget-defense.md), lifted to portfolio scope. It is the item most often missing in first-pass TCO models and the item most often the reason the model gets pushed back.

## The section shape of the TCO model

The workbook has this structure:

1. **Executive summary (one page)** — three tables and one paragraph. The paragraph names the total, the year-over-year change, and the top-two levers. The tables show total by workload class, total by capacity mode, and the sensitivity table.
2. **Assumptions register (one page)** — every material assumption in the model, one per line, with the source (program brief, MLPerf reference, prior-year actuals, vendor pricing sheet) and the confidence tier. Sensitivity depends on this being honest.
3. **Baseline breakdown (one page)** — steady-state inference, retraining, continuous eval, feature-store and pipeline compute. Reads from chapter 2's baseline forecast.
4. **Program-driven breakdown (one-to-two pages)** — one row per training program and per new inference workload. Cites program briefs; does not re-derive.
5. **Amortisation section (one page)** — training-program cost amortised over model-family lifetime, per point 1 above. This is the section the finance partner will spend the most time on.
6. **Marginal cost table (one page)** — per-workload marginal cost, current and twelve-month target, per point 2 above. Ties to chapter 4's utilisation review.
7. **Capacity-portfolio dollar breakdown (one page)** — chapter 3's five buckets, dollar spend per bucket, discount rate applied, exit-cost commentary.
8. **Sensitivity and re-plan triggers (one page)** — the four scenarios above, plus the trigger for each.
9. **Cross-references (half a page)** — pointer to the FinOps team's chargeback / showback report, pointer to each cited program brief, pointer to the peer platform team's paved-road roadmap that the model assumes.

The one-pager on top is the artifact the CEO and board see. The workbook is what the CFO and finance partner work through. The 5-8 page companion document is the *narrative* — why the numbers are what they are, what changed year-over-year, what the two-to-three biggest risks are.

## The one-page executive summary

The one-pager's structure:

- **The number.** Fiscal-year total in dollars, year-over-year delta as a percent. One sentence.
- **The workload-class breakdown.** Table with three rows — training, inference (self-hosted), inference (hosted-API) — and a percent-of-total column. Second column: year-over-year delta per row.
- **The top three levers.** Three-line list. Each line: the lever, the dollar impact if pulled, the trade-off (what breaks). Usually one of: shift more workload to reserved commit, migrate hosted-API workload to self-hosted, consolidate serving fleet, defer a speculative training program.
- **The top three risks.** Three-line list. Each line: the risk, the sensitivity in dollars, the pre-agreed re-plan trigger. From the sensitivity section above.
- **The ask.** One sentence naming what leadership is being asked to approve — the fiscal-year budget, the reserved-capacity commit, the headcount to run FinOps at the required cadence, etc.

The one-pager is a real deliverable, not a summary of the workbook. It is what leadership actually reads; the rest of the model exists to defend it. Every claim on the one-pager should be traceable to a numbered section of the workbook.

## Sensitivity to model-size and hardware-generation shifts

Two sensitivities deserve their own paragraph in the companion doc because they dominate the three-year envelope:

**Model-size shift.** If the org's LLM workloads move from a 70B-class base model to a 400B-class model over the horizon (or the reverse — from 70B to 7B with a mixture-of-experts substrate), inference cost changes non-linearly. A 6× larger dense model is not 6× more expensive to serve — the KV-cache-memory constraint means the effective cost can be 10-15× at the same throughput. The Staff engineer should quote both a "model shape stays constant" case and a "model shape shifts to next-generation" case, and name which is more likely.

**Hardware-generation shift.** The NVIDIA H100 → H200 → B200 cadence has been roughly a two-year step; each generation has offered materially better performance per dollar *if the workload can absorb the new precision or memory shape*. The TCO model quotes the current-generation baseline and a "next generation adopted for new workloads at time X" scenario. The transition cost — dual-running, validation, MFU regression during ramp — is real and should be a named line item.

Both sensitivities interact — a shift to a much larger model at the same time as a new hardware generation ships can either save the org or bankrupt it depending on adoption speed. The Staff engineer's job is to price both.

## What Staff owns vs. what finance and leadership own

**Finance owns** the amortisation policy (three-year vs. five-year straight-line, capex vs. opex treatment), the reconciliation to the company plan of record, and the CFO's calendar. The Staff engineer names the *engineering-side* assumptions but not the *accounting-side* policies.

**Leadership owns** the ask — the fiscal-year budget, the reserved-capacity commit, the headcount, the risk tolerance on the sensitivity axes. The Staff engineer's job is to present the choices, not to make them.

**Staff owns** the engineering-side numbers, the assumptions register, the sensitivity analysis, the one-pager, and the defence of every number in review. The finance partner is a co-author on the workbook; the Staff engineer is the primary author on the one-pager and the companion doc.

The dividing line is worth restating: if the CFO or the finance partner is doing the engineering math in the model, either they have taken on the Staff engineer's job or the Staff engineer is not showing their work. Neither is the intended failure mode.

## Summary

- The **CFO-readable TCO model** is a workbook plus 5-8 page companion plus a one-page executive summary. Every downstream approval — fiscal-year budget, reserved commit, headcount — hangs off it.
- Four arithmetic patterns: **training amortisation** over model-family lifetime, **inference marginal cost** per unit, **per-request cost target** twelve months out, **sensitivity analysis** across the four dominant axes.
- The **assumptions register** is the section that separates a defensible model from a wish list. Sensitivity depends on it being honest.
- The **one-page executive summary** is the artifact leadership reads. Every claim on it traces to a numbered workbook section.
- **Model-size shifts** and **hardware-generation shifts** dominate the three-year envelope. Both must be priced; both must have named re-plan triggers.
- The dividing line: the Staff engineer owns the engineering numbers and the presentation; finance owns amortisation policy and reconciliation; leadership owns the ask.

This module closes here. The next step is either **[`project-401-multi-team-ml-blueprint`](../../projects/project-401-multi-team-ml-blueprint/)**, which combines this module's TCO model with mod-402's portfolio blueprint into a portfolio-grade capstone deliverable, or **[`mod-408`](../mod-408-portfolio-reliability-and-incident-command/)**, which walks portfolio-scope reliability using the same review-and-escalation discipline this module has grounded.
