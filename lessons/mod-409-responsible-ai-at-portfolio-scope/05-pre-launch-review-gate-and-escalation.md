# The Pre-Launch Review Gate — Charter, Signoffs, Escalation

## Motivation

The four artifacts of the preceding chapters — risk-register row, EU AI Act classification, model card, datasheet — are inert until something forces the launching team to produce them and something forces the reviewing roles to read them. That something is the **pre-launch review gate**. It is the moment where the portfolio's responsible-AI posture stops being a wall of documentation and starts being a decision: *do we ship this system tomorrow, or not?*

A gate that never blocks anything is decoration. A gate that blocks everything is a bottleneck the org routes around within a quarter. This chapter walks the charter, the required-artifact list, the signoff routing, the block-vs-flag rule, and the escalation path to the head of AI governance for policy conflicts. The point is a gate that is proportional — heavy for high-risk systems, light for minimal-risk systems, and *predictable* enough that a launching team can plan for it.

## What the gate is

The **pre-launch review gate** in this module is:

> A documented step in the ML launch workflow at which a specific packet of artifacts must be present and specific roles must sign off before the launch proceeds, with a proportionality rule that scales the depth of review to the system's EU AI Act tier (chapter 3), and an escalation path to the head of AI governance for policy conflicts the standing signoffs cannot resolve.

Three properties matter.

**A documented step in the workflow.** The gate is on the launch runbook. It is on the deployment-tool checklist. It is on the release-review agenda. A team that skips the gate skips it against a written process, which the gate charter defines as a launch-eligibility violation. A gate that only exists because someone remembered it is not a gate.

**A packet, not a conversation.** The gate consumes a specific set of artifacts — the register row, the classification-table row, the model card, the datasheets, the eval report(s) referenced by the card, the red-team artifact (chapter 6) — and produces a specific decision. Conversations are the reviewer's tool for evaluating the packet, not a substitute for it. A gate without a required artifact list is a gate that becomes a favour-based ritual.

**Proportional to tier.** A minimal-risk recommender launch is not reviewed with the same depth as a high-risk fraud model. The gate charter names the depth per tier so launching teams can plan their timeline and reviewers can allocate their attention.

## The four required artifacts

The gate's packet is short. Four artifacts are always required; two are conditionally required.

**Always required.**

1. **Risk-register row.** The register row for the launching system, on the current template, cross-linked to the classification-table row. Freshness less than 30 days.
2. **EU AI Act classification-table row.** The engineering-read tier, the obligation-surface subset, the legal-review status. Freshness less than 90 days or since the last material product change.
3. **Model card.** The org-standard model card (chapter 4) with the signoff footer partially completed — product owner and ML tech lead pre-signed, awaiting the gate signoffs.
4. **Datasheet(s).** One datasheet per dataset the system was trained, fine-tuned, or evaluated on, in the org-standard shape.

**Conditionally required, driven by tier and use-case.**

5. **Red-team / adversarial-eval report** (chapter 6). Required for any gen-AI system, any system with adversary-shaped inputs, and any EU AI Act high-risk system. Freshness driven by the cadence chapter 6 sets.
6. **Human-oversight design.** Required for EU AI Act high-risk systems and for any system whose product surface includes automated decisions materially affecting users. This is a short design doc, often two pages, that describes how the human overseeing the system is empowered to intervene.

The list is short deliberately. A gate that requires nine artifacts is a gate that produces nine fill-in-the-blank artifacts. A gate that requires four (plus two conditionals) is a gate that produces four honest artifacts.

## The signoff matrix

The gate requires signoffs from four roles for high-risk systems, three for limited-risk, two for minimal-risk. Each role's signoff has a specific mandate; a role that finds itself signing off on things outside its mandate is a role the gate has misused.

| Role | Mandate at the gate | Required for tier |
|---|---|---|
| **Product owner** | The intended use in the model card matches what product will actually launch. | All tiers |
| **ML tech lead** | The eval numbers and mitigations are honest and current. | All tiers |
| **Governance analyst** | The packet meets the org's responsible-AI policy for the tier. | Limited-risk and above |
| **Legal counsel** | The classification-table row is consistent with legal's binding read. | High-risk (and any tier where legal has flagged a specific system) |
| **Security partner** | The red-team / adversarial-eval packet is present and its residual-risk position is signed. | Any tier with a required red-team artifact (chapter 6) |

Two disciplines keep the matrix honest. Signoffs are **role, not person** — the product-owner-role signs, and if the person rotates, the next role-holder signs the next launch, no institutional-memory carry-over. And signoffs are **time-boxed** — a review that has not moved in five business days escalates to the head of AI governance, both to prevent quiet blocking and to prevent quiet skipping.

## The block-vs-flag rule

The gate has two verdict shapes: **block** (the launch cannot proceed until specific defects are fixed) and **flag** (the launch proceeds but a defect is tracked and must be fixed by a named date). Distinguishing between them is the gate's most consequential political move; a gate that always blocks becomes hated, a gate that always flags becomes ignored.

**Block when:**

- Any always-required artifact is missing or below the freshness threshold.
- The EU AI Act classification-table row is `unreviewed` for a system the engineering read classifies as high-risk or unacceptable.
- The model card's intended use materially disagrees with what the product owner is about to ship.
- The eval evidence cited in the model card is stale (older than the org's per-tier freshness threshold) or the system's most recent eval has regressed materially against a named metric.
- The security partner has an unresolved *finding of risk* from the red-team artifact.
- Any *policy conflict* the standing signoffs cannot resolve — for those the block continues through the escalation path below.

**Flag when:**

- A required section within an artifact is thin but the artifact is present and the launching team has committed to a filled-out version by a named date.
- A conditional artifact is present but its recommended-follow-up (a next-quarter red-team, an additional fairness cut) is not yet scheduled — flag with the follow-up on a calendar.
- A non-blocking observation about the human-oversight design (chapter 4's Article-14-adjacent obligation) that the launch can proceed under with a named improvement.

Every flag has a named owner and a named date. A flag whose date passes without resolution graduates to a block on the next launch of the same system — that is the enforcement mechanism that keeps flags from becoming quietly permanent.

## Escalation to the head of AI governance

Some conflicts are outside the gate's four-role signoff matrix. The launching product owner and the governance analyst genuinely disagree on whether the system's intended use is acceptable. Legal reads the tier one way, the Staff engineer reads it another. A red-team finding raises a question about the *policy* rather than a defect in the system. These conflicts escalate.

The escalation path is documented and short:

1. The reviewer identifying the conflict writes a short escalation memo — one page — naming the conflict, the two positions, and the decision requested.
2. The head of AI governance receives the memo and either resolves the conflict directly, chairs a decision meeting, or refers to a policy waiver process.
3. The resolution is recorded on the classification-table row (chapter 3) and, if it modifies the org's policy, propagates into the analyst's policy document.

Two anti-patterns to avoid. First, escalating too early — a reviewer who escalates every disagreement is a reviewer who has misread the gate as a rubber stamp. The gate's own discussion is where most disagreements resolve; escalation is for the ones that will not. Second, escalating too late — a reviewer who blocks a launch quietly for weeks without escalating is running an unofficial process that no one can appeal to. The five-business-day time-box on the standing signoffs is what forces the escalation clock.

## What Staff owns vs. what the analyst owns at the gate

The gate is a shared process. The clean split:

**Staff ML engineer owns.**

- The **gate charter** — the document that defines the required artifacts, the signoff matrix, the block-vs-flag rule, the freshness thresholds, the escalation path.
- The **operational running** — the deployment tool integration, the calendar of scheduled reviews, the escalation clock, the pre-review-quality check that the packet is complete before the reviewers spend attention on it.
- **Chairing the review meeting** for high-risk systems; the analyst chairs for policy-specific reviews.
- **Reporting on gate health** to the head of AI governance — median review latency, block rate, flag-graduation rate, escalation frequency. The gate is measurable and the Staff engineer keeps the measure.

**Governance analyst owns.**

- **The policy** the gate enforces — what "acceptable use" means, what the transparency threshold is, what the human-oversight standard is.
- **Chairing the review** for launches that are primarily policy-review-shaped rather than eval-review-shaped.
- **Sign-off** on the policy exceptions and the waivers the head of AI governance issues.
- **External-audience gate summaries** — the numbers a regulator or a board would see.

Exercise-04 lands the charter authorship.

## The gate as a career signal

A Staff engineer who has authored a gate charter that survived three quarters of use, blocked at least one launch that would have caused a real problem, flagged at least one launch that resolved cleanly, and produced measurable review-latency numbers, has a piece of the promotion packet the Right Hand archetype needs. Conversely, a Staff engineer whose gate never blocked anything either has a portfolio with genuinely low risk (rare in a portfolio with regulatory exposure) or a gate that no one is taking seriously. The gate's *audit trail* is what a promotion committee reads — not just "we have a gate", but the specific block that mattered and the specific flag that closed on schedule.

## Summary

- The **pre-launch review gate** is a documented workflow step consuming a specific artifact packet and producing a specific decision — block, flag, or clear — with a proportionality rule keyed to the EU AI Act tier.
- The **required artifacts** are four (register row, classification row, model card, datasheet) plus two conditional (red-team artifact, human-oversight design). Keep the list short; long lists produce fill-in-the-blank artifacts.
- The **signoff matrix** has product, ML tech lead, governance analyst, legal counsel, security partner — signoffs are by role and time-boxed to five business days.
- **Block vs. flag** is the gate's most consequential move. Always-required artifacts missing, contested classifications, stale eval, unresolved security findings *block*. Thin sections with named-date owners *flag*. Flags that miss their date graduate to blocks.
- **Escalation to the head of AI governance** is a documented, short-memo process for conflicts the standing signoffs cannot resolve. Not too early, not too late.
- **Staff owns** the charter and operational running; the **analyst owns** the policy the gate enforces. The gate's audit trail is a career signal for the Right Hand archetype.

The next chapter walks the red-team cadence and the delegation contract to the peer security track — the piece of the gate that consumes the deepest specialist work.

Exercise-04 lands this chapter: author the gate charter for your portfolio, walk it past a peer analyst reviewer, and simulate a review of one launching system to shake out defects.
