# Hand-Off Contracts to Peer Specialist and Peer Platform Tracks

## Motivation

The failure mode this chapter exists to prevent is the same one at every scale: the Staff ML engineer hoards the work. There are always three good reasons to do it — *"the specialist isn't ready yet"*, *"the platform team hasn't shipped it"*, *"I've done this before and it will be faster if I do it"* — and each of them, cumulatively, keeps the engineer at Senior scope. The observable outcome is a Staff engineer who has become the org's single point of failure for six things they should have handed off two quarters ago.

The countermeasure is not to work less. It is to *write down the hand-off*. A written hand-off contract shifts the accountability, sets the API between the Staff engineer and the peer track, and makes the hand-off visible so the org can staff around it. This chapter defines the shape of that contract and demonstrates it against the specific peer tracks the Staff ML Engineer track routes work to.

## What a hand-off contract is

A hand-off contract is a **short document** — one to three pages — that names, for a specific piece of ML work:

1. **What the peer track owns end-to-end.** The verbs and objects the peer track is accountable for. When the CTO asks *"who owns X?"*, the answer is the peer track.
2. **What the Staff ML engineer owns as the consumer, program owner, or coordinator.** The verbs and objects on the Staff side of the API.
3. **The API between the two.** The specific artifacts, cadences, and decision rights that cross the boundary.
4. **What happens on conflict or ambiguity.** The escalation path.
5. **Explicit non-goals.** Work neither side will hold — usually because it belongs to a *third* track. Naming it kills scope drift.

A hand-off contract is not an RFC — it does not defend a specific technical choice. It is not an org chart — it does not name individuals. It is a durable API between two roles, written so it survives both individuals rotating.

The contract lives with the Staff engineer's other portfolio artifacts (mod-402 blueprint, mod-405 platform strategy, mod-410 roadmap) and is refreshed once per program cycle.

## Which peer tracks the Staff ML Engineer hands off to

The [Prerequisites](../../PREREQUISITES.md) doc and the [CURRICULUM](../../CURRICULUM.md) list the peer tracks by name. This chapter groups them into three classes, each with a distinct hand-off shape.

### Class 1 — Peer specialist tracks (IC depth inside a domain)

The Staff ML engineer owns the *program that coordinates the specialists*; each specialist owns *IC-level depth inside their domain*.

- `training-pipeline-engineer-learning` — distributed-training implementation depth (DDP, FSDP, NCCL tuning, checkpoint sharding, kernel-level ops).
- `fine-tuning-engineer-learning` — LoRA / QLoRA, DPO / RLHF, dataset engineering for fine-tunes.
- `nlp-engineer-learning`, `rag-engineer-learning`, `llm-application-developer-learning`, `applied-ai-engineer-learning` — applied-LLM and applied-ML specialisations.
- `model-evaluation-engineer-learning`, `ai-eval-engineer-learning` — evaluation-platform depth.

### Class 2 — Peer platform tracks (paved-road build)

The Staff ML engineer owns the *consumer contract*; the peer platform tracks own the *paved-road build*.

- `ai-infra-ml-platform-learning` — feature store, model registry, training platform, inference gateway.
- `ai-infra-mlops-learning` — CI/CD for ML, orchestration, drift monitoring, deploy tooling.
- `ai-infra-performance-learning` — cluster performance, kernel tuning, cross-model utilisation.

### Class 3 — Peer higher-level or governance tracks

The Staff ML engineer authors the *ML-side* program; the peer track owns *depth* in security or governance.

- `ai-infra-security-learning` (L35) — ML/AI security depth (data poisoning, model extraction, adversarial inputs, red-team ops).
- `ai-governance-analyst-learning`, `head-of-ai-governance-learning` — governance policy, regulator engagement.

The peer L40 EM (`ai-infra-team-lead-learning`) is a *fourth* class of peer and gets its own chapter (chapter 5).

## The shape of a hand-off contract

A working contract has six sections. This is the shape used across every worked example below and the shape learners produce in exercise-04.

### 1. Header

- Contract name: *"Hand-off contract: Staff ML Engineer ↔ Peer Specialist (Training Pipeline)"*
- Version and effective quarter.
- Program owner (Staff ML side).
- Peer track owner (specialist / platform / governance side).
- Peer L40 EM co-signer (chapter 5 explains why).

### 2. What the peer track owns end-to-end

Verbatim verbs plus objects. Read like a bulletpoint list from that track's job description. Example — training-pipeline peer specialist:

> - Owns the parallelism recipe *implementation* for programs the Staff ML program authorises.
> - Owns the checkpoint sharding, restart, and dataloader implementation.
> - Owns kernel-level tuning, NCCL configuration, and cluster-level distributed-training bug reproduction.
> - Owns the training-run runbook and the on-call rotation for training-run failures.

### 3. What the Staff ML engineer owns on their side of the API

Also verbatim. Example — Staff ML side of the same contract:

> - Owns the *strategy* — parallelism recipe *choice* for a given (model, cluster, dataset) shape.
> - Owns the *program* — which training runs the org sponsors, in what sequence, at what cluster-hour reserve.
> - Owns the executive framing of the program's cost and risk.
> - Owns the cross-team decision on shared vs. team-specific training substrate.

Note the pattern: **strategy vs. implementation**, **program vs. run**, **contract vs. build**. The Staff side is always the *why, when, and how much*; the peer side is always the *how*.

### 4. The API between the two

The specific artifacts and cadences. Example:

> - **RFC-in.** The Staff ML program authors the parallelism-recipe RFC and the failure-budget document. Peer specialist reviews and signs off.
> - **RFC-out.** The peer specialist authors the runbook, on-call playbook, and cluster configuration RFC. Staff ML program reviews for consistency with the program budget.
> - **Weekly cadence.** 30-minute sync on training-run status. Owned by the peer specialist; Staff ML program attends.
> - **Monthly cadence.** 60-minute program review — cluster-hour burn vs. budget, failure-mode incidence, program roadmap slip. Owned by Staff ML program; peer specialist attends.
> - **On escalation.** Multi-team training-run failure → peer specialist declares incident, Staff ML program takes IC role for cross-team coordination (mod-408).

### 5. Escalation path on ambiguity

Ambiguity is normal; unowned ambiguity is not. Example:

> - Ambiguous *implementation-vs-strategy* call → peer L40 EM (chapter 5).
> - Cost overrun that changes program scope → Director of ML.
> - Cross-team conflict on shared substrate → Staff ML program owns the RFC that resolves it; peer specialist scoping is bound by the RFC.

### 6. Non-goals

The most important section. Example:

> - Neither side owns *the model architecture itself* — that lives with the modelling team that runs the training program.
> - Neither side owns *dataset licensing and legal review* — that lives with `head-of-ai-governance`.
> - Neither side owns *cluster procurement* — that lives with `ai-infra-performance-learning` and Finance.

If those non-goals were unwritten, both sides would spend cycles arguing over them. Writing them down kills the argument in advance.

## Worked example 1 — hand-off to a peer specialist (training-pipeline)

The full contract above is the worked example for the peer specialist class. Two additional observations:

- **The Staff engineer keeps the strategy verb even if they know the implementation better.** The temptation to take an implementation call away from the specialist because *"I know FSDP better"* is the most common way this contract quietly dissolves. Every implementation call the Staff engineer holds is a call the peer specialist did not make — and did not learn to make.
- **The Staff engineer earns the right to escalate on strategy by writing the RFC.** The corollary: if the Staff engineer has *not* written the parallelism-recipe RFC, they do not get to override the specialist's implementation choice. The contract binds both sides; the Staff engineer is not exempt.

## Worked example 2 — hand-off to a peer platform track (ML platform / model registry)

**Peer track owns end-to-end.**

> - Owns the model-registry service — API, storage, RBAC, retention policy.
> - Owns model-registry SLOs and on-call.
> - Owns the model-registry roadmap and the platform team's build sequencing.
> - Owns capacity and cost for the model-registry service.

**Staff ML engineer owns on their side.**

> - Owns the *ML-side consumer contract* — what modelling teams commit to send to and read from the registry, on what schedule, with what promotion gates.
> - Owns the *build-vs-adopt call* — decides whether the org's model registry is built in-house, adopted from a paved-road platform, or extended from an OSS baseline (mod-405).
> - Owns the *gap RFC* when the paved-road registry does not meet a modelling team's need — sponsoring the RFC with the platform team.
> - Owns the *contribute-back RFC* when a modelling team ships a registry extension that should merge upstream.

**API.**

> - **Quarterly consumer-contract review** — Staff ML program owner and platform team's staff / EM. Refresh the promotion-gate contract, the SLI/SLO commitments, and the roadmap dependencies.
> - **RFC-in from Staff side** — gap RFCs, contribute-back RFCs. Reviewed by the platform track and sequenced into their roadmap.
> - **RFC-in from platform side** — platform-roadmap RFCs. Reviewed by Staff ML side for consumer impact and for the consumer contract's implications.
> - **On registry incident** — platform IC leads, Staff ML program owner represents modelling teams in cross-team command.

**Non-goals.**

> - The Staff ML engineer *does not* own the registry service's build.
> - The platform team *does not* own the promotion-gate policy — that is a modelling-org call codified in the consumer contract.

**Failure mode this contract prevents.** The most common ML-platform failure at growing orgs: modelling teams write their own model registries because *"the platform team doesn't ship what we need."* Once the Staff ML engineer authors the *gap RFC* against a concrete consumer-contract asks-list, the platform team can prioritise or explicitly defer. Either outcome is progress; unwritten disagreement is not.

## Worked example 3 — hand-off to a peer governance track (mod-409 setup)

**Peer track owns end-to-end** (`head-of-ai-governance` or `ai-governance-analyst`).

> - Owns the *policy* — the company's AI use policy, the regulator engagement plan, the legal review of model releases.
> - Owns the *classification* — the EU AI Act risk classification of each system, with legal sign-off.
> - Owns the *external* framing — regulator responses, external audits.

**Staff ML engineer owns on their side.**

> - Owns the *ML-side risk register* — the portfolio-scope AI risk register (NIST AI RMF + Gen-AI profile), the mapping from ML systems to policy categories, the pre-launch review gate (mod-409).
> - Owns *"is our ML system technically compliant?"* — the technical evaluation harness, the model-card and datasheet standards, the pre-launch gate.
> - Owns the *ML-side program authorship* — running the pre-launch review, feeding classified risk items into the peer governance track.

**Non-goals.**

> - The Staff ML engineer *does not* author policy or engage regulators directly.
> - The governance track *does not* author the technical evaluation harness or the model card standard.

The pattern here — *program authorship on this side, depth and policy on the other side* — is the same one used for `ai-infra-security-learning` (mod-409's red-team hand-off) and for the eval and experimentation platform hand-offs (mod-406).

## When the peer track does not exist yet

A frequent early-stage-company case: there is no peer specialist or peer platform team to hand off to. Two rules:

- **Do not pretend the peer track exists.** The hand-off contract has "TBD — peer track not staffed" on the peer side, and the Staff ML engineer explicitly holds the work until the peer is staffed.
- **Author the hand-off contract *in advance*.** The contract becomes the *hiring spec* for the peer role. When the peer track is staffed, the incumbent walks into an existing API. This is the second-most-common way Staff engineers create leverage after the multi-team RFC.

Chapter 5 (staff/EM partnership) covers the third case: no peer L40 EM yet.

## Common failure modes when writing hand-off contracts

- **The contract only names one side.** If it says "the peer specialist owns X" without naming what the Staff side keeps, the contract will not survive the first cross-team disagreement.
- **The contract names people instead of roles.** Individuals rotate. The contract should read the same after a rotation.
- **Non-goals are missing.** The whole point of a non-goals list is to kill scope arguments before they happen. A contract without non-goals is a scope argument waiting to happen.
- **The cadence section is aspirational.** If the weekly sync is not on real calendars, the contract is a wish-list. Real contracts have running cadences.
- **The Staff engineer keeps veto over implementation.** This voids the contract silently — the specialist will not accept accountability for a call they cannot make. The Staff engineer's veto is on the *RFC*, not on individual implementation calls.

## Summary

- The hand-off contract is a one-to-three-page document that names, for a specific piece of ML work, what the peer track owns end-to-end, what the Staff engineer owns on the consumer/program/coordinator side, the API between the two, the escalation path, and the explicit non-goals.
- Three classes of peer track that the Staff ML engineer routinely hands off to: peer specialist (IC depth in a domain), peer platform (paved-road build), peer governance or security (policy or depth outside ML engineering). Each has a distinct shape.
- The Staff engineer keeps the *strategy, program, and consumer contract*; the peer track owns the *implementation, build, or policy*.
- If the peer track is not staffed, the contract becomes the hiring spec for the peer role — do not pretend it exists.
- Common failure modes: naming only one side, naming individuals not roles, omitting non-goals, aspirational cadence, Staff-side implementation veto.

Exercise-04 puts this into practice by asking the learner to author a hand-off map from a real Staff ML program to three peer tracks. The next chapter turns to the fourth class of hand-off — the partnership contract with the peer L40 EM.
