# exercise-02: Capacity Portfolio Selection

**Estimated effort:** 3 hours

## Objective

Author the **capacity-portfolio recommendation** for the exercise-01 forecast: pick the mix across the five capacity buckets (reserved / committed-use / on-demand / spot / self-hosted) with multi-cloud as an orthogonal risk overlay, size each bucket against a specific tier of demand, price the vendor-lock-in and hardware-generation-transition risk explicitly, and cross-check the fleet-sizing arithmetic against MLPerf-derived throughput. The deliverable is a 3-4 page recommendation document plus a workbook that shows the per-bucket dollar allocation and the exit-cost arithmetic behind every multi-year commit.

This is section 2 of the mod-407 four-artifact stack. It reads the exercise-01 forecast as input and hands its dollar-per-bucket breakdown forward to exercise-04's TCO model. If exercise-01's demand-tier assignments are wrong, this exercise's capacity mix is wrong; do not paper over disagreements between the two documents — cycle back and fix exercise-01.

## Prerequisites

- Exercise-01 delivered — the 3×3 bucket-by-tier table and the one-year quarterly detail are the inputs this exercise reads.
- Chapter 03 — the five capacity buckets, the per-bucket trade-offs and sizing rules of thumb, MLPerf as a size input, the vendor-lock-in and exit-cost framing. Read in full.
- Chapter 02 — enough to know which tier of the forecast each capacity bucket is meant to cover (baseline → reserved, medium-confidence program-driven → committed, P75 surge → on-demand, ablation / batch → spot, high-utilisation baseline at scale → self-hosted).
- Mod-403 chapter 3 — the single-program cluster-hour budget that the reserved-bucket sizing per-program reconciles against.
- Recommended: read the specific cloud-provider docs for the SKU your portfolio uses — [AWS Savings Plans](https://docs.aws.amazon.com/savingsplans/latest/userguide/what-is-savings-plans.html), [GCP Committed Use Discounts](https://cloud.google.com/docs/cuds), [Azure Reserved VM Instances](https://learn.microsoft.com/en-us/azure/cost-management-billing/reservations/save-compute-costs-reservations) — plus the [MLPerf Training](https://mlcommons.org/benchmarks/training/) and [MLPerf Inference](https://mlcommons.org/benchmarks/inference/) results tables (see `../resources.md`). Do not quote discount ranges from memory; the numbers move every year.
- If self-hosted is a candidate: read one of the hyperscaler engineering-blog fleet-economics posts referenced in `../resources.md` to understand the operational scope self-hosting implies.

## Steps

1. **Restate the forecast input.** In one paragraph, restate the exercise-01 P50 / P75 / P90 totals and the workload-class breakdown (training / batch-inference / online-inference). This exercise's mix is bought against these numbers; if the numbers changed between the two exercises, restate why.
2. **Choose the primary cloud posture.** Single-cloud, multi-cloud, or single-cloud-with-escape-hatch. Cite the chapter-3 threshold heuristic — under ~$5M annual spend single-cloud is usually right; $5M-$50M multi-cloud is a judgement call driven by sovereign requirements and negotiating posture; above ~$50M multi-cloud is usually necessary. Name the specific reason (sovereign compliance, negotiating leverage, prior single-provider capacity-crunch experience, none). If multi-cloud, name the split.
3. **Size the reserved bucket.** Cover baseline demand at the P50 tier plus the highest-confidence P90 program-driven demand from exercise-01 sections 2 and 3. For each program-driven line included, cite its brief. State the commit term (one year / three years), the payment structure (all-upfront / partial / no-upfront), and the effective discount rate against on-demand. Explicitly *exclude* speculative demand and P50 program-driven that is more than 12 months out; name what you excluded and why.
4. **Size the committed-use / Compute Savings Plans bucket.** Cover P50 program-driven demand that has not been placed in reserved and the medium-confidence portion of the fleet where the SKU shape is not yet settled. State the commit term (usually 1 year — Compute Savings Plans lock spend, not SKU), the effective discount, and the reconciliation rule (what happens if actual spend is under-commit).
5. **Size the on-demand bucket.** Cover the delta between P50 and P75 program-driven demand, plus the reactive-capacity buffer from exercise-01 section 4 (spot cannot serve reactive workloads with an SLO). State the target as a percentage of the total forecast; chapter 3's rule of thumb is 15-25%.
6. **Size the spot / preemptible bucket.** Cover the ablation-phase compute of every named training program and any batch-inference workload with a weak-latency SLO. For each workload placed on spot, name the checkpointing / restart-tolerance requirement it must satisfy (mod-404 chapter 6 material). Do not size spot into the baseline; size it as *"if available, we consume against these workloads; if not, we run them on on-demand."*
7. **Make the self-hosted call.** Explicit yes / no / hybrid, with arithmetic behind the call. Use chapter 3's rough break-even test — ~2500 GPU-hours per month per GPU purchased at published rates. Compute the number for your portfolio's demand and state the call. If yes, name the operational-team requirement (does the org have the peer platform track's infrastructure people already? If not, self-hosting is not defensible even if the arithmetic says it wins). If hybrid, name which workload classes go where and why.
8. **Cross-check the fleet size against MLPerf.** For at least one online-inference workload in the portfolio, quote the closest MLPerf Inference reference number (throughput per GPU under a standard latency-and-batch constraint) and translate the workload's forecast requests-per-second into an implied GPU count. Compare to the actual fleet size the org runs. If the org's implied throughput is >2× worse than the MLPerf reference, either the reference does not apply (name the reason) or there is a fleet-sizing bug the exercise-03 utilisation review will surface. For at least one training program, quote the closest MLPerf Training submission and cross-check the program brief's implied MFU. Chapter 3 was explicit that MLPerf is a *sanity check*, not a target.
9. **Compose the total mix.** As a percentage-of-forecast table: reserved / committed / on-demand / spot / self-hosted, each row summing to 100%. Flag the two chapter-3 red-flag mixes: >80% reserved (over-committed, will pay for idle GPUs at next hardware transition); >50% on-demand for orgs above ~$5M annual spend (leaving 30-50% on the table).
10. **Price the vendor-lock-in and exit costs.** For every multi-year commit in the reserved bucket, name three explicit line items:
    - **Hardware-generation risk.** Assume the next-generation NVIDIA hardware ships mid-commit; quote the "commit continues on stale hardware for X quarters" dollar impact.
    - **Exit cost.** If the org needs to leave this cloud at the end of the commit, what does egress / re-platforming / re-training cost? Include a specific egress-fee estimate if the workload is data-heavy.
    - **Discount realisation risk.** If actual demand comes in below the commit, what fraction of the commit is wasted?
    Publish as a 3-column table per multi-year commit.
11. **Author the worked-shape summary.** Chapter 3 gave a hypothetical $18M-portfolio shape as a template. Author the equivalent for your portfolio: one paragraph per bucket naming the dollar allocation, the demand-tier it covers, and the top single-sentence trade-off.
12. **State the re-plan triggers.** For each bucket, name the numeric trigger that forces a re-plan outside the annual cadence. Chapter 3 examples: "if actual reserved-utilisation drops below 70% for two consecutive quarters, flag for exchange"; "if next-generation hardware ships two quarters before we expected, re-price the commit's residual dollar-impact." Numeric per bucket.

## Deliverable

A recommendation document, 3-4 pages, plus a companion workbook. Sections:

- **Section 0 — Forecast input restatement.** One paragraph (step 1).
- **Section 1 — Cloud posture.** Single vs. multi vs. escape-hatch with named justification (step 2).
- **Section 2 — Bucket sizing.** One subsection per bucket (reserved / committed / on-demand / spot / self-hosted). Each subsection: which demand tier it covers, the dollar allocation, the effective discount and commit term where applicable, the citations back to exercise-01 (steps 3-7).
- **Section 3 — MLPerf cross-checks.** At least one training-program and one inference-workload cross-check with the reference number, the implied throughput, and the delta from the actual (step 8).
- **Section 4 — Total mix.** Percentage table with red-flag flag rows if any (step 9).
- **Section 5 — Vendor-lock-in and exit-cost register.** 3-column table per multi-year commit (step 10).
- **Section 6 — Worked-shape summary.** One paragraph per bucket (step 11).
- **Section 7 — Re-plan triggers.** Numeric per bucket (step 12).

Companion workbook: one sheet per bucket with per-workload allocation; one sheet with the total-mix percentage and red-flag flags; one sheet with the exit-cost arithmetic per commit.

## Starter guidance

- **Every bucket line ties to a demand tier from exercise-01.** Chapter 3 was explicit — reserved covers baseline plus P90 program-driven; committed covers P50 program-driven without SKU lock-in; on-demand covers the P50→P75 surge and reactive buffer; spot covers ablation and batch. If a bucket line in your document has no corresponding demand-tier citation, delete it or fix the citation.
- **The commit-term choice is the highest-leverage number in this document.** A one-year reserved commit at a smaller discount is often correct when hardware-generation risk is high; a three-year commit at a deeper discount is correct when the workload is truly steady-state and unlikely to migrate. Name the choice per commit; do not default to three years to maximise the headline discount.
- **Self-hosted is almost always "no" for orgs the reader will describe.** Chapter 3 gave the ~2500-GPU-hours-per-month-per-GPU heuristic and the operational-team gate. Unless your portfolio has both the utilisation and the platform team already in place, the correct answer is "not yet" and the correct next step is to name what would have to be true for the answer to change. Do not force a self-hosted line to make the document look sophisticated.
- **Multi-cloud is a risk overlay, not a cost play.** Chapter 3 was explicit. If your Section 1 justifies multi-cloud on cost grounds, either your primary cloud is being priced badly (fix the negotiation, not the architecture) or you are underestimating the multi-cloud overhead. Multi-cloud is defensible for sovereign / regulatory / resilience / negotiating-leverage reasons; not for headline unit-price reasons.
- **MLPerf is a sanity check.** Do not derive fleet size from MLPerf directly. Do use MLPerf to catch a 2× or worse throughput miss that would otherwise get quietly baked into a reserved commit for three years. Cite the specific MLPerf submission (round, submitter, model, hardware) — the number moves round-over-round.
- **Exit cost is not zero.** Every multi-year reserved commit is a lock-in bet. If the exit-cost line in Section 5 is a placeholder or reads "minimal," the section is not doing its work. Real egress-fee estimates from the cloud provider's public rate card are the floor.
- **3-4 pages plus the workbook is fine.** Chapter 3 is dense; the document is not. If it grows past five pages, either implementation detail has crept in (delete) or the bucket subsections are re-deriving demand (cite exercise-01 instead).

## Acceptance criteria

- Section 0 restates the exercise-01 forecast in one paragraph; any changes between exercises are called out.
- Section 1 names the cloud posture (single / multi / escape-hatch) with a specific chapter-3-cited justification and a named threshold reference.
- Section 2 has one subsection per bucket (reserved / committed / on-demand / spot / self-hosted). Each subsection cites the demand tier it covers and the exercise-01 section it reads from. Each multi-year commit has a term, a discount rate, and a payment structure named.
- Reserved bucket covers baseline plus P90 program-driven only; speculative demand and >12-month-out P50 programs are explicitly excluded.
- Self-hosted is a specific yes / no / hybrid call with the chapter-3 break-even arithmetic and the operational-team gate check shown.
- Section 3 cross-checks at least one inference workload against MLPerf Inference and at least one training program against MLPerf Training; each cross-check names the specific submission cited (round, submitter, model, hardware) and the delta from the actual implied throughput.
- Section 4's percentage table sums to 100% and flags any red-flag mix (>80% reserved, >50% on-demand at scale) with a stated remediation.
- Section 5 lists every multi-year commit with the three exit-cost columns (hardware-generation risk, exit cost, discount realisation risk); each entry has a specific dollar or percent number, not a placeholder.
- Section 6 has one paragraph per bucket naming allocation, demand tier, and top trade-off.
- Section 7 has one numeric re-plan trigger per bucket, each with a specific threshold and action.
- No entry in Section 2 sizes spot into baseline or reactive-capacity coverage.
- The workbook backs every Section 2, Section 4, and Section 5 number with visible arithmetic, not hardcoded totals.

## Stretch goals

- **Redo the sizing at P75 and P90 tiers.** Chapter 3 sized against P50 baseline plus P90 program-driven; the P75 and P90 versions of the same mix are the "if the forecast slides up" and "if everything on the roadmap actually lands" cases. The delta is the "how expensive is our commit-inflexibility?" number that exercise-04's sensitivity section reads.
- **Model the neocloud alternative.** Chapter 3 named CoreWeave / Lambda / RunPod / Together as a sixth mode. For your portfolio's reserved bucket, quote the neocloud alternative's per-hour rate and re-run the reserved-bucket dollar allocation. Where does the mix shift? Name the operational and lock-in trade-offs (fewer regions, less service surface, higher operational involvement).
- **Simulate the hardware-generation-transition scenario.** Assume the next NVIDIA generation ships six months into your three-year reserved commit. Walk the year-by-year dollar impact — how much of the reserved commit is on stale-generation hardware for how long, and what is the marginal cost of running new demand on the new generation while the old commit runs down. This is the arithmetic that turns "hardware-generation risk" from a bullet into a defended number.
- **Compare against a published capacity-portfolio disclosure.** Pick one publicly reported cloud commit or infrastructure investment (from a hyperscaler-customer case study or a public-company earnings-call disclosure — see `../resources.md`) and compare its shape to yours. Where does your mix diverge, and defend the divergence with a specific portfolio characteristic.
- **Take the draft to the finance partner (or procurement) for pre-review.** For each multi-year commit, ask them to push back on the term choice or the payment structure. Absorb the pushback. The commit-term choice is the number they will most often want to challenge; rehearsing the challenge before the real review saves face.
- **Draft the exec one-liner.** In one sentence at the top of the doc: what the mix is, why the shape, what the top risk is. "The recommended mix is 52% reserved / 15% committed / 22% on-demand / 8% spot / 3% self-hosted; the reserved-bucket concentration bets on H100 remaining competitive through mid-2027 and carries $3M of hardware-generation risk in years 2-3 which we accept in exchange for the 38% headline discount." That sentence is what the director reads before opening the workbook.
