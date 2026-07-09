# The Portfolio Demand Forecast: Baseline, Program-Driven, and Speculative

## Motivation

Every downstream artifact in this module — the capacity portfolio in chapter 3, the utilisation review in chapter 4, the FinOps cadence in chapter 5, the TCO model in chapter 6 — reads from one number: **how much compute the ML org will need next fiscal year, broken down in a way finance and capacity planners can act on**. That number is the **portfolio demand forecast**. It is the artifact this module produces first and re-produces on a quarterly cadence, because every other artifact goes stale when demand shifts.

The forecast is easy to do badly. The two common failure modes are (a) *"add every team's ask together and multiply by 1.5 for headroom"*, which produces a number the finance partner cannot defend and which caps everyone's roadmap when it turns out to be wrong, and (b) *"assume next year looks like this year"*, which misses the fact that a new training program, a new inference product, or a hardware-generation shift is about to change the picture materially. Chapter 2's method separates demand into three buckets — baseline, program-driven, and speculative — each defended on its own terms, each carrying its own confidence band.

## What the forecast is as an artifact

The **portfolio demand forecast** is a spreadsheet plus a two-to-four-page text companion that answers, for the ML org, over a one-year and a three-year horizon:

1. What **baseline demand** already exists — the steady-state training and inference footprint the org has committed to keep running, absent any new program.
2. What **program-driven demand** is coming — the training programs and new inference products that the org has already approved or will plausibly approve in the horizon, with the compute each requires and the confidence that it will actually happen.
3. What **speculative demand** could arrive — the roadmap items whose payoff is uncertain but whose compute footprint would be material if they landed.
4. What the **totals** look like at three confidence tiers (P50, P75, P90), broken down by workload class (training / batch-inference / online-inference) and by hardware generation.

The output units are **accelerator-hours per quarter** and **dollars per quarter** at both the org's actual internal cost and the public-cloud on-demand cost, exactly the same two-column format [`mod-403 chapter 3`](../mod-403-foundation-model-training-programs/03-cluster-hour-budget-defense.md) uses. Reusing the format matters — the portfolio forecast has to reconcile with every program's cluster-hour budget, and format drift makes reconciliation impossible.

Pairs with [`exercise-01`](exercises/exercise-01-portfolio-demand-forecast.md), which walks a real forecast end to end.

## The three buckets, defined precisely

### 1. Baseline demand

**Baseline** is what the org will spend next quarter *if it approves no new programs and launches no new products*. Concretely:

- **Steady-state inference serving.** Every model currently in production, running against its current traffic pattern, at its current fleet size. The ranking model, the recommender, the chat product, the batch-embedding pipeline. Include managed-API line items (hosted LLM APIs the org is already paying for) as baseline even though they are opex not GPU-hours — the CFO cares about total spend, not just hours.
- **Steady-state retraining cadence.** Every production model has a retraining schedule (nightly, weekly, monthly). Sum those. This is often 5-15% of inference in dollar terms and gets forgotten in first-pass forecasts.
- **Continuous evaluation and offline-eval harness compute.** Chapter 6 of [`mod-406`](../mod-406-cross-team-eval-and-experimentation/) walks the eval-harness footprint. It is not zero. Include it in baseline.
- **Bookkeeping compute.** Feature-store recomputes, embedding refreshes, ETL that only runs because of ML workloads. Draw the line at *what would stop running if the ML org shut down tomorrow*. If a pipeline is genuinely dual-use (also used by analytics), split proportionally; otherwise it is baseline.

The baseline number is the easiest to defend and the number the finance partner will most trust — because it is *retrospective*. Prior-year actuals from the FinOps team's chargeback report are the correct starting point. The Staff engineer's job is not to re-forecast baseline from first principles; it is to (a) sanity-check the FinOps allocation against known workload shape, (b) name the workloads that will *shrink* or *grow* organically (traffic growth on the chat product, decommissioning an old ranking version), and (c) publish the baseline as a defensible number with a small (~10%) uncertainty band.

### 2. Program-driven demand

**Program-driven** is the compute footprint of specific, named, approved-or-plausible training programs and new inference products, each of which is *someone's committed roadmap item* with a program owner. Concretely:

- **Named training programs.** Every program that has a mod-403-shape brief in flight, whether it has been fully approved yet or not. The forecast quotes each program's own cluster-hour budget from its brief, with its own risk multiplier already applied. Do not re-derive; do cite.
- **Named new inference products.** New model launches with a plausible traffic estimate. The product-management side of the org will resist quoting a traffic estimate before launch; the forecast quotes their public estimate and marks the confidence tier accordingly. If they refuse to quote, forecast at three tiers (low / mid / high) and let the P50 be the mid.
- **Hardware-generation transitions.** If the org is planning an H100 → H200 → B200 migration in the horizon, the transition itself has a compute cost (dual-running during migration, validation runs on the new hardware, potential MFU regression in the first months). Budget it here. This is the item most-often forgotten and it can add 10-20% to a fiscal year.

Program-driven demand carries a per-program confidence tier. A program with an approved brief and staffed team is P90. A program that is on the roadmap but has not started ablations is P50. A program that leadership has said "we should probably do" but has no owner is P25 — and honestly belongs in the speculative bucket, not here. The tier discipline is what separates a defensible forecast from a wish list.

### 3. Speculative demand

**Speculative** is what the org *might* need if certain bets pay off — new agent product lines, a research-scale training program, a large-scale internal fine-tune, an unplanned hyperscaler-partnership commitment. Concretely:

- **Named-but-unfunded roadmap items.** A specific proposal exists, no team is staffed to own it, leadership has floated it. Compute footprint is estimated to order-of-magnitude (10K H100-hours, 100K H100-hours, 1M H100-hours) rather than to two decimal places.
- **Reactive-capacity buffer.** Every ML org gets asked to serve *something* it did not plan for — an executive-visibility demo, an incident response that requires re-running old batch jobs, a hackathon week. Budget 5-10% of baseline as reactive-capacity buffer. It is not slush; it is the empirical rate at which unplanned demand shows up.
- **Frontier-following.** If a competitor or peer lab ships a model class the org will feel forced to match (e.g., a new reasoning-model paradigm), the compute to *investigate* and possibly *train* a response lives here. This is the item finance least understands and the item most likely to be cut from a first-pass forecast; leave it in with an explicit low tier.

Speculative demand carries **explicit "if X then Y" clauses**. "If the agent product launches, add 40K H200-hours per quarter for serving. If it does not launch, subtract that line." The finance partner will value the honesty. The alternative — hiding speculative demand inside program-driven — is the fastest way to burn credibility when the speculative bet does not materialise.

## The one-year and three-year horizons

The forecast quotes two horizons and they answer different questions.

**One-year (fiscal-year-detail) horizon.** Quarter-by-quarter, workload-class by workload-class. This is the number that ties into the reserved-capacity commit decision in chapter 3, the fiscal-year budget the CFO approves, and the FinOps operating cadence in chapter 5. Uncertainty is bounded — a program that has not started by end of Q2 is not going to consume its budget in Q3. The one-year forecast is the artifact leadership makes commitments against.

**Three-year (envelope) horizon.** Annual totals, no quarterly detail, three explicit scenarios: base case, growth case (business grows faster than expected), and headwind case (business grows slower, or a hardware generation slips). This is the horizon the reserved-capacity commit decision uses to price a three-year vs. one-year vs. no-commit choice. It is *not* a commitment — nobody signs off a three-year detailed plan honestly — but it is a *directional envelope* that keeps the one-year forecast from painting the org into a corner. The three-year forecast is the artifact that keeps chapter 3's five-bucket portfolio from over-committing on the wrong hardware generation.

Longer than three years is not staff-scope work. Five-year hardware roadmap forecasting is Principal-and-above territory and is deferred to `principal-ml-engineer-learning`.

## Confidence tiers and how to publish them

The forecast publishes three tiers per bucket:

- **P50** — the "if the world behaves close to how we expect" number. Baseline is roughly retrospective actuals plus organic growth. Program-driven includes all P50-and-above programs. Speculative includes only the reactive-capacity buffer.
- **P75** — the "some things go worse than expected" number. Baseline grows 15-20% because some model that was scheduled for decommissioning slips. Program-driven adds a program whose brief exists but has not been formally approved. Speculative includes the base reactive buffer plus one named speculative item.
- **P90** — the "you would be surprised if we spent more than this" number. Baseline plus program-driven includes everything on the roadmap with a plausible owner. Speculative includes all named items plus a growth-case buffer.

Publishing three tiers protects the forecast from the two political pressures the Staff engineer will feel: (a) *"give me one number so I can plan"* — the CFO wants a single line-item — and (b) *"you have to include our team's ambitious plan"* — a program owner wants their speculative bet in the funded plan. The right rebuttal is *"P50 for the number to plan against, P75 for the number to hold in reserve, P90 for the ceiling we won't blow through — your program is in P75."* The three-tier discipline puts the forecast conversation in the finance vocabulary the CFO already thinks in, which is often the moment the Staff engineer earns credibility.

## What Staff owns vs. what program owners own

**Program owners own** their program's cluster-hour budget (mod-403 chapter 3 shape). The portfolio forecast **cites** the program's own budget — it does not re-derive, and it does not overrule. If the Staff engineer thinks a program budget is under-priced, the correct move is a private conversation with the program's Staff engineer and, if unresolved, a note in the forecast's assumptions section ("Program X's budget assumes 45% MFU; peer track's measurement suggests 35-40%; forecast uses 40% and notes the delta"). Do not silently correct.

**Product owners own** their product's traffic estimate for new-launch inference. Same discipline — cite, do not re-derive.

**Staff at portfolio scope owns** the aggregation, the bucket assignment (is this program-driven or speculative?), the confidence-tier call, the reconciliation to prior-year actuals, and the sensitivity analysis on hardware-generation and demand shifts. Those calls are the ones that require seeing the whole picture — the ones no L30 tech-lead was hired for.

## The forecast as an artifact — section shape

The two-to-four-page companion doc follows this section shape:

1. **Executive summary** — three sentences and one table. The table has three rows (baseline / program-driven / speculative) and three columns (P50 / P75 / P90) in dollars.
2. **Baseline breakdown** — a table of steady-state inference, retraining, continuous eval, and bookkeeping, tied to the prior-year actuals from FinOps. One paragraph on organic growth and decommissioning.
3. **Program-driven breakdown** — a table with one row per program, each row citing the program's own brief, with confidence tier and cluster-hour total. One paragraph on the hardware-generation transition budget.
4. **Speculative breakdown** — a table with one row per named speculative item, each with an "if X then Y" clause. One paragraph on the reactive-capacity buffer with justification.
5. **Three-year envelope** — a small table (three years, three scenarios) with annual totals, no quarterly detail.
6. **Sensitivity table** — what the P50 total changes to if: (a) inference traffic grows 30% instead of 15%, (b) H100 → H200 delivery slips two quarters, (c) a named speculative item does not launch, (d) a program's MFU misses by ten points.
7. **Reconciliation notes** — where the forecast agrees and disagrees with the FinOps team's forward-looking model, if one exists, and where it agrees and disagrees with each program's own budget. The agreement is not the interesting part; the disagreement is what leadership needs to see.

Sections 6 and 7 are the ones most often skipped by first-time forecast authors and the ones most often the reason the forecast gets sent back for a rewrite. Sensitivity analysis is where the forecast earns the right to be trusted; reconciliation is where it earns the right to be *believed* by the program owners.

## Summary

- The **portfolio demand forecast** aggregates three buckets — baseline, program-driven, speculative — over a one-year (quarterly detail) and three-year (annual envelope) horizon.
- **Baseline** starts from prior-year FinOps actuals and adjusts for organic growth and decommissioning. It is the easiest to defend and the number the CFO trusts first.
- **Program-driven** cites each program's own cluster-hour budget under a confidence tier. Do not re-derive; do reconcile.
- **Speculative** carries explicit "if X then Y" clauses and includes the reactive-capacity buffer that funds unplanned demand.
- Publish three tiers (P50 / P75 / P90) and a sensitivity table. The three-tier discipline is what turns the forecast from a wish list into a defensible planning artifact.
- The forecast is a **quarterly-refresh** artifact. Every downstream chapter reads from it; every downstream chapter goes stale when it does.

The next chapter walks the capacity portfolio — the five buckets of purchase mode (reserved / committed-use / on-demand / spot-or-preemptible / self-hosted / multi-cloud) that the forecast is bought against.
