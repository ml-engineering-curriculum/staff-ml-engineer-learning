# The Build-vs-Adopt Rubric: Seven Criteria, Weighted Scoring, and Two Worked Examples

## Motivation

The build-vs-adopt call is the single most common mis-made decision at Staff altitude in ML orgs. The failure modes it produces are among the most expensive on the entire ladder:

- **Build when adopt would have worked.** Two years of engineering time on an in-house feature store that Feast + a materialisation harness would have done in three months. The org's ranking model shipped a year late. Every downstream team's on-call inherits the homegrown quirks.
- **Adopt when build was right.** The inference gateway is a managed vendor; the vendor's canary primitives do not match the org's product SLOs; a year later the ML org's Staff engineer is writing shim code around the vendor to reproduce what a bespoke gateway would have shipped natively.
- **Contribute-back when fork was right.** The ML org's homegrown eval harness gets contributed back to the platform team; six months later the platform team has moved slowly and the harness now blocks the ML org from evolving. The contribute-back was a giveaway rather than a partnership.
- **Defer when the org needed the substrate now.** The registry-fragmentation failure mode from mod-402 chapter 3, seen a year later at the exec review.

This chapter's job is to give the Staff engineer a rubric that resists all four failure modes and produces a defensible decision doc — the artifact exercise-01 walks the learner through building for a real component.

## What the rubric is (and is not)

> A **build-vs-adopt rubric** is a weighted-scoring instrument scored against seven criteria — team size, differentiation, TCO, vendor lock-in, time-to-adoption, contribution ergonomics, exit cost — that produces one of four verdicts (build / adopt-managed / adopt-OSS / defer) for one platform component in one ML org at one point in time.

Three things fall out.

**One component at a time.** Do not try to write a single rubric that covers "the platform." The five components have different weightings and would produce different verdicts under any honest scoring; a blended rubric averages the answer to meaninglessness. Chapter 2's inventory sheet is the input; the rubric is applied per row.

**Scored, not narrated.** The verdict is defensible because the scoring is visible. A director reviewing the doc can push back on a specific criterion's score and re-derive the verdict. A narrative build-vs-adopt argument that hides its weights is not defensible under adversarial review — and platform-strategy decisions are always adversarially reviewed.

**A point-in-time call.** The rubric embeds a *re-visit trigger* — the change that would flip the verdict (org grows past 20 ML teams, vendor deprecates the feature you rely on, TCO passes 40% of the equivalent build cost, etc). A rubric without a re-visit trigger is a rubric that produces a fossil.

## The seven criteria

Read each criterion twice. The first pass builds the intuition; the second pass answers "for this specific component in this specific org, what score is defensible."

### 1. Team size and platform-org readiness

> Does the ML org have — or can it borrow from the peer platform team — enough engineering muscle to build and *operate* the component for the next 8 quarters?

Build requires two headcounts, minimum, sustained. One senior engineer to keep the code alive, one to carry the on-call. That is a floor, not a target. Below this floor, "build" produces an artefact nobody has the operational budget to maintain, and it degrades into a homegrown component nobody trusts by year two.

Score high for build if: the org already has a functioning platform-team peer with headcount to absorb the component, or the ML org itself is large enough (20+ engineers, three-plus teams) to sustain the two-engineer floor.

Score low for build if: the org has three-to-five ML engineers total, or the platform team is one person, or the on-call rotation is already at capacity.

### 2. Differentiation (is this component a competitive advantage?)

> If we build this, does the built version give us a capability our competitors do not have and would take material effort to replicate?

Fowler's ["Utility vs. Strategic"](https://martinfowler.com/bliki/UtilityVsStrategicDichotomy.html) distinction — utility software (build once, adopt widely) versus strategic software (differentiates you and is worth building) — is the load-bearing frame here. The feature store is almost always utility; two ML orgs' feature stores that both provide point-in-time-correct joins are indistinguishable to their consumers. The inference gateway is *sometimes* strategic — Meta's, Netflix's, Uber's are all custom because their product SLOs and canary requirements exceed what off-the-shelf gives. But most orgs' gateways are utility too.

Score high for build if: the component sits close to a product SLO no vendor supports, or embodies IP the org protects (specific ranking heuristics, distinctive routing logic).

Score low for build if: the component would be indistinguishable from Feast / MLflow / KServe if you built it — you'd be reinventing OSS with more bugs.

### 3. TCO across three-year horizon

> What does building cost — engineering + operations + opportunity — versus what does adopting cost — license + integration + ongoing vendor management?

TCO is where most rubrics get sloppy. The load-bearing move is to build a three-year cost table with the following rows:

- **Engineering.** Build: FTE-years × loaded cost. Adopt: integration FTE-months × loaded cost.
- **Operations.** Build: 0.5 FTE for on-call, prorated. Adopt: 0.1 FTE for vendor liaison, prorated.
- **License / cloud spend.** Build: your own compute for the substrate itself. Adopt: license or per-request pricing.
- **Opportunity cost.** Build: the ML features the two engineers do *not* build during the two years. Adopt: the vendor-imposed constraints you cannot fix.

Every line accounts. Fowler-tradition rule of thumb (adapted): the naive build estimate underestimates real build cost by 1.5-2×; the naive TCO underestimates operations by 3-5× in year 2-3. Multiply.

Score high for build if: three-year TCO is under 60% of the vendor's three-year TCO, *after* multipliers.

Score low for build if: the vendor's three-year TCO is under half the build's three-year TCO, *and* the vendor covers 80%+ of the differentiation.

### 4. Vendor lock-in and switching cost

> If the adopted vendor deprecates the feature, doubles the price, or is acquired, what is the cost of migrating off?

Lock-in has three dimensions:

- **Data lock-in.** Is the vendor's format proprietary, or is it open (Parquet, ONNX, MLflow schema)? Feast is low; Vertex AI Feature Store is moderate; some proprietary platforms are extreme.
- **API lock-in.** Is the vendor's API a superset of an OSS standard (KServe, MLflow), or is it wholly proprietary? Databricks is somewhere in the middle; SageMaker leans proprietary.
- **Operational lock-in.** Is the vendor stitched into your identity, network, and billing in ways that would take quarters to unstitch? True for most managed cloud offerings, which is fine — as long as the RFC names it.

Score high for build if: any vendor's failure-mode analysis produces switching-cost estimates over 4 engineer-months.

Score high for adopt if: switching cost is under 2 engineer-months (typical for OSS-based vendors) or the vendor's roadmap is open (Databricks / MLflow-compatible).

### 5. Time-to-adoption

> How quickly does the ML org need the component in production?

The rubric's most under-appreciated criterion. A "build" that takes six quarters is not competing with an adopt that takes six weeks — it is competing with what the ML org shipped in those five extra quarters. The opportunity cost is the whole business.

Score high for build if: the org's roadmap can tolerate 4+ quarters of build time (usually only true for foundational infrastructure, not for filling an immediate gap).

Score high for adopt if: the ML org needs the component in the next two quarters (nearly all real gap RFCs), or the vendor's time-to-first-production-use is measured in weeks.

Score high for defer if: the org can honestly do without the component for two more quarters and the pain-list from mod-402 has more urgent items.

### 6. Contribution ergonomics

> How possible is it to contribute upstream to an adopted OSS component when we hit a gap?

If we adopt Feast and hit a point-in-time-correctness edge-case in six months, can we author the fix and land it in a reasonable window? For OSS with an active maintainer body and a clean contribution guide (MLflow, Feast, KServe, Ray), the answer is yes on 6-12 week timelines. For OSS with slow maintainer response or an opaque design-review process (some ML frameworks in the transitional period), the answer is no, and the ergonomics penalty is high.

Score high for adopt-OSS if: the project has monthly-or-better release cadence, active PR review, and a design-review process the ML org's Staff engineer can navigate.

Score low for adopt-OSS if: the project has stalled maintainership or the design-review process is closed to non-maintainers. The org that adopts here is really adopting a codebase they will inevitably fork; the fork risk is what the criterion is warning against.

### 7. Exit cost

> If we build it and it turns out to be wrong, or the org shifts direction, how expensive is it to sunset?

This is the flip side of vendor lock-in. Bespoke code that four teams have consumed for a year is not cheap to sunset. Every consumer's migration is engineering cost, and the political cost — the team that owned the sunset gets branded — is real.

Score high for build if: the component is meaningfully modular from consumers (versioned API, adapters, clear migration story), so sunset can be a one-quarter deprecation.

Score low for build if: the component becomes org-load-bearing quickly and any sunset would require a full migration RFC of its own.

## The weighted-scoring template

For each criterion, score 1-5:

- 1: strongly favours adopt / defer.
- 3: neutral.
- 5: strongly favours build.

Multiply each criterion by a component-specific weight — you set the weights in the RFC, and *the weights are themselves reviewable*. Suggested starting weights per component:

| Criterion                     | Feature store | Model registry | Eval platform | Training platform | Inference gateway |
| ----------------------------- | ------------- | -------------- | ------------- | ----------------- | ----------------- |
| 1 Team size / readiness       | 3             | 2              | 2             | 3                 | 3                 |
| 2 Differentiation             | 1             | 1              | 2             | 3                 | 4                 |
| 3 TCO                         | 3             | 3              | 2             | 4                 | 3                 |
| 4 Vendor lock-in              | 2             | 3              | 2             | 4                 | 4                 |
| 5 Time-to-adoption            | 3             | 3              | 3             | 3                 | 3                 |
| 6 Contribution ergonomics     | 2             | 2              | 2             | 2                 | 2                 |
| 7 Exit cost                   | 2             | 2              | 2             | 3                 | 3                 |

Weights sum to ~16-22 per component; the raw score sums to a `build_score` in the 16-110 range. Do not report the number as the answer. Report it as *the check on your gut*. If your gut said "build" and the score says 40, either your weights are wrong (defensible — argue them) or your gut was wrong (also fine — that is what the rubric is for).

The rubric is *deliberately* not a hard threshold. Its output is a defensible ranking of the four verdicts (build / adopt-managed / adopt-OSS / defer) and the criteria most load-bearing for the ranking. The Staff engineer picks the top verdict, states the runner-up, and documents the score gap.

## Worked example 1 — small org adopting a managed feature store

**Setup.** A five-team ML org at a Series-C SaaS company, 15 ML engineers total, three production ML systems (recommendation, fraud scoring, churn prediction). Data warehouse is Snowflake; product infra is Kubernetes on GCP. No existing feature store. Real pain: fraud and recommendation teams have both built independent point-in-time joins in dbt, and they have started drifting.

**Component.** Feature store.

**Scoring.** Suggested weights above.

| Criterion                | Score (1-5) | Weight | Weighted | Reasoning                                                                        |
| ------------------------ | ----------- | ------ | -------- | -------------------------------------------------------------------------------- |
| 1 Team size / readiness  | 1           | 3      | 3        | 15 ML engineers, no platform team; the two-engineer floor for build not there.   |
| 2 Differentiation        | 1           | 1      | 1        | Point-in-time-correct joins are utility; no product differentiation here.        |
| 3 TCO                    | 2           | 3      | 6        | Build: ~2 FTE-years to first prod use. Adopt-Tecton: ~$300k/yr + 0.5 FTE.        |
| 4 Vendor lock-in         | 3           | 2      | 6        | Tecton is Feast-derived; switching to OSS Feast later is 2-3 engineer-months.    |
| 5 Time-to-adoption       | 1           | 3      | 3        | Need it in two quarters; build path is four quarters minimum.                    |
| 6 Contribution ergonomics| 2           | 2      | 4        | If we hit Tecton gaps, we can PR against Feast upstream; ergonomics fine.        |
| 7 Exit cost              | 2           | 2      | 4        | Sunsetting Tecton back to in-warehouse joins is well-scoped.                     |
| **Total**                |             |        | **27**   | Score of 27 out of a ceiling of ~80 — clearly favours adopt.                     |

**Verdict.** Adopt-managed (Tecton). Runner-up: adopt-OSS (Feast) if TCO forecasting shifts. Rejected: build (fails on team-size and differentiation), defer (drift pain is already real).

**Re-visit trigger.** If the org grows past 25 ML engineers or Tecton's pricing exceeds $500k/yr, revisit adopt-managed → adopt-OSS.

**Consumer contract implications.** Chapter 4's contract must include: which teams migrate in which order (fraud first, given the current drift), the freshness SLA per feature, the online-offline skew SLO, and the fallback path when the online layer is unavailable.

## Worked example 2 — large org building a bespoke inference gateway

**Setup.** A large consumer-app ML org, 200 ML engineers across 12 teams, 40+ production models serving 100k+ QPS peak. Existing platform team of 15. Current inference layer is a stitched mix of KServe on some models and per-team Flask on others. Canary rollout is manual, model versioning is fragmented (mod-402 registry-fragmentation, unresolved), and two recent SEV-2 incidents traced to the inability to fast-roll-back a bad model version.

**Component.** Inference gateway.

**Scoring.** Suggested weights above.

| Criterion                | Score (1-5) | Weight | Weighted | Reasoning                                                                        |
| ------------------------ | ----------- | ------ | -------- | -------------------------------------------------------------------------------- |
| 1 Team size / readiness  | 5           | 3      | 15       | Platform team of 15; ML org of 200; two-engineer floor easily met.               |
| 2 Differentiation        | 4           | 4      | 16       | Product-SLO-specific canary and rollback; no vendor primitive matches.           |
| 3 TCO                    | 3           | 3      | 9        | Build ~3 FTE-years; managed (Vertex Inference) at scale would exceed build.      |
| 4 Vendor lock-in         | 5           | 4      | 20       | Managed vendors would lock 40+ models to a proprietary routing layer.            |
| 5 Time-to-adoption       | 3           | 3      | 9        | Can tolerate 3 quarters; SEV-2 incidents are urgent but not blocking-day-one.    |
| 6 Contribution ergonomics| 2           | 2      | 4        | KServe upstream is active; contribution is possible but does not solve the gap.  |
| 7 Exit cost              | 3           | 3      | 9        | Modular build with per-model adapters keeps sunset achievable.                   |
| **Total**                |             |        | **82**   | Score of 82 out of a ceiling of ~110 — clearly favours build.                    |

**Verdict.** Build (bespoke gateway in front of KServe / vLLM serving runtimes). Runner-up: adopt-OSS (KServe + heavy customisation) if platform-team capacity slips. Rejected: adopt-managed (vendor lock-in and differentiation dominate), defer (SEV-2 tolerance is exhausted).

**Re-visit trigger.** If open-source gateway projects (KServe's control plane, an emerging LLM-gateway OSS) mature to cover the product-SLO gap, revisit at the two-year mark.

**Consumer contract implications.** Chapter 4's contract must include: canary progression schedule per model class, rollback SLO (seconds, not minutes), model-version pinning API, and per-model observability schema.

## Common failure modes when using the rubric

Four traps to catch in yourself when scoring:

- **Scoring for a hypothetical org, not the real one.** "In a year we'll have 30 ML engineers" is not a scorable claim in criterion 1. Score the current org. If the near-future headcount changes the verdict, put it in the re-visit trigger.
- **Handwaving TCO.** Every honest TCO exercise finds build costs 1.5-2× the naive estimate. If your TCO shows build at exactly parity with adopt, redo it with the multipliers on. If it still shows parity, adopt.
- **Ignoring exit cost when scoring build.** The build column tends to under-price sunset. The Sculley et al. (*Hidden Technical Debt*, 2015) CACE point applies at the platform level too: once four teams depend on your homegrown component, changing anything changes everything.
- **Treating the rubric as the answer.** The rubric is decision support, not decision automation. If the score says build and every senior engineer in the room disagrees, the weights are wrong. Argue the weights until the rubric and the room agree. That argument *is* the RFC's Alternatives Considered section.

## The artifact this chapter produces

The **build-vs-adopt decision doc** — one component, scored on the seven criteria with defended weights, verdict named, runner-up named, re-visit trigger stated, consumer-contract implications summarised. Exercise-01 walks the learner through authoring this for a real component from the chapter-2 inventory. Two-to-four pages plus the scoring table.

The doc has a specific structure the exercise enforces: (1) component and current state, (2) the four verdicts under consideration, (3) the rubric scoring table with weights and reasoning, (4) the verdict and runner-up, (5) the re-visit trigger, (6) the consumer-contract implications carried into chapter 4. Nothing else. Do not try to shove the whole platform strategy into one doc.

## Summary

- The **build-vs-adopt rubric** is a weighted-scoring instrument on seven criteria — team size, differentiation, TCO, vendor lock-in, time-to-adoption, contribution ergonomics, exit cost — that produces a defensible verdict per component.
- The verdict is *defensible*, not automatic. The scoring is visible; the weights are reviewable; the re-visit trigger acknowledges that the answer will change.
- Component weights differ: feature store leans utility (adopt), inference gateway can lean strategic (build), and the rest sit between.
- The two worked examples — small org adopting managed Tecton for the feature store, large org building a bespoke inference gateway — show the rubric producing opposite verdicts on defensible weights.
- The decision doc is a two-to-four page artifact bound tight to one component; every component gets its own doc.

The next chapter walks the paved-road consumer contract — what the ML org guarantees the platform team, and what the platform team guarantees back — the contract every verdict from this chapter lands in.
