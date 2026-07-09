# Vendor and Partnership Judgement at Staff Scope

## Motivation

By the middle of the 2020s no ML org of any consequence runs entirely on internally-built substrate. Every portfolio depends on at least one hyperscale cloud, at least one foundation-model API vendor (OpenAI, Anthropic, Google, Mistral, Meta releases via a hosting partner), often one or more specialty ML vendors (feature store, model registry, eval platform, inference gateway), and increasingly a mix of self-hosted open-weight and third-party hosted models. The mix of vendors, the shape of the commitments, and the exit path from each is a *staff-scope* call — too consequential for an L30 tech lead to make in isolation, too technical for the Director to make without staff-plus support.

This chapter names the vendor and partnership judgement calls that sit on the staff engineer's desk, the framework for making them defensibly, and the specific patterns that separate a well-run vendor mix from a lock-in trap or an under-committed everything-optional posture. Pairs with exercise-05. Grounds on Martin Fowler's utility-vs-strategic distinction on adopting external capabilities, Simon Wardley's mapping method for reasoning about where a capability sits on the commodity-vs-custom curve, and the mod-402 political-cost / mod-405 build-vs-adopt / mod-407 TCO models this chapter reuses.

## The four vendor-decision classes the staff engineer owns

At ML L40 the staff engineer is called into four recurring classes of vendor decision. Each has a different shape and each has its own failure modes:

**1. LLM API vendor mix.** *Which of OpenAI, Anthropic, Google, Mistral, Cohere, or open-weight-via-hosting-partner does the org standardise on for what workload?* This is the highest-frequency call. It hits every product surface that uses an LLM. It changes as the frontier vendors' capability leapfrogs. And it hits real cost — the difference between "Claude for everything" and a mixed policy is usually low-to-mid single-digit-percent of the ML org's variable spend.

**2. Foundation-model provider strategy.** *Do we self-host open-weight models (Llama, Mistral open releases, DeepSeek, Qwen) for a subset of workloads, and if so which?* Related to but distinct from (1) — the API-vendor call is about which frontier vendor to route to; the self-host call is about which workloads leave the API model entirely for a hosted-open-weight path. mod-407's TCO model lives inside this decision.

**3. Cloud provider trade-offs.** *AWS vs. GCP vs. Azure vs. specialty ML clouds (CoreWeave, Lambda, Together, Modal).* For most ML orgs the primary cloud is a company-wide decision — the ML org inherits it. But the ML org's *specialty compute* — training clusters, inference for open-weight models, batch offline eval — often runs on a secondary cloud, and choosing which is a staff-scope call.

**4. MLOps vendor selection.** *Weights & Biases vs. MLflow-on-Databricks vs. Neptune vs. Comet for experiment tracking; Feast vs. Tecton vs. Feathr for feature store; Ray-on-Anyscale vs. Modal vs. hosted-Kubeflow for orchestration; and so on.* These calls sit at the boundary of mod-405 (platform strategy). The peer platform teams own the build side; the staff ML engineer owns the ML-org consumer decision.

Each class has different reversibility, different cost, and different political ownership. The staff engineer's job is to name which class the decision is in, apply the right framework, and land the call with the appropriate exec framing from chapter 3.

## The reversibility axis — Type 1 vs. Type 2 doors, applied to vendors

Bezos's 1997 shareholder letter names Type 1 (irreversible) and Type 2 (reversible) decisions. Vendor decisions live on the same axis and the axis position sets the review bar:

- **Type 2 vendor decisions** — reversible in a quarter or two, cost of reversal is real but bounded, decision is a one-way door with a nearby exit. Most LLM API routing calls are Type 2. Most experiment-tracker calls are Type 2 (the historical data is annoying to migrate but not lost). These decisions should be made *quickly*, with reasonable analysis, and revisited on a set cadence.
- **Type 1 vendor decisions** — irreversible in practice within a two-year window. Deep integration with a specific vendor's SDK across the entire codebase, a multi-year committed spend contract with early-termination penalties, a data-egress trap where the vendor holds the historical dataset, a feature-store choice that shapes every ML pipeline. These decisions should be *slow*, exec-visible, and never made without a written exit contract.

The staff engineer's first move on any vendor decision is to name the class. Vendors will consistently frame their offering as Type 2 (*"you can leave any time"*) and the technical reality is often Type 1. Naming it correctly in the exec doc is a staff-level move.

## The utility-vs-strategic distinction

Martin Fowler's writings distinguish *utility* capabilities — things every ML org needs, differentiation is nil, price and reliability are the only axes — from *strategic* capabilities — things that carry the ML org's competitive shape, where investing internally protects a moat. Simon Wardley's mapping method formalises the same axis: capabilities move from *genesis* (novel, custom-built) to *custom* to *product* to *commodity* over time, and the right posture is different at each stage. <!-- needs-research: cite a specific Wardley-mapping post or a Fowler article that names the utility-vs-strategic distinction with an accessible URL; both are widely-cited but the exact reference needs a WebSearch pass. -->

Applied to ML vendor decisions:

- **Utility → prefer commodity vendor.** Object storage, experiment tracking, log aggregation, secrets management, most feature-store storage engines. Buy, do not build. The internal cost of maintaining a bespoke solution is not repaid by any strategic advantage. Optimise on reliability and price.
- **Strategic → prefer build or shape.** The ML capabilities that carry the product's differentiation — the specific ranking model architecture, the specific eval standard the org's product depends on, the proprietary feature layer that captures the org's data advantage. Do not outsource. The vendor's roadmap will not match the org's differentiation trajectory.
- **In-between (the interesting cases).** LLM APIs, foundation-model training compute, inference gateways. These are on the utility-strategic boundary and are exactly where staff-plus judgement is needed. They may be strategic for one product surface and utility for another inside the same portfolio.

Wardley's contribution is the observation that the same capability can be at *different points on the axis* for different sub-workloads. A ranker-specific eval harness is strategic for the ranking team; the generic offline-eval scaffolding underneath is utility. The staff engineer's job is to distinguish the two and buy the utility while investing in the strategic.

## The vendor-decision framework — five questions

For each vendor decision on the staff engineer's desk, five questions before the call:

**1. What is the reversibility class?** Type 1 or Type 2? If Type 1, what is the migration cost and what is the exit contract?

**2. Is this utility or strategic?** Is the capability a commodity the ML org should buy, a differentiator the ML org should build, or an in-between where the answer depends on the workload split?

**3. What does the total cost of ownership look like?** Not the sticker price — the mod-407 TCO model: variable spend + engineering hours to integrate + engineering hours to maintain the integration + exit cost + opportunity cost of the vendor's rate of feature release outpacing internal build. For most non-trivial vendor decisions, engineering hours dominate sticker price.

**4. What is the political shape?** Which teams have to change their workflow to adopt? Which peer platform team's roadmap is affected? Who benefits, who pays the migration cost, and does the migration cost land on the team that benefits?

**5. What is the exit contract?** For every vendor decision — even Type 2 — what does the exit look like? Data portability, contract termination clause, engineering hours to migrate, the fallback vendor. If the exit contract cannot be written, the decision is more Type 1 than the vendor is telling you.

The five questions map onto the exec one-pager structure from chapter 3: the reversibility class and utility-vs-strategic go in the *background* section, the TCO goes in the *trade-offs* section, the political shape goes in the *ask* section, and the exit contract lives in the appendix and gets referenced in the trade-offs.

## LLM API vendor mix — the concrete decision most staff ML engineers face weekly

Because LLM API vendor mix is the highest-frequency staff-scope vendor decision, it is worth walking a concrete pattern for it:

- **Segment by workload class.** Not every workload has the same vendor requirements. Latency-critical customer-facing generation, batch offline enrichment, agentic tool use, embedding, LLM-as-judge eval — each is a different vendor-quality curve.
- **Prefer routing over standardisation.** For most ML orgs, standardising on a single frontier vendor for every workload is dominated by a routing policy: primary vendor for latency-critical workloads where their quality leads, secondary vendors for workloads where price or specific-capability leads, self-hosted open-weight for workloads where predictability and cost-per-token justify the ops load. Route the workload to the vendor that dominates on its specific curve.
- **Standardise the interface, not the vendor.** The org's internal LLM-call wrapper (a thin abstraction over the vendor SDKs) is where the interface stability sits. Vendor-specific tuning lives behind the wrapper. The wrapper carries the observability, cost tracking, rate limiting, and the routing policy. Switching a workload from one vendor to another is a config change, not a code change.
- **Cap the maximum commitment per vendor.** Even for the primary vendor, keep the org's spend below the threshold where switching costs become prohibitive. A useful heuristic is *no single vendor accounts for more than 60-70% of the ML org's LLM spend*, tuned against the org's specific negotiation and reliability shape. The routing layer above enforces the cap in practice.
- **Renew intentionally.** The frontier vendor market moves fast — quality leader today is not necessarily quality leader in eighteen months. Set an explicit vendor-review cadence (quarterly is common) and treat it as first-class staff work, not as an ad-hoc "should we switch" conversation.

The routing-layer pattern is the single most repeatable vendor discipline in this space. It converts a Type 1 lock-in call ("we standardised on X") into a Type 2 workload-routing call, which is exactly where staff engineers want the reversibility axis for a rapidly-moving vendor market.

## Foundation-model provider — self-host or API

The self-host vs. API call is closely coupled to the LLM API vendor mix but has its own shape. The self-host case wins when:

- The workload has predictable volume (batch, background, high-throughput internal) where the amortised GPU cost beats the per-token API cost. mod-407's TCO model is what carries this defence.
- The workload has data-egress constraints (regulated data, contractual restrictions on sending to third-party APIs) that make an API vendor non-viable.
- The workload has latency requirements that a hosted API cannot meet even in principle.
- The workload requires model modifications (LoRAs, fine-tunes, custom quantisation) that API vendors do not expose.

The self-host case loses when:

- The workload is variable-volume and infrequent — the amortised GPU cost dominates.
- The org does not have the ops load to run a serving stack reliably. The vendor's SLA is the org's SLA in that case, and self-hosting is a downgrade.
- The frontier model's quality lead is large enough that the open-weight best-in-class is materially behind on the workload's task.

The most durable posture for a healthy ML org is *hybrid* — API vendors for the frontier-quality latency-sensitive workloads, self-hosted open-weight for predictable batch and cost-sensitive workloads, with the routing layer above deciding which is which. The staff engineer's job is to publish the routing policy, review it quarterly, and defend it in front of the CFO's team on cost and in front of the CTO on capability.

## Cloud-provider trade-offs

For ML orgs whose primary cloud is a company-wide inheritance, the ML-specific cloud choice is *where does specialty compute run*. Three patterns worth naming:

- **Same cloud as primary.** Simpler ops story. Higher variable cost (the primary cloud rarely wins on GPU pricing). Best for orgs where the ML-specific compute cost is small relative to the ops overhead of a second cloud.
- **Specialty cloud (CoreWeave, Lambda, Together, Modal, Runpod).** Better GPU pricing and often better GPU availability. Adds a second ops surface, a second security posture, a second on-call surface. Usually the right choice for training clusters and for open-weight inference at scale.
- **Multi-primary.** Two hyperscalers in parallel. Rare and expensive — usually a regulatory or vendor-negotiation posture rather than a technical one. Do not default to it.

The reversibility class matters here: cloud migrations are Type 1 within a two-year window. The staff engineer authors the ML-specific cloud call carefully, with the exit contract (how would we move back to the primary cloud if the specialty cloud's price advantage evaporates) written down in advance.

## MLOps vendor selection at the mod-405 boundary

MLOps vendors — experiment tracking, feature store, model registry, orchestration, serving — sit at the boundary with mod-405 platform strategy. The peer platform teams often *build* the paved-road version of these substrates; the staff ML engineer's job is to decide *what the ML org consumes off the paved road* and *what off-paved-road tools are worth adopting instead*.

The default posture — consume the paved road unless there is a defended reason not to — comes from mod-405. The reasons to adopt an off-paved-road vendor are the usual four:

- The paved road does not cover the capability at all, and the vendor does. This is the mod-405 gap-RFC case — usually the right response is to sponsor the platform team to build it, not to adopt the vendor. The vendor is a bridge.
- The paved road covers the capability but the vendor's version dominates enough on quality that the migration cost is worth it. Rare; the paved road's uniformity advantage usually wins.
- The paved road covers the capability at internal-cloud cost but the vendor's version is materially cheaper at the ML org's scale. Common for experiment tracking at heavy usage.
- The paved road has an unresolved reliability issue and the vendor is the workaround. Should be time-boxed and revisited.

For each off-paved-road vendor adoption, the staff engineer writes the decision as a peer-visible artifact — either a mod-405-style RFC or a lighter one-pager — and reviews with the peer platform team before signing the contract. Adopting a vendor without the platform team in the loop is a durable partnership failure.

## The exit contract — non-negotiable

Every vendor decision, of every reversibility class, ships with a written exit contract in the appendix of the decision doc. The contract answers:

- **Data portability.** Can we get our data out? In what format? At what cost? Over what timeline?
- **Contract termination.** Notice period, penalties, any lock-in provisions.
- **Engineering migration.** What internal engineering is needed to move off the vendor? How long? Who owns it?
- **Fallback.** Which vendor or internal capability do we migrate *to*? Do we have a written commitment from that fallback team?
- **Trigger.** Under what conditions do we invoke the exit? Cost thresholds, capability regressions, contract-renewal decisions, strategic pivots.

A vendor decision without a written exit contract is a Type 1 door dressed as a Type 2 door. The exec review should catch it if the technical review didn't.

## What Staff owns vs. what the peer L40 EM owns

Vendor decisions sit on a slightly different boundary than the artifacts in the earlier chapters — the EM boundary here overlaps with Finance and Legal.

- **Staff owns.** The technical shape of the decision — reversibility class, utility-vs-strategic, TCO model input, routing policy, exit-contract engineering, peer-platform-team implication.
- **EM owns.** The delivery framing — team capacity to adopt or migrate, on-call impact, engineering-hours estimate for integration and maintenance.
- **Finance owns.** Contract negotiation, spend commitment level, vendor management, MSA.
- **Legal owns.** Data-processing agreements, indemnification, IP terms, regulatory review.
- **All four.** Sign the vendor decision jointly. The staff engineer authors the technical recommendation; Finance and Legal are non-optional signatories on any material commitment.

The mod-401 chapter 5 partnership contract does not name Finance and Legal explicitly. The staff engineer adds them to the vendor-decision routing without needing to renegotiate the contract; they are additional signatories on a specific class of decision, not new boundary partners.

## Common failure modes on vendor judgement

- **The vendor's slides become the analysis.** The staff engineer's TCO model is a copy of the vendor's cost comparison. This is a debias failure and should be caught in review.
- **Standardisation without a routing layer.** The org standardises on a single frontier vendor and finds itself unable to move workloads when the market shifts. The routing layer converts this to a Type 2 posture.
- **Exit contract in the vendor's language.** *"You can leave any time"* absent a written internal exit plan. The internal exit plan is the exit contract.
- **Off-paved-road adoption without the platform team.** The ML org buys a vendor that competes with the peer platform team's roadmap. This is a partnership failure; the mod-405 partnership rules apply.
- **Under-commitment to the primary vendor.** The mirror image: the org spreads spend across so many vendors that none of them negotiate meaningfully, and the discipline of the routing layer is lost. Cap and floor both need to be intentional.

## Summary

- Four staff-scope vendor decision classes: **LLM API vendor mix**, **foundation-model provider strategy** (self-host vs. API), **cloud-provider trade-offs**, and **MLOps vendor selection**.
- **Reversibility class** (Type 1 / Type 2) is the first move on any vendor decision. Vendors frame everything as Type 2; the staff engineer names the actual class.
- **Utility-vs-strategic** (Fowler / Wardley): buy commodity, build differentiator, split the in-between along the workload axis.
- **Five-question framework**: reversibility class, utility vs. strategic, TCO, political shape, exit contract.
- **LLM vendor mix**: segment by workload, route rather than standardise, standardise the interface not the vendor, cap the per-vendor commitment, renew intentionally.
- **Foundation-model provider**: hybrid API + self-host; the routing layer decides. mod-407 TCO carries the defence.
- **Cloud provider**: usually inherited for primary; specialty compute is the staff-scope call. Cloud migrations are Type 1.
- **MLOps vendors** sit at the mod-405 boundary. Consume the paved road unless the RFC defends otherwise; do not adopt without the peer platform team in the loop.
- Every decision ships with a written **exit contract** — data portability, termination, engineering migration, fallback, trigger.
- The **Staff/EM boundary** expands to include **Finance** (contract, spend) and **Legal** (DPA, IP, regulatory). All sign on material commitments.

The next chapter is the module's capstone — sustaining the technical bar without sliding into IC-only work or into management-only work, staying in the archetype the learner picked in mod-401.
