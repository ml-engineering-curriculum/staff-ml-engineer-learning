# Postmortems and Portfolio Lessons

## Motivation

The incident from chapter 4 closed at 14:07. The war room drops off, the Comms Lead posts the final update, the ranking team quietly rolls the model to the pre-incident checkpoint. Everyone is tired. This is where a normal org files the incident, writes a shallow "we'll add an alert" retrospective, and gets around to reading it three weeks later. And that is where the same class of failure re-occurs on a *different* team's system six months later — the recommender re-learns exactly what ranking learned in June, because the lesson was filed on ranking's wiki page and the recommender team's on-call never read it.

The Staff ML engineer's contribution at this point is not writing a better postmortem for the incident that just happened — that is the L30 tech-lead's craft, and the SRE Book's chapter 15 on postmortem culture, plus Allspaw's Etsy code-as-craft piece on blameless postmortems, cover it well. The Staff ML engineer's contribution is the **portfolio-lesson mechanism** that makes the lesson carry across teams. That is what this chapter is about.

It pairs with **exercise-04 — Portfolio Postmortem Template**.

## Blameless framing, briefly

Read John Allspaw's *Blameless PostMortems and a Just Culture* (Etsy code-as-craft blog, 2012) before this chapter; the argument is short and load-bearing. Also read the SRE Book's chapter 15 for the operational form. The three properties a blameless postmortem must have:

- **Focus on systems, not individuals.** "The on-call did not know the runbook was stale" is a systems statement. "Alex should have known the runbook was stale" is a blame statement. The first produces an action item (update the runbook, add a review cadence); the second produces defensiveness and no fix.
- **Include the honest hypotheses, not just the correct one.** Show what people thought was happening at each timeline point. The failure modes are usually visible in the discarded hypotheses, not in the fix.
- **Publish. Widely.** A postmortem read only by the team that produced it is 20% of the value. Chapter 6 of the Site Reliability Workbook is unambiguous that the postmortem review process — the meeting where a wider group reads the doc — is what actually spreads the lesson.

The John-Allspaw / David-Rasmussen / Sidney-Dekker "Learning from Incidents" community — Rein Henrichs and Nora Jones's LFI work is a good starting point — extends this into the human-factors literature. The extension is out of scope for this module; the operational form above is the load-bearing subset.

## The template — sections

The org-standard postmortem template exercise-04 asks you to author has ten sections. Not all are ML-specific; the ML-specific ones are the last three.

**1. Summary.** Two-to-four sentences. What broke, what the impact was, how long it lasted. Read-in-30-seconds; the exec who sees this in their weekly digest reads only this.

**2. Impact.** Users affected, revenue impact if computable, downstream systems affected. Include the *silent-failure* impact if applicable — the CTR-down-4%-for-six-weeks number belongs here, not just the acute outage number.

**3. Detection.** How was it discovered. Which SLI fired, which alert routed, which human noticed. If a human noticed before the SLI fired, that is the most important line in the postmortem — the SLI is missing.

**4. Timeline.** The live-timeline from chapter 4, cleaned up but not sanitised. Timestamped, with the honest hypotheses at each point.

**5. Root cause / contributing factors.** Plural on purpose — most ML incidents have three or four contributing factors, not one root cause. The "five whys" pattern from lean manufacturing gets you started; do not stop at the first cause you find.

**6. What went well.** The parts of the response that worked. This section is not decorative — it identifies practices you want to preserve as the rotation grows. If nothing went well, the postmortem is dishonest.

**7. What went wrong.** The parts of the response that failed. Includes the pathologies from chapter 4 (no IC named for 40 minutes, exec chat run by three people, timeline stopped after 2 hours) as well as the technical failures.

**8. Action items — team-scope.** The fixes the affected team will ship. Each action item has an owner, a due date, and a Sev-severity if it fires an SLO into the chapter-2 catalogue.

**9. Action items — portfolio-scope.** *The load-bearing ML-specific section.* The fixes that apply to other systems in the portfolio. Owner is the Staff ML engineer (accountable for landing), executor is the platform team or the receiving system's team. This section is why the postmortem is portfolio-scope and not single-team; without it the lesson stays on one team's wiki and the same failure repeats on another team.

**10. Prevention — catalogue changes.** Whether the incident revealed a missing or mis-tuned entry in the chapter-2 SLI/SLO catalogue, the chapter-3 severity matrix, or the chapter-6 retraining-as-deploy contract. This section is what makes the module's artifacts *iterated*, not decorative. The postmortem is where the catalogue grows.

## The portfolio-lessons section — the mechanism

Section 9 is what most orgs skip. Three properties it must have:

**1. Every action item has a portfolio-scope owner, not a team owner.** "Recommender team will add a canary check on user-embedding freshness" is a team-scope action item — belongs in section 8. "The portfolio will require a shared-feature-freshness canary check on every model that consumes a shared feature" is portfolio-scope. The owner is the Staff ML engineer; the executor is the platform team or each affected team; the artifact is a change to the catalogue in chapter 2 or the contract in chapter 6.

**2. Every action item names the other systems it applies to.** Ranking had the incident. The section explicitly says "this also applies to recommender, fraud, and email personalization because they consume shared features from the same pipeline family." The naming is the load-bearing bit; a portfolio-scope action item without a list of receiving systems is a wishlist.

**3. Every action item has a review date.** Not just a due date — a review date. On the review date, the Staff ML engineer verifies the action item landed on each named receiving system, or documents why not. Postmortem review-of-review is the discipline that makes the mechanism actually work; SRE Workbook's chapter 6 covers this pattern under "PIR (Post-Incident Review) tracking".

## The ML-specific action-item categories

Section 8 and 9 both use action items, and the categories are load-bearing. A postmortem whose action items don't fit clean categories is one whose lessons the org can't aggregate. The seven categories:

- **Catalogue change.** Add, remove, or re-band an SLI in the chapter-2 SLI/SLO catalogue. Owner: Staff ML engineer.
- **Severity change.** Re-route an alert, promote a Sev-3 signal to Sev-2, adjust the exec-notification threshold. Owner: Staff ML engineer.
- **Runbook change.** The on-call reached for a runbook that was stale, missing, or wrong. Owner: system owner (L30 tech lead).
- **Feature-pipeline contract change.** A shared feature needs a new consumer, a tighter SLO, a schema-registry entry, a canary check. Owner: feature-pipeline owner + platform team.
- **Retraining contract change.** The retraining-as-deploy contract in chapter 6 needs adjustment (a required offline replay slice, a canary criterion, a rollback condition). Owner: Staff ML engineer + platform team.
- **Monitoring substrate change.** The metrics substrate cannot compute an SLI the catalogue requires. Owner: platform / MLOps team; unblocks a Staff-owned catalogue change.
- **Training / staffing change.** The rotation needs a body, the L30 needs coaching on IC, the peer EM needs a partnership refresh (mod-401 chapter 5). Owner: peer EM.

Every action item in every postmortem should map to one of these. If it doesn't, either the category set is incomplete (rare) or the action item is not actually actionable.

## Blameless framing carried into ML — two ML-specific pitfalls

Two blame patterns are load-bearing at ML portfolio scope and worth calling out.

**1. Blaming "the model".** "The model made a bad prediction and we shipped it" is a blame pattern — it blames the artifact, and blaming the artifact usually means blaming the team that shipped it. The blameless reframe is: "the offline eval did not catch the failure mode; the canary criterion did not catch it either; the drift SLI did not catch it in the first 48 hours." That reframe produces action items in three categories (offline eval, canary, drift SLI); the artifact-blame reframe produces one and often the wrong one.

**2. Blaming "the data".** "The upstream event stream changed and we didn't know" is a blame pattern hidden as a systems statement. The blameless reframe: "the schema-registry contract between the upstream stream and our feature pipeline did not require compatibility checks; the feature pipeline did not have a schema-drift SLI; the model retrain did not detect the input-distribution shift." Three action items again; three categories.

The pattern is that at ML portfolio scope, blame usually attaches to the *artifact* (model, dataset) or the *upstream* (data). The blameless reframe always produces *at least three* action items — because the incident always had at least three loose defences to shore up. Chapter 5 of the SRE Book calls this the "defence in depth" property of a mature postmortem.

## The postmortem review meeting

The doc is 30% of the value; the meeting is 70%. Run it two weeks after the incident closes. Attendees: the affected team, the other teams the portfolio-lessons section calls out, the peer EM, the platform-team lead. The Staff ML engineer runs the meeting.

Format: 15 minutes for the affected team to walk the timeline. 15 minutes for the portfolio-lessons section — every named receiving team explicitly says whether the action item makes sense on their system, and disagreements are called out for follow-up. 10 minutes on the catalogue changes. 15 minutes on the two blameless pitfalls above — the group looks for language in the doc that is still artifact-blame or data-blame and rewrites it. Five minutes to schedule the action-item review date.

The meeting has a fixed script; skipping any of the four blocks leaks value.

## Summary

- The Staff ML engineer's contribution to postmortems is **not** writing better ones — it is the **portfolio-lesson mechanism** that carries a lesson from one team's incident to prevent the same class of failure on another team's system.
- The **template** has 10 sections; the ML-specific load-bearing ones are (8) team-scope action items, (9) portfolio-scope action items, and (10) catalogue changes.
- Section 9 works when every action item has a **portfolio-scope owner, an explicit list of receiving systems, and a review date**.
- Action items map into **seven ML-specific categories** — catalogue, severity, runbook, feature-pipeline contract, retraining contract, monitoring substrate, staffing. If they don't fit, they aren't actionable.
- **Blameless framing** carries over from Allspaw / SRE Book / LFI community, with two ML-specific pitfalls: **blaming the model** and **blaming the data**. Both reframes produce at least three action items instead of one.
- The **review meeting** runs two weeks post-close; it converts the doc from 30% of the value to 100%.

The next chapter closes the module on the retraining-as-deploy contract — the mechanism the platform team enforces so a retrained model, the most common source of a bad-predictions incident, cannot ship without a canary path, a rollback trigger, and an offline replay.
