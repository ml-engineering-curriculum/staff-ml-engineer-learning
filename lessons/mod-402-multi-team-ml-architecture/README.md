# mod-402-multi-team-ml-architecture: Multi-Team ML Systems Architecture — Portfolios, Shared Substrates, Consolidation

**Estimated effort:** 14 hours

The Architect archetype's centre of gravity in the Staff Machine Learning Engineer track. Names what a "portfolio of ML systems" is as scope of work, hands the learner the blueprint template that walks the portfolio across four shared substrates (data planes, feature layer, model registry, eval contract), catalogues the four portfolio-scope failure modes senior engineers rarely see (pipeline-jungle, duplicated-feature drift, model-registry fragmentation, eval-metric conflicts), teaches the merge / split / share decision framework with an explicit migration- and political-cost model, walks the multi-system RFC template that carries the decision, and finishes on how to run the architecture review that accepts, nudges, blocks, or escalates the RFC.

Builds on [`mod-401-staff-ml-role-scope`](../mod-401-staff-ml-role-scope/) — specifically the Architect archetype, the L30 → L40 altitude vocabulary, and the hand-off contracts to peer platform and peer specialist tracks. Feeds forward into [`mod-405-ml-platform-strategy`](../mod-405-ml-platform-strategy/) (the platform-team side of the substrate conversation), [`mod-406-cross-team-eval-and-experimentation`](../mod-406-cross-team-eval-and-experimentation/) (the eval-contract deep-dive), and the capstone [`project-401-multi-team-ml-blueprint`](../../projects/project-401-multi-team-ml-blueprint/).

## Learning objectives

- Blueprint 3-5 production ML systems as one portfolio — identify shared data planes, shared feature layer, shared model-registry contract, shared eval contract — and articulate the coupling and duplication that emerges.
- Diagnose the portfolio-scope failure modes senior engineers rarely see: pipeline-jungle across teams, duplicated features that drift apart, model-registry fragmentation, eval-metric conflicts.
- Author a multi-system RFC that argues for merge / split / share of a substrate across teams and defends the migration cost and the political cost.
- Run an architecture review across peer staff engineers and platform leads — decide what blocks, what nudges, and what escalates.

## Lecture chapters

1. [`01-what-is-portfolio-scope.md`](01-what-is-portfolio-scope.md) — What "multi-team ML architecture" is as a scope of work. Why the L30 tech-lead's habits break at three-to-five systems. Names the four substrate layers, the Architect archetype's centre of gravity, and the "am I doing portfolio work?" heuristic.
2. [`02-portfolio-blueprint-shared-substrates.md`](02-portfolio-blueprint-shared-substrates.md) — The portfolio × substrate matrix and the cross-system dependency diagram. The four cell labels (`shared`, `duplicated`, `forked`, `unique-by-design`). The seven-question interview that grounds the labels in current reality. A worked three-system consumer example.
3. [`03-portfolio-failure-modes.md`](03-portfolio-failure-modes.md) — The four canonical failure modes: pipeline-jungle across teams, duplicated-feature drift, model-registry fragmentation, eval-metric conflicts. Each with definition, mechanism, three diagnostic questions, and the anti-pattern that looks like it but is not. A fifth failure mode — silent contract erosion — named as a bonus.
4. [`04-merge-split-share-decisions.md`](04-merge-split-share-decisions.md) — The three verdicts (merge / split / share-behind-a-contract), plus the fourth (defer). The five-question decision framework. The migration-cost model. The political-cost model with its three components (ownership loss, roadmap disruption, precedent risk). Two worked examples and the frequency map per failure mode.
5. [`05-multi-system-rfc-authoring.md`](05-multi-system-rfc-authoring.md) — The multi-system RFC template: Rust-RFC-shaped with three multi-team additions (portfolio context, political cost, rollout and rollback). Section-by-section pitfalls. Authoring cadence. The exec one-pager. What a rejected RFC teaches you.
6. [`06-architecture-review-across-teams.md`](06-architecture-review-across-teams.md) — Running the review. The four outputs (accept / nudge / block / escalate) plus defer. The nudge-vs-block rule. Author-side and reviewer-side discipline. Composition of the review body. Handling technical vs. political disagreements. The review as a career signal.

## Exercises

- [`exercises/exercise-01-portfolio-blueprint-first-pass.md`](exercises/exercise-01-portfolio-blueprint-first-pass.md) — Portfolio blueprint first pass on a real three-to-five-system portfolio. 3 hours.
- [`exercises/exercise-02-identify-duplication-and-drift.md`](exercises/exercise-02-identify-duplication-and-drift.md) — Diagnose the `duplicated` and `forked` cells from exercise-01 against the four failure modes. 3 hours.
- [`exercises/exercise-03-merge-split-share-decision-doc.md`](exercises/exercise-03-merge-split-share-decision-doc.md) — Author the decision doc for one painful cell with the five-question framework, migration cost, and political cost. 4 hours.
- [`exercises/exercise-04-multi-system-rfc-authoring.md`](exercises/exercise-04-multi-system-rfc-authoring.md) — Roll the decision doc up into a full multi-system RFC using the chapter-5 template. 3 hours.

## Structure

```
mod-402-multi-team-ml-architecture/
├── 01-…md … 06-…md    lecture chapters
├── exercises/          per-exercise prompts (solutions live in the paired solutions repo)
├── labs/               long-form hands-on labs (planned)
├── quizzes/            knowledge checks (planned)
├── resources.md        curated external references
└── README.md           this file
```

## Suggested sequencing

Read all six chapters in order. Chapters 1-3 are the diagnostic side and can be read in one sitting; chapters 4-6 are the decision-and-shipping side and reward a break in between. Take exercise-01 immediately after chapter 2 (do not wait until you have read all six — the blueprint interview skill decays with delay). Take exercise-02 immediately after exercise-01; the same portfolio should carry through. Exercises 03 and 04 sit on top of the same portfolio and should be sequenced without a long gap.

The four exercises assemble into project-401's capstone deliverable. Learners planning to take project-401 should keep their exercise portfolio choice consistent with their capstone-project scope.

## What comes next

- **[`mod-405-ml-platform-strategy`](../mod-405-ml-platform-strategy/)** — the platform-team side of every substrate decision this module makes; what the portfolio staff engineer defers to the peer platform track.
- **[`mod-406-cross-team-eval-and-experimentation`](../mod-406-cross-team-eval-and-experimentation/)** — the eval-contract deep-dive; where the eval-metric-conflict failure mode from chapter 3 gets its systemic fix.
- **[`project-401-multi-team-ml-blueprint`](../../projects/project-401-multi-team-ml-blueprint/)** — the capstone that combines this module's blueprint and RFC with mod-407's TCO model into one portfolio-grade deliverable.

## Related tracks

- [`senior-ml-engineer-learning`](https://github.com/ml-engineering-curriculum/senior-ml-engineer-learning) (L30) — the prerequisite single-team tech-lead track; see [`PREREQUISITES.md`](../../PREREQUISITES.md).
- [`ai-infra-ml-platform-learning`](https://github.com/ai-infra-curriculum/ai-infra-ml-platform-learning) — the peer platform track that owns paved-road *build*; this module owns the *consumer contract* side of the conversation.
- [`ai-infra-mlops-learning`](https://github.com/ai-infra-curriculum/ai-infra-mlops-learning) and [`ai-infra-performance-learning`](https://github.com/ai-infra-curriculum/ai-infra-performance-learning) — the peer platform tracks for MLOps tooling and inference performance.
- [`ai-infra-team-lead-learning`](https://github.com/ai-infra-curriculum/ai-infra-team-lead-learning) — the peer L40 EM track; chapter 6's review body includes the peer EM per the mod-401 chapter 5 partnership contract.
