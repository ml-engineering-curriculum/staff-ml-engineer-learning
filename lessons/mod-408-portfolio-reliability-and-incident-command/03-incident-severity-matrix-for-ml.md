# The Incident Severity Matrix for ML

## Motivation

The org already has a severity matrix. Sev-0 through Sev-3, page-vs-ticket rules, exec-notification thresholds — the peer platform SRE team wrote it years ago and it works well for "the checkout API returned 500s to 12% of users for six minutes." What it does not have a column for is: *the recommender was serving well-formed, low-latency, low-error-rate predictions for six weeks that were quietly 4% worse.* That is a portfolio-scale ML incident nobody paged for, and if you drop it into the existing matrix it slots as Sev-4 ("informational") and dies as a ticket.

This chapter authors the ML-side severity matrix — the one that is *layered on top of* the org's existing SRE severity matrix rather than replacing it, and that adds the silent-failure column classical SRE has no vocabulary for. It pairs with **exercise-02 — ML Incident Severity Matrix**.

The load-bearing decision in this chapter is where the *page* threshold sits. The Site Reliability Workbook's chapter on managing incidents is unambiguous that alert fatigue is the primary failure mode of a maturing on-call rotation; paging on every drift SLI breach is the fastest way to burn out the ML on-call and make everyone stop trusting the pager. Get this line right and the rest of the module lands; get it wrong and the catalogue in chapter 2 loses credibility inside a quarter.

## Two shapes of ML incident

Before writing the matrix, name the two shapes it must cover — because they behave differently under severity, differently under command, and differently under postmortem.

**Shape A — User-visible ML incident.** The service is up, but the ML output is *materially, immediately* wrong in a way a user notices. The recommender starts recommending a single item on every impression because a shared user-embedding pipeline returned zero-vectors for six hours. Fraud starts blocking every legitimate transaction because a feature-schema drift pushed all scores above threshold. These look like classical SRE incidents from the outside — angry customers, tweets, exec pings — but the *cause* is in the ML failure surface, so the classical SRE playbook does not diagnose them. Shape A is where "the platform SRE paged us" turns into "the ML team paged the platform SRE."

**Shape B — Silent-failure ML incident.** The service is up, the users have not noticed, no page has fired — and something is quietly wrong. Six weeks of steady 4% CTR decline; a fraud-model recall that has drifted from 92% to 87% over a month; a stalled retraining pipeline that hasn't produced a promotable candidate in fifteen days. Nobody paged because classical SRE doesn't know these are events. The ML on-call may not even see them without deliberate instrumentation. The severity is real — the cumulative business impact of a silent-failure incident routinely exceeds the acute user-visible one — but the response tempo is completely different.

The two shapes need different columns in the severity matrix, different command structures (chapter 4), and different postmortem categories (chapter 5). Trying to run a silent-drift incident with a Sev-1 shape — "war room, five people on a call, 15-minute status updates" — makes people cynical about severity. Trying to run a user-visible outage with a Sev-3 shape — "we'll get to it this sprint" — costs the ML org's credibility.

## The four severity tiers

Adopt the org's existing severity nomenclature — Sev-0 through Sev-3 or P0 through P3, whichever the platform SRE team is using — but layer the ML-specific criteria and response shape into it. A canonical set:

### Sev-0 — Portfolio outage

**Definition.** Two or more ML systems in the portfolio are user-visibly broken *or* one system serving material user traffic (>1M requests/day, tier-1 product surface) is unusable.

**ML failure classes that qualify.** Feature-pipeline breakage cascading across shared consumers; a shared user-embedding pipeline returning zeros to fraud, recommender, and ranking simultaneously; an inference-substrate outage affecting the primary revenue model.

**Response shape.** Page immediately. War room. Incident Commander (chapter 4) named within 15 minutes. Exec notification within 30 minutes — the CTO, VP Eng, and the peer EM whose team owns the affected surface. Public-facing status update if the failure is user-visible.

**Duration expectation.** Resolve or fully mitigate within 4 hours; escalate to Sev-0-extended (an exec-tracked incident) if it runs longer. The Site Reliability Handbook / SRE Workbook's chapter on incident command is where the 4-hour tempo comes from.

### Sev-1 — Single-system incident

**Definition.** One production ML system is materially degraded — either user-visible (Shape A) or a hard-red silent-failure signal (Shape B) that has crossed the error-budget ceiling in a way that predicts imminent user visibility.

**ML failure classes that qualify.** Bad-predictions incident with a downstream business-metric shift confirmed by dashboard; feature-freshness SLO breach on a shared feature for >30 minutes; retraining SLA breach >2× (model is ≥2× older than SLO); calibration SLI in "red" per the chapter-2 error-budget policy.

**Response shape.** Page the on-call for the affected system. Ping the Staff ML engineer for portfolio-scope coordination. Incident Commander named if the incident is expected to run >1 hour. Exec notification only if the incident is user-visible.

**Duration expectation.** Resolve within 8 hours. Postmortem is required; portfolio-lesson section is required.

### Sev-2 — Ticketed portfolio issue

**Definition.** A single-system SLO breach that has not yet crossed into user-visible territory, or a portfolio-standard breach where the SLI is in "yellow" territory per the chapter-2 policy. Also: a silent-failure incident whose measurement window is short enough (e.g., two consecutive daily calibration windows) that immediate action is appropriate but pager-grade urgency is not.

**ML failure classes that qualify.** Drift SLI in "yellow"; feature-freshness p99 breach with no p50 breach; a single failed retraining pipeline run that is expected to recover on the next scheduled attempt; a judge-model agreement rate breach on the LLM-side SLI.

**Response shape.** Ticket. On-call acknowledges within 24 hours; work begins the same business day. No war room, no exec ping. Portfolio-lesson tag on the ticket if the underlying failure class has been seen elsewhere in the portfolio in the last 90 days.

**Duration expectation.** Resolve or promote to Sev-1 within 5 business days.

### Sev-3 — Informational / rolling ticket

**Definition.** A single-window SLI observation below SLO that has not yet reached the two-window rule from chapter 2, or a silent-failure signal that is worth tracking but not worth acting on until it accumulates.

**Response shape.** Log to the rolling reliability review (weekly or biweekly). No pager, no on-call action. The Staff ML engineer reviews the accumulated Sev-3s at the review and promotes to Sev-2 if a pattern emerges.

**Purpose.** The Sev-3 tier exists specifically to prevent alert fatigue on the flapping-window signals — a single-window PSI spike, a single-batch judge-agreement dip. If you don't have a Sev-3, teams will either ignore Sev-2 (alert fatigue) or paper over Sev-3-shape signals (invisibility). Both routes fail.

## The page-vs-ticket-vs-log line

The single most important rule the matrix must encode is: **which SLI breaches page, which ticket, which log**. Get this wrong and the on-call trusts nothing.

Three heuristics for drawing the line:

**1. Pages require an actionable immediate response.** If the on-call gets paged at 3 AM and there is no action they can take before business hours, that page should not have fired. A single-window PSI breach at 3 AM: no action, all diagnosis, wait for a second window. That is a ticket, not a page. A shared-feature freshness SLO breach at 3 AM: the on-call can trigger the feature-pipeline recovery runbook. That is a page.

**2. Silent-failure signals rarely page in the moment.** Silent-drift, retraining-stall, and calibration-decay SLIs *almost never* page on a single-window breach. They page when they cross the two-window rule from chapter 2, or when they have breached SLO for a duration that predicts user-visible impact (the retraining SLA at 2×, the calibration SLI in "red"). This is the load-bearing distinction between Shape A and Shape B incidents.

**3. Multi-consumer feature breakages page the feature owner, notify the model owners.** A shared feature's freshness SLO breach pages the *feature pipeline owner*. Every downstream model's on-call gets a courtesy notification. This is the portfolio-scope discipline from the chapter-2 catalogue applied to alert routing.

## The exec-notification threshold

Separate from the page/ticket line: the **exec-notification threshold** — when the CTO, VP Eng, or the peer EM should be told an incident is running. Two rules:

**1. Sev-0 always. Sev-1 sometimes.** Sev-0 pages the exec chat within 30 minutes. Sev-1 only pings the exec chat if the incident is user-visible or expected to become so within the working day. A silent-failure Sev-1 that is under active investigation and not expected to reach users is a Staff-ML-engineer conversation with the peer EM, not an exec ping.

**2. Duration is the exec's real trigger.** A Sev-1 running past 4 hours becomes an exec conversation regardless of shape — because it is no longer a routine on-call event, it is a program risk. The 4-hour threshold is roughly aligned with the SRE Workbook's incident-command "prolonged incident" definition; borrow their tempo.

Exec notifications are opinionated — early is better than late. The failure mode is the exec learning about an ML incident from Twitter or an analyst report before the ML org tells them. Once. The org never re-trusts the ML on-call after that. If in doubt, notify.

## The silent-failure column classical SRE lacks

Everything above works because we forced the silent-failure signals to have severity mappings and page/ticket/log rules. In a matrix imported unmodified from the platform SRE team, calibration decay and drift SLIs default to Sev-3 informational and disappear. **The single largest content contribution the ML severity matrix makes over the peer SRE matrix is the Sev-1 and Sev-2 mappings for silent-failure signals.** Chapter 5 (postmortems) counts action items that came out of silent-failure Sev-1s as the most valuable ones in the portfolio — because they prevented an outage that would otherwise have been a Sev-0 later.

If the org resists this — if the platform SRE team says "we only page on user-visible signals" — the ML org runs a *parallel* alert routing (a distinct channel, distinct rotation) rather than trying to fit into the SRE-shaped one. Chapter 4 covers when the two rotations merge into a single Incident Commander and when they run parallel.

## Building the matrix — practical shape

The output of exercise-02 is a table. Rows are the SLIs from the chapter-2 catalogue (one row per SLI per system, or one row per SLI family with system-column overrides). Columns are:

- **SLI + threshold.** Which SLI, what breach threshold triggers the row.
- **Shape.** Shape A (user-visible) or Shape B (silent).
- **Severity.** Sev-0 through Sev-3.
- **Route.** Page / ticket / log.
- **Owner.** Feature owner / model owner / platform SRE.
- **Exec-notify?** Yes always / yes if user-visible / no.
- **Runbook.** Link to the runbook the on-call reads first.

Aim for one page. If the table is longer than one page, either you have collapsed too much per-system nuance into the portfolio row (split per system) or you have too many SLIs in the catalogue (prune in chapter 2). The severity matrix is a **read-in-30-seconds artifact** the on-call opens at 3 AM; it is not a policy manual.

## Summary

- ML incidents come in two shapes: **user-visible (Shape A)** and **silent-failure (Shape B)**. Classical SRE severity matrices cover Shape A well and Shape B not at all.
- The ML severity matrix is **layered on top of the org's existing SRE matrix** with the four canonical tiers **Sev-0 (portfolio outage) / Sev-1 (single-system incident) / Sev-2 (ticketed) / Sev-3 (rolling)**. Silent-failure signals from the chapter-2 catalogue must have Sev-1 and Sev-2 mappings; if they default to Sev-3, they are invisible.
- The **page-vs-ticket line** is the single most load-bearing rule. Pages require an actionable immediate response; silent-failure signals almost never page on a single-window breach.
- The **exec-notification threshold** is Sev-0 always, Sev-1 sometimes (user-visible or long-running), and duration escalates the rule.
- The output is a **one-page table** the on-call reads at 3 AM. Longer than one page and the matrix is not doing its job.

The next chapter turns the severity into a command structure — who is Incident Commander, who is Ops Lead, who is Comms Lead, and how the command survives contact with three ML teams and the platform SRE for five hours.
