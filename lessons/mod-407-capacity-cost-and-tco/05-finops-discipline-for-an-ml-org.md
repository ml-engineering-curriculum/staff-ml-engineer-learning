# FinOps Discipline for an ML Org — Inform, Optimise, Operate, Adapted

## Motivation

The demand forecast, capacity portfolio, and utilisation review from chapters 2-4 are point-in-time artifacts. The Staff engineer authors the first version this quarter; every artifact goes stale next quarter. Without an **operating discipline** — a named cadence for who does what, when, and how disagreements escalate — the artifacts drift into "documents nobody re-opens" within two quarters. The FinOps Foundation's *Cloud FinOps* (Storment & Fuller, O'Reilly, 2nd ed. 2023) names the operating model that every mature FinOps function runs against; this chapter walks it, adapts it to an ML org's specific patterns, and names the parts where ML economics require adaptation from the general FinOps advice.

The FinOps Foundation's operating model has three phases — **Inform, Optimise, Operate** — that are run continuously and in parallel across the org's workloads. Each phase names a set of activities, a set of artifacts, and a set of participants. This chapter walks each phase for the ML-org case, names the ML-specific patterns that general FinOps advice does not cover (token-based pricing, KV-cache economics, retraining as a recurring workload, GPU-generation transitions), and closes on the review cadence and escalation gates the Staff engineer runs.

## The three-phase model, in one paragraph

**Inform** is the visibility layer — the FinOps team's chargeback / showback substrate, the tags and labels that make cost attributable, the dashboards that make current state readable. Without Inform, nothing else works — the utilisation review from chapter 4 cannot even start. **Optimise** is the change layer — the migrations, the reservations, the workload consolidations that shift cost. This is where the Staff engineer's utilisation review docs live. **Operate** is the discipline layer — the review cadences, the policy defaults, the escalation gates that keep the org from drifting back into un-priced choices. The three phases run continuously; every workload sits somewhere in each phase at any given quarter.

## Inform — the visibility layer

The Inform phase's artifact is **cost attribution the ML org can act on**. Concretely, that means every dollar of GPU spend is tagged to a team, a workload, and a capacity mode, and the FinOps team's dashboards (or chargeback report) surface those breakdowns on demand.

**What is table-stakes.**

- **Tagging discipline.** Every GPU workload — training pod, inference fleet, batch pipeline — is tagged with `team`, `workload`, `environment`, `capacity_mode` at minimum. Untagged spend goes into a shared bucket that everyone treats as a leak. Enforcement is the peer FinOps team's job; the Staff engineer's job is to name untagged spend in the utilisation review and refuse to accept "we don't know who owns that" as an answer.
- **Chargeback vs. showback.** The org runs on **chargeback** (each team's budget is reduced by their actual spend) or **showback** (each team sees their spend but the central org pays the bill). Chargeback creates the sharpest incentives; showback creates fewer political fights and is a defensible starting point for an org that has never done either. See *Cloud FinOps* chapter 12 for the case-by-case.
- **Unit economics.** For every production workload the org can quote, in near-real time, a unit-economic number that leadership can compare against a target: **dollars per 1000 requests**, **dollars per 1000 tokens**, **dollars per trained parameter-token**. Chapter 6's TCO model reads from these; the utilisation review from chapter 4 compares against them.

**What is ML-specific.**

- **Retraining as a recurring workload.** General FinOps advice treats production workloads as steady-state. ML production workloads have a *retraining component* that runs on a cadence (nightly, weekly, monthly). Retraining is a training workload with an inference-workload cost-allocation shape — it should be attributed to the *model* whose inference it supports, not to a "training" catch-all bucket.
- **Feature-store and pipeline compute.** The feature store, the offline eval harness, the embedding-refresh pipelines are all ML-specific line items that general FinOps advice will not have a template for. Give each its own tag and its own reference number in the dashboards.
- **Hosted-API line items.** The org's hosted-API spend (OpenAI, Anthropic, Google, Bedrock) is opex on the cloud bill but *behaves like inference cost* — it should be reported alongside self-hosted inference for like-for-like comparison, not tucked into a "SaaS" catch-all.

**Inform-phase failure modes.**

- **Un-tagged shared substrate.** A shared feature store or shared inference gateway with no per-consumer allocation shows up as a mystery line item that nobody optimises. Fix by requiring proportional allocation at the point of use.
- **Chargeback theatre.** Charges get allocated but no team's budget actually gets reduced. Teams stop caring about the numbers. Escalate to leadership; either fix or admit the org is on showback.
- **Dashboards that only finance reads.** ML teams find the FinOps dashboards unusable and build their own. Fix by having the Staff engineer author the ML-team-facing dashboard *once* against the FinOps substrate, so there is one source of truth.

## Optimise — the change layer

The Optimise phase's artifact is **workload-level and portfolio-level moves that shift cost meaningfully**. Chapters 3 and 4 wrote against Optimise directly — the capacity-portfolio decision is an Optimise move, and every migration doc from chapter 4 is an Optimise artifact.

**What is table-stakes.**

- **Reserved-capacity commit management.** Commits are re-priced against actual utilisation quarterly. Under-utilised commits get flagged for exchange, extension, or explicit acceptance of the waste (sometimes correct, e.g., because the commit was made to hedge a hardware-generation risk).
- **Rightsizing.** Fleet sizes are re-evaluated against measured utilisation quarterly. If an inference fleet runs at 15% peak utilisation and 5% off-peak, rightsize.
- **Spot / preemptible adoption.** Workloads that can tolerate preemption are moved to spot as capacity allows. The MLOps side — checkpointing, restart tolerance — is peer-track work.
- **Managed-API ↔ self-hosted migration.** Chapter 4's #1 migration type. Reviewed against a break-even threshold.

**What is ML-specific.**

- **Token-based pricing arithmetic.** Managed-API spend is billed per token, not per hour. The Optimise conversation for a managed-API workload is different — it looks at *prompt length*, *response length*, *cache hit rate*, and the *model tier* (a smaller model can serve most of the traffic; a larger model handles the tail). This is not the FinOps Foundation's default vocabulary; the ML org has to author the vocabulary.
- **KV-cache economics.** Self-hosted LLM inference is throughput-limited by KV-cache memory, not by raw FLOPs. The PagedAttention / vLLM paper (Kwon et al. 2023) is the reference. Optimisation moves — model quantisation, cache-aware routing, prefix-caching — are peer-specialist work; the Staff engineer's job is to *price* the optimisation opportunity in the TCO model, not to implement it.
- **Retraining schedule tuning.** A model retrained weekly might do just as well retrained monthly. This is a *quality* question the ML team owns, but the *cost* question the Staff engineer surfaces. The Optimise conversation is a joint one — "does the org get any measurable business value from weekly-vs-monthly?"
- **Speculative decoding and draft-model economics.** Serving a large model with a small draft model in front (Leviathan et al. 2023 on speculative decoding) can meaningfully change per-request cost. Same pattern — peer-specialist implements, portfolio Staff prices.

**Optimise-phase failure modes.**

- **Chasing pennies.** The Optimise phase should be triaged by dollar impact. A migration that saves $5K/year is not worth the two-week engineering cost of the migration doc; a migration that saves $500K/year is.
- **Ignoring quality regressions.** An optimisation that saves 30% on cost while regressing tail-latency SLO or model-quality is not a win. The utilisation review's migration doc requires an SLO / quality impact section for exactly this reason.
- **Optimising the same workload every quarter.** After the second or third optimisation, further wins are usually inside a peer specialist's scope; portfolio-scope Optimise should move on to a different workload.

## Operate — the discipline layer

The Operate phase's artifact is **the cadence, the policies, and the escalation gates that keep Inform and Optimise running**. This is where the Staff engineer's operational overhead lives.

**The review cadence.**

- **Weekly (peer FinOps + Staff engineer, 30 minutes).** Cost anomalies, un-tagged spend, this-week's commit-utilisation numbers. Tactical.
- **Monthly (Staff engineer + finance partner, 60 minutes).** Chapter 2 demand-forecast delta, current-quarter spend vs. forecast, Optimise pipeline status. Reconciliation.
- **Quarterly (utilisation review body per chapter 4, 120 minutes).** Full cost / throughput / utilisation review, migration-doc commissioning, capacity-portfolio re-pricing. Strategic.
- **Annual (leadership review, 90 minutes).** TCO model refresh, three-year envelope refresh, headcount / capex justification. Board-level.

**Policy defaults.** The Operate phase names the org's defaults for the choices that come up repeatedly:

- **Default capacity mode for a new inference workload.** On-demand for first three months (understand traffic), then re-price into reserved / committed / spot.
- **Default capacity mode for a new training program.** Spot for ablation, reserved or on-demand for main run, depending on program size.
- **Default retraining cadence for a new production model.** Monthly, with a written argument required to justify anything more frequent.
- **Default hosted-API vs. self-hosted starting point.** Hosted-API until the workload hits ~$10K/month, then re-price.

Defaults do not need to be perfect; they need to be *written down* so that teams making a first-time decision have a defensible starting point that a review can then challenge.

**Escalation gates.**

- **Un-tagged spend crosses 10% of the ML org's total.** Escalate to leadership; this is a FinOps investment ask.
- **A workload's cost doubles quarter-over-quarter without a demand-forecast justification.** Escalate to the workload's L30 owner and the peer platform team.
- **A migration doc has been open for more than two quarters without adoption.** Escalate to the peer-EM per the mod-401 chapter 5 partnership contract.
- **The org spends more than 20% over the annual TCO envelope.** Escalate to the finance partner and CFO. This is the trigger for a re-plan.

## Where general FinOps advice needs adaptation for ML

The FinOps Foundation's framework is CPU-and-general-cloud-shaped. It ports well to ML with three specific adaptations the Staff engineer should name explicitly:

1. **The unit economics vocabulary is different.** General FinOps talks in dollars-per-request. ML talks in dollars-per-1000-tokens or dollars-per-trained-parameter-token. The Staff engineer authors the ML-specific unit-economic dictionary and hands it to the peer FinOps team so the dashboards can use the ML vocabulary.
2. **Retraining is a first-class workload class.** General FinOps treats compute as either "steady-state inference" or "batch training." Retraining is neither — it is a recurring training-shaped workload attached to an inference workload. Give it its own tag and its own dashboard line.
3. **Hardware-generation transitions dominate multi-year TCO.** General FinOps advice on reserved commits underweights the risk of a generation transition mid-commit. Chapter 3's capacity-portfolio math prices this; the Operate phase should name it explicitly at every quarterly review.

## The showback / chargeback dynamics for ML

An ML-org-specific note: chargeback makes some ML workloads politically difficult in a way general-purpose engineering workloads are not. A ranking-team's model that improves conversion by 0.3% may cost $2M/year to serve. If the ranking team is on chargeback, the CFO reads "$2M spent by ranking" and asks whether the ROI is worth it. If the ranking team is on showback, the same $2M is buried in the central bucket and the ROI conversation never happens. The Staff engineer's job is not to pick between them — that is a leadership call — but to *make the ROI conversation possible* by reporting the per-workload unit economics reliably. The peer FinOps team's substrate makes the reporting possible; the Staff engineer's TCO model (chapter 6) makes the reporting readable to leadership.

## Summary

- The FinOps Foundation's **Inform / Optimise / Operate** model is the operating discipline this module runs against. The three phases run continuously; every workload sits in each phase at any given quarter.
- **Inform** is the visibility layer — tagging, chargeback / showback, unit economics. Peer FinOps owns the substrate; the Staff engineer owns the ML-specific vocabulary that goes into it.
- **Optimise** is the change layer — the migration docs from chapter 4 and the capacity-portfolio moves from chapter 3. Chase dollar impact, not pennies.
- **Operate** is the discipline layer — the review cadence (weekly / monthly / quarterly / annual), the policy defaults, and the escalation gates.
- **Three adaptations** general FinOps needs for an ML org: the unit-economic vocabulary, retraining as a first-class workload class, hardware-generation transitions priced into commits.
- Chargeback vs. showback is a leadership call, not an engineering call — but the Staff engineer's job is to make the ROI conversation possible either way.

The next chapter walks the CFO-readable TCO model — the final artifact this module produces, which reads from every artifact this module has produced so far.
