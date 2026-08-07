# exercise-03: Failure Mode Budget and Mitigation Plan

**Estimated effort:** 3 hours

## Objective

Fill in the **failure-mode budget table** for the recipe from exercises 01-02: six named modes, each with signature, detection cadence, response SLO, and per-week compute and wall-clock cost. Reason about how the six-mode weights differ for your program (pretraining, continued pretraining, fine-tune, or MoE). Roll the per-mode wall-clock costs into a failure-mode fraction that feeds back into mod-403 chapter 3's cluster-hour risk multiplier. Adjust the reserve-and-checkpoint numbers (exercise-04 input) if the aggregate exceeds the mod-403 risk budget.

This is section 3 of the mod-404 five-artifact stack. The deliverable is a table plus a short narrative that the on-call runbook (contract-A responsibility of chapter 7) is derived from.

## Prerequisites

- Chapter 05 — Failure modes and mitigation cadence (the six-mode taxonomy, cost-per-event vs. cost-per-cycle framework, pretraining vs. fine-tune weights, the budget table shape).
- Exercise-01 and exercise-02 delivered: recipe tuple, MFU plan, wall-clock forecast, expected bottleneck.
- Mod-403 chapter 3 for the cluster-hour risk multiplier the failure budget feeds.

## Steps

1. **Restate the program regime.** Pretraining from scratch, continued pretraining, fine-tune, or MoE. This determines the per-mode weight adjustment from chapter 5's "mode weights differ" section.
2. **State the failure-rate assumptions.** For each of the six modes, name the assumption you are budgeting against and cite a source or a stated basis:
   - **Mode 1 (silent NaN / bit-flip).** Expected NaN events per 100 000 accelerator-hours; ECC-uncorrectable events per accelerator-year. Cite: vendor MTBF, OPT-175B or LLaMA-3 logbook, your platform team's fleet history.
   - **Mode 2 (straggler node).** Expected straggler-eviction events per week at your cluster size. Cite: MegaScale numbers, platform-team fleet history.
   - **Mode 3 (network partition).** Expected partitions per week. Cite: platform-team fabric-health numbers or a defensible plausibility ceiling.
   - **Mode 4 (checkpoint corruption).** Expected save-time verification failures per 100 saves; expected undetected corruptions per 100 saves. Cite: OPT-175B logbook, platform storage SLO.
   - **Mode 5 (loss spike).** Expected spikes per week at your program regime. Cite: OPT-175B, PaLM, BLOOM chronicles.
   - **Mode 6 (dataloader stall).** Expected chronic-stall onset events per week. Cite: platform IO SLO or defensible estimate.

   If you do not have a citation for a mode, use a plausibility ceiling and *state it as such* — do not hide the guess.
3. **Fill in the six-row budget table.** Chapter 5's template. For each mode:
   - **Detection cadence.** Concrete — "per-step loss finite-ness check via all-reduce of scalar", "per-rank per-step timing with rolling P99 over 100 steps", etc.
   - **Response SLO.** Numeric — "stop within 1 step; restart within one restart-latency budget (30 min target)"; "evict within 200-1000 steps; replace within one restart-latency budget."
   - **Compute cost per week.** Order-of-magnitude estimate; most rows are ~negligible; mode 4 (checkpoint verification doubles IO) and mode 6 (pre-tokenisation offloads to data side) have measurable line items.
   - **Wall-clock cost per week.** `events_per_week × restart-latency_budget` (or MFU-loss × window for chronic modes). Compute for each mode.
4. **Sum the wall-clock costs into the failure-mode fraction.** As a percentage of a 168-hour week.
5. **Reconcile with mod-403 chapter 3's risk multiplier.** State the risk multiplier the mod-403 budget was defended with. If your failure-mode fraction exceeds it, either (a) revise mode-by-mode assumptions and re-derive, or (b) call out the delta as a hand-back to the program brief.
6. **Write the per-mode narrative.** One paragraph per mode, in the shape chapter 5 uses: signature, detection, response, cost. This is the source-of-truth text the on-call runbook is derived from.
7. **Adjust weights for the program regime.** For a pretraining program: raise mode 5 (spikes) weight, raise mode 1 (long-run corruption) weight. For a fine-tune: lower mode 5, raise mode 4 (checkpoint format compatibility with base checkpoint). State the adjustment and how it changes the table.
8. **State the re-plan trigger.** Numeric per-mode: "if observed spike rate > 2× budget → recalibrate mode 5 SLO"; "if observed straggler rate > 2× budget → grow warm-standby (exercise-04 input)".

## Deliverable

A single document, 3-5 pages plus the table, containing:

- **Section 1** — Program regime restated and the per-mode weight-adjustment rationale.
- **Section 2** — Failure-rate assumptions per mode, each with citation or explicit plausibility caveat.
- **Section 3** — The six-row budget table (chapter 5 template).
- **Section 4** — Per-mode narrative (one paragraph per mode).
- **Section 5** — Aggregate failure-mode wall-clock fraction, per-week; reconciliation with mod-403 risk multiplier.
- **Section 6** — Re-plan triggers per mode.
- **Section 7** — Runbook hand-off note: which of the six per-mode narratives will the training-pipeline engineer's on-call runbook derive from directly.
- **Section 8** — Three-sentence personal reflection: which mode's cost surprised you most vs. the "just handle failures as they come" intuition you had before?

## Starter guidance

- **Do not assume zero for any mode.** Every published pretraining logbook reports incidents in every category. The mode with the *smallest* per-week count is still non-zero. If your budget rounds a mode to zero, either (a) you have a citation for a genuine floor rate at your cluster size (unusual), or (b) you are burying a risk the review will find.
- **The restart-latency budget from exercise-04 is a joint input.** You have not written exercise-04 yet; use a target restart-latency of 20-45 minutes as a placeholder and iterate once exercise-04 is drafted. State the placeholder explicitly.
- **Silent NaN detection is not a specialist call — it is a Staff call.** The per-step finite-ness check costs nothing and prevents unbounded loss. Any recipe that budgets less-than-per-step detection for mode 1 is under-budgeted. Do not defer this.
- **Loss-spike on-call decision windows are program-scope commitments.** The N-minute window in chapter 5 mode 5 is *how quickly* the on-call must decide. Setting N = 30 minutes commits the training-pipeline team to a rotation that responds in 30 minutes. Do not set N without thinking about the rotation shape.
- **Do not implement.** The exercise deliverable is the budget and the narrative. The specific `torch.isfinite` incantation, the NCCL watchdog config, the checkpoint verification pipeline — those are training-pipeline-engineer artifacts, not this exercise's.
- **Cross-check with the reserve-and-checkpoint chapter.** Modes 1, 3, 5 imply restart-from-checkpoint; mode 2 implies node replacement from warm standby; mode 4 implies save-retry-or-fall-back. Exercise-04 dimensions the reserve and checkpoint sides that your response SLOs here consume.

## Acceptance criteria

- The program regime is stated (pretraining / continued-pretrain / fine-tune / MoE) and the mode-weight adjustment is explicit.
- All six modes have failure-rate assumptions with a citation or an explicit plausibility caveat — no mode has an unjustified zero.
- The six-row budget table has all four columns populated: detection cadence, response SLO, compute cost per week, wall-clock cost per week.
- The aggregate wall-clock fraction is computed and reconciled with mod-403's risk multiplier; if it exceeds, the reconciliation direction is stated.
- Per-mode narrative is one paragraph per mode with the four sub-parts (signature, detection, response, cost).
- Re-plan triggers are stated per mode, numeric, with a specific action.
- The runbook hand-off note names which of the six narratives the training-pipeline engineer's on-call runbook derives from (chapter 7 contract A).
- Reflection paragraph makes a specific observation about which mode's budget line surprised you.

## Stretch goals

- **Failure-mode tabletop.** Take the recipe and pretend a specific incident happens (e.g., "at step 45 000, gradient norm spikes 20×, loss increases by 0.4"). Walk the response: what detection fires, what SLO applies, what the on-call decides, what the cost is. Do this for at least three modes.
- **Compare against a published logbook.** Read the OPT-175B logbook or the BLOOM training chronicles. Which of your budget numbers land within one order of magnitude of theirs? Where you diverge more than 5×, defend the divergence (different cluster, different hardware generation, different failure-detection maturity).
- **MoE-specific mode.** If your recipe is MoE, add a seventh row: expert-routing imbalance. State the signature (AllToAll skew), detection cadence (per-N-step expert utilisation histogram), response SLO (auxiliary loss weight tune, or restart if collapsed), and cost. Cite: chapter 2's MoE-load-balance discussion, GShard / Switch Transformer papers.
- **Bit-flip deep dive.** For mode 1, distinguish DRAM-vs-HBM ECC handling, single-event-upset probability at your accelerator count over the main run, and whether the recipe requires bit-exact reproducibility (which affects the response SLO for un-correctable ECC events). Cite: NVIDIA DCGM ECC event categories.
- **Cost-benefit of aggressive vs. conservative detection.** For any two modes, quote the per-cycle detection cost and the per-event uncaught cost. Solve for the cadence where they equal. Compare against the cadence you chose. Is your cadence conservative-side-of-optimum or aggressive-side, and why?
