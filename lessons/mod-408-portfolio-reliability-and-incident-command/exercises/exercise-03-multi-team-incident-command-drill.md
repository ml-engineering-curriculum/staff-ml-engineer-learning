# exercise-03: Multi-Team Incident Command Drill

**Estimated effort:** 3 hours

## Objective

Run a **tabletop drill** of a multi-team ML incident on the portfolio you carried through exercises 01 and 02, using the Google-SRE-ICS-borrowed Incident Commander / Ops Lead / Comms Lead structure adapted to ML failure classes. The deliverable is a drill scenario package (pre-brief plus injects) plus a post-drill retrospective document — the artifact the Staff ML engineer runs as a rotation-training exercise and the artifact whose findings become the first inputs to exercise-04's postmortem template. After this exercise the learner should hold the third of the four artifacts this module produces and have first-hand experience with the coordination failures a real incident produces.

This is the input to exercise-04. The post-drill retrospective in this exercise is the raw material exercise-04's postmortem template is populated with; the ICS role failures observed in the drill become the `staffing change` and `runbook change` action-item categories in exercise-04.

## Prerequisites

- Exercise-01's SLI catalogue and exercise-02's severity matrix for the same portfolio. The drill scenario's triggers are catalogue SLIs at breach thresholds; the severity assignment is read from the matrix.
- Chapter 04 — the three roles (Incident Commander, Ops Lead, Comms Lead), the role-assignment-by-failure-class pattern, the six coherence practices for a five-hour incident, the escalate-up / escalate-sideways / hand-off rules, and the six pathologies. Read in full.
- Chapter 03 for the severity and shape vocabulary the drill scenario uses. The drill runs a Sev-0 shape; if you have not read chapter 3 you will not recognise the exec-notification tempo the scenario imposes.
- Site Reliability Workbook chapter 8 (Managing Incidents) and SRE Book chapter 14 (Managing Incidents). The three roles and the tempo are drawn directly from these chapters.
- Recommended: skim [FEMA's ICS-100 course overview](https://training.fema.gov/emiweb/is/icsresource/) for one lens on the wildland-fire ICS origin, and [PagerDuty's incident response documentation](https://response.pagerduty.com/) for the modern operational form. The drill's role assignments are consistent with both.

## Choose the drill format

The drill is a *tabletop* — nobody actually touches production. You need two-to-six participants. Priority order:

1. **Live tabletop with peers.** Two-to-six real engineers (Senior tech leads, peer EM, the platform SRE on-call, the Staff ML engineer as facilitator) run the drill in a 90-minute session. This is what the exercise is designed for; the coordination failures the drill catches are not visible when one person plays every role.
2. **Live tabletop with a single peer.** Two people can run a compressed drill in 60 minutes; one plays the IC + Ops Lead, the other plays the Comms Lead + the paging exec. Weaker but usable.
3. **Solo written walk-through.** As a last resort, the learner runs the drill as a written exercise — write the pre-brief, walk through each inject in turn, write the response for each role. The retrospective is weaker (no observed coordination failures) but the scenario-authoring and inject-authoring work still lands.

Do not skip the drill. Chapter 4 was explicit that the coordination failures the drill catches are the load-bearing lessons; a scenario package that is never run is not the deliverable.

## Steps

1. **Choose the drill scenario.** A shared-substrate breakage across two-to-three systems in the portfolio is the recommended shape — it exercises the multi-team command structure chapter 4 is designed for. Concrete candidates:
    - A shared user-embedding pipeline emits zero vectors for one hour; three downstream models (recommender, ranking, fraud) degrade simultaneously.
    - A schema-drift on an upstream event stream is silently absorbed by the feature pipeline; two models retrain on a subtly corrupted view and degrade over the following two weeks.
    - A retraining pipeline stall on a platform-team-owned shared trainer means fraud and email-personalization ship stale models for nine days; a downstream business-metric shift surfaces on day 10.
    Pick one. The scenario must span at least two teams in the portfolio; a single-team incident does not exercise the multi-team command structure.
2. **Write the pre-brief.** One page. Names the scenario, the participants, the roles being drilled, the length of the drill (90 minutes for a full tabletop, 60 for a compressed one), and the two-to-four learning objectives (which chapter-4 pathologies you want the drill to surface). Chapter 4's IC-is-also-Ops pathology and the two-ICs pathology are the two most commonly-surfaced in a first drill; scenario 1 above is designed to catch both.
3. **Write the injects.** Five-to-eight timed injects that arrive as the drill runs — a new symptom, a new dashboard datum, an exec ping, a hand-off request. Each inject has a timestamp (T+00, T+15, T+30, ...) and a target (the IC, the Ops Lead, the Comms Lead, everyone). Chapter 4's five-hour coherence practices are what the injects stress-test:
    - An early inject that forces the IC-naming decision within the first message. (Tests the IC-not-named pathology.)
    - A mid-drill inject that gives the IC deep technical context they are tempted to debug. (Tests the IC-is-also-Ops pathology.)
    - An exec ping from a random participant to the CTO chat. (Tests the exec-chat-run-by-whoever-answers pathology.)
    - A hand-off request at T+45 asking the IC to rotate. (Tests hand-off rigor.)
    - A parallel-rotation inject: the platform SRE has been running their own IC in a parallel war room for the last twenty minutes. (Tests the two-ICs pathology.)
    - A "false close" inject: the incident appears resolved, everyone starts to drift off, then the metric re-degrades. (Tests the never-officially-closed pathology.)
4. **Write the facilitator script.** A one-to-two-page document only the facilitator (usually the Staff ML engineer author of the drill) sees. Names when to deliver each inject, what the "correct" response to each looks like per chapter 4, and the coaching prompts to use if a participant misses. Chapter 4's six pathologies map one-to-one to the injects in step 3; the script names which pathology each inject is designed to surface.
5. **Assign roles.** Named for each participant. The IC is the Staff ML engineer for a shared-substrate scenario per chapter 4's role-assignment pattern; the Ops Lead is the pipeline owner's L30 tech lead; the Comms Lead is the peer EM or a TPM. A parallel-rotation IC (the platform SRE lead) is assigned to a fourth participant. If you have only two participants, roles double up per the compressed-drill guidance in step 0.
6. **Run the drill.** 90 minutes for the full form, 60 for the compressed. Facilitator delivers the injects on the timestamps in the script. Participants respond in role. The facilitator does not interrupt the response unless a hard block occurs (nobody speaks for two minutes; a participant slides fully out of role); coaching happens in the retrospective, not mid-drill.
7. **Write the retrospective.** Two-to-four pages, the load-bearing deliverable of this exercise. Structured as:
    - **Timeline.** The actual sequence of events during the drill — every inject, every response, every role-transition, every pathology observed. Chapter 4's single-timeline practice applies here; write the drill's timeline the way you would want the real incident's timeline written.
    - **Which pathologies surfaced.** For each of chapter 4's six pathologies, mark whether it surfaced (name the moment it did) or did not surface (with a caveat that a drill under-samples the pathologies a real incident produces).
    - **Where the ICS structure held.** The parts of the drill where the chapter-4 practices worked — the IC name landing in the first message, the timeline being kept, the Comms Lead absorbing the exec ping cleanly.
    - **Where the ICS structure broke.** The parts of the drill where a chapter-4 practice failed — the IC drifting into Ops, the two-ICs moment, the never-officially-closed moment.
    - **Role-specific coaching notes.** One-to-three lines per role on what the person playing that role does next time. Chapter 4 was explicit that IC coaching is one of the load-bearing peer-EM partnership functions from mod-401 chapter 5; if the peer EM was in the drill, they own this section.
    - **Portfolio-scope carry-overs.** Any pathology surfaced by the drill that is also visible in the last three months of real portfolio incidents. Chapter 5's portfolio-lesson mechanism starts with this line; the drill's coaching becomes a real action item if the pattern is portfolio-wide.
8. **Draft the follow-up action items.** Explicit list, chapter-5-shaped: which runbook needs updating, which pathology needs coaching-rotation across the on-call, which severity-matrix row needs walking back, which chapter-6 canary contract needs the sharpened rollback criterion the drill surfaced. Each action item has an owner, a due date, and a follow-up review date. This list becomes the seed of exercise-04's postmortem template's action-item section.

## Deliverable

Two documents plus an artifact-set:

**Document 1 — Drill scenario package** (1-2 pages plus injects):

- **Pre-brief.** Scenario, participants, roles, length, learning objectives (step 2).
- **Injects.** Five-to-eight timed injects with timestamps and targets (step 3).
- **Facilitator script.** Coaching prompts and pathology-mapping per inject (step 4). Facilitator-only.

**Document 2 — Post-drill retrospective** (2-4 pages):

- **Timeline.** Actual drill events, chapter-4-timeline-shaped (step 7).
- **Pathology surfacing analysis.** Six chapter-4 pathologies, marked (step 7).
- **Where the structure held / broke.** Two paragraphs each (step 7).
- **Role-specific coaching notes.** One-to-three lines per role (step 7).
- **Portfolio-scope carry-overs.** Which pathologies are portfolio-wide vs. drill-only (step 7).
- **Follow-up action items.** Owner / due / review date per item, chapter-5-shaped (step 8).

**Artifact-set (optional but recommended):** A short (two-to-three sentences) message posted to the ML-incidents channel afterwards summarising what the drill was, what surfaced, and what changes each participating team is committing to. Chapter 5's postmortem review meeting relies on cross-team visibility; the artifact-set is the mechanism by which the drill's lesson carries beyond the participants.

## Starter guidance

- **The drill must actually be run.** Chapter 4 was explicit that the coordination failures are the deliverable, not the scenario document. A scenario package that is never played is a template, not a drill. Run it — even with two people, in an hour, over a video call — before writing the retrospective.
- **The facilitator does not play a role.** If the Staff ML engineer facilitates the drill, they cannot also be the IC. Chapter 4's IC-is-also-Ops pathology applies at the meta level: a facilitator who is playing a role is not observing the drill and cannot coach the pathologies that surface. Assign facilitation to a dedicated person, or accept that the retrospective will be weaker.
- **The injects stress-test the pathologies deliberately.** Chapter 4's six pathologies map one-to-one to the injects in step 3. If your injects are all "here is another symptom", the drill exercises the technical response but not the command structure — and the command structure is what the drill is for. Include at least one inject per pathology.
- **The exec-ping inject is the most valuable.** In first drills, the exec ping is where the Comms Lead pathology surfaces — three people answer the exec chat, three different status updates go out, the exec's next message is "who is actually running this?". Chapter 4's Comms-Lead-not-named pathology is what this inject catches; a drill without an exec ping under-samples it.
- **The retrospective is where the value lands.** The 90-minute drill produces 30% of the value; the 30-minute retrospective produces the other 70%. Chapter 5's postmortem-meeting logic applies here — do not skip it. If you have to compress, compress the drill, not the retro.
- **Portfolio-scope carry-overs are the load-bearing bridge to exercise-04.** Chapter 5's portfolio-lesson mechanism is what makes drill findings matter beyond the participants. If your retrospective's carry-overs section is empty, either the drill was too narrow (single-team scenario) or the retrospective under-generalised. Push on it.
- **Do not run a scenario your portfolio would not actually see.** A LLM-hallucination scenario is fashionable but if your portfolio does not include an LLM system, it does not test your real command shape. Chapter 4's role-assignment-by-failure-class pattern is what you are drilling; pick a scenario that exercises the shape your portfolio will actually produce.

## Acceptance criteria

- Document 1 (scenario package) is complete: pre-brief, five-to-eight timed injects, and facilitator script including the pathology mapping for each inject.
- Document 2 (post-drill retrospective) is complete: timeline, pathology surfacing analysis for all six chapter-4 pathologies, where-held / where-broke paragraphs, role-specific coaching, portfolio-scope carry-overs, and follow-up action items.
- The drill scenario spans at least two teams in the portfolio (chapter 4's multi-team focus).
- The injects include at least one that stress-tests each of the following pathologies: IC-not-named, IC-is-also-Ops, exec-chat-run-by-whoever, two-ICs.
- Roles are named (real names for a live drill; role labels are acceptable for a solo walk-through).
- The retrospective's action-item list has owner, due date, and review date per item, chapter-5-shaped.
- The portfolio-scope carry-overs section names at least one pathology that is portfolio-wide (or explicitly declares none and defends the claim).
- The retrospective nowhere blames a participant by name; chapter 5's blameless framing carries over from real incidents to drills.
- The scenario nowhere requires a real production action (rollback, restart, page); it is a tabletop.

## Stretch goals

- **Run the drill twice.** Second drill uses a different scenario from step 1 and rotates participants through the IC / Ops / Comms roles. Chapter 4's coaching-rotation practice reads directly against this — a rotation whose IC pool is one person is under-resourced. Second-drill retrospective can be shorter but must call out which pathologies surfaced in both drills (portfolio-wide) vs. only the first (scenario-specific).
- **Draft the on-call IC coaching curriculum.** A one-page outline of what the L30 tech leads in the portfolio need to be able to do before they can be IC. Chapter 4's role-assignment pattern is what this reads against; the peer EM from mod-401 chapter 5 owns the coaching cadence. This is the artifact that turns the drill from a one-off event into a rotation-training program.
- **Run a parallel-rotation drill deliberately.** Recruit the peer platform SRE lead as a participant playing their own IC in a parallel war room. The chapter-3 rotation-model seam is what the drill exercises here; the retrospective captures the merge-or-parallel decision the two ICs made and whether it was the right one.
- **Simulate the exec debrief.** A one-page mock document — the summary the Comms Lead sends to the CTO chat 30 minutes after the drill starts and the follow-up written brief the CTO gets after the drill closes. Chapter 3's exec-notification threshold is what this exercises; a Comms Lead who cannot brief the CTO cannot do the role.
- **Take the drill scenario to the platform SRE team.** Ask their on-call to run the same scenario through their standard SRE playbook and post the transcript. Chapter 1's argument — that classical SRE catches zero of the four ML failure classes — is either supported or refuted by their transcript. Either way the finding is a chapter-5 portfolio-scope action item.
- **Draft the exec one-liner.** In one sentence at the top of Document 2: what the drill surfaced, what changed. "The drill surfaced the IC-is-also-Ops pathology and the parallel-rotation coordination failure; the ML on-call now assigns a dedicated IC on Sev-0, and the parallel-rotation escalation runbook lands next sprint." That sentence is what the director reads; the retrospective justifies it.
