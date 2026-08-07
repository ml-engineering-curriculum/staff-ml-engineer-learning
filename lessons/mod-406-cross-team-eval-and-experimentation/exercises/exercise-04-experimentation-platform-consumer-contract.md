# exercise-04: Experimentation-Platform Consumer Contract

**Estimated effort:** 3 hours

## Objective

Author the **experimentation-platform consumer contract** between the ML org and the platform team that owns (or will own, or that vends) the experimentation platform, using chapter 5's seven-required-capability framing. The deliverable is a two-to-three-page contract that names each of the seven capabilities the ML org demands (SRM diagnostics, CUPED or equivalent, sequential testing, guardrail-metric monitoring, interference detection, novelty / primacy visibility, metric registry), the review cadence with the platform team that keeps the contract honest, and the exception path when a capability is not yet delivered. After this exercise the learner should hold the fourth of the four eval-program contracts and the substrate that chapter 4's canary and progressive-rollout stages run on.

The contract is a *consumer* contract in the mod-405 sense — it names capabilities, not implementations. The platform team's build is peer-platform-track territory; this document is what the ML org requires. If your draft slips into naming the specific traffic-splitting algorithm or the specific data pipeline, delete and re-anchor at capability.

## Prerequisites

- Exercises 01, 02, and 03 complete — the standard, the judge policy, and the review body. The contract's guardrail-metric monitoring reads the guardrails from exercise-01 Section 2 Bucket 4; the interference detection is read by the review body from exercise-03 Section 4.
- Chapter 05 — the seven required capabilities in detail, the "what the ML org does not demand" boundary, and the pairing with the review-body charter. Read in full.
- Chapter 04 — enough to know that canary and progressive-rollout are A/B experiments in the statistical sense, and the platform's machinery is what makes them trustworthy.
- mod-405 — the paved-road consumer-contract vocabulary. This contract's shape is the mod-405 chapter-4 template applied to the experimentation platform specifically. If you completed mod-405 exercise-02, use that template's Section 0-8 shape as the skeleton for this contract.
- Recommended: read Deng, Xu, Kohavi, Walker 2013 [CUPED paper](https://dl.acm.org/doi/10.1145/2433396.2433413), the Fabijan et al. 2019 [SRM taxonomy paper](https://dl.acm.org/doi/10.1145/3292500.3330722), and one of the platform-team engineering-blog posts on ExP / XLNT / ERF (see `../resources.md`) to ground the capabilities in real prior art.
- Identify the counter-party. Is the experimentation platform (a) built by an in-house peer platform team, (b) adopted from a managed vendor (Statsig, Split, Eppo, Optimizely), (c) adopted from OSS (GrowthBook, Wasabi), or (d) missing altogether and this contract is the forcing function that motivates its build? The contract's counter-party section changes with the answer.

## Steps

1. **Draft Section 0 — Counter-party and current state.** One paragraph naming the counter-party (in-house platform team lead, managed vendor account team, OSS project maintainers, or the "no counter-party today; this contract motivates the build" case) and the current state of the platform (green-field, MVP, mature, deprecating). Chapter 5 was explicit that the contract is a *consumer* contract; naming the counter-party is what makes it a contract rather than a wishlist.
2. **Draft Section 1 — Contract term.** Effective date (`YYYY-MM-DD`), review date (six months out per the mod-405 default), renewal / renegotiation triggers (a capability commitment slips, a portfolio addition changes the required capability set, the platform team ships a breaking change). A contract without a term is not a contract.
3. **Draft Section 2 — Scope.** Which experiments the contract applies to — canary and progressive-rollout stages of the chapter-4 rollout contract, offline A/B on shadow data, and any batch-scored eval that consumes platform metric-registry entries. Which experiments are out of scope — one-off analyses, non-ML product experiments that share the platform, etc.
4. **Draft Section 3 — The seven required capabilities.** For each of the seven chapter-5 capabilities, write:
    - **Capability name** and one-sentence definition in the ML org's vocabulary.
    - **What the ML org demands the platform expose** — the specific interface, dashboard, or API surface. Not the implementation; the surface.
    - **The current state** — delivered / partially delivered / not yet delivered. Be honest; a contract that pretends everything is green is theatre.
    - **The delivery commitment** — for capabilities not yet delivered, the named date by which they will be, or the named `NEGOTIATE` marker if the platform team has not yet committed.
    - **The consumer's usage commitment** — how the ML org will use the capability once it exists (e.g., "every ML experiment will register its guardrail set with the platform" or "the ML org will consume the SRM p-value from the platform, not from a per-team notebook").
    Chapter 5's seven capabilities are the floor, not the ceiling. If the portfolio has a specific need not covered by the seven (e.g., a metric-explainability capability for a regulated portfolio), add an eighth — but justify why the chapter-5 seven do not cover it.
5. **Draft Section 4 — Consumer commitments beyond capability usage.** Adoption volume (how many teams will migrate their eval / experiments onto the platform by when), feedback / review-time commitment (how many hours per month the ML org commits to consumer-review meetings with the platform team), consumption discipline (the ML org will not go around the platform for statistical analysis — no shadow notebooks that recompute what the platform reports), migration commitment (when the platform ships a breaking change, the ML org migrates within the announced deprecation window). This is where the mod-405 chapter-4 four consumer-commitment categories attach.
6. **Draft Section 5 — Producer commitments beyond capability delivery.** API stability window (the platform team will not break a documented capability without notice — 6 months is the mod-405 default), on-call SLO for platform incidents that affect ML experiments (15-minute page-to-first-response is the mod-405 default), migration-support hours per week during a breaking change, gap-RFC response SLA (4 weeks per mod-405 default) for a new capability requested by the ML org. Mark unknown commitments `NEGOTIATE`.
7. **Draft Section 6 — Review cadence with the platform team.** Named monthly consumer-review meeting (60 minutes) between the ML-org Staff engineer and the platform-team Staff or lead. Named quarterly steering meeting with both EMs. Named agenda: (a) capability delivery status against Section 3, (b) SLO adherence against Section 5, (c) new-capability RFCs from the ML org, (d) upcoming breaking changes from the platform team. This is the mod-405 chapter-4 review cadence applied to the specific counter-party.
8. **Draft Section 7 — Exception path.** When a capability is not yet delivered, what does the ML org do in the interim? Named options: (a) a temporary in-org implementation (e.g., an SRM check in an ML-org notebook) with a named sunset date when the platform capability lands, (b) a documented gap the review body from exercise-03 accepts as a standard-of-care exception during the migration, (c) an escalation per Section 8 if the gap is not fixed by the delivery commitment. Same shape as exercises 01 / 02 / 03 exception paths.
9. **Draft Section 8 — Escalation.** Same three-tier staircase as exercise-03 adapted to the platform-team counter-party: (Tier 1) ML-org Staff to platform-team Staff / lead, (Tier 2) both EMs, (Tier 3) both directors and the mod-410 leadership-comms channel. Named triggers: a delivery commitment slips by more than 30 days, an SLO is missed twice in a quarter, a breaking change is shipped without the API stability window.
10. **Draft Section 9 — Signatures.** ML-org signer (Staff engineer), platform-team signer (Staff or lead), counter-signers (peer EMs, and if the contract touches procurement — a managed vendor — the procurement / vendor-management counter-signer). Sign the term.
11. **Draft the mod-402 Section-9 fold-in.** One-to-two sentences for the portfolio RFC — "the ML org's contract with the experimentation platform names the seven required capabilities the eval program's rollout contract depends on." Composes with the exercise-01 fold-in on the eval-metric-conflict cell.

## Deliverable

A single document, 2-3 pages, containing:

- **Section 0 — Counter-party and current state.** One paragraph (step 1).
- **Section 1 — Contract term.** Effective date, review date, renegotiation triggers (step 2).
- **Section 2 — Scope.** In-scope experiments, out-of-scope experiments (step 3).
- **Section 3 — Required capabilities.** Seven (or eight, justified) capabilities with definition, demanded surface, current state, delivery commitment, consumer usage commitment (step 4).
- **Section 4 — Consumer commitments.** Adoption volume, feedback / review time, consumption discipline, migration commitment (step 5).
- **Section 5 — Producer commitments.** API stability window, on-call SLO, migration support, gap-RFC response SLA (step 6).
- **Section 6 — Review cadence.** Monthly consumer review, quarterly steering, standing agenda (step 7).
- **Section 7 — Exception path.** In-interim options for undelivered capabilities (step 8).
- **Section 8 — Escalation.** Three-tier staircase with named triggers (step 9).
- **Section 9 — Signatures.** ML-org, platform-team, counter-signers (step 10).
- **Appendix — mod-402 Section-9 fold-in.** One-to-two sentences (step 11).

## Starter guidance

- **Capabilities, not implementations.** Chapter 5 was emphatic. Section 3 for "SRM diagnostics" says "the platform exposes a per-experiment SRM p-value, an alerting mechanism at the org threshold, and a portfolio-wide dashboard" — not "the platform runs a chi-square test on the assignment log using pandas." Implementation is platform-team territory; if you drift into it, the contract fails the mod-405 boundary test.
- **The current-state column is where honest contracts live.** For an in-house platform, some capabilities will be `delivered`, some `partially delivered`, some `not yet delivered`. For a managed vendor, some capabilities will be `native`, some `expose via metadata export`, some `not supported`. The contract is what happens next; a contract that pretends the current state is already green does not do the work.
- **`NEGOTIATE` is legitimate.** The mod-405 chapter-4 vocabulary applies here — a commitment number the ML org side would advocate for but does not know the platform team would accept is marked `NEGOTIATE`. A contract with three `NEGOTIATE` markers is honest and negotiable; one that pretends the platform team has agreed to numbers they have not is fantasy.
- **Section 4 — no shadow notebooks.** Chapter 5's fifth capability (interference detection) and the review body from exercise-03 depend on the platform being the *source of truth* for the statistical analysis. If ML teams keep private notebooks that recompute what the platform reports, the review body ends up arbitrating between two sets of numbers. Section 4's "consumption discipline" commitment is what forbids that; make it explicit.
- **The metric registry is where composability lives.** Chapter 5's seventh capability is the metric registry. Every metric the platform reports has an owner, a definition, a computation, a unit, a direction of goodness. This is the eval-time counterpart of mod-402's feature-store feature registry; two teams whose metrics do not agree on definitions will have interference disputes the review body cannot arbitrate. The metric registry is what makes the review body's job feasible.
- **Do not include the platform team's roadmap.** Chapter 5 named that as out of scope. The contract says what the ML org requires; the platform team's sequencing of capability delivery is negotiated in the mod-405 platform-strategy conversation. If your Section 3 delivery commitments read like a platform-team roadmap, either move them into the mod-405 companion doc or delete them.
- **Two-to-three pages is fine.** Chapter 5's contract is not long. If it grows past four pages, either implementation detail has crept in (delete) or a capability is being over-specified (compress to a single sentence at the demanded surface).

## Acceptance criteria

- Section 0 names a real counter-party (in-house lead, vendor account team, OSS maintainers, or explicit no-counter-party stipulation).
- Section 1 has a specific effective date, a specific review date (6 months out is the default), and at least two named renegotiation triggers.
- Section 3 covers all seven chapter-5 required capabilities (SRM, CUPED-or-equivalent, sequential testing, guardrail-metric monitoring, interference detection, novelty / primacy, metric registry) each with definition, demanded surface, current state, delivery commitment, consumer usage commitment.
- No entry in Section 3 dictates the platform team's internal implementation; every entry names a surface or capability.
- Sections 4 and 5 cover all four mod-405 chapter-4 categories per side (four consumer-commitment categories, four producer-commitment categories). Every commitment has a number, a named counter-party, or a named accountable role — nothing reads "reasonable" or "timely" without a numeric backstop.
- Any unknown producer-side number is explicitly marked `NEGOTIATE`.
- Section 6 has a specific monthly consumer-review meeting and a specific quarterly steering meeting, with a named standing agenda.
- Section 7 names at least three in-interim options for a capability that is not yet delivered.
- Section 8 has the three-tier escalation staircase with specific named triggers.
- Section 9 has a signature block with ML-org signer, platform-team signer, and named counter-signers.
- The mod-402 Section-9 fold-in is present.
- The contract nowhere includes code snippets, schema definitions, per-team migration steps, or vendor pricing (those live in mod-407, in per-team docs derived from this contract, or in the platform team's own design docs).

## Stretch goals

- **Score the current platform against the contract.** For each of the seven capabilities in Section 3, mark green / yellow / red based on today's state. The resulting 7-row status is the "how far are we from a working contract" summary the first review meeting will consume. If more than four capabilities are red, the contract's delivery commitments dominate the platform team's next-two-quarter roadmap; that is a mod-405 platform-strategy conversation, not just a mod-406 review.
- **Draft the gap-RFC template.** One page. The template the ML org uses to request a new capability from the platform team beyond the chapter-5 seven. Fields: capability name, motivation (which experiment cannot proceed without it), demanded surface, urgency, willingness to co-build. This is the artifact the Section-6 review cadence's item (c) consumes.
- **Simulate the six-month review meeting.** One-page agenda. The Section-3 capability delivery status table, the Section-5 SLO adherence numbers, the gap-RFC backlog, one escalation-path stress-test question ("what would trigger a Tier-2 escalation before the next review?"). This turns Section 6 from an abstract cadence into a rehearsed workflow.
- **Draft the mirror contract for a different platform choice.** If the current counter-party is in-house, draft the same contract for a managed vendor (Statsig, Split, Eppo, Optimizely) as the counter-party. What changes? What stays invariant? The comparison is a check on whether the contract's shape depends on the counter-party — chapter 5 argued the seven required capabilities are invariant, but the delivery commitments and escalation paths are not. Useful preparation for the mod-405 build-vs-adopt conversation.
- **Take the draft to the platform-team counter-party for pre-review.** If the counter-party is a real named team, hand them the contract. For each producer commitment in Section 5, they push back or accept. Absorb the pushback; adjust or mark `NEGOTIATE` more honestly. This is the single highest-signal way to catch producer-side commitments that would not survive a real negotiation.
- **Cross-reference to mod-407.** In an appendix, name the capabilities in Section 3 whose delivery has a material cost (managed vendor licensing, in-house engineering headcount, on-call load). Point forward to the mod-407 TCO conversation. Chapter 5 explicitly deferred cost to mod-407; naming the cost pointers here keeps the two conversations from drifting.
- **Draft the exec one-liner.** In one sentence at the top of the doc: what the ML org requires from the platform, when it takes effect, what breaks if it does not. "The ML org requires seven experimentation-platform capabilities by end-Q3; without SRM diagnostics and interference detection, the cross-team review body cannot arbitrate two-team disputes and the eval-metric-conflict cell reopens." That sentence is what the director reads before signing.
