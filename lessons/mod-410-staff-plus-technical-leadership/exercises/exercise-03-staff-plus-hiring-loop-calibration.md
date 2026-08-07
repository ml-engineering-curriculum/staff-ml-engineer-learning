# exercise-03: Staff-Plus Hiring Loop Calibration

**Estimated effort:** 3 hours

## Objective

Design a **rubric-based hiring loop** for a senior (L30) or staff (L40) ML engineer role, and run **one debrief simulation** against three composed candidates to check the rubric holds calibration under the pressure conditions chapter 4 named. After this exercise the learner should hold: the loop design (slot allocation, question bank, per-slot rubric with dimensions × levels), a debias-practice checklist, a simulated debrief transcript with a hire / no-hire verdict on each candidate, and a one-page hiring-committee coaching brief the staff engineer would circulate to the interviewer pool before the loop runs.

Pairs with chapter 4. Feeder for [`project-403-staff-plus-tech-leadership-simulation`](../../../projects/project-403-staff-plus-tech-leadership-simulation/).

## Prerequisites

- Chapter 04 — the four technical slot definitions (system design, fundamentals depth, coding/hands-on, staff-plus judgement), the rubric structure (3-5 dimensions per slot × 4 levels), the four-verdict calibration (strong-hire / lean-hire / no-hire / lean-no-hire with *lean-hire is a hire* and *tie goes to no-hire*), the senior-vs-staff downlevel-only rule, the six debias practices, the three interviewer failure modes, and the five common loop failure modes. Read in full.
- Chapter 01 — the staff/EM boundary on hiring (boundary 3 in mod-401 chapter 5); this exercise operates on the *Staff-owned* half of that boundary.
- Recommended: skim Google's [re:Work structured-interviewing guide](https://rework.withgoogle.com/subjects/hiring-decisions/) for the structured-interviews evidence base. Optional but useful: one of Camille Fournier's blog posts on senior-engineer hiring for the *tie goes to no-hire* framing (find via search — the phrase is Fournier-adjacent and appears in her writing on interview calibration). <!-- needs-research: link the specific Fournier post that names *tie goes to no-hire* once WebSearch is available. -->
- If the learner has run at least one hiring loop as an interviewer, keep the debrief notes from that loop nearby. The most common finding when reading a real debrief against chapter 4's rubric is that at least one interviewer's evidence paragraph was ungrounded. That is the calibration data the exercise is trying to build.

## Choose the role and level

Pick **one** target role and level for the loop. The choice determines the rubric's calibration bar. Priority order:

1. If the exercise-01 roadmap named a *TBD hire* in Section 5's resource asks, that role at that level is the natural choice. Consistency across exercises is worth more than novelty; hiring against a real Section-5 ask reads through to project-403.
2. Otherwise, pick a role from the learner's current org's real hiring pipeline (past six months or next six months). Real inputs produce a defensible rubric; hypothetical inputs collapse into a debate about what "senior" or "staff" means at some fictional company.
3. If neither above applies, pick from this exercise's stipulated shape: *"L30 senior ML engineer for the ranking team, in a portfolio with a shared feature store, a shared model registry, and a peer platform team on the paved-road-in-progress track."*

Do not scope the loop across multiple roles. Chapter 4 was explicit: a loop calibrated to two bars at once collapses into the *rubric drift under headcount pressure* failure mode. Pick one target, design one loop, and if a downlevel decision becomes relevant, use chapter 4's senior-vs-staff rule to handle it.

## Steps

### Part A — Loop design (1.5 hours)

1. **State the role, level, and target bar in one paragraph.** Role, level (L30 senior or L40 staff), the specific team or archetype (Tech Lead / Architect / Solver / Right Hand — mod-401 chapter 2 vocabulary) the hire is intended for, and the portfolio context (from exercise-01 if reused). Cite the Section-5 resource ask this loop is against if applicable.
2. **Design the slot allocation.** Five-to-seven slots total. Four technical slots (chapter 4's core) plus one-to-three behavioural / values slots the EM co-owns. For each slot:
    - **Slot name and shape.** *"Slot 1 — ML system design (120 min, virtual whiteboard)"*, and so on.
    - **Interviewer profile.** *"Staff-plus ML engineer, ideally not from the hiring team"* / *"Senior ML engineer from a peer team"* / *"Peer EM"* / etc.
    - **What the slot answers.** One-to-two sentences on what the loop learns from this slot that it cannot learn from another slot.
    - **Duration and format.** Time-boxed. Chapter 4's shapes: 120 min for design, 60 min for fundamentals, 90 min for coding/hands-on, 60 min for staff-plus judgement, 45-60 min for behavioural.
3. **Author the rubric for each of the four technical slots.** For each slot, 3-5 named dimensions. For each dimension, four levels (**L1 — below senior bar** / **L2 — senior bar** / **L3 — staff bar** / **L4 — above staff bar**). Chapter 4's example is the shape:

   ```
   Slot 1 — ML system design
     Dimension: portfolio-scope thinking
       L1: Designs the system in isolation. Does not ask about other systems.
       L2: Asks about dependencies but scopes the design to one system.
       L3: Proactively names 2-3 dependent systems and designs for the shared
           contract. Names one trade-off in that framing.
       L4: Names substrate consolidation opportunities and proposes a design
           that lets other systems adopt it incrementally.
     Dimension: substrate awareness
       L1: ...
     [3-5 dimensions total per slot]
   ```

   Chapter 4 named the two slots that carry the senior-vs-staff distinction — Slot 1 (system design) and Slot 4 (staff-plus judgement). Weight the dimensions of those two slots accordingly; a candidate who clears L3 on slots 2-3 but only L2 on slots 1 and 4 is a strong senior, not a staff candidate.
4. **Draft the question bank per slot.** For each of the four technical slots, three-to-five approved questions per bank. Interviewers pick from the bank; they do not invent questions. Each question:
    - The question itself, in one sentence.
    - The core signal it produces — which rubric dimensions it exercises.
    - A one-line note on what a good vs. bad answer looks like (for interviewer calibration, not for scoring).
    Chapter 4's *unstructured interview* failure mode is caught by the bank; interviewers who ask their own questions produce noise plus bias.
5. **Author the debias-practice checklist.** All six practices from chapter 4 — same-questions-per-role, structured note-taking, independent scoring before debrief, interviewer rotation, blind resume review where possible, dedicated facilitator — with a one-line named implementation for each. *"Same questions: bank owner is the hiring manager; deviations require sign-off."* *"Independent scoring: rubric submitted to the ATS before debrief; facilitator reads scores aloud in order of interview."* Do not check *"implement" on a practice the org's shape cannot support — note that with the reason and the mitigation (e.g., *"blind review not viable in this ATS; mitigate with facilitator-led resume-screen calibration once per quarter"*).
6. **Author the one-page hiring-committee coaching brief.** The document the staff engineer sends to every interviewer *before* every loop. Structure:
    - **The role, level, and target bar** (one paragraph — pulled from step 1).
    - **The rubric read-through** (half a page): each interviewer reads the rubric for their slot and confirms understanding. Named: the two rubric dimensions where interviewer drift is most likely (staff engineer's diagnosis).
    - **The three interviewer failure modes from chapter 4** (half a page): *"they gave the answer I would have given"*, *"they didn't get to the answer, but they were on the right track"*, *"they're a rockstar, we should snap them up"*. Named coaching prompt for each — the question the interviewer should ask themselves in the write-up.
    - **The debrief protocol** (one paragraph): independent scoring first, evidence-first discussion, facilitator calls the vote, tie goes to no-hire.

### Part B — Debrief simulation (1.5 hours)

7. **Compose three candidates.** Each candidate is a two-paragraph profile:
    - **Background.** Level, current employer type, years of relevant ML experience, one-line summary of resume signal.
    - **Loop performance.** How the candidate performed across the four technical slots. Specific rubric-dimension scores (L1-L4) per slot with a one-sentence evidence note. The three candidates should represent the three most-common calibration challenges:
      - **Candidate A** — a **clear strong-hire** for the target level. All-slot L3 or L4 with one L4-standout. This candidate calibrates *what the target bar is* for the exercise.
      - **Candidate B** — a **lean-hire / lean-no-hire boundary case** for the target level. Two slots at target, two slots below, with strengths that could be argued to compensate. This candidate tests the *tie goes to no-hire* rule and the *lean-hire is a hire* rule under real pressure.
      - **Candidate C** — a **senior-vs-staff downlevel case**. Cleared the senior bar (L2 across the board, plus L3 on slots 2 and 3) but not the staff bar on slots 1 and 4. This candidate tests the *downlevel is allowed, uplevel is not* rule.
    The candidate profiles are the *inputs to the debrief*; the rubric evidence is per-dimension, not per-slot summary.
8. **Simulate the debrief for each candidate.** For each candidate, write a debrief transcript in the form:
    - **Interviewer scores round** (independent, submitted before discussion): each interviewer's per-dimension L-score with the one-sentence evidence note.
    - **Facilitator-led discussion** (5-8 exchanges): the facilitator walks each interviewer through their evidence dimension by dimension. Where interviewers disagree, the discussion tests which piece of evidence is closer to the rubric level definition — not which interviewer feels more strongly.
    - **The vote and the verdict.** Named: strong-hire / lean-hire / no-hire / lean-no-hire. For Candidate B, the verdict is the *hard call* — write out the reasoning that resolves it, holding to chapter 4's two rules. For Candidate C, name the downlevel decision and the specific two-slot evidence that makes senior-not-staff the right verdict.
9. **Write the coaching notes to interviewers.** For each candidate's debrief, one paragraph the staff engineer would send back to interviewers *after* the debrief:
    - Which interviewer's evidence was strongest?
    - Which interviewer showed one of the three failure modes from chapter 4 (*"gave the answer I would"* / *"on the right track"* / *"rockstar"*), and what the coaching correction is?
    - If applicable: which rubric dimension needs clarification for the next loop (the loop's own calibration feedback).
10. **Cross-check against chapter 4's five loop-level failure modes.** In one paragraph, confirm the loop design and debrief protocol do not exhibit: vibes-based debriefs, rubric drift under headcount pressure, interviewer-pool concentration, *"culture fit"* as a hidden rubric, staff engineer as the closer. If any of the five apply, name the mitigation.

## Deliverable

A single packet with these sections:

- **Section 1 — Role, level, and target bar.** One paragraph (step 1).
- **Section 2 — Slot allocation.** 5-7 slots with interviewer profile, what-it-answers, duration, format (step 2).
- **Section 3 — Rubric.** All four technical slots, each with 3-5 dimensions × 4 levels. Slots 1 and 4 are weighted for the senior-vs-staff distinction (step 3).
- **Section 4 — Question bank.** 3-5 approved questions per technical slot, each with signal + good/bad-answer note (step 4).
- **Section 5 — Debias-practice checklist.** All six practices with named implementation (or a documented mitigation for any that are not viable) (step 5).
- **Section 6 — Hiring-committee coaching brief.** One page for pre-loop circulation (step 6).
- **Section 7 — Three candidate profiles.** Each two paragraphs with per-dimension rubric evidence (step 7). Candidates A, B, C in the three calibration shapes.
- **Section 8 — Three simulated debrief transcripts.** One per candidate. Independent-scoring round, facilitator-led discussion, vote, verdict (step 8). Candidate B's transcript resolves the tie-goes-to-no-hire tension; Candidate C's transcript names the downlevel decision.
- **Section 9 — Coaching notes to interviewers.** One paragraph per debrief (step 9).
- **Section 10 — Loop-level failure-mode audit.** One paragraph confirming chapter 4's five loop-level failure modes are addressed (step 10).
- **Reflection.** Two-to-three sentences: which candidate was hardest to reach a verdict on? Which rubric dimension was hardest to level?

## Starter guidance

- **The rubric is a shared definition, not a scoring form.** Chapter 4's most-repeated point. Interviewers score against the rubric; the rubric is authored *before* the loop and every interviewer reads it *before every candidate*. If you are tempted to compress the rubric to fit on one page, cut dimensions rather than levels — a two-level rubric is a binary and does not calibrate senior-vs-staff.
- **Slots 1 and 4 are the load-bearing slots for staff-vs-senior.** Chapter 4 was direct: if a candidate is L3 on slots 2 (fundamentals) and 3 (hands-on) but only L2 on slots 1 (system design) and 4 (staff-plus judgement), they are a strong senior, not a staff candidate. Weight the rubric dimensions of slots 1 and 4 with that discipline; do not let a strong hands-on slot compensate for a weak system-design slot.
- **Candidate B is where the exercise's real learning happens.** The clear-hire (A) calibrates the target bar; the downlevel case (C) tests the senior-vs-staff rule; but the *lean-hire vs. lean-no-hire boundary case* is where chapter 4's two rules (*lean-hire is a hire*, *tie goes to no-hire*) get tested under adversarial pressure. Write Candidate B carefully — the evidence should genuinely resolve neither way on the surface, and the debrief should have to reach for the rubric definition to resolve it.
- **Write the debrief transcript in dialogue, not summary.** *"The facilitator asked Interviewer 1 for their L-score on 'substrate awareness'. Interviewer 1 said L3, evidence: candidate asked about the shared feature layer within 10 minutes and named its version constraint. Facilitator turned to Interviewer 2 ..."* This granularity is what surfaces the interviewer-failure modes from chapter 4 in the exercise; summary-form transcripts hide them.
- **The staff engineer participates as an interviewer, not as facilitator.** Chapter 4 named this: the staff engineer's voice as *rubric-bar-setter* is diluted if they also facilitate. If your simulated debrief has the staff engineer as facilitator, revise — the facilitator is the recruiter or the peer EM.
- **Do not skip the *"culture fit"* audit.** Chapter 4 named unrubricked culture-fit as a debias failure hidden inside otherwise-structured loops. If your loop design has any behavioural / values slot, its rubric should be as scorable as the technical slots — dimensions with L1-L4 levels — or that slot's signal is impression, not evidence.
- **The coaching brief is a real artifact, not an exercise formalism.** If you have run any hiring loops, this brief is the document you wish had been circulated before the loop. Write it as if you will actually send it — plain, brief, decisive on the three interviewer failure modes.

## Acceptance criteria

- Section 1 names role, level (L30 or L40), target archetype, and portfolio context. If exercise-01 named a Section-5 hire, this loop is against that ask.
- Section 2 has 5-7 slots. Four are technical (system design, fundamentals depth, coding/hands-on, staff-plus judgement); one-to-three are behavioural/values. Every slot has interviewer profile, what-it-answers, duration, format.
- Section 3 has a full rubric for **all four** technical slots. Each slot has 3-5 dimensions. Each dimension has 4 levels (L1-L4) with a two-to-three-sentence definition per level.
- Slots 1 (system design) and 4 (staff-plus judgement) have at least one dimension explicitly designed to distinguish senior from staff.
- Section 4 has 3-5 approved questions per technical slot, each with rubric-dimension signal named and a one-line good-vs-bad-answer note.
- Section 5 addresses all six debias practices from chapter 4, with a named implementation or a documented mitigation.
- Section 6 fits on one page and contains: role/level/target-bar paragraph, rubric read-through with the two most-drift-prone dimensions named, the three interviewer failure modes with coaching prompt, the debrief protocol.
- Section 7 has three candidate profiles: A (clear strong-hire), B (lean-hire/lean-no-hire boundary), C (senior-vs-staff downlevel). Each has per-dimension rubric evidence, not per-slot summary.
- Section 8 has three debrief transcripts in dialogue form. Each contains: independent-scoring round, facilitator-led discussion (5-8 exchanges), vote, named verdict.
- Candidate B's transcript explicitly holds to chapter 4's two rules (*lean-hire is a hire*, *tie goes to no-hire*) and names which one resolves the case.
- Candidate C's transcript names the downlevel decision and cites the specific slot-1 and slot-4 evidence that makes senior-not-staff the right verdict.
- Section 9 has one paragraph of coaching notes per debrief, calling out interviewer strengths and at least one instance of one of chapter 4's three interviewer failure modes with the coaching correction.
- Section 10 audits the loop design against chapter 4's five loop-level failure modes and names any mitigations.
- The staff engineer participates as an interviewer, not as facilitator, in every debrief.

## Stretch goals

- **Take the rubric to a peer for calibration.** A staff-plus peer, ideally not from the hiring team. Ask them to score Candidate B independently against your rubric using only the per-dimension evidence in Section 7. Compare their verdict against yours. Silent disagreement on Candidate B is the calibration signal — that is exactly where chapter 4's rules are most contested, and the debrief practice they will need is what this exercise is trying to install.
- **Simulate the debrief with a real interviewer pool.** If you can gather two-to-three peers to role-play the interviewer pool for 45 minutes, run Candidate B's debrief live. The interviewer failure modes from chapter 4 will show up under real conversational pressure in ways the written transcript does not. Note which pattern appeared and what the coaching correction would have been.
- **Author the rubric-drift audit for the hypothetical Q+4 quarter.** Chapter 4's *rubric drift under headcount pressure* failure mode is silent and slow. In one page: what does the audit look like — who reads what evidence against which rubric levels, how often, and what triggers a re-authoring of the rubric. This is the ongoing discipline; the loop design is the one-time authoring.
- **Compose Candidate D — the false positive.** A candidate whose loop performance looks strong at read-time but whose evidence, when read carefully, does not clear the target bar. Rockstar-narrative energy without the rubric evidence to back it. Run the debrief. Verdict: no-hire. Chapter 4's *"they're a rockstar, we should snap them up"* failure mode is easy to name in theory; drafting Candidate D forces you to write the specific evidence pattern that reveals it.
- **Compose Candidate E — the false negative.** A candidate whose interview style makes them look weak on surface signal (quiet, methodical, does not narrate as they think) but whose evidence, when read carefully, clears the target bar. Run the debrief. Verdict: hire. This is the calibration for the debias-practice reason for *evidence, not impression*.
- **Cross-reference to exercise-04.** The L30 tech lead the learner coaches in exercise-04 was hired through a loop like this one. If Section 1 of exercise-04 names a specific L30, note in this exercise's reflection: which slot of this loop's rubric did that L30 clear at L3 (their staff-plus growth edge for exercise-04's coaching), and which dimension is the coaching target. This is the *hiring → coaching* consistency loop chapter 5 depends on.
- **Draft the "we lost the candidate" post-mortem template.** For the case where the loop reaches a strong-hire verdict but the candidate declines the offer. What does the retrospective look like — what does the ML org learn about its offer competitiveness, its rubric, its interviewer pool? Half a page. The template lives outside the staff engineer's boundary (offer competitiveness is EM/comp/recruiter territory) but the staff-owned column — *did our loop's technical framing land well with the candidate* — is the source of half the *why they declined* signal.
