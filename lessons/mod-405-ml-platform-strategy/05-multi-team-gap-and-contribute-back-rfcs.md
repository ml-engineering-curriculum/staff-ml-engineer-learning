# The Gap RFC and the Contribute-Back RFC: Two Shapes, One Contract

## Motivation

The consumer contract from chapter 4 is the standing agreement. But two specific events shift the paved-road relationship enough that they need their own RFCs, each with its own section-by-section template. Both cross the consumer / producer line explicitly. Both are staff-scope artifacts; both are signed on the ML-org side by the Staff engineer whose name goes on the promotion packet.

- **The gap RFC** — the ML org tells the platform team: *the paved road is missing feature X. Here is the business impact. Here is our committed adoption volume. Please build it in quarter N.*
- **The contribute-back RFC** — the ML org tells the platform team: *we built thing Y because you had not yet. Here is a proposal to hand it to you, with an ownership contract that makes acceptance a partnership rather than a giveaway.*

Both RFCs fail in specific ways when written by learners who are new to the artifact. The gap RFC's classic failure is that it reads like a complaint — asking for the feature without offering the adoption commitment or business-impact evidence that would make the platform PM prioritise it. The contribute-back RFC's classic failure is that it reads like a hand-off — offering the code but not the operational contract that makes the platform team's acceptance rational.

This chapter walks both. Each is paired with an exercise (exercise-03 and exercise-04 respectively).

## What the two RFCs are (and when to reach for which)

> The **gap RFC** is a written proposal by the ML org to the peer platform team requesting a specific paved-road feature be built, argued with business-impact evidence, committed adoption volume, a proposed quarter of delivery, and the ML-org side's investment (design review, integration effort, dogfooding).

> The **contribute-back RFC** is a written proposal by the ML org to the peer platform team offering an ML-org-built component or feature as an addition to the paved road, argued with adoption case, and paired with a written ownership contract covering code ownership, on-call, API-stability commitment, and migration support for existing homegrown users.

**When to reach for the gap RFC.** The ML org's build-vs-adopt rubric (chapter 3) landed on adopt, the consumer contract (chapter 4) is in force, and the ML org hit a specific missing feature in the paved road that blocks a portfolio-scope roadmap. The right response is a gap RFC — not a homegrown fork of the platform's substrate, not a slack complaint.

**When to reach for the contribute-back RFC.** The ML org built a thing (because chapter 3's rubric landed on build, or because the platform team declined to build it via a rejected gap RFC), it has proven itself with two-plus adopting teams inside the ML org, and the ML org has concluded — via the "when contribute-back is right" analysis later in this chapter — that long-term stewardship belongs on the platform team.

**When to reach for neither.** If the gap is small and can be worked around with a client-side adapter, an RFC is the wrong tool. If the built thing is deeply coupled to the ML org's product model in a way the platform team could not sensibly own, the right verdict is *fork* — the ML org keeps the code, keeps the on-call, and documents the fork explicitly so no future engineer mistakes it for a platform component. Not every homegrown component wants to be contributed back.

## Section template — the gap RFC

The gap RFC is Rust-RFC-shaped ([`rust-lang/rfcs`](https://github.com/rust-lang/rfcs)) with three ML-consumer additions: business impact, adoption commitment, and integration investment. The template has nine sections. Aim for five-to-eight pages total.

**Section 1 — Summary.** One paragraph naming the feature, the platform component (feature store, model registry, etc.), the target quarter, and the ML-org sponsor (Staff engineer). If the reader reads only this section, they should be able to say "yes, this is worth the roadmap slot" or "no, and here's why."

**Section 2 — Motivation.** Why the feature is needed. Named ML use case — "the fraud model's real-time-freshness requirement is not met by the current feature store's 5-minute refresh SLA" — not abstract framing.

**Section 3 — Business impact.** The load-bearing section, and the one gap RFCs most often bungle. Named business impact: cost (this feature saves N engineer-months across the ML org per year), revenue (this feature unlocks a product experience worth $M in projected impact), risk (this feature closes a governance gap the org has committed to close in Y quarter). If you cannot state the impact in one of those three languages, the RFC is not ready for review; the platform team is not being asked to do charity, and a business case they can carry to their exec makes their acceptance decision defensible.

**Section 4 — Design proposal.** The proposed feature at API sketch level — signatures, behaviour, error modes, compatibility with the existing surface. This is not a design *specification*; the platform team owns the specification. The ML org's design proposal is enough for the platform team to size the work and identify design questions.

**Section 5 — Adoption commitment.** The other load-bearing section. Named: which ML teams commit to adopting the feature within N months of release, on what use cases, at what expected volume. Signed by those teams' tech leads or the ML-org Staff engineer on their behalf. Without adoption commitment, the RFC is a wish; with adoption commitment, it is a business case.

**Section 6 — Integration investment.** What the ML org will do to make integration cheap for the platform team: dogfooding (the ML org runs the alpha internally), staff-level design review (N hours from named engineers), documentation-and-runbook contribution, migration codemods where applicable. This is where the ML org tells the platform team: *we are not just asking; we are investing.*

**Section 7 — Alternatives considered.** What the ML org considered before writing this RFC. Includes: the client-side workaround the ML org rejected and why; the OSS alternative the ML org could adopt directly and why the platform-team version is preferred; the fork-and-maintain-ourselves option and why the RFC concluded contribute-later would be worse than build-now.

**Section 8 — Risks and unknowns.** The three-to-five risks the platform team should know about. Includes: adoption risk (the committing teams could change priorities), design risk (the proposed API might collide with an existing paved-road pattern), operational risk (the feature increases the platform's on-call load).

**Section 9 — Timeline and review triggers.** Requested target quarter, the ML org's review triggers (if the platform team cannot commit, what alternative the ML org will pursue and by when), and the escalation path if the RFC is rejected the ML org considers business-critical.

Common gap-RFC failure modes:

- **Missing business impact.** The section reads "this would be useful." The platform PM's queue has ten such RFCs. Without a specific impact language, the RFC will sit.
- **Missing adoption commitment.** The RFC asks the platform team to build the feature "so teams can adopt it if they choose." Every one of those RFCs gets deprioritised for the ones with signed adopters.
- **Bundled requests.** The RFC asks for four unrelated features at once. Split into four gap RFCs; each gets independently scored.
- **Section 4 as specification.** The RFC dictates the API. The platform team owns the API. The RFC's Section 4 is a proposal, not a mandate; if the platform team's design differs materially, the ML org reviews it in return.

## Section template — the contribute-back RFC

The contribute-back RFC is longer and harder. It has ten sections. Six-to-ten pages is typical.

**Section 1 — Summary.** One paragraph naming the component, the current adopters inside the ML org, the proposed hand-off quarter, and the ML-org sponsor.

**Section 2 — Motivation.** Why contribute back rather than keep as ML-org-owned. Named: multiple non-ML-org consumers could benefit (analytics org, fraud org, etc.); the ML org's continued ownership becomes a scope-drift risk at chapter 1's *silently-becoming-the-platform-team* failure mode; the platform team is better positioned to sustain and evolve it.

**Section 3 — Component description and current state.** What the component does; who currently uses it; what its current SLO looks like; what its documentation state is; what technical-debt items the ML org has been carrying. Honest about limitations; the platform team will discover them post-acceptance if not disclosed.

**Section 4 — Adoption case beyond the ML org.** Named: which non-ML-org consumers benefit; what interviews have been done to validate this (yes, the ML org must have done the interviews); whether those consumers have counter-signed as pilot adopters. Without this, the RFC is really a fork-the-ML-org's-code proposal, and the platform team should reject it.

**Section 5 — Proposed API and technical shape.** The current API, the proposed post-hand-off API (which may include the platform team's suggested changes on acceptance), and the migration story from the current API for existing ML-org adopters.

**Section 6 — Ownership contract.** The load-bearing section. Named:

- **Code ownership.** Post-acceptance, the platform team owns the code. Names the specific repo, the CODEOWNERS entries, the review-approval requirements.
- **On-call.** Post-acceptance, the platform team's on-call rotation covers the component. The ML org's engineers rotate off — with an explicit transition-period commitment (typically 2 quarters) where ML-org engineers remain in a secondary escalation role.
- **API stability commitment.** The platform team applies its paved-road stability window to the component (see chapter 4's producer commitments).
- **Migration support for existing homegrown users.** ML-org teams that were consumers of the pre-hand-off version get a migration plan and the platform team's tooling support to get onto the post-hand-off version.
- **Feature-request routing.** Post-acceptance, feature requests from any consumer (including the ML org) become gap RFCs against the platform team's roadmap, not direct implementation asks against the ML-org engineers who originally wrote the code.

**Section 7 — Transition plan.** Named: what happens in the two-to-three quarters after acceptance. Repo transfer date, on-call transition date, staff-time budget from both sides during the transition, exit criteria for calling the transition complete.

**Section 8 — Sunset alternative.** What happens if the platform team declines. The ML org's options: keep-and-maintain (the current state), fork-and-freeze (stop investing, migrate off), or shut down and adopt the closest OSS alternative. Naming this section is important because it makes the contribute-back a *bilateral* offer — the platform team can decline without leaving the ML org holding an unowned artefact.

**Section 9 — Risks.** The three-to-five risks the platform team should evaluate. Includes: operational risk (the component's SLO history is imperfect), scope creep (the platform team may inherit an unbounded feature-request stream), maintenance backlog (technical debt the platform team inherits).

**Section 10 — Signatures and review triggers.** ML-org signatures (Staff engineer, ML-org EM if operational commitments cross team lines); requested platform-team review timeline (typically 4-6 weeks); escalation path if the review overruns.

Common contribute-back failure modes:

- **Ownership contract too thin.** The RFC offers the code but says on-call and stability commitments are "to be worked out." That RFC gets rejected — correctly. The platform team is not signing a blank cheque.
- **Adoption case too narrow.** The RFC's Section 4 names only the ML org as consumer. That RFC is asking the platform team to inherit code the ML org is the only user of. It should be labelled *transfer to platform ownership* rather than *contribute back*, and the platform team's decision changes.
- **Undisclosed debt.** The component has three critical open bugs the ML org has been managing informally. Section 3 does not name them. When the platform team discovers them post-acceptance, the trust between the two staff-plus ICs takes damage that outlasts the specific incident.
- **No sunset alternative.** Section 8 is missing; the RFC reads as ultimatum ("accept this or the ML org will suffer"). Platform teams read that as a scope smell and often reject on procedural grounds.

## When contribute-back is right — and when fork is right

Not every homegrown ML-org component belongs on the paved road. The decision test:

**Contribute-back if:**

- More than one consumer group (beyond the ML org) would benefit from the component being paved-road, and the interviews in Section 4 validate this.
- The component's shape is generic enough that the platform team could sensibly evolve it — not deeply coupled to the ML org's specific product model.
- The ML org's ongoing ownership carries the scope-drift risk from chapter 1 (the ML org is silently becoming the platform team).
- The platform team has bandwidth to accept, or at least the willingness to prioritise in a near quarter.

**Fork (keep as ML-org-owned) if:**

- The component is intrinsically coupled to the ML org's product model — e.g., an eval harness with product-specific slice definitions the platform team could not sensibly own.
- The consumer base is entirely inside the ML org and shows no signs of expanding.
- The component's evolution cadence is fast enough that a platform-team roadmap slot would slow it down materially.
- The platform team has already rejected a gap RFC for the same shape; contribute-back is being used to route around the rejection, which is a political move that damages both sides.

**Adopt-OSS-and-sunset if:**

- An OSS equivalent has become available since the ML org built the component.
- The ML org's version is not materially better than the OSS equivalent.
- The consumer contract with the OSS project (via the platform team) is achievable.

The decision test is itself an RFC section in some larger contribute-back-or-fork decision doc; if the learner is unsure which verdict applies, exercise-04 walks the analysis.

## What Staff owns vs. what the platform team owns for these RFCs

Staff (this module) owns:

- **Drafting both RFCs.** The ML-org side is the author; the RFC bears the Staff engineer's name.
- **Business-impact and adoption-commitment evidence.** For gap RFCs — collecting the numbers, signing up the adopters. For contribute-back RFCs — the adoption case beyond the ML org.
- **The ownership contract in a contribute-back RFC.** Drafted by the ML-org Staff engineer, subject to platform-team review; the platform-team staff-plus IC may push back on specific terms and the two negotiate.
- **The sunset alternative.** The ML org's answer to "what happens if this is rejected" is the ML org's own to decide.

The peer platform track owns:

- **Reviewing and accepting / rejecting.** The platform team's staff-plus IC leads the review; the platform-team PM and EM sign as appropriate.
- **Sizing the work.** For gap RFCs — the engineering estimate, the quarter it fits into. For contribute-back RFCs — the transition-cost estimate, the ongoing maintenance load.
- **Counter-proposal on API shape and ownership terms.** Where the platform team's expertise says the ML org's proposal will not scale — a common outcome; not an insult — the counter-proposal is theirs to make.

## The artifacts this chapter produces

Two artifacts, one per exercise:

- The **multi-team gap RFC** (exercise-03) — five-to-eight pages, nine sections, one specific feature-store / registry / eval / training / gateway gap, with business impact and adoption commitment.
- The **contribute-back RFC** (exercise-04) — six-to-ten pages, ten sections, one ML-org-built component being offered to the platform team, with an ownership contract that names code, on-call, API stability, migration, and feature-request routing.

Both artifacts are the concrete outputs of mod-405. Together with the build-vs-adopt decision doc (exercise-01) and the consumer contract (exercise-02), they are the four documents the Staff engineer's promotion packet points to when the review committee asks "what did they own at platform-strategy scope."

## Summary

- Two staff-scope RFC shapes carry the load beyond the standing consumer contract: the **gap RFC** (consumer asks producer to build) and the **contribute-back RFC** (consumer offers producer a built thing plus an ownership contract).
- The gap RFC's two load-bearing sections are business impact and adoption commitment; missing either turns the RFC into a wish list.
- The contribute-back RFC's load-bearing section is the ownership contract — code, on-call, API-stability commitment, migration, feature-request routing. Missing any turns the RFC into a hand-off the platform team correctly rejects.
- Not every homegrown component wants to be contributed back. **Fork** is the right verdict when the component is intrinsically ML-org-coupled, when the consumer base is not expanding, or when the platform team has already declined the gap RFC. Contribute-back used to route around a rejection is a political misfire.
- Staff owns drafting, business-impact evidence, adoption commitments, and the ownership contract. The peer platform track owns the review, the sizing, and the counter-proposal on API shape.

That closes the substantive chapters of mod-405. The next files in this module are the exercises — [`exercise-01`](exercises/exercise-01-build-vs-adopt-decision-rubric.md) walks the rubric on a real component, [`exercise-02`](exercises/exercise-02-paved-road-consumer-contract-authoring.md) drafts the consumer contract, [`exercise-03`](exercises/exercise-03-multi-team-gap-rfc.md) authors the gap RFC, and [`exercise-04`](exercises/exercise-04-contribute-back-rfc-with-ownership-contract.md) authors the contribute-back RFC. The four documents feed into [`project-401`](../../projects/project-401-multi-team-ml-blueprint/)'s capstone deliverable.
