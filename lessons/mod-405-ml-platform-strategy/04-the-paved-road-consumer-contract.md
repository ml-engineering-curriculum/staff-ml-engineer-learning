# The Paved-Road Consumer Contract: What the ML Org Guarantees, What the Platform Team Guarantees Back

## Motivation

Chapter 3's rubric produces a verdict — adopt-managed, adopt-OSS, or (the hardest case) build-and-hand-to-the-platform-team. In every case except pure "build-inside-the-ML-org" the verdict has a counter-party: the peer platform team (`ai-infra-ml-platform-learning`, `ai-infra-mlops-learning`), or the OSS maintainer body, or the managed vendor. That counter-party is being asked to invest time, roadmap slots, or account-management effort in the ML org's needs. Without a written contract, the two sides silently assume the other owns cells in the middle of the responsibility diagram — the failure state chapter 1 named.

The **paved-road consumer contract** is the artifact that makes the trade explicit. It is what turns a build-vs-adopt verdict from a design decision into a durable partnership. In Team Topologies (Skelton & Pais, 2019) language, it is the *interaction mode* contract between the stream-aligned ML teams and the platform team. In Google SRE language, it is a two-sided SLA: the platform commits to SLOs; the consumer commits to consumption discipline.

Two failure modes the contract prevents:

- **The platform team ships a paved road nobody adopts.** The classic form: the platform team spends two quarters building the feature store; the ML org's teams say "we'll evaluate it next quarter" — every quarter. Contract-less adoption is a coin flip.
- **The ML org adopts and then complains.** Almost as classic: teams adopt on their own schedule, hit an edge case, and file complaint tickets rather than gap RFCs (chapter 5). Without a contract, both sides are pattern-matching complaints against roadmap; nobody agrees on what the trade was.

## What "consumer contract" means here

> A **paved-road consumer contract** in this module is a written, staff-signed agreement between the ML org (consumer) and the platform team (producer) that names, for one platform component: what adoption volume and shape the ML org commits to, what feedback and review time the ML org commits to invest, what API stability window and on-call SLO the platform team commits to, and what migration support the platform team ships when it changes the API in a breaking way.

Three things fall out:

**It is per component.** Chapter 3's rubric was per component; the contract is per component too. A blanket "the ML org will adopt the platform" contract is meaningless — the feature-store adoption trade is different from the inference-gateway adoption trade. Every component the ML org adopts gets its own contract.

**It is bilateral and signed.** Signed by the Staff engineer on the ML-org side and the equivalent staff-plus IC (or Principal or platform-team EM) on the platform-team side. Unsigned contracts are wishes. The signing ceremony is what changes the two sides' behaviour.

**It is time-bounded.** Two to four quarters, with a scheduled review. Not "forever" — the contract will be renegotiated when the ML org's portfolio shifts or the platform team's roadmap changes.

## The three-column responsibility matrix

Before the contract itself, the two sides sit down and fill in a three-column matrix:

| Cell                             | ML org owns | Platform team owns | Shared / negotiated |
| -------------------------------- | ----------- | ------------------ | ------------------- |
| Deciding to adopt                | ✓           |                    |                     |
| Migration schedule per ML team   | ✓           |                    | ✓ (support cadence) |
| API design                       |             | ✓                  | ✓ (staff review)    |
| Implementation                   |             | ✓                  |                     |
| Deprecation notice window        |             | ✓                  |                     |
| Sunset execution per team        | ✓           |                    | ✓ (tooling)         |
| Consumption discipline           | ✓           |                    |                     |
| On-call SLO for the substrate    |             | ✓                  |                     |
| Escalation path when SLO breached|             |                    | ✓                   |
| Feature gap discovery            | ✓           |                    | ✓ (gap RFC review)  |
| Feature gap prioritisation       |             | ✓                  |                     |
| Contribute-back offers           | ✓           |                    | ✓ (acceptance)      |

The matrix is not the contract. It is the pre-contract exercise both sides run so nobody argues about column boundaries during contract drafting. Do this matrix in the room with the platform-team counter-party physically or synchronously present. Async matrix-filling produces the *default* answers — both sides pick "shared" for anything ambiguous — and shared is where drift lives.

## The ML org's commitments (what the consumer guarantees)

Four categories of commitment the ML org's Staff engineer signs on:

### 1. Adoption volume and shape

- **How many teams adopt on what timeline.** Named: "Teams A, B, C adopt in Q1; Teams D, E in Q2; Team F remains on the legacy path with an explicit deprecation date." Not "we'll roll it out to everyone eventually."
- **Adoption depth per team.** Named: "Team A uses the online serving path; Team B uses offline only; Team C uses both online and offline plus custom transformations." Depth matters because the platform team's roadmap depends on which paths are load-bearing.
- **Exclusive adoption.** Named: "adopting teams commit to not maintaining a parallel homegrown implementation." Without this, adopting teams keep their homegrown fallback and the platform team's dogfooding signal is corrupted.

### 2. Feedback and review time

- **Staff-level API review time.** The ML org's Staff engineer commits N hours per month (typically 4-8) reviewing the platform team's API changes. Not "we'll be available"; a specific budget the ML org's staffing plan accounts for.
- **Adoption-quality feedback.** The ML org runs a monthly consumer-review meeting where each adopting team surfaces friction; the aggregated notes go to the platform-team PM.
- **Bug-report and gap-RFC discipline.** When the ML org hits a gap, the team's tech lead files a gap RFC (chapter 5), not a "the platform is broken" slack thread. The ML org's Staff engineer commits to catching the second style and re-routing to the first.

### 3. Consumption discipline

The consumer side of an SLO. Platform teams' SLOs implicitly assume well-behaved consumers.

- **Rate-limit compliance.** Adopting teams respect the platform's published QPS quotas per model / per team.
- **Retry and back-off discipline.** No unbounded retries against a struggling platform substrate. Exponential back-off is table stakes.
- **Version-pinning where required.** If the platform team publishes "pin your client to a minor version and accept patch upgrades automatically," adopting teams comply.
- **Observability contribution.** Adopting teams emit the platform-required telemetry (client-side latency histograms, error breakdowns) so the platform team can debug consumer-side incidents.

### 4. Migration commitment

- **Named migration owner per team.** When the platform team ships a breaking API change with a migration path, each adopting team names an owner. Not "someone from Team A."
- **In-window migration.** Migrations complete inside the deprecation window (the platform team's commitment; see below). Missing the window has an escalation cost the contract names.
- **Sunset of the legacy path.** After migration, the old implementation is removed from adopting teams' codebases. Not "we'll clean it up when we have time."

## The platform team's commitments (what the producer guarantees)

Four categories the platform-team staff-plus IC signs on. These are what makes adoption a rational move for the ML org's teams — without them, adopting is worse than a homegrown build the team controls.

### 1. API stability window

- **Semver-shaped stability.** Named: major versions bring breaking changes; minor bring additions; patch brings fixes. Enforced by the platform team's CI, not just by convention.
- **Breaking-change deprecation window.** Named: N months (typically 6 or 12 for staff-scope contracts) between a breaking-change announcement and enforcement. Long enough for consumers to fit migration into a normal quarter.
- **Backward compatibility for supported versions.** The platform team commits to running at least the current and one prior minor version in parallel during the deprecation window.

### 2. On-call SLO

- **Substrate availability SLO.** Named: 99.9% availability for the online path, 99.5% for the offline path (numbers illustrative — real numbers come from the platform team's error budget). Includes what "availability" means precisely — a latency-and-error definition.
- **On-call response SLO.** Named: 15-minute page-to-first-response for SEV-1 during business hours, 30-minute page-to-first-response for SEV-1 off-hours. Not "we have on-call"; the response commitment.
- **Communication SLO during incidents.** Named: consumer-facing status updates every 30 minutes during ongoing SEV-1; a written postmortem within 5 business days; consumer input into the postmortem's action items.

### 3. Migration support

- **Migration tooling.** For every breaking API change, the platform team ships a migration guide, an automated codemod where feasible, and a validation harness the consumer teams run to check their migration.
- **Staffed migration liaison.** Named: N hours per week from a platform engineer during the deprecation window to unblock migration questions.
- **Extension protocol.** Named: if a consumer team needs an extension past the deprecation window, the escalation path (usually to the platform-team EM and the ML-org EM together) and the cost (usually the consumer team paying the platform-team's incremental cost of continuing to maintain the old path).

### 4. Roadmap transparency and gap-RFC responsiveness

- **Published roadmap.** The platform team publishes its next two quarters' roadmap; the ML org sees it in advance of internal planning; there is no *surprise* about what the platform will and won't ship.
- **Gap-RFC review SLA.** Named: gap RFCs from the ML org get a written response within N weeks (typically 4-6), which is either "accepted for quarter X," "rejected with reason," or "held pending information." Silence is not an option.
- **Roadmap slot allocation.** Named: some fraction of the platform team's quarterly capacity (typically 20-30%) is committed to consumer-driven work — gap RFCs, migrations for existing consumers, and reactive support — rather than the platform team's own initiatives.

## The contract template

The contract itself is a document of typically three-to-five pages. Sections:

**Section 1 — Component and scope.** One paragraph naming the component (feature store, model registry, etc.), the specific implementation adopted (Tecton, in-house v2, KServe, etc.), and the ML-org portfolio boundary (which teams are in scope, which are explicitly out).

**Section 2 — Contract term.** Effective date, review date (typically 6 months out), renewal / renegotiation trigger.

**Section 3 — Responsibility matrix.** The three-column table from earlier in this chapter, populated.

**Section 4 — Consumer commitments.** The four categories above, each with specific commitments named.

**Section 5 — Producer commitments.** The four categories above, each with specific commitments named.

**Section 6 — Escalation and dispute resolution.** When SLOs are breached, when a migration overruns, when a gap RFC is rejected the ML org considers critical — the named escalation path. Typically: ML-org Staff → platform-team Staff → both EMs → both directors.

**Section 7 — Success metrics and review triggers.** What does "the contract is working" look like six months out — adoption percentage, incident count, gap-RFC turnaround. What triggers an out-of-cycle review — a SEV-1 outage, a doubling of gap-RFC backlog, a platform-team roadmap change that affects adoption.

**Section 8 — Signatures.** Named signers on both sides. This is not ceremonial; the signature is what makes the escalation path work.

Do not include: technical implementation details (those live in the platform team's design docs), pricing (contract lives above the FinOps layer; mod-407 owns pricing), individual-team migration steps (those live in per-team migration docs derived from this contract). The contract is a staff-scope agreement; details belong in the sibling documents.

## Two failure modes to avoid when authoring

- **Over-specification.** A contract that names 40 SLOs and 30 consumer commitments is a contract nobody enforces. Aim for 6-10 commitments per side; each measurable in a single monthly review meeting.
- **Under-specification.** A contract that says "the platform team will provide reliable service" is not a contract. Reliability without a number is aspiration; SLO with a number is enforceable. If you find yourself writing "reasonable," "timely," or "as appropriate," go back and put a number on it.

## What Staff owns vs. what the peer platform track owns

Staff (this module) owns:

- **Drafting the consumer side of the contract.** Adoption volume, feedback cadence, migration commitment, consumption discipline.
- **Negotiating the producer side.** Not writing it — the platform-team staff-plus IC drafts producer commitments — but making sure they are legible, measurable, and match what the ML org needs.
- **Signing the contract on the ML-org side.** Under the staff-plus IC's name, with the ML-org director's counter-sign (typical for contracts touching more than one team).
- **Holding the ML org accountable to consumer commitments.** When Team B misses a migration deadline, the Staff engineer runs the escalation, not the platform-team PM. This is the load-bearing part of the ownership.

The peer platform track owns:

- **Drafting the producer side.** The SLOs, deprecation windows, migration tooling commitments.
- **Signing on the platform-team side.** Under their staff-plus IC's name.
- **Holding the platform team accountable to producer commitments.** When the deprecation window is missed, the platform-team Staff engineer runs the escalation.

The peer EM tracks partner as needed for escalation. Mod-401 chapter 5's staff-plus-EM partnership contract is the vocabulary.

## The artifact this chapter produces

The **paved-road consumer contract** — a three-to-five-page signed agreement covering one component. Exercise-02 walks the learner through authoring this for the component whose build-vs-adopt call was made in exercise-01. The exercise expects a redlined draft the learner would actually take into a staff-to-staff negotiation with a peer platform IC.

Contracts on components with adopt-managed verdicts have the platform team as counter-party (the platform team is stitching the vendor in and reselling it internally). Contracts on adopt-OSS have the same shape but with an OSS-community-liaison sub-clause. Contracts on build-inside-the-ML-org do not need one — the ML org is both consumer and producer, and the artifact is a cross-team API contract instead. Chapter 5's contribute-back RFC picks that case up.

## Summary

- The **paved-road consumer contract** is what turns a build-vs-adopt verdict into a durable partnership between the ML org and the peer platform team. It is bilateral, signed, per-component, and time-bounded.
- The consumer commits: adoption volume, feedback cadence, consumption discipline, migration ownership. The producer commits: API stability window, on-call SLO, migration support, gap-RFC responsiveness.
- The three-column responsibility matrix is the pre-contract exercise; do it synchronously with the platform team, not async.
- Six-to-ten commitments per side, each measurable, is the enforceable band. Under-specification means the contract is aspiration; over-specification means nobody enforces it.
- Staff owns drafting the consumer side and holding the ML org accountable to its commitments; the peer platform track owns the producer side.

The next chapter walks the two RFC shapes the contract enables — the multi-team gap RFC (the consumer asking the platform team to build a missing feature) and the contribute-back RFC (the consumer offering an ML-org-built thing back to the platform with an ownership contract).
