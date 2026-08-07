# exercise-03: Multi-Team Gap RFC

**Estimated effort:** 3 hours

## Objective

Author a **multi-team gap RFC** against a specific paved-road feature the peer platform team should build but has not yet, using chapter 5's nine-section template. The deliverable is a five-to-eight-page RFC that the peer platform team's staff-plus IC could review this week and route to their PM for the next quarter's roadmap — with defended business impact, a signed adoption commitment, and an integration-investment offer that says *we are not just asking; we are investing.*

The RFC's success criterion is not that the platform team accepts it (that is out of your hands). The criterion is: *the platform team's response is either "accepted for quarter X", "rejected with a specific reason", or "held pending information X" — but never "unclear what is being asked or why."* Chapter 5 named the two most common failure modes: RFC-as-complaint (no adoption commitment) and RFC-as-wish-list (no business impact). Avoid both.

## Prerequisites

- Exercise-01 complete — the verdict for the chosen component is `adopt-managed` or `adopt-OSS` (a `build-inside-the-ML-org` verdict has no gap RFC against the platform team; the artifact for that path is exercise-04's contribute-back RFC).
- Exercise-02 complete — the consumer contract in force. The gap RFC lives inside a standing consumer contract; the RFC's adoption commitment reads against Section 4 of that contract.
- Chapter 05 — the gap RFC's nine sections, its two load-bearing sections (business impact and adoption commitment), the four common failure modes, and the "when to reach for the gap RFC vs. neither" test. Read in full.
- Recommended: read one real Rust RFC and one Kubernetes Enhancement Proposal end-to-end. Twenty minutes. The section shape and the compression style are what your RFC should imitate.
- Sanity-check: chapter 5 named three cases where the right answer is *neither* RFC — a small gap workable-around with a client-side adapter, a gap that would be a contribute-back candidate instead, and a gap the ML org can honestly defer. Confirm your candidate gap is not one of those three before starting.

## Choose the gap

Pick **one** specific missing feature in the paved-road implementation of the component from exercise-01. Priority order for selection:

1. A gap that materially blocks a portfolio-scope roadmap item — the fraud team cannot ship real-time scoring because the feature store's freshness SLA is 5 minutes and they need sub-second; the ranking team cannot ship a canary rollout because the inference gateway has no per-model rollback primitive.
2. A gap named in the consumer-contract implications section of exercise-01 or in Section 7 (out-of-cycle review triggers) of exercise-02. Consistency across the module's four artifacts is the deliverable at project scope.
3. A gap that is small enough to write about in three hours but consequential enough to be worth the platform team's roadmap slot. "Add a new dashboard widget" is too small; "rewrite the feature store" is too large. The chapter 5 sweet spot is one feature at one component's API surface with one-to-three primary adopters.

Do not bundle multiple unrelated gaps into one RFC. Chapter 5's failure mode of *bundled requests* is caught here — split into multiple RFCs, each independently scored.

## Steps

1. **State the gap and its target quarter in one paragraph** at the top. Named: which component, which feature, which quarter you are requesting delivery. If you cannot name the target quarter, the RFC is not yet a request — it is a wish. Chapter 5's Section 1 is this paragraph.
2. **Author Section 2 — Motivation.** Named ML use case: *the fraud model's real-time-freshness requirement is not met by the feature store's 5-minute refresh SLA*, not *"features should be fresher."* One-to-two paragraphs. Concrete before abstract.
3. **Author Section 3 — Business impact.** The load-bearing section, and the one gap RFCs most often bungle. Named impact in one of three languages:
    - **Cost.** "This feature saves N engineer-months per year across teams A, B, C." Name the maths.
    - **Revenue.** "This feature unlocks the product experience projected to add $M in annual revenue per the product team's forecast." Cite the forecast.
    - **Risk.** "This feature closes a governance gap the org committed to close in Q3 per the regulatory response plan." Cite the plan.
    If you cannot state the impact in one of these three languages, chapter 5 was explicit: the RFC is not ready for review. Do not proceed to the other sections until Section 3 has a defensible number.
4. **Author Section 4 — Design proposal.** API sketch level — signatures, behaviour, error modes, compatibility with the existing surface. This is not a design specification; the platform team owns the specification. The design proposal is enough for the platform team to size the work and to identify design questions. One-to-two pages, with a code-fence or two showing the proposed API shape.
5. **Author Section 5 — Adoption commitment.** The other load-bearing section. Named: which ML teams commit to adopting the feature within N months of release, on what use cases, at what expected volume (QPS, model count, feature count). Signed by the teams' tech leads or by the ML-org Staff engineer on their behalf. Without signed adopters, the RFC is a wish.
6. **Author Section 6 — Integration investment.** What the ML org will do to make integration cheap for the platform team: dogfooding (the ML org runs the alpha internally against use case X), staff-level design review (N hours from named engineers), documentation-and-runbook contribution, migration codemods where applicable. This is where the ML org tells the platform team: *we are not just asking; we are investing.*
7. **Author Section 7 — Alternatives considered.** Three sub-sections. **7a — Client-side workaround** the ML org considered and rejected, with the reason (typically: does not scale beyond one team). **7b — OSS or vendor alternative** the ML org could adopt directly rather than asking the platform team, with the reason for the platform-team version being preferred (typically: consumer-contract integration, org-scope adoption). **7c — Fork-and-maintain-ourselves** as the ML org keeping the fix in-tree, with the reason the RFC concluded contribute-later would be worse than build-now (typically: scope-drift risk from chapter 1).
8. **Author Section 8 — Risks and unknowns.** Three-to-five risks the platform team should know about. Adoption risk (the committing teams' priorities could shift), design risk (the proposed API might collide with an existing paved-road pattern), operational risk (the feature increases the platform's on-call load), scope risk (the feature is one of a family the platform team will get more requests for once you ship the first).
9. **Author Section 9 — Timeline and review triggers.** Requested target quarter, the ML org's review triggers (if the platform team cannot commit, what alternative the ML org will pursue and by when), and the escalation path if the RFC is rejected the ML org considers business-critical. Chapter 5's Section 9 is what makes the RFC a bilateral document — the ML org names its Plan B before the platform team responds, which is what makes the response tractable rather than a negotiation ambush.
10. **Return to Section 1 (Summary).** Rewrite the summary paragraph now that the rest of the RFC is drafted. One paragraph, one sentence per: what is being asked, target quarter, business impact, adoption volume. A summary that takes more than a paragraph is a summary that is doing analysis-section work.
11. **Sanity-check against chapter 5's four common failure modes.** Missing business impact (Section 3 without a defensible number) — go back to step 3. Missing adoption commitment (Section 5 without signed adopters) — go back to step 5. Bundled requests (multiple features in one RFC) — split. Section 4 as specification (RFC dictates the API rather than proposes one) — soften the design proposal to a sketch.

## Deliverable

A single document, 5-8 pages, in the chapter 5 nine-section template:

- **Section 1 — Summary.** One paragraph including feature, component, target quarter, and ML-org sponsor (step 1, rewritten in step 10).
- **Section 2 — Motivation.** One-to-two paragraphs, named ML use case (step 2).
- **Section 3 — Business impact.** Named impact in cost / revenue / risk language, with a defensible number (step 3).
- **Section 4 — Design proposal.** API sketch level, one-to-two pages with code-fence API examples (step 4).
- **Section 5 — Adoption commitment.** Named ML teams, use cases, volume, and signed acknowledgement (step 5).
- **Section 6 — Integration investment.** Dogfooding, review time, documentation, migration codemods (step 6).
- **Section 7 — Alternatives considered.** Three sub-sections — client-side workaround, OSS/vendor alternative, fork-and-maintain (step 7).
- **Section 8 — Risks and unknowns.** Three-to-five risks (step 8).
- **Section 9 — Timeline and review triggers.** Requested quarter, ML-org Plan B, escalation path (step 9).
- **Appendix — Failure-mode audit.** One paragraph confirming none of chapter 5's four common failure modes apply (step 11).

## Starter guidance

- **Author Section 1 last.** Chapter 5's authoring cadence — same discipline as mod-402 exercise-04. A first-draft summary always over-promises; a last-draft summary compresses what the RFC actually decided.
- **Section 3 fails alone.** The single most common gap-RFC failure mode is a Section 3 that reads *"this would be useful"* rather than *"this saves N engineer-months / $M revenue / Y risk."* If your Section 3 has no number, either the number is discoverable and you should discover it (ask the fraud team's tech lead for the impact figure) or the number is not discoverable and the RFC is not ready to file. Do not paper over.
- **Section 5's signatures matter.** *"Team A will probably adopt"* is not adoption commitment. *"Team A commits to adopting within 30 days of GA, running on use case X at expected volume 10k QPS, signed by <tech-lead-name>"* is. The platform PM's decision to prioritise is a function of how many QPS × how many teams × how many months to first production use. Give them the numbers.
- **Section 4 is a proposal, not a spec.** Chapter 5 was explicit — the platform team owns the API. If you find yourself writing a full IDL definition, you have crossed into specification. Pull back to signatures, behaviours, and one or two examples. The platform team will counter-propose the actual API in review.
- **Section 7 is where new authors under-invest.** Three sub-sections is not one paragraph total. Each rejected alternative gets its own paragraph presented in the alternative's strongest form. If your Section 7 is a hand-wave, reviewers will ask "why not use X" as their first question — and you will answer live rather than in the RFC.
- **Cite prior art in Section 4.** If the feature you are proposing has a canonical implementation elsewhere (Feast's on-demand feature views, MLflow's aliases, KServe's canary controller), name it and cite it. The platform team's design review starts from the prior art; skipping the citation makes them do the reading themselves.
- **Do not smuggle in a second gap.** If while authoring you notice a related gap the same feature would nearly close, note it in Section 8 as a follow-on and file a separate gap RFC. Chapter 5's bundled-requests failure mode is caught here.

## Acceptance criteria

- All nine chapter-5 sections are present; the summary in Section 1 is one paragraph including feature, component, target quarter, and sponsor.
- Section 3 (Business impact) names a specific number in cost / revenue / risk language, with a citable source (a forecast, a plan, or a bottom-up estimate showing the maths).
- Section 4 (Design proposal) is API-sketch level — signatures and behaviours, not a full specification. Includes at least one code-fence example.
- Section 5 (Adoption commitment) names specific ML teams, use cases, expected volume, and either a signed acknowledgement from a tech lead or a stated commitment by the ML-org Staff engineer on the team's behalf.
- Section 6 (Integration investment) names concrete offers — dogfooding target, review-hours commitment, documentation contribution, migration codemod availability.
- Section 7 (Alternatives) has three named alternatives (client-side workaround, OSS/vendor alternative, fork-and-maintain), each in its strongest form, each with a stated reason for rejection.
- Section 8 (Risks) has 3-5 named risks including at least one adoption risk and one operational risk.
- Section 9 (Timeline) names a target quarter, the ML org's Plan B if rejection or delay, and the escalation path.
- The appendix (Failure-mode audit) confirms none of chapter 5's four common failure modes (missing business impact, missing adoption commitment, bundled requests, Section 4 as specification) apply.
- The RFC bundles **one** feature — not two, not five. Related follow-on features are noted in Section 8 but not smuggled into the primary ask.

## Stretch goals

- **Take the draft to a peer role-playing the platform-team staff engineer.** Ask them to write a one-paragraph disposition: accepted for target quarter, rejected with reason, or held pending information. If the disposition is "held pending information", that tells you which section is under-specified — revise and re-send.
- **Draft the platform team's counter-proposal on API shape.** In two paragraphs: how would the platform team's design review push back on Section 4, and what counter-shape would they propose? The exercise sharpens whether Section 4 is genuinely a sketch (open to counter-proposal) or covertly a specification (the platform team's design review will grind on it).
- **Author the ML-org Plan B in detail.** Section 9 named the alternative the ML org will pursue if the RFC is rejected. Draft that plan as a one-page mini-RFC: option, cost, timeline, ownership. This is the artifact that turns the escalation path from a threat into a plan; escalations backed by a real Plan B tend to be shorter meetings than escalations backed by a vague *"we'll figure something out."*
- **Draft the paired exec-brief one-liner.** One sentence for the director: what the RFC asks the platform team to build, why the ML org needs it, and what the ML org commits to in exchange. The director's decision — whether to advocate for the RFC at cross-org leadership — reads from that sentence.
- **Cross-reference to project-401.** Note which sections of this RFC would land in project-401's platform-strategy artifact and which are RFC-specific detail that would not. The consistency check is worth an hour before the capstone lands.
- **Track a real RFC through to a decision.** If the gap is a real gap at your current employer, offer the RFC to your team's staff-plus channel or your platform-team counter-parts as a "practice RFC" for their feedback. Even without a formal review meeting, feedback from three peers is the highest-signal input for the next revision — and sometimes the RFC lands for real.
