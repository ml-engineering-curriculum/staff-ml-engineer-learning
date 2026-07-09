# mod-405-ml-platform-strategy: ML Platform Strategy at Org Scope — Build vs. Adopt, Contribute-Back, Consumer Contract

**Estimated effort:** 12 hours

The consumer-side counterpart to the peer platform tracks' paved-road build. Names what "ML platform strategy at org scope" is as scope of work, catalogues the five components an ML org's platform-strategy conversation lives against (feature store, model registry, eval platform, training platform, inference gateway), hands the learner the build-vs-adopt rubric with its seven weighted criteria and two worked examples, walks the paved-road consumer contract that turns a verdict into a durable partnership with the peer platform team, and closes on the two RFC shapes — the multi-team gap RFC and the contribute-back RFC — that carry specific asks and offers across the consumer-producer line.

Builds on [`mod-401-staff-ml-role-scope`](../mod-401-staff-ml-role-scope/) — specifically the Architect and Right Hand archetypes and the hand-off contracts to peer platform tracks — and on [`mod-402-multi-team-ml-architecture`](../mod-402-multi-team-ml-architecture/), whose portfolio blueprint's painful substrate cells become the input to this module's build-vs-adopt calls. Feeds forward into [`mod-406-cross-team-eval-and-experimentation`](../mod-406-cross-team-eval-and-experimentation/) (which owns the eval-platform charter this module names as scope) and [`mod-407-capacity-cost-and-tco`](../mod-407-capacity-cost-and-tco/) (which owns the FinOps and TCO discipline this module's rubric references), and into the capstone [`project-401-multi-team-ml-blueprint`](../../projects/project-401-multi-team-ml-blueprint/).

## Learning objectives

- Make org-scope build-vs-adopt calls on the feature store, model registry, eval platform, training platform, and inference gateway; enumerate the criteria (team size, differentiation, TCO, vendor lock-in, time-to-adoption, contribution ergonomics, exit cost).
- Negotiate the paved-road roadmap with the peer platform teams (`ai-infra-ml-platform-learning`, `ai-infra-mlops-learning`) — set the consumer contract that says what the ML org guarantees to adopt in exchange for what.
- Author the multi-team gap RFC that argues for a specific paved-road feature the platform team should build, with defended business impact and adoption commitment.
- Author the contribute-back RFC that a peer platform team can review and act on — including the ownership, on-call, and API-stability contract.

## Lecture chapters

1. [`01-what-platform-strategy-means-at-org-scope.md`](01-what-platform-strategy-means-at-org-scope.md) — What "ML platform strategy at org scope" is as a scope of work. The consumer-vs-producer framing, the Architect / Right Hand archetype overlap, the "am I doing platform-strategy work?" heuristic, and how the module differs from the mod-402 portfolio blueprint and from the peer platform tracks' actual build work.
2. [`02-the-five-platform-components.md`](02-the-five-platform-components.md) — The five components the platform-strategy conversation lives against: feature store, model registry, eval platform, training platform, inference gateway. What each is, what "adopt vs build" means for each, and what the peer platform tracks own for each.
3. [`03-build-vs-adopt-decision-rubric.md`](03-build-vs-adopt-decision-rubric.md) — The seven-criteria weighted rubric (team size, differentiation, TCO, vendor lock-in, time-to-adoption, contribution ergonomics, exit cost). The scoring template with per-component weight suggestions. Two worked examples: a small org adopting managed Tecton for the feature store, and a large org building a bespoke inference gateway. Pairs with exercise-01.
4. [`04-the-paved-road-consumer-contract.md`](04-the-paved-road-consumer-contract.md) — What a paved-road consumer contract looks like. The three-column responsibility matrix. The consumer-side commitments (adoption volume, feedback cadence, consumption discipline, migration) and the producer-side commitments (API stability window, on-call SLO, migration support, gap-RFC responsiveness). The eight-section contract template. Pairs with exercise-02.
5. [`05-multi-team-gap-and-contribute-back-rfcs.md`](05-multi-team-gap-and-contribute-back-rfcs.md) — The two RFC shapes with section-by-section templates. The gap RFC's nine sections and its business-impact and adoption-commitment discipline. The contribute-back RFC's ten sections and its ownership-contract discipline. When contribute-back is right vs. when fork is right. Pairs with exercises 03 and 04.

## Exercises

- [`exercises/exercise-01-build-vs-adopt-decision-rubric.md`](exercises/exercise-01-build-vs-adopt-decision-rubric.md) — Score a real component (feature store, model registry, eval platform, training platform, or inference gateway) on the chapter-3 rubric and author the decision doc. 3 hours.
- [`exercises/exercise-02-paved-road-consumer-contract-authoring.md`](exercises/exercise-02-paved-road-consumer-contract-authoring.md) — Author the paved-road consumer contract for the component whose build-vs-adopt call was made in exercise-01, using the chapter-4 template. 3 hours.
- [`exercises/exercise-03-multi-team-gap-rfc.md`](exercises/exercise-03-multi-team-gap-rfc.md) — Author a gap RFC against a specific missing paved-road feature using the chapter-5 nine-section template. 3 hours.
- [`exercises/exercise-04-contribute-back-rfc-with-ownership-contract.md`](exercises/exercise-04-contribute-back-rfc-with-ownership-contract.md) — Author a contribute-back RFC for one ML-org-built component using the chapter-5 ten-section template with the ownership-contract section. 3 hours.

## Structure

```
mod-405-ml-platform-strategy/
├── 01-…md … 05-…md    lecture chapters
├── exercises/          per-exercise prompts (solutions live in the paired solutions repo)
├── labs/               long-form hands-on labs (planned)
├── quizzes/            knowledge checks (planned)
├── resources.md        curated external references
└── README.md           this file
```

## Suggested sequencing

Read chapters 1 and 2 in one sitting; they establish the consumer-vs-producer framing and the five-component vocabulary the rest of the module runs on. Take a break, then read chapter 3 and immediately begin exercise-01 — the rubric is a skill that decays if you read the chapter without applying it. Chapters 4 and 5 sit closer to the RFC-authoring craft; read them together and take exercises 02, 03, and 04 in a tight cluster (typically over a single week) so the component choice from exercise-01 carries through all four artifacts.

The four exercises assemble into the platform-strategy section of project-401's capstone deliverable. Learners planning project-401 should keep the component they pick in exercise-01 consistent with the portfolio they carried through mod-402.

## What comes next

- **[`mod-406-cross-team-eval-and-experimentation`](../mod-406-cross-team-eval-and-experimentation/)** — the deep-dive on the eval-and-experimentation platform strategy this module names as component 3; owns the offline eval harness charter and the LLM-as-judge policy.
- **[`mod-407-capacity-cost-and-tco`](../mod-407-capacity-cost-and-tco/)** — the FinOps and TCO discipline this module's build-vs-adopt rubric depends on for its cost analysis; owns the CFO-readable TCO model.
- **[`project-401-multi-team-ml-blueprint`](../../projects/project-401-multi-team-ml-blueprint/)** — the capstone that combines this module's build-vs-adopt calls, consumer contract, and RFCs with mod-402's blueprint and mod-407's TCO model into a portfolio-grade deliverable.

## Related tracks

- [`senior-ml-engineer-learning`](https://github.com/ml-engineering-curriculum/senior-ml-engineer-learning) (L30) — the prerequisite single-team tech-lead track; see [`PREREQUISITES.md`](../../PREREQUISITES.md).
- [`ai-infra-ml-platform-learning`](https://github.com/ai-infra-curriculum/ai-infra-ml-platform-learning) — the peer platform track that owns the paved-road *build* for the feature store, training platform, and inference gateway; this module owns the *consumer contract* side of the conversation.
- [`ai-infra-mlops-learning`](https://github.com/ai-infra-curriculum/ai-infra-mlops-learning) — the peer platform track for MLOps tooling (model registry operations, experimentation platform build); this module's contracts are with them for those components.
- [`ai-infra-performance-learning`](https://github.com/ai-infra-curriculum/ai-infra-performance-learning) — the peer platform track for inference performance; this module's inference-gateway contract references their per-model latency and throughput work.
- [`ai-infra-team-lead-learning`](https://github.com/ai-infra-curriculum/ai-infra-team-lead-learning) — the peer L40 EM track; escalation paths in the consumer contract flow through the peer EM partnership from mod-401 chapter 5.
