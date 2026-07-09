# Red-Teaming, Adversarial Eval, and the Security Delegation Contract

## Motivation

Chapters 2 through 5 stopped short of the deepest specialist work in the responsible-AI stack: the adversarial evaluation, the prompt-injection and jailbreak testing, the model-supply-chain audit, the data-extraction attack surface. That work belongs to the peer security track — [`ai-infra-security-learning`](https://github.com/ai-infra-curriculum/ai-infra-security-learning) — and this chapter is the Staff ML engineer's *contract* with that track. It names the cadence, the artifact shapes the gate consumes, the escalation path when a red-team finding is a policy question rather than a technical one, and the small handful of things the Staff ML engineer keeps rather than delegating.

The point is a portfolio where red-team work happens on a schedule, produces a document the pre-launch gate can consume without asking follow-up questions, and does not become the Staff engineer's second job. A Staff engineer who is running adversarial evaluations personally has taken a specialist track's role; a Staff engineer who never engages with red-team findings has not taken the Right Hand archetype seriously.

## What "red-teaming at portfolio scope" means here

**Red-teaming at portfolio scope** in this module means:

> A standing cadence of adversarial evaluations across the ML portfolio — proportional to system tier — whose outputs feed into the risk register (chapter 2) and the pre-launch gate (chapter 5), and whose execution is delegated by a written contract to the peer security track while the Staff ML engineer retains ownership of the cadence, the artifact schema, and the escalation path.

It is a *cadence*, not a launch-adjacent event only. A red-team artifact produced at launch and never refreshed is a dated snapshot; the attack surface evolves and the artifact must too.

## The cadence

The cadence is proportional to tier and to system class. As a working default the Staff engineer negotiates with the security partner and the head of AI governance:

- **EU AI Act high-risk systems.** Red-team artifact required at launch. Refresh every 6 months, and on any material product change that opens new attack surface (new input modality, new autonomous action, new integration with an external tool).
- **Gen-AI systems regardless of tier.** Red-team artifact required at launch focused on the OWASP LLM Top 10 categories (prompt injection, insecure output handling, training-data poisoning, model DoS, supply chain vulnerabilities, sensitive-information disclosure, insecure plugin design, excessive agency, overreliance, model theft) as the coverage checklist. Refresh at least annually. <!-- needs-research: verify the current OWASP LLM Top 10 category list against the published document at owasp.org; the ten-category enumeration above tracks the initial published version but the project revises annually. -->
- **Classical-ML high-risk systems.** Red-team artifact required at launch focused on adversarial-example robustness and data-extraction risk, using the NIST AI 100-2 taxonomy as the coverage checklist. Refresh at least annually.
- **Classical-ML minimal-risk systems.** Red-team artifact not required at launch; the security partner reviews the risk-register row and either concurs or requests a targeted evaluation.

The cadence is on the same calendar as the register and the classification table. Missing a scheduled red-team refresh graduates to a gate defect on the next launch of that system.

## The artifact the gate consumes

The security partner produces a red-team report. The Staff engineer specifies the *shape* of that report so the pre-launch gate can consume it without asking follow-up questions. A minimal, gate-consumable report has:

- **Scope.** What system version was tested. What attack surface was covered. What was explicitly out of scope.
- **Methodology.** What taxonomy the tests were designed against — OWASP LLM Top 10, NIST AI 100-2 categories, an internal red-team framework. What proportion of the taxonomy was exercised.
- **Findings.** Each finding with a severity, a reproduction, the affected component, and the recommended remediation. A finding severity system aligned to the org's incident-severity system (mod-408) so a Sev-2 security finding and a Sev-2 reliability incident live in the same vocabulary.
- **Residual risks.** The findings the team is accepting as residual, with the signoff role and date.
- **Follow-up plan.** What will be re-tested next, when, and by whom.

The report is a document, not a slide deck. Slide decks are the report's summary layer, not its substance.

## OWASP and NIST as the shared vocabulary

Two publicly available catalogues carry most of the vocabulary the red-team work runs against; both are worth reading before contracting with the security partner.

**OWASP Top 10 for LLM Applications.** The OWASP Foundation maintains a Top 10 list of risks specific to LLM-integrated applications, updated on a rough annual cadence at [owasp.org](https://owasp.org/). It is the shortest reference the Staff engineer, the launching team, and the security partner can all agree describes the attack surface of a gen-AI feature. Any gen-AI launch's red-team artifact should name which of the current Top 10 categories were exercised and which were not, with the argument for the "not".

**NIST AI 100-2, *Adversarial Machine Learning: A Taxonomy and Terminology of Attacks and Mitigations*.** NIST's adversarial-ML taxonomy is the analogous vocabulary for classical-ML systems: the attack classes (evasion, poisoning, extraction, inference), the assumed adversary capabilities, the mitigation shapes. Any classical-ML high-risk launch's red-team artifact should map its scope against this taxonomy.

Using publicly named vocabularies matters for the same reason the NIST AI RMF matters for the register (chapter 2): a red-team report grounded in OWASP and NIST is portable. A red-team report using the security partner's internal taxonomy alone is not.

## What Staff owns vs. what the security partner owns

The clean split, once more:

**Staff ML engineer owns.**

- **The cadence** — the calendar, the tier-proportional schedule, the refresh triggers on product change.
- **The artifact schema** — the shape of the report the gate will accept, aligned to OWASP / NIST vocabularies.
- **The register linkage** — every red-team artifact updates the corresponding register row's Manage column (residual risks) and Measure column (attack-surface evidence).
- **The escalation** for findings that are policy questions rather than technical defects. A red-team finding that says "this system reveals fine-grained information about individuals in the training data" is not just a technical bug; it is a policy question that goes to the analyst and the head of AI governance.
- **The delegation contract itself** — a written document that names the responsibilities on both sides, updated when either side's scope or staffing changes.

**Security partner (from `ai-infra-security-learning`) owns.**

- **Executing the red-team** — designing the attacks, running them, writing the findings, proposing remediations. This is deep specialist work; the security engineer's promotion packet lives here.
- **The taxonomy currency** — keeping the OWASP / NIST vocabulary the org runs against up to date as the public catalogues update.
- **The supply-chain audit** — training-data provenance verification, model-weight provenance, dependency-scanning of the serving stack. This is where the datasheet's licensing section (chapter 4) meets a runtime attestation.
- **Runtime security controls** — output filters, rate-limits, red-team-informed guardrails at inference. The Staff engineer knows these exist and defers on their design.
- **Security-incident response** for security-classified events, in partnership with the mod-408 incident-command process.

The delegation contract is the artifact that keeps the split legible. Exercise-04's gate charter references it; the mod-408 incident-command playbook references it; the mod-401 hand-off contract framing is what shapes it.

## What the Staff engineer keeps rather than delegating

A well-written delegation contract still names three things the Staff ML engineer does not hand over.

**The register interpretation.** When the security partner produces a finding, the Staff engineer decides how it lands in the risk-register row's Manage column. That is a portfolio-scope call — how the finding compares across systems, what precedent it sets, whether it changes the tier read in the classification table. The security partner has depth on the finding; the Staff engineer has scope on how it lives in the register.

**The policy-escalation call.** When a finding is a policy question, the Staff engineer escalates. The security engineer might be silent because they have no relationship with the analyst; the Staff engineer's Right Hand archetype is exactly for this bridge.

**The cadence discipline.** When the security partner is over-loaded and the cadence slips, the Staff engineer is the one who negotiates the re-scoping — narrower coverage for the next quarter, an explicit deprioritisation on a minimal-risk system — rather than letting the cadence quietly drift. Cadence drift is the failure mode that shows up in an audit six months later; the Staff engineer catches it before that.

## The delegation contract in one page

The contract itself is short — one page, sometimes two. It names:

1. **Scope.** Which systems in the portfolio are in the delegation. Which classes of work are delegated (red-team, supply-chain, runtime controls). Which are not (register interpretation, policy escalation, cadence discipline).
2. **Cadence.** The tier-proportional schedule from earlier in this chapter, with named calendar reviews.
3. **Artifact schema.** The gate-consumable report shape from earlier in this chapter, with an example.
4. **Escalation.** The path when a finding is a policy question, when a system misses a cadence, when a Sev-1 security finding lands mid-quarter.
5. **Signoff.** The Staff ML engineer, the security-partner lead, and — for high-risk delegations — the head of AI governance and the head of security. Refreshed annually or on a role change.
6. **Hand-back triggers.** The conditions under which the contract is renegotiated — a new class of system enters the portfolio, the security partner's headcount changes materially, an external event (regulation, incident, industry disclosure) changes the risk picture.

Mod-401's hand-off contract chapter is the shape reference; the specifics in this chapter are the responsible-AI specialisation.

## Summary

- **Red-teaming at portfolio scope** is a standing cadence, tier-proportional, with outputs that feed the risk register and the pre-launch gate. It is not launch-only.
- **The Staff engineer keeps** the cadence, the artifact schema, register-linkage, policy-escalation calls, and the delegation contract itself. **The peer security track (`ai-infra-security-learning`) owns** the execution — designing and running attacks, keeping taxonomy currency, supply-chain audit, runtime controls, and security-incident response.
- **OWASP LLM Top 10** and **NIST AI 100-2** are the shared vocabularies the artifact schema aligns to. Publicly grounded vocabulary is what makes the artifact portable.
- **The delegation contract** is a one-page document with scope, cadence, artifact schema, escalation, signoff, and hand-back triggers. It is renegotiated on named triggers, not silently.
- The Staff engineer's temptation is to run the red-team themselves; the failure mode is to disengage entirely. The middle path — set the cadence, consume the artifact, interpret it into the register, escalate the policy questions — is the Right Hand archetype in operating form.

This closes the mod-409 chapter arc. The five artifacts of the module — register, classification table, model card, datasheet, gate charter — plus this chapter's delegation contract compose into the portfolio's responsible-AI posture that a governance analyst, a regulator, or a new head of AI can read from one place.

The next steps live outside this module. [`mod-410`](../mod-410-staff-plus-technical-leadership/) walks the leadership craft the gate charter's political defence draws on. [`project-402`](../../projects/project-402-foundation-model-training-program/) integrates this module's register into a full training-program deliverable. [`project-403`](../../projects/project-403-staff-plus-tech-leadership-simulation/) integrates the gate charter into a leadership simulation.
