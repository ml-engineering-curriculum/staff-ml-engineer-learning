# The Multi-Quarter ML Program Roadmap Leadership Actually Commits To

## Motivation

A "roadmap" at Senior tech-lead scope is usually a Jira epic list — the features the team ships this quarter and possibly next. A roadmap at staff-plus scope is a materially different artifact. It commits *the ML org* over two-to-four quarters to a sequenced set of programs, defends why the sequence is that order, names what the org is *not* doing (and why), and states what has to be true of peer platform teams for the plan to land. Executives read it, commit budget against it, and use it as the frame for every subsequent RFC and hiring conversation for the horizon it covers.

This chapter names what makes a staff-authored multi-quarter roadmap defensible. It grounds heavily on Larson *Staff Engineer* chapter 3 ("Working on what matters"), Rumelt *Good Strategy Bad Strategy* (Crown Business, 2011) on the difference between strategy and a wish list, and Marty Cagan *Inspired* on the product-vs-engineering split of the roadmap conversation. Pairs with exercise-01.

## What a staff-plus roadmap is not

Two shapes routinely masquerade as a staff roadmap and should be named first so they can be avoided.

**A feature list.** A quarter-by-quarter enumeration of the ranking-team's, retrieval-team's, and personalization-team's shipped features. Every line is a real project. There is no defended sequencing beyond "this comes before that because it does". Rumelt calls this **"bad strategy" — the substitute of goals and lists for the diagnosis, guiding policy, and coherent actions that make up an actual strategy**. Rumelt's central claim is that most strategy documents are lists of desirable outcomes without a defence of *why this list, in this order, given this diagnosis of the situation*. The staff-plus roadmap explicitly avoids this.

**A wish list from leadership.** The Director's or VP's slide from the last off-site translated into a per-team commit. This is worse than a feature list because it also encodes assumptions leadership made without technical grounding. The staff engineer's job is to *interpret* the leadership signal, not transcribe it.

The staff-plus roadmap is what sits between the two — a defended sequence of programs that carries a specific technical diagnosis of the portfolio's state and a specific plan for moving it two-to-four quarters out.

## The five columns every staff roadmap answers

At staff-plus scope a multi-quarter ML program roadmap is a document (typically five-to-ten pages plus a one-page Gantt-shaped diagram) that answers, for the two-to-four-quarter horizon:

1. **Diagnosis.** In one page: what shape is the portfolio in today, what are the two-to-three biggest technical constraints, and what changed recently that the leadership team should register? This is Rumelt's "diagnosis" step and it is the load-bearing paragraph the rest of the roadmap defends against.
2. **Programs.** The three-to-six programs the ML org will run over the horizon, each one a paragraph naming the business object, the technical shape, the owning team (or "TBD hire" if the program depends on a hire), and the peer-team dependencies.
3. **Sequencing.** A one-page dependency diagram plus a two-page defence of the order. Why this program before that one? What breaks if we swap them? Which programs can run in parallel and which cannot?
4. **Non-goals.** Explicit, first-class. Two-to-three programs the ML org will *not* start in this horizon, each with a one-paragraph defence. This is the single most-cut section in draft one and the single most-cited section in the exec review.
5. **Resource asks.** Headcount, capacity, budget, platform-team investment. Not a wish list — each ask tied to a specific program above, with a delta ("if we get X, we can ship Y two quarters earlier; if we don't, we defer Y").

Exercise-01 walks the learner through authoring the roadmap end-to-end for a real portfolio. The rest of the chapter drills on the four decisions that reliably go wrong.

## Sequencing as a first-class defence

Sequencing is the load-bearing part of the roadmap. It is also the part that senior engineers newly at staff routinely under-defend. The temptation is to assume the reader will see why program A should precede program B because it is obvious to the author.

Two heuristics for making sequencing defensible on paper:

**Name the dependency, not the priority.** "We ship the shared feature layer before the ranker redesign because the ranker redesign's eval story depends on time-travel-correct features that only the shared layer provides" is a sequencing defence. "The shared feature layer is higher priority than the ranker redesign" is not — it invites the exec to reorder.

**Draw the counterfactual.** For each sequencing choice, write one sentence naming *what breaks if you do them in the opposite order*. If nothing breaks, the sequencing is arbitrary and the reader is right to reorder. If something breaks, name it — usually it is a cost multiplier ("we would migrate the feature layer twice", "we would train the model on a data mixture that we will replace two quarters later") or a coordination cost ("the platform team's Q3 slot is committed to this substrate; if we defer, we lose the slot").

A useful pattern is to write the sequencing section as a two-column table: **program → what breaks if it comes later than we planned**. Every row that answers "nothing" is a program that does not belong on this quarter's plan.

## Non-goals as a first-class output

Rumelt is very direct about this: **strategy without non-goals is bad strategy**. In a portfolio of three-to-five ML systems and a peer platform team, everything looks worth doing, and the roadmap that lists everything worth doing is undeliverable. The two-to-three explicit non-goals do more work in the exec review than any single program on the "will do" list.

Two rules make non-goals actually useful:

**A non-goal has to be something a reasonable reader would expect to see on the "will do" list.** "We are not going to build a new orchestration framework" is not a useful non-goal if no one expected the ML org to do that anyway. "We are not going to redesign the model-registry contract this year, despite the fragmentation the mod-402 blueprint surfaced, because the migration cost lands in the same quarter as the pretraining run and we cannot absorb both" — that is a useful non-goal. It names something a reader plausibly expected and defends why not now.

**Non-goals get a rev-date.** "Not this year" is different from "not ever". Every non-goal names the horizon it applies to and the condition under which it should be re-opened. This is what protects the staff engineer from the exec who comes back in six months asking why the registry-fragmentation problem is still open — the answer is "here is the non-goal, here is the condition we set, here is whether the condition is now met".

## Resource asks tied to a specific delta

Resource asks are where staff-engineer roadmaps most often fail exec review. The failure mode is generic: "we need two more senior ML engineers and 15% more training capacity". No exec can act on that. It reads as a wish and gets deferred.

The pattern that works is to tie every ask to a *program-level delta* on the roadmap. Two examples:

- **Headcount.** "Program C (the eval-standardisation program in mod-406 shape) is currently sequenced for Q4 with our existing team. If we hire one L30 who can own the eval-harness rewrite, Program C moves to Q3 and does not compete with Program D for Q4 attention. If we do not hire, Program C's Q4 sequencing survives and Program D's exec metric slips by a month."
- **Capacity.** "Program A (the mod-403 pretraining program) currently plans against 800 H100 weeks. If we get the FSDP recipe from mod-404 to 40% MFU by the end of Q1 — a peer-specialist dependency called out in Program A row 3 — the 800 H100 weeks holds. If we do not, the delta is 200 H100 weeks and we are asking to defer Program E to make room."

Doerr *Measure What Matters* (2018) frames the same point through OKR discipline: the ask has to be tied to a measurable delta on the outcome. That framing is compatible with the roadmap, and OKRs adopted at staff-plus scope end up looking a lot like the "program → delta" pairs above.

## Dependencies on peer platform teams — the paved-road contract

Every non-trivial staff ML roadmap has at least two dependencies on peer platform teams — [`ai-infra-ml-platform-learning`](https://github.com/ai-infra-curriculum/ai-infra-ml-platform-learning), [`ai-infra-mlops-learning`](https://github.com/ai-infra-curriculum/ai-infra-mlops-learning), [`ai-infra-performance-learning`](https://github.com/ai-infra-curriculum/ai-infra-performance-learning). These dependencies get their own section for two reasons: they are the single biggest source of slip risk on the ML side, and they are the interface with the peer platform teams' own roadmap-review cycle.

The pattern for each platform-team dependency in the roadmap:

- **What we consume.** A concrete platform-team artifact — the feature-store online-serving contract, the model-registry alias policy, the inference-gateway multi-tenant SLO — with the specific version we are committing to.
- **What we commit back.** The consumer commitment the mod-405 chapter 4 hand-off contract requires — adoption timeline, feedback cadence, contribute-back opportunities.
- **When we need it.** The quarter, tied to the program on the roadmap that depends on it.
- **What we do if it slips.** The fallback plan. This is the section that separates staff-plus from senior. A senior tech lead's roadmap says "we depend on the platform team shipping X"; a staff-plus roadmap adds "if X slips by a quarter, program Y falls back to the per-team implementation from mod-402 and we absorb the migration cost in Q4-plus-1".

The peer-platform-team dependencies are why the mod-405 platform consumer contract exists. This chapter's roadmap is the ML org's side of that conversation; mod-405 is the platform-team side.

## When "no" is the roadmap's most important output

Larson's chapter 3 makes a point worth internalising here: at staff scope, **saying no publicly and in writing is one of the highest-leverage things the role does**. The non-goals section carries most of the load, but the roadmap also frequently rejects specific proposals that would otherwise absorb the ML org's slack.

Three shapes of "no" that staff engineers own:

1. **No to a proposed program that duplicates existing capability.** Two teams' senior tech leads independently propose LLM-based rewriter services. The staff-plus response is not "let both build" — it is a roadmap non-goal that names one as the org-wide capability and defers the other, with the technical defence written out.
2. **No to a program leadership asked for.** Rare, high-stakes, and the roadmap is the right venue. "The CEO would like us to ship a customer-facing chatbot in Q3" gets a written response: what is the staff engineer's technical assessment, what would the org drop to make room, and if the answer is still "no", what is the specific technical constraint that makes it so.
3. **No to more scope inside a program.** The mod-403 pretraining program does *not* also cover a real-time serving policy; that goes to the mod-406 eval-standardisation program. Named on the roadmap so no one is surprised.

Every no is written down. Every no cites a condition under which it could be revisited. Reilly calls this *"the ability to disagree in writing"* and it is the practice that keeps the roadmap from becoming an agreement to do everything.

## What Staff owns vs. what the peer L40 EM owns

The mod-401 chapter 5 partnership contract splits the roadmap boundary as follows:

- **Staff owns.** The *content* — diagnosis, programs, sequencing, non-goals, resource-ask shape, peer-platform dependency structure. The technical bets.
- **EM owns.** The *commitment* — quarterly capacity, hiring pipeline against the resource asks, on-call load, delivery-risk framing.
- **Both.** Stand side-by-side in front of the Director / VP in the roadmap review.

A staff engineer who authors a roadmap without EM buy-in has produced a beautiful document no team will commit to. An EM who commits the team to a roadmap the staff engineer has not vetted has committed to programs that may be technically undeliverable. The one-page partnership contract, refreshed once a quarter, is what keeps the two halves aligned.

## Cadence — how often the roadmap changes

The staff-plus roadmap is a *live* document with a specific rhythm:

- **Refresh quarterly.** At the start of each quarter, refresh the horizon — drop the completed quarter, add a new quarter at the far end, re-defend the sequencing.
- **Update ad-hoc when a program changes shape.** A material change to a program's scope, sequencing, or resource ask goes back through the roadmap and gets re-defended. A one-page addendum is fine; the whole roadmap does not need re-authoring.
- **Do not update for small slips.** A two-week slip is not a roadmap event; it is an execution event owned by the EM. Reserving the roadmap for two-to-four-quarter changes protects it from becoming a status report.

The exec review of the roadmap is typically once a quarter, aligned with the org's planning cadence. Between reviews, the roadmap is the reference the staff engineer points at when a new proposal arrives.

## Summary

- A staff-plus multi-quarter roadmap is a defended sequence of programs, not a feature list or a wish transcript. It answers five columns: **diagnosis, programs, sequencing, non-goals, resource asks**.
- **Sequencing** is defended by naming what breaks if the order changes, not by asserting priority. **Non-goals** are first-class outputs — Rumelt's rule that strategy without them is bad strategy applies directly.
- **Resource asks** are tied to a specific program-level delta — hire X, ship program C two months earlier; ship the recipe upgrade, absorb the training capacity ask.
- **Peer-platform dependencies** get an explicit section with what we consume, what we commit back, when we need it, and what we do if it slips.
- Saying **"no" in writing** — to duplicated programs, to leadership-asked programs, to scope creep inside a program — is one of the roadmap's most valuable outputs.
- The **Staff/EM boundary** on the roadmap is *content vs. commitment*. Both sides sign; both stand in front of the exec together.
- The cadence is a *quarterly refresh* with ad-hoc updates for material changes; not a status report.

The next chapter walks executive-audience RFCs and one-pagers — the exec-facing artifacts the roadmap sits behind and inside.
