# exercise-01: Multi-Quarter ML Program Roadmap

**Estimated effort:** 3 hours

## Objective

Author the **multi-quarter ML program roadmap** for a real (or realistically-composed) portfolio using chapter 2's five-column template — **diagnosis, programs, sequencing, non-goals, resource asks** — for a two-to-four-quarter horizon. After this exercise the learner should hold a five-to-ten-page document that leadership could commit to: a defended sequence of three-to-six programs, explicit non-goals with rev-dates, resource asks tied to program-level deltas, peer-platform-team dependencies with written slip fallbacks, and at least one *no in writing* the roadmap owns publicly.

This is the first of the five artifacts that assemble into the module's contribution to [`project-403-staff-plus-tech-leadership-simulation`](../../../projects/project-403-staff-plus-tech-leadership-simulation/). Every downstream exercise reads against the portfolio this roadmap commits to — the exec one-pager in exercise-02, the coaching plan in exercise-04, and the vendor decision in exercise-05 should all name programs from this roadmap when relevant. Choose the portfolio you can carry across the module.

## Prerequisites

- Chapter 01 — the sustaining-staff-plus frame and the *am I sustaining staff-plus scope?* heuristic; the four archetypes as ongoing weighting constraints.
- Chapter 02 — the five columns, the sequencing-as-counterfactual pattern, the non-goals-with-rev-date rule, the resource-ask-tied-to-delta pattern, the peer-platform dependency structure, and the *no in writing* discipline. Read in full.
- Recommended: one pass of Rumelt's *Good Strategy Bad Strategy* chapter 3 ("The Kernel of Good Strategy" — diagnosis / guiding policy / coherent action) before drafting Section 1's diagnosis paragraph; and one pass of Larson's *Staff Engineer* chapter 3 ("Working on What Matters") before drafting Section 5 (resource asks).
- If the learner has carried a portfolio through mod-402 (the multi-team ML architecture blueprint) and mod-407 (the TCO model), those two artifacts are the natural upstream inputs for the diagnosis and the resource-ask defence, respectively. Consistency across exercises is worth more than novelty.

## Choose the portfolio

Pick **one** portfolio. Priority order:

1. If the learner completed [`project-401-multi-team-ml-blueprint`](../../../projects/project-401-multi-team-ml-blueprint/) or mod-402 exercise-01, use that portfolio. The blueprint's painful-cells list is the input to this roadmap's diagnosis.
2. Otherwise, use a real portfolio from the learner's current or prior employer, with three-to-five ML systems and at least one peer platform team dependency. Real inputs produce a defensible roadmap; hypothetical inputs collapse into a wish list.
3. Only if neither of the above applies: pick the *chapter-2 worked example portfolio* stipulated at the top of this exercise's Section 1 — a three-team ML org (ranking, retrieval, personalisation) sitting on top of a shared feature store and model registry, with a pretraining program landing in Q3 and a peer eval-platform team on a paved-road-in-progress track.

Do not scope the roadmap to a single team. That is a team roadmap, which is an L30 artifact. The mod-401 chapter 1 heuristic — *does the artifact I produced this week outlive my continuous attention?* — is the check; a team roadmap that outlives one L30's tenure is L30 work, a portfolio roadmap that outlives one staff engineer's tenure is L40 work.

## Steps

1. **State the portfolio and horizon in one paragraph.** Named: portfolio scope (which teams, which ML systems), current quarter, roadmap horizon (2-4 quarters; typical is Q(n) through Q(n+3)), current staff engineer(s), peer L40 EM, sponsoring director. Chapter 2's worked example is the shape — three-to-five sentences. This paragraph is the frame every subsequent section reads against.
2. **Author Section 1 — Diagnosis (one page).** Rumelt's diagnosis step: what shape is the portfolio in today, what are the two-to-three biggest technical constraints, what changed recently that leadership should register. Do not list everything. Two-to-three constraints, named concretely — *the shared feature layer's freshness ceiling is now the bottleneck on ranking's recall gains*, *the pretraining program consumed the training-capacity headroom the org had built up in Q1*, *the org's second frontier-model-API vendor cut prices, opening a routing-policy conversation*. This paragraph is the load-bearing paragraph the rest of the roadmap defends against.
3. **Author Section 2 — Programs (2-3 pages).** Three-to-six programs for the horizon. For each program, one paragraph naming:
    - **Business object.** What outcome the program is on the hook for. Named in the exec's units (dollars, quarters, business metrics), not ML-engineering vocabulary.
    - **Technical shape.** One-to-two sentences on the approach — *foundation-model pretraining program per mod-403 shape*, *shared eval-standard rollout per mod-406 shape*, *inference-gateway multi-tenant SLO consolidation per mod-408 shape*. Cite the intermediate-module vocabulary where it applies.
    - **Owning team.** Named team, or *TBD hire* if the program depends on a resource ask in Section 5.
    - **Peer-team dependencies.** Which peer platform team(s) or peer-specialist team(s) the program depends on; forward-pointer to Section 4.
4. **Author Section 3 — Sequencing (2 pages).** Two artifacts:
    - **Dependency diagram (one page).** A Gantt-shaped or dependency-graph diagram of the programs across the horizon. Programs on the vertical axis, quarters on the horizontal axis, arrows for dependencies. ASCII or Mermaid is fine; polish is not the point.
    - **Sequencing defence (one page).** A two-column table: **program → what breaks if it comes later than we planned**. Every row's *what breaks* is a specific technical or coordination cost — usually a cost multiplier (*we would migrate the feature layer twice*, *we would train on a data mixture we will replace two quarters later*) or a lost-slot coordination cost (*the peer platform team's Q3 slot is committed to this substrate; if we defer, we lose the slot*). If any row's answer is "nothing", that program does not belong on the roadmap for this quarter — remove it or re-defend the placement.
5. **Author Section 4 — Non-goals (one page).** Two-to-three explicit non-goals. Each with:
    - **The non-goal itself.** One sentence, in the vocabulary a reasonable reader would expect to see on the *will do* list. Chapter 2 was emphatic: a non-goal a reader would not have expected on the *will do* list does no work in the exec review.
    - **The defence.** One-to-two sentences on why not now — usually a capacity conflict with a *will do* program, a dependency that has not resolved, or a mis-scoped ask from leadership that this roadmap is redirecting.
    - **The rev-date.** The specific quarter and observable condition under which the non-goal re-opens. *Revisit Q(n+4) if the pretraining run has stabilised*, *revisit at annual planning if the registry-fragmentation cost estimate crosses $X*, *revisit if the peer platform team ships their gap-RFC substrate on schedule*. Not "revisit in a year".
6. **Author Section 5 — Resource asks (one page).** Every resource ask is tied to a program-level delta. Chapter 2's pattern: *if we get X, we can ship Y two quarters earlier; if we do not, we defer Y*. Named categories:
    - **Headcount.** Named level (L20 / L30 / L40), named program the hire is against, named counterfactual (*without the L30 hire, Program C's Q4 sequencing survives but Program D's exec metric slips by a month*).
    - **Capacity.** Named workload (training capacity, inference capacity, batch enrichment), named program, named counterfactual (*without the 200 H100-week increment, Program A's schedule holds but Program E defers to Q(n+4)*).
    - **Budget.** Named vendor spend or capex ask, named program, named counterfactual — grounded on the mod-407 TCO model where applicable.
    - **Peer-platform-team investment.** Named substrate the peer platform team must ship, named program, named counterfactual. This ask is the entry point to Section 6.
7. **Author Section 6 — Peer-platform-team dependencies (one page).** For each dependency from Section 2's programs, four fields:
    - **What we consume.** A specific platform-team artifact and version — the feature-store online-serving contract v3, the model-registry alias policy v2, the inference-gateway multi-tenant SLO. If the artifact is a paved-road not yet on the platform team's roadmap, name that too — the dependency is on a *hoped-for* substrate and Section 7 must call it out.
    - **What we commit back.** The consumer commitment from the mod-405 chapter 4 hand-off contract vocabulary — adoption timeline, feedback cadence, contribute-back opportunity, integration hours the ML org will invest.
    - **When we need it.** The quarter, tied to the program on the roadmap that depends on it.
    - **What we do if it slips.** The fallback plan. Chapter 2's separator between senior and staff-plus roadmaps: a senior tech lead's roadmap says *we depend on the platform team shipping X*; a staff-plus roadmap adds *if X slips by a quarter, program Y falls back to the per-team implementation from mod-402 and we absorb the migration cost in Q(n+2)*.
8. **Author Section 7 — What the roadmap says no to (half a page).** Explicit written *no* to at least one of the three shapes chapter 2 named:
    - **No to a proposed program that duplicates existing capability.** Two teams' L30s independently propose the same LLM-based service; the roadmap names one as the org-wide capability and defers the other.
    - **No to a program leadership asked for.** *The CTO's off-site slide asked for a customer-facing chatbot in Q3*; roadmap's response with a specific technical constraint that makes it not-now, and what the roadmap would drop to make room if leadership overrides.
    - **No to scope creep inside a program.** *Program A (pretraining) does not also cover real-time serving policy*; that goes to Program D.
    Every no cites the condition under which it could be revisited.
9. **Author Section 8 — Staff/EM co-signature block (half a page).** Chapter 2's Staff/EM boundary: Staff owns the content; EM owns the commitment. In one paragraph each:
    - The Staff engineer's technical-content signature: *"I attest that the diagnosis, sequencing, non-goals, and resource-ask shape reflect my staff-plus technical judgement of the portfolio's state."*
    - The EM's delivery-commitment signature: *"I attest that the capacity, hiring pipeline, and on-call load reflect the team's actual delivery commitment for this horizon."*
    - The cadence commitment: *quarterly refresh, ad-hoc updates for material scope changes, no updates for two-week slips (those are execution events).*
10. **Rewrite Section 1's diagnosis last.** Chapter 2's Rumelt point: the diagnosis is what everything else reads against, and it usually needs sharpening once the rest of the roadmap is on paper. Two-to-three sentences, no more, that any exec could read in 60 seconds and connect to Sections 2-5 without having read them.

## Deliverable

A single document, 5-10 pages, in this structure:

- **Portfolio and horizon.** The one-paragraph frame from step 1.
- **Section 1 — Diagnosis.** One page (steps 2, 10). Two-to-three named constraints. No lists of everything.
- **Section 2 — Programs.** 2-3 pages (step 3). Three-to-six programs, one paragraph each: business object / technical shape / owning team / peer-team dependencies.
- **Section 3 — Sequencing.** 2 pages (step 4). Dependency diagram + *what breaks if reordered* table.
- **Section 4 — Non-goals.** 1 page (step 5). Two-to-three explicit non-goals with defence and rev-date.
- **Section 5 — Resource asks.** 1 page (step 6). Every ask tied to a program-level delta with counterfactual.
- **Section 6 — Peer-platform-team dependencies.** 1 page (step 7). Four-field format per dependency, including fallback if slip.
- **Section 7 — What the roadmap says no to.** Half a page (step 8). At least one written *no*.
- **Section 8 — Staff/EM co-signature and cadence.** Half a page (step 9). Both signatures, quarterly refresh cadence.
- **Reflection.** Two-to-three sentences: which sequencing choice was hardest to defend? Which non-goal was hardest to write?

## Starter guidance

- **Diagnose the portfolio you have, not the one you want.** Chapter 2's first pattern: the diagnosis has to name the constraints that are actually load-bearing today. Aspirational constraints — *"the ranking system is limited by our lack of a shared feature layer that we plan to build"* — are non-diagnoses. If you would build the feature layer whether or not the constraint existed, the constraint is a *program*, not a *diagnosis*.
- **Sequencing defence lives in the counterfactual, not in priority language.** Chapter 2 was direct: *"the shared feature layer is higher priority than the ranker redesign"* invites the exec to reorder. *"We ship the shared feature layer before the ranker redesign because the ranker redesign's eval story depends on time-travel-correct features that only the shared layer provides"* does not. Every row in the Section 3 table needs the "because" — priority language is a red flag.
- **Non-goals are the section drafts of this document most reliably cut. Do not cut them.** Chapter 2 named this: non-goals are the single most-cut section in draft one and the single most-cited section in the exec review. If you have only one non-goal because the others "felt obvious", write them anyway. The exec reads the non-goals first; they should find at least two the ML org proactively decided against.
- **Resource asks without a counterfactual are a wish list.** *"We need two L30 hires and 15% more training capacity"* is a wish. *"L30 hire → Program C moves to Q3; without it, Program D's exec metric slips a month"* is a resource ask. If you cannot state the counterfactual, either the ask is not real or the roadmap does not need it.
- **Section 6's *what we do if it slips* is what makes the roadmap a staff-plus artifact.** A senior tech lead's roadmap depends on the platform team; a staff-plus roadmap depends on the platform team *and* has a fallback. If any dependency does not have a fallback, the roadmap is a single point of failure. Either write the fallback or elevate the dependency to Section 7 as a *no if the substrate does not land*.
- **Do not conflate a two-week slip with a scope change.** Chapter 2's cadence rule: the roadmap is not a status report. If a program slips a sprint, the EM owns the response and the roadmap does not update. If a program's *shape* changes — the pretraining run's parameter count drops from 400B to 200B, the eval standard's rollout compresses from three teams to one — that is a scope change and the roadmap gets a one-page addendum.
- **Cap yourself at ten pages.** Chapter 2 named the roadmap as five-to-ten pages plus a diagram. If you are over ten pages, either the diagnosis is doing work the programs section should do, or Section 2's programs are being over-described. The exec-audience one-pager in exercise-02 will collapse this document further; keep the roadmap tight enough that the collapse is a real compression, not a duplication.

## Acceptance criteria

- The portfolio and horizon are named concretely — team count, ML systems, current quarter, roadmap horizon (2-4 quarters), Staff engineer, peer L40 EM, sponsoring director.
- Section 1 (Diagnosis) fits on one page and names 2-3 specific technical constraints. It does not list every problem the portfolio has.
- Section 2 (Programs) names 3-6 programs. Each program has all four fields: business object (in exec units), technical shape (with intermediate-module citation), owning team (or TBD-hire), peer-team dependencies.
- Section 3 (Sequencing) includes a dependency diagram *and* a "what breaks if reordered" table. Every table row's *what breaks* names a specific technical or coordination cost — no rows with "nothing" or "just later".
- Section 4 (Non-goals) contains 2-3 explicit non-goals. Each has: one-sentence non-goal, one-to-two-sentence defence, specific rev-date with an observable trigger condition. No "revisit in a year".
- Section 5 (Resource asks) ties every ask to a program-level delta with a stated counterfactual (what happens if we get it, what happens if we do not).
- Section 6 (Peer-platform-team dependencies) has all four fields for every dependency: what we consume (with version), what we commit back (from mod-405 vocabulary), when we need it (tied to a Section 2 program), what we do if it slips (concrete fallback).
- Section 7 (What the roadmap says no to) contains at least one written *no* — to a duplicate program, a leadership-asked program, or scope creep — with the condition for revisit.
- Section 8 (Staff/EM co-signature and cadence) has both signature blocks and names the quarterly refresh cadence.
- Section 1 (Diagnosis) is the last section rewritten, and any exec could read it in under 60 seconds and connect it to Sections 2-5.
- Total length is 5-10 pages plus the dependency diagram.

## Stretch goals

- **Draft the exec one-liner.** In one sentence at the top of the doc: what shape the portfolio takes at the end of the horizon, and what the two most-consequential resource asks are. Example: *"By Q(n+3) the portfolio has consolidated onto the shared feature layer, shipped the 200B pretraining run, and standardised the eval contract — subject to one L30 hire and the peer platform team's inference-gateway substrate."* That sentence is what the director will read; the rest of the roadmap defends it. Exercise-02 will build on this line.
- **Author a *no* the org has not yet accepted.** Chapter 2's most valuable *no* is the one leadership asked for but the roadmap declines. If your Section 7 only names *no*s the ML org invented, add one that pushes back on a real leadership ask — even if the pushback fails, drafting it is the practice.
- **Rehearse the exec review out loud.** Set a 20-minute timer. Read the diagnosis in 60 seconds. Read Section 3's *what breaks if reordered* table row-by-row. Field one adversarial question per program. Note where you found yourself waving hands — those are the places the roadmap needs more defence, not more diplomacy.
- **Cross-reference to project-403 downstream artifacts.** Note which programs from Section 2 the exec one-pager in exercise-02 will collapse, which peer-platform dependencies from Section 6 the vendor doc in exercise-05 will address, and which L30 tech leads own which programs (feeder for exercise-04's coaching plan). Consistency across the module is the capstone deliverable; catching drift early is worth an hour.
- **Take the draft to the peer EM.** The Section 8 co-signature is where this exercise's staff/EM boundary lives; drafting the roadmap without EM input is exactly the failure mode chapter 2 named as *"a beautiful document no team will commit to"*. Ask the peer EM (or a staff-plus peer role-playing the EM) to read Section 5 (resource asks) and Section 6 (peer-platform-team dependencies) and flag any ask the team's actual capacity cannot commit to. Incorporate the pushback as a revision.
- **Compare against a real roadmap you have seen.** If you have access to a real portfolio roadmap (your employer's, a public write-up, a role-play from a peer), read it against this exercise's rubric. Which of the five columns did the real roadmap under-defend? Which did it over-defend? The comparison is calibration data for the next quarter's refresh.
