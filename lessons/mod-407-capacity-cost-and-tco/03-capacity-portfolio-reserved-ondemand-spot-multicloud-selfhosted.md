# The Capacity Portfolio: Reserved, Committed-Use, On-Demand, Spot, Multi-Cloud, Self-Hosted

## Motivation

The chapter 2 demand forecast tells the org *how much* compute it will need, quarter by quarter, at three confidence tiers. It does not tell the org *how to buy it*. That decision — the **capacity portfolio** — is the highest-leverage financial call the Staff engineer makes, because the same fiscal-year demand can be met at wildly different unit prices depending on the mix of capacity buckets chosen. A poorly chosen mix that over-commits on reserved capacity leaves the org paying for idle GPUs for years; a poorly chosen mix that under-commits pays on-demand rates for baseline demand that could have been reserved at a 30-50% discount. Both mistakes are quiet, both are expensive, and both are visible only in retrospect if the model does not price them up front.

This chapter walks the five (six, depending how you count) capacity buckets, the trade-offs each carries, the sizing rules of thumb that decide how much of the forecast goes into each bucket, and the vendor-lock-in and exit-cost math that keeps a reserved commit from becoming a strategic mistake. It pairs with [`exercise-02`](exercises/exercise-02-capacity-portfolio-selection.md), which walks a real capacity-portfolio selection against the chapter-2 forecast.

## The five capacity buckets

The buckets, in order of decreasing commitment length and (usually) decreasing unit price:

1. **Reserved capacity** — a multi-year committed reservation of a specific hardware SKU on a specific cloud (AWS Capacity Reservations plus Savings Plans / Reserved Instances, Google Cloud Committed Use Discounts, Azure Reserved VM Instances). Typical discount vs. on-demand: **~30-60% depending on term and payment structure**. Typical minimum: one year, often three. See official AWS Savings Plans docs, Google Cloud CUD docs, and Azure Reserved VM Instances docs for the current programme structure.
2. **Committed-use / prepaid** — a shorter-horizon committed spend without a specific-instance lock (AWS Compute Savings Plans, Google Cloud flexible CUDs). Discount is smaller than reserved (~15-30%) but the org can use the commit against any matching instance type. Useful when the org knows it will spend *X dollars on GPU* but does not yet know exactly which SKU. <!-- needs-research: verify current AWS Compute Savings Plans discount ranges before quoting to leadership. -->
3. **On-demand** — the standard hourly rate for the SKU. No commitment, no discount. This is the reference price everything else is quoted against.
4. **Spot / preemptible** — surplus capacity offered at a large discount (**~60-90% vs. on-demand** on major clouds) with the property that the cloud can reclaim it with short notice (from seconds to two minutes, depending on the provider). See AWS EC2 Spot, Google Compute Engine Spot VMs, and Azure Spot VMs docs. GPU spot supply is thinner than CPU spot supply and much more variable — availability of 8xH100 spot instances is not something an org can plan a baseline against, though it is something an org can price a training program's ablation phase against.
5. **Self-hosted** — the org buys GPUs (usually a colocation-hosted or on-prem cluster), pays capex up front, and runs it. Effective hourly cost is the depreciation of hardware plus power plus cooling plus operations. For H100-class hardware operated well, published TCO analyses put the effective hourly cost in the **~$1-2/hour range** compared to ~$3-5/hour public-cloud on-demand, though the up-front capex and the operational overhead are both real. <!-- needs-research: cite specific recent industry TCO writeups (Coreweave, Lambda, Meta's public infrastructure posts) for a defensible cost-per-hour range. -->
6. **Multi-cloud** — deliberately splitting the fleet across two or more clouds. Not a "cheap-vs-expensive" axis but a **risk-vs-lock-in** axis: multi-cloud raises the per-unit cost (fewer commits earn deeper discounts on any single provider) in exchange for negotiating leverage, sovereign requirements, and resilience against a single-provider capacity crunch. Multi-cloud is orthogonal to the other five buckets; the org still picks reserved / on-demand / spot within each cloud.

Neocloud providers (CoreWeave, Lambda, RunPod, Together) sit between "public cloud on-demand" and "self-hosted" in the price stack, often offering better per-hour rates than the hyperscalers on GPU SKUs in exchange for narrower service surface and higher operational involvement. Treat them as a sixth mode with its own trade-offs; do not fold them into reserved or on-demand silently.

## Trade-offs, per bucket

### Reserved capacity

**Wins.** Deepest discount. Guaranteed availability of the specific SKU (important for pretraining programs that need contiguous multi-week blocks of a specific pod topology). Predictable line item for the CFO.

**Costs.** Multi-year lock-in to a specific hardware generation. If H200 or B200 ships six months into a three-year H100 reservation, the org is paying for hardware that is one generation stale for the remainder of the commit. Cloud providers do offer some flexibility (instance conversion, exchange programmes on some SKUs), but the exchange is rarely one-for-one and usually costs the discount. If demand drops (a training program cancelled, a product deprecated), the reservation is still charged.

**Sizing rule of thumb.** Reserved should cover **the baseline demand from chapter 2 at the P50 confidence tier**, plus the highest-confidence program-driven demand (P90 programs — approved, staffed, brief-in-flight). Do not reserve against speculative demand. Do not reserve against P50 program-driven if the program's completion is a year out.

### Committed-use / Compute Savings Plans

**Wins.** Meaningful discount without SKU lock-in. Good for demand that is *definitely coming* but whose exact SKU shape is not settled — for example, "we will spend $X on GPU next year but the H100/H200 mix depends on a supplier delivery date."

**Costs.** Smaller discount. Requires the commit-plan-vs-actual reconciliation the FinOps team runs monthly; overspend is fine, underspend is a wasted commit.

**Sizing rule of thumb.** Cover the P50 program-driven demand that has not been placed in the reserved bucket. This is the "we know we will spend it, we do not know exactly on what" tier.

### On-demand

**Wins.** No commit. Full flexibility. The bucket that soaks up P75 / P90 program-driven demand that may or may not materialise.

**Costs.** Highest per-hour price on the price curve. If baseline is being served on-demand, the org is leaving 30-50% on the table.

**Sizing rule of thumb.** Cover the P75 tier of program-driven demand *minus* what is already in reserved and committed. Also covers the reactive-capacity buffer for speculative demand, since spot is too flaky for reactive work that has an SLO.

### Spot / preemptible

**Wins.** Deepest per-hour discount on the market. Preemption is not a problem for workloads that checkpoint at short intervals and can restart cleanly — ablations, hyperparameter sweeps, embedding-refresh batch jobs, offline eval sweeps.

**Costs.** Variable availability. Cannot be planned against as steady capacity. Cannot serve online-inference at any reasonable SLO. Requires the training job to actually implement preemption-friendly checkpointing (Young/Daly-style interval math from [`mod-404 chapter 6`](../mod-404-distributed-training-systems-at-scale/) applies).

**Sizing rule of thumb.** Use spot for the **ablation-phase compute** of every training program and for **batch-inference workloads** with weak-latency SLOs. Do not size spot into the baseline; size it as *"if it is available, we will consume it against these workloads, at these hours; if not, we run the workloads on on-demand"*. Multi-region spot with automatic-failover tooling (SkyPilot-style — see Yang et al. 2023) improves the effective availability, but the org still cannot promise SLOs against spot.

### Self-hosted

**Wins.** Lowest steady-state cost per GPU-hour for workloads with predictable, high-utilisation demand. Full control over the topology, the network, and the storage. Not subject to hyperscaler pricing changes. Deep tax advantages in some jurisdictions (capex depreciation vs. opex).

**Costs.** Very large up-front capex. Long delivery lead times (H100/H200 supply is constrained). Real operational overhead — the org now runs a datacenter (or pays a colo provider to run one). The operational load is exactly the peer-platform-track scope from [`ai-infra-ml-platform-learning`](https://github.com/ai-infra-curriculum/ai-infra-ml-platform-learning), and if the ML org does not have that platform capability *before* it commits to self-hosting, the self-hosted TCO will be much worse than the spreadsheet suggested. The Meta and Microsoft public writeups on ML fleet economics document the operational scope this represents; the Staff engineer authoring the capacity portfolio should read those before recommending self-hosted. <!-- needs-research: cite specific Meta ("Applied ML at Facebook" HPCA 2018 has been used before; look for more recent Meta AI infrastructure posts) and Microsoft posts on fleet economics. -->

**Sizing rule of thumb.** Self-hosted is defensible for orgs whose baseline plus P90 program-driven demand fills a pod at high utilisation *and* who already have the platform team to run it. A rough break-even at published rates is **~2500 GPU-hours per month per GPU purchased** — below that, cloud reserved wins; above that, self-hosted starts to. <!-- needs-research: verify break-even math against published Coreweave / Lambda cost sheets and against Meta / Microsoft posts. --> If the org is under 2000-3000 GPUs of steady demand, the answer is almost always "not yet." Above 10,000 GPUs of steady demand, hyperscaler-only is almost always leaving 30-50% on the table.

### Multi-cloud

**Wins.** Negotiating leverage — every cloud knows the org can move workloads if pricing gets uncompetitive. Sovereign / regulatory compliance for orgs that need EU + US + APAC presence. Resilience against a single-provider capacity crunch (Q4 2022–Q3 2023 GPU squeeze is the recent example).

**Costs.** Higher per-unit cost — reserved commits are per-cloud, so splitting a $10M annual spend across two clouds usually earns less discount than $10M on one cloud. Higher operational overhead — every ML platform primitive (training cluster, model registry, feature store) now needs a multi-cloud story. Cross-cloud egress fees are non-trivial for data-heavy workloads.

**Sizing rule of thumb.** Multi-cloud below a certain scale is a tax without a real benefit. A rough threshold: if the org's annual ML compute spend is below **~$5M**, single-cloud is almost always right. Between $5M and $50M, multi-cloud is a judgement call driven by sovereign requirements and negotiating posture. Above $50M, multi-cloud is usually necessary — if only to protect against a single-provider capacity crunch. <!-- needs-research: verify these threshold ranges against publicly reported ML org spend data. -->

## Sizing the mix — the target percentages

A defensible first-pass mix for a mature ML org, with the caveat that every number here shifts based on the org's specific demand curve:

- **Reserved: 40-60% of forecast dollars.** Covers baseline plus high-confidence program-driven.
- **Committed-use: 10-20% of forecast dollars.** Covers medium-confidence program-driven without SKU lock-in.
- **On-demand: 15-25% of forecast dollars.** Covers P75 program-driven surge, new-launch inference, and reactive-capacity buffer.
- **Spot: 5-15% of forecast dollars.** Covers ablation-phase training, batch inference, offline eval sweeps.
- **Self-hosted: 0% for orgs under ~2500 GPU-hours-per-month-per-GPU utilisation of baseline demand; 20-50% for orgs above.**
- **Multi-cloud: pure risk-management overlay; not a percentage of the mix but a decision about splitting the above across providers.**

Two red-flag mixes worth naming:

- **>80% reserved** — the org is over-committed and will pay for idle GPUs when the next hardware generation ships. Almost always a sign that a previous reservation was renewed on autopilot.
- **>50% on-demand for orgs above ~$5M annual spend** — the org is leaving 30-50% on the table by not committing against clearly-forecastable baseline demand. Almost always a sign that the finance partner and the ML org have not sat down together.

## MLPerf as a size input

The [MLPerf](https://mlcommons.org/) benchmarks (Training and Inference) are the closest thing the industry has to a standardised throughput reference. Two ways the portfolio decision uses them:

**Sizing training capacity.** MLPerf Training results publish the wall-clock and hardware-count for reference models trained on reference datasets. When a program's brief quotes a target of *N* accelerator-hours, the portfolio Staff engineer can cross-check the number against the closest MLPerf submission (same model class, same or larger scale) — if the program's implied throughput is materially worse than the MLPerf reference, either the program's MFU planning number is optimistic or the reference does not apply. Do not use MLPerf as a target ("we should hit the MLPerf number"); use it as a *sanity check* ("we should be within 2× of it, and if we are not, we should know why").

**Sizing inference capacity.** MLPerf Inference publishes throughput results for reference models under standard latency-and-batch constraints. When the demand forecast says an inference workload will serve *M* requests per second, MLPerf-derived throughput per GPU is the reference number that turns *M* into a GPU count. The peer performance track — [`ai-infra-performance-learning`](https://github.com/ai-infra-curriculum/ai-infra-performance-learning) — owns the deeper throughput-tuning work. The portfolio TCO model owns the *aggregation* across workloads and the *reconciliation* to the demand forecast.

## Vendor lock-in and exit costs

Every reserved-capacity commit is a lock-in bet — the org bets that (a) the hardware generation the commit covers will remain competitive for the commit's duration, and (b) the cloud provider will not raise the *effective* cost through egress fees, storage-tier changes, or feature-gating of newer services. Both bets fail routinely on multi-year horizons.

The portfolio model prices lock-in explicitly:

- **Hardware-generation risk.** Assume the next-generation hardware ships at some point in the commit period and quote the "commit continues on stale hardware for X quarters" line-item. NVIDIA's public roadmap cadence has been ~2-year steps (V100 2017, A100 2020, H100 2022, H200 2023, B200 2024); a three-year commit almost always spans a generation boundary. <!-- needs-research: verify current NVIDIA roadmap cadence. -->
- **Exit cost.** If the org needs to leave the cloud provider at the end of the commit (or before), what does it cost to migrate the workloads? Data egress fees, retraining models against a different accelerator, re-plumbing the training platform. This is not zero and it should be a named line item in the three-year envelope.
- **Contribute-back leverage.** Some vendor contracts include price adjustments if the org commits to public co-marketing (a case study, a joint benchmark submission). The Staff engineer does not negotiate these but should surface them to the finance partner and the platform team's leadership.

The general framing is: **treat reserved capacity as a hedge against price risk, not as free money**. If the discount looks too good to be true (e.g., an offered 70% discount on a three-year H100 commit right after B200 announcement), the vendor is pricing the imminent generation transition into the offer and the org is being invited to bear the depreciation risk. Read the offer twice.

## Combining the buckets — a worked shape

For a hypothetical mid-sized ML org with an $18M annual forecast at P50 (baseline $8M, program-driven $8M, speculative $2M), a defensible mix might be:

- **Reserved.** $9M (50% of forecast). Covers $8M baseline plus the highest-confidence $1M program-driven. Three-year commit on H100-generation hardware, with an explicit "we accept two quarters of stale-generation risk at the end of the commit" clause in the model.
- **Committed-use / Savings Plans.** $3M (17%). Covers $3M of medium-confidence program-driven without SKU lock-in.
- **On-demand.** $4M (22%). Covers the remaining $4M of program-driven under P75 tier, plus the reactive-capacity buffer.
- **Spot.** $1.5M (8%). Ablation phases of two named training programs; a batch-inference workload.
- **Self-hosted.** $0. Org is below the utilisation threshold to justify the operational overhead.
- **Multi-cloud.** Single-cloud primary with a $500K/quarter "escape hatch" reservation on a secondary cloud for sovereign compliance.

The specific numbers matter less than the shape of the argument — every line item has a demand-tier justification, a lock-in analysis, and a reference to the chapter-2 forecast. Chapter 6's TCO model rolls these into the CFO-facing summary.

## Summary

- The capacity portfolio has five (six with neocloud) buckets: **reserved, committed-use, on-demand, spot, self-hosted**, with **multi-cloud** as an orthogonal risk overlay.
- Reserved covers baseline plus high-confidence programs; on-demand covers surge and reactive; spot covers ablation and batch; self-hosted requires the operational platform to already exist; multi-cloud is a risk/negotiation play, not a cost play.
- Use **MLPerf** as a sanity check for both training and inference sizing; do not use it as a target and do not derive first-principles throughput without cross-referencing.
- Price **hardware-generation risk** and **exit cost** into every multi-year commit. A three-year commit almost always spans an NVIDIA generation boundary.
- The mix percentages are directional — the discipline is *every bucket ties to a demand tier from chapter 2, every commit has a stated exit cost, and no bucket is >80% by dollar*.

The next chapter walks the cost / throughput / utilisation review that surfaces the workload-migration decisions the capacity portfolio is bought against.
