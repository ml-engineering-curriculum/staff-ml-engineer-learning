# What "Responsible AI at Portfolio Scope" Actually Means

## Motivation

Every ML org has a story about the launch that shipped with a fair-lens issue caught by the press, or the model whose training data provenance nobody could produce when a regulator asked. In small orgs those stories get fixed by a single senior engineer writing a checklist and pinning it in Slack. At the portfolio scope this module lives at — three to five production ML systems, each with its own team — the checklist stops working. The teams drift. The checklist gets forked. The governance analyst who is supposed to sign off on launches receives four different launch packets in four different shapes and cannot review any of them fairly. The org's answer to "are your ML systems responsible" becomes "we think so, on a good day, if you ask the right person."

This module is the Staff ML engineer's answer to that. It re-uses the L30 → L40 altitude framing from [`mod-401`](../mod-401-staff-ml-role-scope/) and specialises it to the **Right Hand** archetype's centre of gravity — the artifact set that keeps the ML org's responsible-AI posture *legible*, *auditable*, and *portable across teams* without collapsing back onto one person's Slack pin.

## What "portfolio-scope responsible AI" means here

**Responsible AI at portfolio scope** in this module means:

> The set of written artifacts and standing processes — the risk register, the EU AI Act classification table, the org-standard model card and datasheet, the pre-launch review gate and its signoff routing, the red-team cadence — that let a governance analyst (or a regulator, or a new head of AI) understand the current risk posture of three to five ML systems from one place, and that let a new ML team ship an in-portfolio system through a known door instead of inventing their own.

Two properties matter to that definition.

**Portfolio scope.** Every system in the portfolio has a row in the register. Every system has a tier in the EU AI Act classification. Every system uses the same model-card template. The point is not that every system has the *same risks* — a search-ranking model and a credit-underwriting model have vastly different risks — but that every system's risks are described in the *same vocabulary*, so a leader can compare them and a policy owner can decide where to spend attention next.

**Written and standing.** The artifacts live in the org's docs system, not in a person's head. The pre-launch gate is a documented step in the launch runbook, not a favour a senior engineer does when they have time. The red-team cadence is on a calendar with a name against each session, not a "we should do that soon". A responsible-AI posture that only survives because one engineer remembers to check things is not a posture at all; it is a single point of failure the exercises in this module are designed to remove.

## What this module is *not*

Naming what the module does not own is as important as what it does.

- **It is not a substitute for the peer governance track.** [`ai-governance-analyst`](https://github.com/ai-infra-curriculum/ai-governance-analyst) owns the *policy* — what the org's position on model transparency, fairness, human oversight, and use-case restrictions actually is. This module owns the ML-side implementation of that policy: the risk register the analyst reviews, the model cards the analyst reads, the gate the analyst signs. If you find yourself writing the policy, you have crossed a track boundary; see the ownership section below.
- **It is not a substitute for the peer security track.** [`ai-infra-security-learning`](https://github.com/ai-infra-curriculum/ai-infra-security-learning) owns the *deep* red-team, adversarial-ML, model-supply-chain, and prompt-injection work. This module owns the *contract* to that track — the cadence, the artifacts, the escalation. Chapter 6 is that contract.
- **It is not a legal opinion.** The EU AI Act tier in chapter 3 is the Staff engineer's *engineering read* of where the system probably sits so the org can plan the compliance work. Legal owns the binding classification. When they disagree with your engineering read, legal wins; the escalation to legal is a documented step, not an argument.
- **It is not a single-model risk assessment.** A single-model risk assessment is a Senior tech-lead artifact and belongs to the L30 track. This module composes those single-model assessments into a portfolio view, adds the cross-system consistency the analysts need, and adds the gate that ships them into a decision.

## Staff owns vs. governance owns vs. security owns

The temptation, for a newly-Staff engineer facing an under-staffed governance function, is to write the policy themselves — to become the head of AI governance in practice. That is a scope error even when the vacuum is real, and it hurts the promotion packet. The right split, in one sentence per role:

**Staff ML engineer owns.**

- The **risk register** as an ML-engineering artifact (chapter 2, exercise-01) — one row per system, columns aligned to NIST AI RMF functions.
- The **EU AI Act engineering classification** per system (chapter 3, exercise-02) — the Staff engineer's read, marked as such, with the legal-review escalation named.
- The **org-standard model card and datasheet template** (chapter 4, exercise-03) — the shape every launch packet uses so the analyst can review any team's without adaptation.
- The **pre-launch review gate as a workflow** (chapter 5, exercise-04) — what artifacts are required, who signs off, where the block-vs-flag line sits, how escalation to the head of AI governance works.
- The **red-team cadence and the delegation contract** (chapter 6) to the peer security track.

**Governance analyst / head-of-ai-governance own.**

- The **policy itself** — what the org's position on transparency, fairness, human oversight, and use-case restrictions is.
- The **binding sign-off** on high-risk systems and on any system whose classification is contested.
- **Regulator-facing correspondence** — filings, responses to inquiries, external disclosures.
- The **policy exceptions process** — when a system ships under a documented waiver.

**Security partner (from [`ai-infra-security-learning`](https://github.com/ai-infra-curriculum/ai-infra-security-learning)) owns.**

- The **deep adversarial evaluation** — prompt injection, jailbreak, data-extraction, model-inversion, and the taxonomy work that catalogues them.
- The **model supply chain** — training-data lineage attestation, model-weight provenance, dependency scanning of the serving stack.
- The **runtime security controls** — output filtering, rate limiting, red-team-informed guardrails at inference.
- The **incident response** for security-classified events, in partnership with the mod-408 incident-command process.

A well-run portfolio produces artifacts these three roles read, sign, or escalate on. Chapters 2 through 6 build one artifact each; the exercises land them.

## The Right Hand archetype's centre of gravity

Mod-401 chapter 2 named the four staff-plus IC archetypes from Larson's *Staff Engineer* (2021): Tech Lead, Architect, Solver, and Right Hand. Responsible-AI-at-portfolio-scope is the **Right Hand** archetype's centre of gravity in the ML engineering ladder. The Right Hand is the engineer who partners with leadership on the non-negotiable work — the work that is not glamorous, that leadership will not always sponsor loudly, and that the org would rather someone else do. Governance work is exactly that. The signature failure mode of the Right Hand is *becoming leadership's assistant* — losing the engineering altitude and turning into a compliance-adjacent PM. The fix, throughout this module, is to stay attached to the ML-engineering artifacts: risk register rows have model-behaviour evidence attached; model cards cite eval results; the gate blocks on measurable criteria.

If your Cut 2 from mod-401 exercise-02 was Right Hand, this module and mod-410 are your target artifacts. If your Cut 2 was Architect or Tech Lead, you still author these artifacts at least once — every Staff ML engineer in an org with regulatory exposure owns at least one turn at the gate — but you will lean more on partnering with the analyst and less on drafting policy language yourself.

## The "am I doing portfolio RAI work?" heuristic

A sibling to the mod-401 and mod-402 heuristics:

> **If my document describes the responsible-AI posture of the org's ML portfolio in a way a governance analyst or a regulator can read without a per-team walk-through, I am doing portfolio RAI work. If it describes one model's fairness eval, I am doing single-model risk-assessment work — an L30 artifact. If it describes the policy itself, I am doing governance-analyst work — a peer-track artifact.**

The two failure modes to catch in yourself:

- **Drifting down into single-model risk assessment.** A beautifully done fairness analysis of one ranking model is not a portfolio artifact. The equivalent portfolio artifact is *the section of the register that says which of the five systems has been fairness-evaluated, against what protected attributes, with what result, and when it will be re-evaluated*. The Staff engineer authors the second; the L30 engineer or the model-evaluation-engineer specialist authors the first.
- **Drifting up into policy authorship.** *"We should not ship any model that scores below 0.9 demographic-parity ratio."* is a policy statement. It belongs to the governance analyst, ratified by the head of AI governance. A Staff engineer who writes it themselves has taken the analyst's job and has usually pushed a number they cannot politically defend when it becomes inconvenient.

## The five artifacts this module produces

The staff-scope deliverables of mod-409, one-to-one with the four exercises plus the delegation contract in chapter 6:

1. **The portfolio AI risk register** (exercise-01, chapter 2). One row per system, columns aligned to NIST AI RMF Govern / Map / Measure / Manage and the Gen-AI profile overlay. The single view of the ML org's risk posture.
2. **The EU AI Act classification table** (exercise-02, chapter 3). One row per system, engineering-read tier, obligations that flow, and the legal-review escalation for contested tiers.
3. **The org-standard model card and datasheet templates** (exercise-03, chapter 4). The two document shapes every launch packet uses, grounded in Mitchell et al. 2019 (model cards) and Gebru et al. 2021 (datasheets).
4. **The pre-launch review gate charter** (exercise-04, chapter 5). Required artifacts, required signoffs, block-vs-flag rule, escalation path.
5. **The red-team cadence and security delegation contract** (chapter 6). Cadence, artifacts, hand-off to the peer security track.

The five artifacts compose. The register (1) references classifications (2), model cards (3), and gate history (4). The gate (4) blocks on the presence and quality of (1), (2), (3). The red-team cadence (5) feeds evidence back into (1). A portfolio whose five artifacts point at each other coherently is a portfolio a governance analyst can review; a portfolio whose five artifacts contradict each other is a portfolio the next inquiry will find.

## Summary

- **Portfolio-scope responsible AI** is the set of written artifacts and standing processes that make the org's responsible-AI posture legible across three to five ML systems, without depending on one engineer's memory.
- The **Staff ML engineer owns the ML-side implementation** — the register, the classification table, the templates, the gate, the delegation contract. The **governance analyst owns the policy**; the **security partner owns deep adversarial work**. Chapter 6 formalises the delegation.
- The module lives at the **Right Hand** archetype's centre of gravity. The signature failure is drifting into leadership-assistant work and away from ML-engineering evidence.
- The **"am I doing portfolio RAI work?"** heuristic catches the two altitude errors: drifting down into single-model risk assessment (an L30 artifact) and drifting up into policy authorship (an analyst artifact).
- The five artifacts — register, classification table, model-card and datasheet templates, gate charter, delegation contract — compose. Chapters 2 through 6 build them in order.

The next chapter walks the portfolio AI risk register: the NIST AI RMF functions, the Gen-AI profile overlay, and the column set every row needs.
