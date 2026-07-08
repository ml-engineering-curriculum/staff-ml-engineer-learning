# Authoring the Multi-System RFC: The Artifact That Carries the Decision

## Motivation

Chapters 2 through 4 produced the analysis: a blueprint matrix, a failure-mode diagnosis, and a merge / split / share verdict. This chapter authors the **document** that carries that analysis into the org — the multi-system RFC. The RFC is the artifact the staff-plus IC is judged on. A brilliant analysis without a defensible RFC does not clear the L40 bar.

Two properties separate a multi-system RFC from the single-system RFCs a Senior tech-lead has already written:

1. It defends a decision that binds **more than one team's roadmap**, so it must be readable and criticiseable by every consumer team's lead — not by only the author's home team.
2. It defends both **migration cost** (engineering weeks) and **political cost** (relationship debt across teams). The single-system RFC rarely has to defend the political cost; the multi-system RFC always does.

Everything else — the section shape, the length, the review cadence — is scaffolding around those two properties.

## Prior art worth reading before you write

Three RFC traditions are worth having read once so your document lands in a familiar shape:

- **The IETF RFC series** — the original engineering RFC vocabulary (specifically the *Best Current Practice*, *Standards Track*, and *Informational* status distinctions). BCP 9 (RFC 2026) is the shortest read on why RFC processes exist.
- **The Rust RFC process** — [`rust-lang/rfcs`](https://github.com/rust-lang/rfcs) — the modern open-source template used by most language and platform projects. Sections: `Summary`, `Motivation`, `Guide-level explanation`, `Reference-level explanation`, `Drawbacks`, `Rationale and alternatives`, `Prior art`, `Unresolved questions`, `Future possibilities`. The Rust RFC template is the closest single template to what a multi-system RFC needs.
- **The Kubernetes Enhancement Proposal (KEP) process** — [`kubernetes/enhancements`](https://github.com/kubernetes/enhancements). KEPs are Rust RFCs plus explicit *stages* — `provisional`, `implementable`, `implemented`, `deferred`, `rejected`, `withdrawn`, `replaced` — which is the shape a multi-system RFC benefits from because portfolio decisions live longer than single-quarter cycles.
- **Google-style design docs** — the internal Google "Design Doc" template, popularised outside Google by Kruchten's *4+1 view model* and by essays like Jacob Kaplan-Moss's "Design docs at Google". These are lighter than Rust/KEP RFCs and are the right shape for the *executive-audience* one-pager the RFC pairs with.

You do not have to adopt any of these verbatim. You do have to write an RFC in a shape the reviewers *recognise*.

## The multi-system RFC template

The template below is Rust-RFC-shaped, with three additions specific to multi-team ML: the **portfolio context** section, the **political cost** section, and the **rollout and rollback** section. It is the shape exercise-04 asks you to author.

```
Title: <short imperative — "Consolidate ranking and home-feed feature namespaces">
Status: draft | in-review | accepted | rejected | superseded
Authors: <staff engineer + one co-author per consumer team>
Reviewers: <peer staff engineers, platform lead, EM peer, director>
Review deadline: <specific date, not "when ready">
Supersedes: <link to prior RFC if any>

1. Summary
    One paragraph. The verdict, the substrate cell, the affected teams, the timeline.

2. Portfolio context
    a. The three-to-five systems in scope, one paragraph each.
    b. The relevant slice of the blueprint matrix (which cells this RFC touches).
    c. What is deliberately out of scope.

3. Motivation
    a. The failure mode being addressed (pipeline-jungle / feature drift / registry fragmentation / eval conflict / other).
    b. The concrete evidence: incidents, drift measurements, launch-decision conflicts, exec asks that could not be answered.
    c. Why this is a staff-scope decision and not a single-team decision.

4. Detailed design
    a. The verdict — merge / split / share-behind-a-contract / defer.
    b. The proposed target-state of the affected cell.
    c. Consumer contract if applicable.
    d. Ownership assignment: specific team, specific on-call rotation.
    e. Enforcement mechanism (lint / CI / schema-registry check / manual review + cadence).

5. Alternatives considered
    a. Enumerate the other three verdicts, one paragraph each, and why they are not proposed.
    b. Optionally: prior proposals in this space, and why they did not land.

6. Migration plan
    a. Per-team migration schedule, in engineering-weeks with error bars.
    b. Backfill / historical-data effort estimate.
    c. Serving-side migration path per team, including canary and rollback.
    d. Decommissioning date for every superseded implementation.
    e. Operational load during the transition — who is on-call for the parallel-run period.

7. Political cost
    a. Per-consumer-team: which of {ownership loss / roadmap disruption / precedent risk} dominates.
    b. Mitigation per team (ownership swap / roadmap-slot deal / migration-help commitment).
    c. Explicit escalation path if any team blocks — director / VP contact and the trigger.

8. Rollout and rollback
    a. Rollout stages: first team, second team, remaining teams.
    b. Guardrail metric per stage (SLO regression thresholds, drift thresholds).
    c. Rollback plan per stage — what fires, who acts, on what timeline.
    d. "Definition of done" — a testable statement.

9. Prior art
    a. Public references for the substrate class (Feast, MLflow, TFX, etc.).
    b. Prior RFCs inside this org that touched adjacent decisions.

10. Unresolved questions
    a. Explicitly list open questions that will be resolved before status → accepted.
    b. Each question has a named owner and a target-resolution date.

11. Future possibilities
    a. What this RFC deliberately does not do that a future RFC will.
    b. If the verdict is defer or share-behind-a-contract, the trigger for the follow-up RFC.

12. Appendix — the blueprint matrix delta
    a. Before / after slices of the matrix.
    b. Any dependency-diagram changes.
```

Most RFCs land at 6-12 pages of prose plus an appendix. Longer than that and you have written a *design*, not an RFC; shorter and you have not defended enough to bind more than one team.

## Section-by-section pitfalls

Each section has a repeat failure mode. Naming them so exercise-04 does not fall into them.

**Section 1 — Summary.** Common failure: the summary is a title with adjectives. Force yourself to compress the verdict + affected teams + timeline into one paragraph a director could read in 20 seconds. If you cannot, you do not yet understand the verdict.

**Section 2 — Portfolio context.** Common failure: "the three systems are ranking, recommender, personalization" and no further colour. The consumer team lead who inherits your RFC after their tenure change deserves the one-paragraph reminder of what each system actually does.

**Section 3 — Motivation.** Common failure: vague "consolidation is good". The RFC that lands is grounded in concrete evidence — an incident number, a drift measurement, an exec ask that could not be answered. If you cannot cite the evidence, the failure is not yet urgent enough to justify the migration; wait a quarter and revisit.

**Section 4 — Detailed design.** Common failure: too much implementation detail. The RFC is a *decision* document, not an implementation guide. If the enforcement-mechanism paragraph needs code snippets, break the implementation guide into a separate ADR or design doc and link from the RFC.

**Section 5 — Alternatives considered.** Common failure: strawman alternatives. Reviewers will notice. Present each alternative in its strongest form — if the reviewer's honest response is "actually, alternative B is what we should do", you have written an RFC that is easier to iterate on than an RFC that only defends the author's choice.

**Section 6 — Migration plan.** Common failure: single-point estimates without error bars. Report as ranges. Name the two-to-three assumptions that dominate the error; when reality diverges from those assumptions the reader knows why.

**Section 7 — Political cost.** Common failure: missing entirely. Chapter 6's review will reject the RFC on this basis. Include it as its own section so it is not skippable.

**Section 8 — Rollout and rollback.** Common failure: rollback plan is *"we roll back if things break"*. Specify what fires the rollback (a specific metric, a specific threshold), who acts, on what timeline, and what the roll-back state is.

**Section 10 — Unresolved questions.** Common failure: too many unresolved questions. A draft can have many; an in-review RFC should have three-to-six; an accepted RFC should have zero. Each open question owns a specific person and a specific date. Otherwise "unresolved" becomes "forever".

## Authoring cadence

An RFC is not authored in one sitting. A typical cadence for a portfolio-scope RFC:

- **Week 0 — pre-write conversations.** Before opening the doc, talk to each consumer team lead and the peer platform lead. Ask them what would make them block. Bake the answers into the first draft. RFCs that go from zero to review without pre-write are rejected on grounds the author could have addressed in advance.
- **Week 1 — first draft.** Land sections 1-6 with placeholders in 7-11. Circulate to two-to-three trusted staff peers for a "does this hang together" pass. Do not circulate to consumer team leads yet.
- **Week 2 — second draft.** Complete sections 7-8, revise 1-6 based on peer feedback. Circulate to consumer team leads with a specific ask ("does the migration plan for your team match your team's next-quarter capacity?"). Give them a one-week window.
- **Week 3 — third draft.** Absorb consumer-team feedback into sections 6-8. Add a "changes from previous version" note at the top. Circulate to reviewers (peer staff engineers, platform lead, EM peer). Announce the review meeting.
- **Week 4 — architecture review.** Chapter 6 owns the review itself. Emerge with `accepted`, `accepted with revisions`, `deferred`, or `rejected`. If revisions are required, land them within a week.

Real cadences run longer. A politically-heavy portfolio RFC — one that touches four teams and asks for a merge — can take six-to-eight weeks from pre-write to accepted. That is not overhead; it is the work. A staff engineer who resents this cadence is either mis-scoped or mis-archetyped.

## The exec one-pager

The RFC itself is a document reviewers read. Leadership rarely reads a 12-page document. The RFC ships with a paired **one-page** exec brief. Its shape:

- **Top of page:** one-sentence verdict, one-sentence business impact, one-sentence risk.
- **Middle:** three-to-five bullets on what changes for which teams and when.
- **Bottom:** the two questions leadership is expected to weigh in on ("do we commit engineering headcount from the platform team", "do we accept the roadmap slip for the Home-feed team next quarter"), and the recommendation.

If the exec one-pager reads as a summary of the RFC, it is doing the wrong job. It should read as *a decision the exec is being asked to endorse*, with the RFC linked as the supporting document.

## What a rejected RFC teaches you

The rejected RFC is not a failure — it is a common outcome for the first-time staff-plus author of a multi-team decision. The four common rejection reasons and what they teach:

- **Scope wrong.** Reviewers say "this belongs to the platform team" (mod-405) or "this belongs to a single team" (L30). Fix: revisit chapter 1's scope definition. The RFC was authored at the wrong altitude.
- **Evidence thin.** Reviewers say "I don't see the drift measurement" or "I don't see the exec ask". Fix: the failure is not urgent enough yet. Author the diagnostic, do not author the merge.
- **Migration cost naive.** Reviewers say "you are asking for four teams' quarters and only defending one team's benefit". Fix: cost model was optimistic or the political-cost section was thin. Revise sections 6 and 7.
- **Owner unclear.** Reviewers say "who runs this after ship". Fix: the merged owner was hand-waved. Revise section 4d.

Absorb the feedback and re-open. The second-round RFC almost always lands. The staff-plus IC who authored an RFC that failed and then landed after a revision has produced a *better* signal than the staff-plus IC whose first draft passed unmodified — the org just watched them navigate cross-team feedback, which is exactly what the L40 bar is testing for.

## What the RFC is *not*

Three things a multi-team RFC deliberately is not, which the first draft usually is:

- **It is not an implementation guide.** Implementation-side detail — code, schema definitions, CI jobs, dashboards — lives in linked ADRs or design docs.
- **It is not a technology comparison.** Section 5 covers alternative *verdicts*, not alternative *vendors*. Vendor selection is a separate document (or a mod-405 conversation).
- **It is not a promotion packet.** The RFC's tone is neutral. The staff engineer's contribution is later described in their packet; the RFC itself does not need to remind the reader who authored it beyond the byline.

## Summary

- The multi-system RFC is the artifact chapters 2-4's analysis becomes; it is what the staff-plus IC is judged on.
- The template is Rust-RFC-shaped, plus three multi-team-specific sections: **portfolio context**, **political cost**, **rollout and rollback**.
- Prior art worth reading: IETF RFC 2026, the Rust RFC template, Kubernetes KEPs, and Google-style design docs.
- Authoring cadence is 3-6 weeks, not one sitting. Pre-write conversations with consumer team leads and the platform peer happen before the doc opens.
- The RFC pairs with a one-page exec brief that is a decision, not a summary.
- Rejected first-draft RFCs are common and expected; the revision cycle is the L40-bar signal.

The next chapter runs the architecture review that decides whether the RFC ships — how to structure the meeting, what to block on, what to nudge on, when to escalate.
