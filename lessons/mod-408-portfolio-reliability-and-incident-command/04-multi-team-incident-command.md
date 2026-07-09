# Multi-Team Incident Command for ML

## Motivation

At 09:12 the recommender's PSI SLI turns red. At 09:14 fraud's calibration SLI turns yellow. At 09:20 the ranking on-call posts to the ML-incidents channel that the ranking p50 latency is up 40%. Three teams are seeing three different symptoms. By 09:45 someone — probably you — realises the shared user-embedding pipeline emitted zero vectors for the last hour because of a schema-drift upstream and all three models have been scoring on garbage inputs. The platform SRE team is looking at their availability dashboards and reporting green.

That is a Sev-0 ML portfolio incident. The Staff ML engineer's job is to make sure the next four hours produce a resolution rather than three parallel triage efforts, a confused exec chat, and a postmortem nobody trusts. This chapter is that playbook.

The framework is Google SRE's **Incident Command System** as documented in chapters 12-14 of *Site Reliability Engineering* and refined in the *Site Reliability Workbook*'s "Managing Incidents" chapter. It is itself a specialisation of the wildland-fire **Incident Command System (ICS)** developed in the US in the 1970s — the origin document is the FIRESCOPE program's post-1970-California-fires organisational analysis. The ICS shape works because it was designed for exactly the situation above: many responders, many sub-teams, one commander, unclear scope, high tempo. Adapt it faithfully; the deltas we introduce are for the ML failure classes, not for ICS itself.

This chapter pairs with **exercise-03 — Multi-Team Incident Command Drill**.

## The three roles

The ICS-borrowed structure has three named roles in an ML portfolio incident. Every Sev-0 and every Sev-1 that is expected to run over an hour gets all three named within 15 minutes.

**Incident Commander (IC).** The single person accountable for the incident. Not necessarily the technically deepest person — usually not, on purpose. Their job is coordination, not debugging: name the roles, run the status cadence, hold the timeline, decide when to escalate, decide when to close. The SRE Book's chapter 14 is unambiguous that **the IC's job is to run the incident, not to fix it**; a Staff ML engineer who drops into IC and starts reading logs has stopped being the IC. Chapter 5's postmortems always call out ICs who slid into Ops as an anti-pattern.

**Ops Lead (Operations Lead).** The technical lead of the response. In a shared-feature-breakage incident, this is the feature-pipeline owner's tech lead; in a bad-predictions incident, this is the model owner's tech lead. The Ops Lead directs the fix — reads the traces, runs the recovery commands, calls in specific engineers. They report status to the IC on the cadence the IC sets. When Ops needs a second responder (someone from the platform SRE, someone from another ML team), Ops asks the IC and the IC brings them in; Ops does not recruit independently.

**Comms Lead (Communications Lead).** The single voice out of the incident. Runs the exec-notification channel, the customer-facing status page (if applicable), and the internal broadcast to peer teams. The Comms Lead is not usually an engineer — the peer EM or a technical program manager frequently fills this role. When the CTO pings the exec chat asking for status, the Comms Lead answers, not the IC, not the Ops Lead. This division is what lets Ops keep debugging and IC keep coordinating.

The classical SRE model also names a **Planning role** for long incidents; on a portfolio ML incident that turns into a multi-day investigation, adopt it. Most Sev-0 and Sev-1 incidents close before Planning is needed.

## Who plays which role in an ML portfolio incident

The role assignments are not fixed by title — they depend on the failure class and the affected surface. A pattern that works:

- **Feature-pipeline breakage across shared consumers (chapter 1's failure class 3).** IC is the Staff ML engineer (the incident spans teams and the coordination overhead is high). Ops Lead is the feature-pipeline owner's L30 tech lead. Comms Lead is the peer EM of the most-affected model team, or a TPM if one exists.
- **Bad predictions confirmed by downstream business metric on one system (failure class 1).** IC is the L30 tech lead of the affected model team. Ops Lead is the same team's on-call. The Staff ML engineer is *not* IC — they are on the call as a portfolio-scope advisor. Comms Lead is the peer EM.
- **Silent-drift Sev-1 that escalated because two consecutive windows breached (failure class 2).** IC is the L30 tech lead. The Staff ML engineer is a consulted party but not IC unless the drift pattern is portfolio-wide. Comms is often skipped or lightweight because the incident is not user-visible.
- **Retraining-loop stall on a shared pipeline (failure class 4).** IC is the retraining pipeline's owning L30 tech lead. If the pipeline is shared across teams — the platform team's pipeline consumed by three model teams — the Staff ML engineer is IC and Ops is the platform team's on-call.

The pattern is the same: **the IC is the person who has to make coordination calls, not the person who has the deepest technical context**. When the Staff ML engineer picks up IC, they consciously trade off *not* debugging; if the org staffs incidents in a way that always makes the Staff ML engineer play Ops, the portfolio is under-staffed for incident command and the postmortem action item is to fix that staffing.

See [`../mod-401-staff-ml-role-scope/`](../mod-401-staff-ml-role-scope/) chapter 5 for the peer-EM partnership contract this role assignment sits inside.

## The five-hour incident: keeping it coherent

Most ML portfolio Sev-0s that get complex run 3-6 hours from page to full recovery. Coherence over that window is the hard part. Six practices that hold:

**1. Fixed cadence status updates.** Every 30 minutes for a Sev-0, every 60 minutes for a Sev-1. The IC writes them, not Ops. The template is: *what is broken, what we know, what we're trying, ETA to next update*. If the IC misses the cadence, either the incident has actually resolved (close it) or the IC has slid into Ops (reset the roles).

**2. A single timeline, updated live.** One shared document — Google Doc, Notion page, Slack canvas — with a running timeline. Every material action, hypothesis, or discovery gets a timestamp. The IC or a scribe (a fourth role, sometimes) owns this. The postmortem in chapter 5 reads directly off this timeline; skip the timeline during the incident and the postmortem is unreliable.

**3. Explicit hand-offs at role rotation.** A five-hour incident is longer than any single person should hold IC. Roll the IC role at three hours if the incident is still open. The rotation has a fixed script: outgoing IC summarises the timeline, the outstanding hypotheses, the exec-notification state, and the next update ETA; incoming IC re-broadcasts to the war room and takes over. The Site Reliability Workbook's incident-management chapter calls out hand-off rigor as one of the two most-missed practices in maturing rotations (the other is the timeline).

**4. Explicit scope for who is *not* in the room.** The other ML teams' on-calls do not need to be in the war room — they need to know the status. The Comms Lead pushes updates on the ML-incidents channel; other teams stay in their own dashboards. A war room with 12 people in it is a war room that produces no decisions. Cap the room at 5-6 named roles.

**5. Explicit "declare and de-escalate" moment.** The IC declares the incident. The IC de-escalates or closes it. Both moments are load-bearing — declaration triggers the exec-notification path; closure freezes the timeline and starts the postmortem countdown. A drifted-closed incident (nobody ever formally closed, everyone stopped paying attention) produces a bad postmortem and often a repeat.

**6. The "am I still IC?" check.** Every 60 minutes the IC asks themselves: am I actually running this, or has the technical work absorbed me? If the answer is the second, escalate — hand off to the peer EM or to a rotating IC. Nobody's ego is worth the coordination failure.

## When to escalate, when to hand off

**Escalate up.** Sev-0 running past 4 hours: notify the CTO's chat and prepare a written brief. Sev-1 that is discovered to be portfolio-wide (the recommender's PSI-breach is actually a shared-embedding failure that also affects fraud): promote to Sev-0 and re-run the role assignments. An IC who is stuck between two ML teams that disagree on the fix: escalate to the peer EM chain for arbitration; the IC role does not adjudicate technical disputes between teams' senior engineers.

**Escalate sideways.** A latency-and-error incident that turned out to be an ML failure class: the platform SRE IC hands off IC to the Staff ML engineer, with an explicit turnover. The reverse: an ML incident that turned out to be a Kubernetes networking bug hands off to the platform SRE IC. This is the load-bearing seam between the two rotations from chapter 3.

**Hand off internally.** Three-hour rule. Long incident: rotate ICs, rotate Ops if the Ops Lead is fatigued. The Ops Lead should not be reading traces after five continuous hours.

## What can go wrong in the incident

Six pathologies you will see and are responsible for preventing:

- **Everybody-debugs, nobody-decides.** Six engineers in the war room, no IC named, every hypothesis has a champion, nothing gets executed. Fix: name IC in the first message, hard.
- **Two ICs.** Platform SRE ran their own IC and the ML org ran theirs; the incident had two war rooms. Fix: the chapter-3 seam between rotations, and an explicit merge as soon as the incident is understood to span both.
- **The IC is also Ops.** Staff ML engineer picks up IC, then starts reading logs. Fix: chapter-3 hand-off, or explicit resignation from IC and re-assignment.
- **Exec chat run by whoever answers first.** Comms Lead not named, three different people give three different status updates to the CTO. Fix: name Comms in the first message, hard, and the IC repeats "route all exec questions to <name>" every 30 minutes.
- **No timeline.** Nobody wrote down what was tried when. Postmortem is guesswork. Fix: appoint a scribe if the IC cannot both run coordination and take notes; long incidents always need one.
- **The incident never officially closed.** Everyone drifted off; no de-escalation moment; no postmortem countdown started. Fix: the IC's last act is an explicit close message with the timeline snapshot attached.

## What this looks like for a silent-failure Sev-1

Most of this chapter is written for Sev-0 shape. For a silent-failure Sev-1, the shape compresses:

- The IC is the L30 tech lead of the affected system.
- Ops Lead is often the same person, wearing two hats — acceptable at Sev-1 as long as the technical work is bounded.
- Comms Lead is skipped; a lightweight update to the ML-incidents channel replaces exec broadcast.
- Cadence is every 2 hours or every business day, not every 30 minutes.
- The Staff ML engineer is on the ticket as a subscriber, not in a war room.

The point is that ICS scales down — it does not require the full ceremony on every incident. It requires the full ceremony on the incidents that need it. Chapter 3's severity matrix is the switch.

## Summary

- Multi-team ML incident command adopts **Google SRE's ICS structure** (Incident Commander, Ops Lead, Comms Lead) faithfully, itself borrowed from the wildland-fire ICS via the FIRESCOPE program.
- The **IC coordinates**, does not debug. A Staff ML engineer who slides into Ops has stopped being the IC. Ops directs the fix. Comms owns the single voice out of the incident.
- **Role assignment depends on failure class**: shared-substrate breakages usually put the Staff ML engineer at IC; single-system incidents put the L30 tech lead at IC with the Staff engineer as consulted party.
- **Five-hour coherence** requires fixed status cadence, a live timeline, explicit hand-offs at rotation, a bounded war room, and an explicit declare-and-close moment.
- **Escalate up** on Sev-0 past 4 hours or Sev-1 discovered to be portfolio-wide; **escalate sideways** at the platform-SRE / ML-org seam; **hand off internally** every three hours on long incidents.
- The command shape **compresses at Sev-1 silent-failure**; the Sev-0 ceremony is for the incidents that need it, not for every incident.

The next chapter turns a well-run incident into a portfolio-scope postmortem — the template, the blameless framing, the ML-specific action-item categories, and the mechanism that stops the same failure repeating on a different team's system.
