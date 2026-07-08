# Merge, Split, Share: Deciding What the Portfolio Does With a Painful Cell

## Motivation

Chapters 2 and 3 tell you where the portfolio hurts. This chapter tells you what to *do* about it. Every `duplicated` and `forked` cell in the blueprint matrix gets one of three verdicts: **merge**, **split**, or **share behind a contract**. This chapter names each verdict precisely, gives the decision criteria that pick between them, and quantifies both the migration cost (engineering weeks) and the political cost (team-relationship debt) that the RFC in chapter 5 has to defend.

Learners often arrive at this chapter believing the answer is always *merge* — that consolidation is inherently virtuous. It is not. A merge that halves the substrate count but takes eighteen months and burns two team-lead relationships is not a win. A share-behind-a-contract that respects existing implementations often ships in a month with no relationship cost. Chapter 4 is deliberately three verdicts, not one.

## The three verdicts

### Merge

**Definition.** Two or more substrate implementations become one — same code, same operational owner, same on-call. All prior consumers migrate to the merged implementation.

**When it is the right call.** Merge when (a) the substrate is doing genuinely the same job in each place, (b) the divergence between implementations is not load-bearing to any team's SLO or business object, and (c) at least one team is willing to take on ownership of the merged implementation as a first-class job.

**Signature failure mode of merge.** *"We built the merged thing but no team migrated to it."* The merged substrate exists in a repo, nobody depends on it, the old implementations continue serving traffic, and a year later the merge shows up in the postmortem as *technical debt we voluntarily added*. This failure happens whenever the merge was authored without a **decommissioning schedule and a first-class owner**.

**What the RFC must include for a merge verdict.**

- The merged implementation's owner team (specific team name, not "the platform team" hand-wave).
- The migration schedule per consumer team (weeks 0-N, per team, with milestones).
- The decommissioning date of every old implementation. If no date is committed, the merge is decorative.
- The rollback plan for each consumer team if the merged implementation regresses their SLO.

### Split

**Definition.** A currently-shared or nominally-shared substrate is deliberately split into two or more independent implementations, each with a clean owner. The prior share is documented as *not-a-share* going forward.

**When it is the right call.** Split when (a) the shared substrate is a source of coupling that hurts more than it saves, (b) two consumers have genuinely divergent SLOs or semantics that no shared implementation can satisfy without becoming a compromise-of-compromises, or (c) the shared substrate is a *nominal* share — nobody really consumed it as a shared thing anyway — and the sharing pretence is causing coordination overhead with no benefit.

Split is often the right call when a merge target does not exist and the political cost of forcing one is worse than the coupling cost.

**Signature failure mode of split.** *"We split it and the two forks reconverged three months later because no one owned 'this is deliberately split'."* Without written rationale, the split looks like an oversight to whoever joins the org next; the new engineer tries to helpfully unify the two implementations and re-creates the failure mode. Split verdicts must ship with **a rationale document and an ADR** so the next architect knows *why* the two implementations are deliberately different.

**What the RFC must include for a split verdict.**

- The rationale: which SLO, semantic, or ownership property justifies the split, and what evidence supports the claim.
- The renamed / disambiguated names of the two split implementations so nobody future confuses them.
- The interfaces (if any) that remain shared. Splitting the data plane does not have to mean splitting the model registry.
- A **sunset trigger** — the condition under which the split should be re-examined. E.g., *"if both systems' SLOs move to the same tier, revisit the split."*

### Share behind a contract

**Definition.** The substrate has multiple implementations *or* a single implementation with multiple consumers, and instead of forcing a merge, the portfolio writes down the **shared contract** each consumer promises to honour — feature naming, freshness SLO, schema evolution rules, promotion state machine, whatever the substrate class demands.

**When it is the right call.** Share-behind-a-contract when (a) forcing a merge would burn more relationship capital than it recovers, (b) the coupling would benefit from *nomenclature and interface* alignment even if implementations continue to differ, or (c) the platform-team peer is willing to own the shared implementation *later* but cannot commit to it *now* — the contract is the way to keep future consolidation cheap without paying for it up front.

This is the most common right answer at portfolio scope, and the one learners under-use because it feels less impressive than a merge. It is not less impressive. It is often more mature.

**Signature failure mode of share-behind-a-contract.** *"The contract was written and nobody reads it."* This is the silent-contract-erosion failure from chapter 3. The RFC must ship the contract with (a) a review cadence, (b) a specific human who owns it, and (c) a lint or CI check that fails when a consumer violates it. Without those three, the contract is decorative.

**What the RFC must include for a share-behind-a-contract verdict.**

- The contract itself — a short, testable document. If it does not fit on one page, it is not a contract; it is a design.
- The consumer-side commitments per team.
- The review cadence (usually quarterly) and the review owner.
- The enforcement mechanism — a lint, a schema-registry check, a CI job, a manual review. Something that *fires* when the contract is violated.

## The decision framework

The decision framework in this chapter is a five-question checklist that resolves a painful cell into one of merge / split / share.

### Question 1 — Is the semantic overlap genuine?

**Test.** For two or more implementations of the same substrate cell — say, two `user_7d_purchase_count` feature definitions — are they computing the same business object from the same underlying events, or are they superficially named the same but semantically different?

If **not genuinely overlapping** — the split verdict, applied preemptively. Rename the implementations to disambiguate. Do not merge things that are not the same.

If **genuinely overlapping** — continue.

### Question 2 — Does at least one team's SLO or business object require the current divergence?

**Test.** Can any team point at a specific SLO (a p99 latency, a freshness bound, a regulatory requirement, a per-region data-residency constraint) or a specific business object (a business-model-specific transformation) that the merged implementation cannot satisfy?

If **yes** — the split verdict, defended. Ship the rationale doc and the sunset trigger.

If **no** — continue.

### Question 3 — Is there an owner for the merged implementation?

**Test.** Naming an owner is a specific-team commitment, not a hand-wave. Is there a team lead who is willing to take ownership as a first-class job and to defend the on-call rotation for it? Do they have the headcount and the roadmap slot?

If **no** — the share-behind-a-contract verdict. Write the contract, ship the enforcement, and revisit the merge in one to two quarters.

If **yes** — continue to question 4. A merge is on the table.

### Question 4 — Is the migration cost tolerable given the relationship debt?

**Test.** Estimate migration cost per consumer team in engineering weeks (see the migration-cost model below). Estimate the relationship debt — political cost — per consumer team (see the political-cost model below). Compare against the recovery: how much recurring pain does the merge save, per quarter, per team?

If migration cost or relationship debt is materially higher than the recovery — **share-behind-a-contract**, with a plan to revisit the merge in one to two quarters when the cost estimate improves.

If migration cost and relationship debt are manageable — continue to question 5.

### Question 5 — Are you willing to write the decommissioning date on the RFC?

**Test.** A merge that does not name specific dates for retiring the old implementations is a decorative merge. Are you, and the consumer teams, willing to put dates on the retirement of the old implementations?

If **no** — **share-behind-a-contract**. The org is not actually committed to the merge.

If **yes** — **merge**, with the decommissioning schedule in the RFC.

## The migration-cost model

The RFC has to defend a migration cost estimate that is not just optimistic self-belief. A defensible estimate rolls up four contributions:

1. **Per-team migration effort** — the engineering weeks per consumer team to swap the substrate. Bottom-up estimate: talk to the consumer team lead, ask *"if this had to be done, how long"* and multiply by 1.5. The 1.5 factor is the routine underestimate of migration work reported everywhere from Fowler's *Refactoring* preface to Google's monorepo-migration writeups; do not skip it.
2. **Backfill / historical-data effort** — if the merged substrate is a feature store or a data pipeline, historical training data has to be re-materialised so that models can be retrained on the merged view. This is often larger than the swap itself and easy to miss.
3. **Serving-side migration** — most substrates are consumed at inference time as well as at training time. Estimate the effort to migrate every serving path, including canary and rollback.
4. **Operational transition** — until the last consumer has migrated, both implementations run in parallel. Estimate the on-call load per week during the transition and the retirement cost of the old implementations.

Sum the contributions. Report engineering-weeks with error bars — the honest form is "6-10 weeks, most-likely 8" rather than "8 weeks" — and identify the two-to-three assumptions that dominate the error.

## The political-cost model

Migration cost is engineering weeks. **Political cost** is the relationship debt across teams that a decision incurs. Every merge decision has a political cost. The RFC is honest about it or it is not defensible.

Political cost has three components in practice:

- **Team ownership loss.** A team that used to own its own substrate — even a duplicated one — is losing a piece of its scope. This lands worse when the team's tech lead is early-Senior (needs the ownership for their promotion packet). It lands better when the team welcomes shedding the substrate.
- **Team roadmap disruption.** A migration eats a consumer team's quarter. The team's own roadmap slips. Their promotion packets slip. Their leadership commitments to their director slip. All of these are real costs.
- **Precedent risk.** Once the org merges one substrate, other staff engineers cite the precedent for their own merges. If the merge lands badly, the precedent is *"we tried consolidation and it hurt"* and every future consolidation gets harder. If it lands well, the precedent is a positive template.

A defensible RFC names, for each consumer team, which of the three components dominates and how the RFC mitigates it. Common mitigations:

- **Ownership-swap deal** — the losing team gains ownership of a different, load-bearing substrate. Zero-sum in scope, positive-sum in clarity.
- **Roadmap-slot deal** — the merge is scheduled in a quarter the consumer team already had slack, or the consumer team's next roadmap commitment slips *with executive cover*.
- **Migration-help commitment** — a specific team commits engineering-weeks to the migration so the consumer team's own headcount is not the bottleneck.

Missing this section entirely is the fastest way for chapter 6's architecture review to reject the RFC — the reviewers will point out that the technical case is fine but the political case is missing, and no team will actually execute against it.

## Two worked examples

### Example A — merge verdict, feature layer

**Cell.** Search and Home-feed teams each define 140 features with the same business semantics in separate Feast namespaces (`duplicated`). Drift observed on `user_7d_purchase_count`: 0.6% relative disagreement on a random day.

**Question 1** — semantic overlap is genuine (same events, same intent).
**Question 2** — no divergent SLO justifies the split; both teams need identical semantics.
**Question 3** — the ML platform team is willing to own a shared `ranking_features_v1` namespace.
**Question 4** — migration cost estimated at 4-6 weeks per team including backfill; relationship debt low because both team leads have flagged the drift.
**Question 5** — both teams commit to a 90-day decommissioning schedule.

**Verdict — merge.** The RFC ships with owner (platform team), schedule (Search team migrates weeks 1-4, Home-feed team migrates weeks 3-6, both old namespaces decommissioned at week 12), a rollback plan (fall back to the old namespace if p99 latency regresses), and a migration-help commitment (platform team allocates one engineer to each consumer team for the transition).

### Example B — share-behind-a-contract verdict, model registry

**Cell.** Search team uses MLflow in a private project; Home-feed team uses MLflow in a separate private project with different metadata schemas; Personalization team uses a homegrown S3 registry (`duplicated` across all three).

**Question 1** — semantic overlap is genuine (all three are the same class of substrate).
**Question 2** — no team has a genuinely divergent SLO; the divergence is historical.
**Question 3** — nobody wants to own the merged registry as a first-class job for the next six months. Platform team is willing to own it *later* but cannot commit now.
**Question 4** — moot; skip to the share.
**Question 5** — moot; the merge is not on the table this quarter.

**Verdict — share-behind-a-contract.** RFC ships a one-page contract: *every model version, wherever stored, must carry (training run ID, training data snapshot ID, evaluation report ID, hyperparameters, git SHA, framework version). Promotion to `production` is logged in the shared model-inventory sheet. The contract is reviewed quarterly by the ML platform lead.* Enforcement is a nightly job that checks the model-inventory sheet against each registry and pages the offending team. When the platform team is ready to own a merged registry, the RFC is revisited with the contract as the starting point — dramatically reducing the future migration cost.

## When "do nothing" is the right verdict

The three canonical verdicts are merge / split / share-behind-a-contract. There is a fourth, occasionally correct, verdict: **defer**.

Defer is the right call when (a) the pain is real but not urgent, (b) the migration cost is high and the relationship debt is high, and (c) an upcoming platform-team roadmap slot (mod-405) is expected to make the merge much cheaper.

A defer verdict is *not* the same as ignoring the failure. The RFC still ships. It ships with the diagnostic, the ranked decision options, the reason a defer is appropriate now, and the trigger for revisiting. The trigger — *"revisit when the ML platform team's paved-road feature store ships"* — is what keeps the deferral from silently becoming permanent.

Under-using the defer verdict is a common failure mode of newly-Staff engineers trying to prove they can drive consolidation. Sometimes the responsible answer is *not this quarter, and here is why*.

## The interaction with the four failure modes

Chapter 3 named four failure modes; chapter 4's verdict map is not uniform across them:

- **Pipeline-jungle** — usually a *merge* verdict, with the merged owner being either the Data Platform team or the highest-throughput consumer team. Rarely a share; the substrate is too load-bearing.
- **Duplicated-feature drift** — usually a *share-behind-a-contract* first, with a merge in the next quarter or two. Feature ownership is culturally hot and rushing a merge burns relationship capital. The contract fixes the drift; the merge follows when it is cheap.
- **Model-registry fragmentation** — almost always a *share-behind-a-contract*. Merging registries is expensive, disruptive, and rarely the highest-leverage move for the org. Getting a contract on lineage metadata gets 80% of the value at 20% of the cost.
- **Eval-metric conflicts** — usually a *split-with-arbitration-body*. The metrics remain team-specific; the arbitration body (a cross-team eval review) is added. Mod-406 owns the deeper conversation.

None of the above is a rule. They are the frequency patterns you will see if you audit five staff-scope portfolio decisions in ML orgs.

## Summary

- Every painful cell in the blueprint matrix gets one of three verdicts: **merge**, **split**, **share-behind-a-contract**. A fourth verdict — **defer** — is occasionally correct.
- The five-question decision framework: (1) is the semantic overlap genuine, (2) is any SLO or business object requiring the divergence, (3) does an owner exist for a merge, (4) is the migration + political cost tolerable, (5) will you name a decommissioning date. Different answers land at different verdicts.
- **Migration cost** rolls up per-team engineering weeks, backfill effort, serving-side migration, and operational transition. Report with error bars; the 1.5× underestimate factor is not optional.
- **Political cost** — team ownership loss, team roadmap disruption, precedent risk — is a first-class part of the RFC. Missing it is the fastest way to get rejected at architecture review.
- **Frequency patterns** — pipeline-jungle usually merges; feature drift usually shares first, merges later; registry fragmentation usually shares; eval conflict usually splits with an arbitration body added.
- **Defer** is a legitimate verdict when a platform-team roadmap slot will make the merge dramatically cheaper. Defer with a trigger, not silently.

The next chapter authors the multi-system RFC that carries the verdicts, the costs, and the migration plan — the artifact the staff engineer takes into the architecture review.
