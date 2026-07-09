# Running the Staff-Plus Hiring Loop and Calibrating the Bar

## Motivation

The single most durable technical decision a staff engineer makes over a two-year window is *who joins the ML org*. Any single RFC, roadmap, or vendor call can be revisited; a below-bar senior hire persists on the team for years, absorbs review attention that should have gone to program work, and — because staff-plus scope depends on L30 tech leads holding their teams' technical bar — silently drops the ceiling of every program the staff engineer authors.

The Staff/EM partnership from [`mod-401 chapter 5`](../mod-401-staff-ml-role-scope/05-staff-and-em-partnership.md) is precise about the hiring boundary: **the Staff engineer owns the technical bar; the EM owns headcount, pipeline, closing, and the final offer**. Both sit on the loop. Both give signal. This chapter unpacks what "owning the technical bar" looks like in practice — the rubric-based interview design, the calibration between hire / no-hire / lean-strong-hire / senior-vs-staff, the debias practices, and the hiring committee coaching that separates a rigorous ML hiring loop from a free-form performance-vibes discussion. Pairs with exercise-03.

## The problem with unstructured ML interviews

An unstructured technical interview — the interviewer walks in, asks whatever ML question they feel like, forms an impression, and files a rating — is the default at many companies and is the single biggest source of hiring miscalibration in ML orgs. Two independent bodies of evidence support this:

- Google's *re:Work* structured-interviewing writeups ([rework.withgoogle.com](https://rework.withgoogle.com/subjects/hiring-decisions/)) document Google's move from unstructured to structured interviews and the associated improvement in signal reliability. The core claim: **structured interviews, where every candidate is asked the same questions against the same rubric, are substantially more predictive of on-the-job performance than unstructured ones**, and *unstructured impressions are largely noise plus interviewer bias*. <!-- needs-research: cite the specific re:Work page or the underlying Schmidt & Hunter 1998 meta-analysis with an accessible link once WebSearch is available. -->
- The organisational-psychology literature (Schmidt & Hunter 1998 meta-analysis) reports the same pattern at broader scope. Structured interviews outperform unstructured ones on validity by a large margin.

The staff engineer running the ML hiring loop cannot rely on interviewer taste. The rubric — and the calibration to it — is what carries the signal.

## The four staff-plus interview slots and what each answers

A staff-plus ML hiring loop typically runs five-to-seven interview slots for a senior (L30) or staff (L40) candidate. Four are the technical core the staff engineer owns; one or two are behavioural / values that the EM co-owns. The four technical slots and what each is designed to distinguish:

**1. ML system design.** A two-hour session where the candidate is given a concrete problem — *"design the ranking system for our marketplace search"* or *"design the eval program for our LLM-as-judge policy"* — and asked to reason through it end-to-end. The rubric tests:

- **Portfolio-scope thinking (staff level).** Does the candidate ask *what other systems in the org depend on this*, or do they design in isolation?
- **Substrate awareness (staff level).** Do they name the shared feature layer, model registry, and eval contract, or design as if none exist?
- **Realistic scoping (senior level).** Do they defend what they are *not* doing, or try to design the whole world?
- **Concrete choices (senior level).** Do they name specific technologies with reasons, or hedge in generalities?

The senior-vs-staff distinction lives largely in slot 1. A candidate who cannot reason across systems in the portfolio is a senior candidate.

**2. ML fundamentals depth.** A one-hour session where the candidate is asked to reason about a specific ML concept in depth — *"how do you diagnose a diverging pretraining loss"*, *"how would you set up an offline eval for a re-ranker where the ground truth is expensive"*. The rubric tests:

- Depth of familiarity with modern ML system failure modes.
- Ability to reason from first principles when the specific answer is not memorised.
- Ability to name what they do not know cleanly.

This is the slot most vulnerable to interviewer-preference bias — every ML engineer has favourite ML topics. The rubric mitigates by naming the *reasoning quality*, not the *specific answer*.

**3. Coding / hands-on ML.** A ninety-minute session with a bounded coding or notebook exercise — implementing a feature-store lookup, debugging a training-loop bug, writing an offline-eval scorer. The rubric tests:

- Comfort with real production ML code (as opposed to research notebooks).
- Debugging strategy under time pressure.
- Judgement about when to prioritise correctness vs. time-to-answer.

For staff candidates this slot sometimes shifts to a code-review exercise instead — hand the candidate a real production PR and ask them to run the review out loud. This tests review-bar-setting judgement directly.

**4. Cross-team collaboration / staff-plus judgement.** A one-hour session where the candidate is walked through a scenario — *"the retrieval team wants to fork the feature store to unblock a launch; what do you do"* — and asked to reason through the technical, political, and org trade-offs. The rubric tests:

- Reading the political shape of a technical decision.
- Naming trade-offs without collapsing them into "just do X".
- Comfort saying no in writing (the roadmap non-goal skill from chapter 2).

Slot 4 is where the loop distinguishes L40 candidates from strong L30 candidates. A candidate who resolves every scenario by proposing to build a new service is a Solver-archetype Senior; a candidate who reasons about ownership, migration cost, and the peer-EM boundary is at staff altitude.

## The rubric — what a good rubric looks like on paper

A rubric is not a scoring form. A rubric is a *shared definition of what the loop is measuring*, written down before the loop runs, that every interviewer scores against. The pattern:

For each slot, the rubric names three-to-five **dimensions** and, for each dimension, defines four **levels**:

- **Level 1 — Below senior bar.** The candidate cannot demonstrate this dimension at the level expected of a mid-level ML engineer.
- **Level 2 — Senior bar.** The candidate meets the L30 expectation.
- **Level 3 — Staff bar.** The candidate meets the L40 expectation.
- **Level 4 — Above staff bar.** The candidate demonstrates this dimension at a level beyond L40 (usually indicating L50 potential or exceptional depth on this axis).

An example rubric row for slot 1, dimension "portfolio-scope thinking":

```
Level 1: Designs the system in isolation. Does not ask about other systems.
Level 2: Asks about dependencies on prompting but scopes the design to one system.
Level 3: Proactively names 2-3 other systems that would consume or depend on this
         one and designs for the shared contract. Names one trade-off in that framing.
Level 4: Names the substrate consolidation opportunities across the portfolio and
         proposes a design that lets other systems adopt it incrementally without
         breaking their current architecture.
```

The rubric lives in a shared doc every interviewer reads *before every loop*, and every interviewer scores against the same rubric. Interviewers write the level *and* one paragraph of evidence per dimension. The paragraph is what carries the loop discussion.

Larson's *An Elegant Puzzle* (Stripe Press, 2019) covers the org-design mechanics of running rubric-based loops at scale, and is worth reading once for the staff engineer who is standing up the rubric in a company that does not have one.

## Calibrating hire / no-hire / lean-strong-hire

The rubric produces per-dimension levels. The hiring committee's job is to synthesise those into an offer decision. Four calibration levels are enough:

- **Strong hire.** Meets the target level on every dimension and exceeds it on at least one. The loop is unanimous or near-unanimous. The candidate is above bar with room.
- **Lean hire.** Meets the target level on most dimensions; may have one dimension at level-below-target, but with concrete strengths that outweigh it. The loop is majority-positive with named concerns the team commits to coaching on.
- **No hire.** Does not meet the target level on one or more central dimensions. The loop is majority-negative on those dimensions.
- **Lean no-hire.** Meets some target dimensions but has one or more concerns the team is not confident coaching to bar in a reasonable time. The loop is split. Defaults to no-hire in mature loops.

The staff engineer's calibration work is on the *interpretation* of these labels. Two rules that prevent drift:

**A "lean hire" is a hire.** Loops that treat lean-hire as *"we should keep looking"* are effectively raising the bar above what was agreed at the loop-design stage. That is a slow, silent way to make headcount unfillable. If lean-hire is not a hire, the rubric was wrong.

**A "lean no-hire" is a no-hire.** Loops that treat lean-no-hire as *"the team wants to make it work"* are lowering the bar. The pattern that emerges — a below-bar hire absorbed under time pressure — is what silently degrades the L30 tech-lead depth of the whole ML org.

Camille Fournier's blog posts on senior hiring make this point in more forceful terms: **the tie goes to no-hire**. The cost of a bad hire is much greater than the cost of a slower search. <!-- needs-research: link the specific Fournier post that names the tie-goes-to-no-hire rule; the post exists but the exact URL requires WebSearch. -->

## The senior-vs-staff calibration — different question, same loop

A common calibration failure at growing ML orgs is running one loop against one bar (staff), and treating candidates who miss the staff bar but clear the senior bar as automatic senior hires. This over-hires seniors under the guise of "we couldn't find staff talent" and produces senior teams with no staff-plus IC. The pattern that works is:

- **Design the loop for the staff bar** — rubric levels 3 and 4 are what the loop is calibrated to.
- **Explicitly allow a downlevel decision.** If the candidate cleared the senior bar (rubric level 2) but not the staff bar, the loop can recommend a senior offer if the EM has senior headcount open.
- **Do not allow an uplevel decision.** A candidate who cleared the senior bar in a senior loop cannot be upleveled to staff mid-loop. That is a re-loop with new interviewers under the staff rubric.
- **Two rubric slots make the senior-vs-staff call.** Slot 1 (system design) and slot 4 (staff-plus judgement) are where staff altitude shows up. If the candidate hits level 3 on slots 2 and 3 but only level 2 on slots 1 and 4, they are a strong senior candidate, not a staff candidate.

The Staff engineer's job in the hiring debrief is to hold this line. The EM's job is to weigh the downlevel-offer decision against the headcount plan.

## Debias practices the staff engineer can install

The re:Work writeups and the broader interviewing literature converge on a handful of practices that reduce bias measurably. The staff engineer standing up or refreshing the ML hiring loop should install all of them:

- **Same questions to every candidate for a role.** Interviewers pick from a small approved question bank per slot, not from their favourites of the week.
- **Structured note-taking during the interview.** Interviewers capture what the candidate said, not their impression. Notes go into the rubric before the debrief; the debrief refers to notes, not memories.
- **Independent scoring before the debrief.** Interviewers submit scores independently. The debrief discussion happens *after* every interviewer's initial rubric score is captured. This prevents the loudest voice from anchoring the room.
- **Rotate the interviewer pool.** No interviewer sees more than one candidate for the same role per week (calibration protection) and the interviewer roster does not concentrate demographically on any dimension the org is trying to broaden.
- **Blind resume review where possible.** Resume screens remove name, school, and (where legal) prior-company signals until after the first-round score.
- **Debrief facilitator.** A dedicated non-scoring facilitator runs the debrief, walks each interviewer through their evidence, and calls the scoring vote. The facilitator is usually the recruiter or the EM; the staff engineer participates as an interviewer, not as facilitator, to preserve their voice as a rubric-bar-setter.

Not every practice is possible in every org — small companies compress interviewers, some regulated industries constrain blind review. Install as many as the org's shape allows.

## Coaching the hiring committee to hold the rubric

The staff engineer's ongoing hiring-loop work is *coaching the interviewer pool to score against the rubric consistently*. Three failure modes to watch for and address in-flight:

**"They gave the answer I would have given."** An interviewer scoring level 4 because the candidate mirrored their own approach is scoring for similarity, not competence. The coaching fix is to point at the rubric level definition and ask *"which dimension is this evidence for?"*.

**"They didn't get to the answer, but they were on the right track."** An interviewer scoring level 3 for a candidate who did not clear the problem is scoring on effort, not outcome. The coaching fix is to name the outcome the rubric level requires and score against it.

**"They're a rockstar, we should snap them up."** An interviewer whose evidence paragraph does not connect to any rubric level, and whose enthusiasm is not grounded in the notes, is scoring on impression. The coaching fix is to ask *"what specific thing did they say that put them at that level?"* and treat "I don't remember" as a downgrade.

Coaching happens in three places: before the loop (rubric read-through), after each interview during the interviewer's write-up (spot-check the evidence), and in the debrief (facilitator calls out ungrounded scoring). Reilly's *Staff Engineer's Path* chapter on influence covers the mechanics of coaching peers on a discipline they are not naturally inclined toward; the same mechanics apply here.

## What Staff owns vs. what the peer L40 EM owns

The mod-401 chapter 5 boundary-3 split (hiring) is the ground rule for this chapter:

- **Staff owns.** The technical rubric, the interview slot design, the debias practices, the calibration coaching, the hire/no-hire technical signal, the senior-vs-staff downlevel call.
- **EM owns.** Headcount, pipeline, sourcing, closing, comp negotiation, org-fit signal, final offer decision.
- **Both.** Sit on the loop. Give independent signal. Meet weekly on the pipeline; meet in real time on offer decisions.

A staff engineer who tries to own the closing or comp-negotiation side has crossed the boundary. An EM who tries to override the technical bar in the debrief has crossed the boundary. The escalation, per mod-401 chapter 5, is the common Director / VP.

## Common failure modes on the hiring loop

- **Vibes-based debriefs.** No rubric, no evidence, just impressions. The most common failure and the most fixable.
- **Rubric drift under headcount pressure.** *"We need to hire this quarter"* becomes *"level 2 is close enough to level 3"*. Silent and slow. Countered by a monthly loop-audit against the rubric and by explicit sign-off from the staff engineer on any lean-hire.
- **Interviewer-pool concentration.** The same three interviewers see every candidate. Their preferences become the bar. Countered by rotation and a broader trained interviewer pool.
- **"Culture fit" as a hidden rubric.** Culture-fit questions that have no rubric become a debias failure. Either operationalise culture into rubric dimensions ("bias for shared ownership" as a scoreable dimension with levels) or remove it from the loop.
- **Staff engineer as the closer.** The staff engineer takes on the reference call, the final sell, the negotiation. That is EM work. Doing it silently redistributes the boundary and the EM loses the offer-authorship signal.

## Summary

- The single most durable technical decision the staff engineer makes is who joins the ML org. Rubric-based, structured interviews carry the signal; unstructured interviews are largely noise plus bias.
- Four technical slots — **ML system design, ML fundamentals depth, coding/hands-on, staff-plus judgement** — cover the loop. Slot 1 and slot 4 carry the senior-vs-staff distinction.
- The **rubric** names three-to-five dimensions per slot and four levels per dimension (below-senior, senior, staff, above-staff). Every interviewer scores with evidence, not impression.
- **Calibration** — strong-hire / lean-hire / no-hire / lean-no-hire — is held to two rules: a **lean-hire is a hire**, and the **tie goes to no-hire** (Fournier).
- **Senior-vs-staff** is a downlevel decision, not an uplevel one. Senior candidates cannot be upleveled to staff mid-loop.
- **Debias practices** — same questions per role, structured note-taking, independent scoring, rotation, blind review, dedicated facilitator — are installed by the staff engineer and enforced by the loop.
- **Coaching the interviewer pool** is ongoing work — pre-loop, post-interview, and in the debrief.
- The **Staff/EM boundary** on hiring is *technical bar and rubric vs. headcount and offer*. Both sit on the loop; neither does the other's job.

The next chapter walks coaching senior tech leads (L30) — where the staff engineer's technical mentorship stops and where the EM's career management takes over.
