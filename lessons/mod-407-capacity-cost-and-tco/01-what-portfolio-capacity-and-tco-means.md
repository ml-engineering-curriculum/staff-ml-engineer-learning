# From a Program Budget to a Portfolio Budget: What Capacity, Cost, and TCO Mean at Staff Scope

## Motivation

The Staff engineer who defended a single training program's cluster-hour budget in [`mod-403 chapter 3`](../mod-403-foundation-model-training-programs/03-cluster-hour-budget-defense.md) is now being asked a different, harder question — *"what should the ML org's whole GPU spend look like next year, across all training and all inference, and why is that number defensible?"* — usually by someone whose job title includes CFO, VP Engineering, or Director of ML. The vocabulary is different (opex vs. capex, commit vs. on-demand, amortisation, per-request cost), the audience is different (finance partner, not a program review body), the units are different (dollars per year across a portfolio, not accelerator-hours across a program), and the failure mode is different — a mis-defended portfolio TCO number can quietly cap the ML org's headcount and roadmap for a year, whereas a mis-defended program budget only threatens one program.

This chapter names what **GPU capacity, cost, and TCO at portfolio scope** is as a scope of work, in enough detail that the rest of the module — the demand forecast in chapter 2, the capacity portfolio in chapter 3, the utilisation and migration review in chapter 4, the FinOps discipline in chapter 5, and the CFO-readable TCO model in chapter 6 — has a stable definition to build on. It re-uses the L30 → L40 altitude framing from [`mod-401`](../mod-401-staff-ml-role-scope/) and specialises it to *the finance-partner-facing artifact* that the ML org's Staff engineer owns.

## What "capacity, cost, and TCO at portfolio scope" means here

A **portfolio TCO model** in this module means **a written, defensible model of what the ML org's compute-and-serving fleet will cost over a rolling one-to-three-year horizon, across every training program and every inference workload the portfolio owns, with the trade-offs between capacity-purchase modes and the sensitivities to model-size and hardware-generation shifts made explicit**. Concretely, the artifact is a workbook (usually a spreadsheet with a text companion doc, 5-8 pages) that a finance partner can read to answer three questions:

1. *"What is the ML org going to spend next fiscal year, and on what?"*
2. *"What levers do we have if the number needs to come down 20%?"*
3. *"How sensitive is the number to things we cannot control — a new hardware generation, a model-size shift, a demand surge on inference?"*

The model is *not*:

- **A single-program cluster-hour budget.** That is [`mod-403 chapter 3`](../mod-403-foundation-model-training-programs/03-cluster-hour-budget-defense.md) territory — one program, accelerator-hours, MFU, restart buffer. The portfolio TCO model *aggregates* many such program budgets, adds inference, and translates to dollars over a year.
- **The org's cloud bill or the FinOps team's chargeback report.** Those are input data, not the artifact. Finance and the platform team already produce them; the Staff engineer's job is to *interpret and forecast against them*, not to re-derive them.
- **A cost-optimisation project.** Cost optimisation is a *use* of the model (chapter 4 walks the utilisation review that surfaces migration targets); the model itself is a planning artifact, not an optimisation output.
- **The ML platform's build-vs-adopt call.** That is [`mod-405`](../mod-405-ml-platform-strategy/) territory. Portfolio TCO informs the build-vs-adopt call by pricing the alternatives; it does not decide them.

The one-to-three-year horizon matters. A quarter is too short — reserved-capacity commits, hardware-generation transitions, and inference-demand curves all play out over a year or more. Five years is too long — model shapes, hardware performance, and cloud pricing all move too fast to forecast with a straight face. The Staff-scope artifact lives at the same horizon a finance partner uses for headcount planning: **fiscal-year detail, three-year envelope**.

## Why single-program framing breaks at portfolio scope

Every failure mode this module addresses — overbuying reserved capacity because a program slipped, paying on-demand rates for workloads that are steady-state, running vLLM at 12% utilisation because nobody consolidated, quoting a per-request cost that ignores KV-cache economics — has the same root cause: **the org priced its ML workloads program-by-program, each defended on its own merits, and no one summed the portfolio**.

Three concrete illustrations of the break:

**1. Two training programs both plan the same 512-H100 pod and the org buys 1024.** Team A's program brief (mod-403 shape) reserves a 512-H100 pod for a Q2 pretrain. Team B's brief reserves 512 for a Q3 fine-tune. Each brief is locally correct. Neither brief acknowledges that Team A's ablation phase and Team B's warmup overlap, that the total wall-clock across both programs sums to well under a year's continuous use of a single 512-pod, or that the org just committed to a three-year reservation on 1024 H100s at ~$1.50/hour effective — call it $13M over three years — half of which will sit idle. Nobody made a bad decision; nobody made the portfolio decision.

**2. Inference eats the training budget silently.** The training programs get the review scrutiny because they are big and lumpy. Inference — a fleet of vLLM instances serving a chat product plus a batch-embedding pipeline plus a hosted-API line-item on the cloud bill — grows steadily in the background. A year in, inference is 60% of the ML compute bill and nobody has ever sat down to look at whether the 40% margin between vLLM's throughput and the throughput leadership was told about is being paid for by the CFO. Sculley's *"only a tiny fraction of the code is the model itself"* observation from mod-402 has a dollar analogue: *only a tiny fraction of the compute spend is training the model.*

**3. The CFO asks "what does a request cost?" and gets four answers.** The ranking team answers in GPU-seconds per request. The chat team answers in dollars per 1000 tokens. The batch-embedding team answers in dollars per document. The fraud team answers in "we don't know, we're on shared infra." Each answer is defensible in its own domain. None of them add up to a portfolio number. Chapter 6 walks the arithmetic that reconciles them into a **per-request cost target** the CFO can compare year-over-year and against the industry.

The pattern is the same in each case. At program-scope the artifact was a defensible cluster-hour budget for one program. At portfolio-scope the artifact is a defensible *dollars-per-fiscal-year* forecast for the whole ML compute footprint, plus the sensitivity to the levers the org actually controls.

## The Staff engineer as the ML-side finance partner

Every ML org above a certain size (usually the org that has authored a training program bigger than $500K) picks up a **finance partner** — a person from the FP&A or engineering-finance organisation whose job is to reconcile the ML org's forecast with the company's overall opex plan. The Staff engineer owning the portfolio TCO model is that partner's *counterpart on the engineering side*. The relationship is closely analogous to the peer-EM partnership from [`mod-401 chapter 5`](../mod-401-staff-ml-role-scope/): a two-way contract where each side owns half of the shared artifact.

**The finance partner owns.** The pricing sheets (contracted cloud rates, private-committed-use discounts, GPU depreciation schedules if the org self-hosts), the amortisation policy (capex vs. opex treatment of self-hosted hardware, three-year vs. five-year straight-line), the reconciliation to the company plan of record, the CFO's review calendar, and the audit trail that finance needs for external reporting.

**The Staff ML engineer owns.** The demand forecast (chapter 2), the capacity-portfolio recommendation (chapter 3), the utilisation and migration diagnostics (chapter 4), the FinOps operating cadence for the ML org (chapter 5), and the TCO model that reconciles the engineering numbers with the finance numbers (chapter 6). Above all, the Staff engineer owns **defending the number in front of leadership** when the CFO asks a hard question — because the finance partner does not know why the H100-to-H200 transition matters, and the Staff engineer does.

If the Staff engineer is quietly writing spreadsheet macros for the finance partner or, worse, negotiating cloud contracts directly with the vendor, the partnership has collapsed — either the finance partner is not staffed, or the Staff engineer has drifted into a role that peer platform / FinOps / procurement already own. Chapter 5 walks the operating-model discipline that keeps the partnership crisp.

## What Staff owns vs. what the peer platform / FinOps team owns

The temptation, especially for a newly-Staff engineer whose org lacks a FinOps function, is to become the FinOps team by default — write the chargeback reports, build the cost dashboards, argue the cloud commits with procurement. That is a scope error at least three times over.

**Peer FinOps team (or the FinOps function inside the platform team) owns.** The cost-allocation substrate: tags, labels, chargeback pipelines, showback dashboards. The Cloud Provider commit negotiation. The unit-economics reporting to finance. The FinOps Foundation calls this the **Inform** phase of the operating model (chapter 5 walks the whole model); it is table-stakes tooling and it is not staff-plus IC work.

**Peer platform tracks — [`ai-infra-ml-platform-learning`](https://github.com/ai-infra-curriculum/ai-infra-ml-platform-learning), [`ai-infra-performance-learning`](https://github.com/ai-infra-curriculum/ai-infra-performance-learning), [`ai-infra-mlops-learning`](https://github.com/ai-infra-curriculum/ai-infra-mlops-learning)** — own the **paved-road implementations** whose efficiency the portfolio model prices. The vLLM serving stack, the Triton config, the training cluster's InfiniBand fabric, the object-storage backend for checkpoints. If chapter 4's utilisation review says "the org's vLLM instances are running at 12% throughput of the MLPerf reference number for this model shape, we have a $2M/year fleet-sizing bug", the *fix* belongs to the performance track; the *diagnosis and the migration doc* are Staff-scope work.

**Peer specialist tracks own** the workload-level cost optimisation. If a specific model needs quantisation, kernel fusion, KV-cache paging, or a smaller draft model for speculative decoding, that call belongs to the domain specialist — not to the portfolio Staff engineer.

**Staff at portfolio scope owns.** The demand forecast. The capacity-portfolio recommendation (the reserved / on-demand / spot / multi-cloud / self-hosted mix). The **workload-migration decisions** across managed-API / self-hosted / hardware-generation boundaries — that is, the calls that materially change what the peer platform team ships and what the peer specialist team optimises. The FinOps operating cadence *inside* the ML org (review meetings, escalation gates, per-team showback discipline). And the TCO model that lets the CFO decide whether to fund the next fiscal year at the level the ML org asked for.

Chapter 3's capacity-portfolio decision framework and chapter 4's migration doc are the two artifacts most easily confused with peer-team work. Both live at Staff scope because both require *arbitrating between programs and workloads that different teams own* — a mandate no L30 tech-lead and no platform engineer has been given.

## The "am I doing portfolio TCO work?" heuristic

A one-sentence sibling to the mod-401, mod-402, and mod-404 heuristics:

> **If my document forecasts what the whole ML compute footprint will cost the org over the next one-to-three fiscal years and defends the number against the alternate calls (buy less, buy differently, migrate workloads, defer programs), I am doing portfolio TCO work. If it prices one program, I am doing mod-403 chapter 3 work. If it optimises one workload, I am doing peer-specialist or peer-platform work.**

Two failure modes to catch in yourself:

- **Drifting down into program budgeting.** *"Program X's MFU is coming in at 32% and we should re-scope."* That is a mod-403 conversation. The portfolio TCO model *cites* program budgets — if the model's assumed MFU disagrees with a program brief, the model re-opens the conversation with that program's Staff engineer; it does not re-litigate the program brief.
- **Drifting sideways into FinOps tooling.** *"Let me build a Grafana dashboard that shows per-team GPU spend."* Legitimate work but not Staff-scope work. If the org lacks the tooling, the correct move is to name the gap in the TCO model's assumptions section ("the org lacks per-team showback, so team allocations in this doc are estimated") and to write the RFC that funds the FinOps function.

## Summary

- Portfolio **capacity, cost, and TCO** is a written, defensible model of the ML org's compute-and-serving spend over a one-to-three-year horizon, across every training program and every inference workload, with capacity-mode trade-offs and sensitivities made explicit.
- The audience is the finance partner and the CFO. The primary hand-off contract is the peer-finance-partner partnership, closely analogous to the peer-EM partnership from mod-401 chapter 5.
- The model *aggregates* program budgets and *interprets* the cloud bill; it does not re-derive either. It sits above single-program scope (mod-403 chapter 3) and above the peer platform team's paved-road implementation.
- Staff owns the demand forecast, the capacity-portfolio recommendation, the migration decisions, the FinOps operating cadence inside the ML org, and the CFO-readable TCO model. Peer FinOps owns the allocation substrate; peer platform owns the paved-road implementation; peer specialist tracks own workload-level optimisation.
- The signature failure is a **portfolio TCO model that reads like a program budget** — dense on MFU arithmetic, silent on inference, silent on hardware-generation risk, silent on the levers finance actually cares about.

The next chapter walks the portfolio demand forecast — the first artifact the module produces, the input to every other artifact, and the number the finance partner uses to reconcile against the company plan.
