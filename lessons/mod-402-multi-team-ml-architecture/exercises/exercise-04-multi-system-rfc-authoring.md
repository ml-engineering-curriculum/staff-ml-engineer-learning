# exercise-04: Multi-System RFC Authoring

**Estimated effort:** 3 hours

## Objective

Roll the decision doc from exercise-03 up into a full multi-system RFC using the chapter-5 template, and pair it with the one-page exec brief. After this exercise the learner should hold the artifact that a staff-plus IC actually walks into an architecture review meeting with — the one whose *shape* will determine whether the decision lands.

The exercise is deliberately narrow: it is not "author a new decision"; it is "convert your exercise-03 analysis into the shape reviewers expect". Learners often discover in this exercise that their exercise-03 analysis had thin sections that were invisible until the RFC template forced them out. That discovery is the point.

## Prerequisites

- Exercise 03 complete, with the decision-doc sections landed.
- Chapter 05 — Multi-System RFC Authoring read in full. The template, the section-by-section pitfalls, the authoring cadence, and the exec one-pager.
- Chapter 06 — Architecture Review skimmed. You do not run the review in this exercise, but knowing what reviewers look for shapes what the RFC includes.
- Recommended: read one real RFC from either [`rust-lang/rfcs`](https://github.com/rust-lang/rfcs) or [`kubernetes/enhancements`](https://github.com/kubernetes/enhancements) end-to-end. Twenty minutes. It re-tunes your sense of the appropriate length and voice.

## Steps

1. **Set up the RFC skeleton** using the chapter-5 template. Copy every section header including the appendix. Do not delete a section because it feels irrelevant; if it is genuinely irrelevant, replace its body with one sentence explaining why.
2. **Land Section 1 (Summary) last.** Skip it initially and come back after the rest of the RFC is drafted. A first-draft summary always over-promises and under-informs; a last-draft summary is a genuine compression of what the RFC decided.
3. **Land Section 2 (Portfolio context).** Pull from exercise-01 for the three-to-five-system overview and the relevant slice of the blueprint matrix. Cite the matrix in the appendix rather than re-drawing it; the RFC should reference the exercise-01 artifact, not duplicate it.
4. **Land Section 3 (Motivation).** Pull from exercise-02 for the failure-mode label and evidence. This is where the drift measurement, incident number, or exec ask goes. If any of these are `unevidenced`, name that explicitly — chapter 5 warns that vague "consolidation is good" motivation reliably fails review.
5. **Land Section 4 (Detailed design).** Pull the verdict from exercise-03. Add the ownership-assignment paragraph — specific team, specific on-call. Add the enforcement-mechanism paragraph if the verdict is share-behind-a-contract.
6. **Land Section 5 (Alternatives considered).** Pull the three non-chosen verdicts from exercise-03, one paragraph each. Present each in its strongest form.
7. **Land Section 6 (Migration plan).** Pull the migration-cost model from exercise-03. Add per-team schedule with milestones. Add the decommissioning date(s). Add the rollback plan per stage.
8. **Land Section 7 (Political cost).** Pull the political-cost model from exercise-03. Add per-team mitigations. Add the escalation path — director-or-higher contact and the trigger — even if you do not expect to use it.
9. **Land Section 8 (Rollout and rollback).** Rollout stages with a guardrail metric per stage. Rollback plan per stage with a fires-when threshold and a named human.
10. **Land Section 9 (Prior art).** Public references for the substrate class — Feast for feature store, MLflow for registry, TFX for data-plane substrate, Kohavi et al. for eval / experimentation, Sculley et al. for the failure-mode taxonomy — plus any prior RFCs in the org that touched the same substrate.
11. **Land Section 10 (Unresolved questions).** Three-to-six is normal for an in-review RFC. Each question has a named owner and a target-resolution date. Zero unresolved questions usually means you are hiding assumptions; more than seven means the RFC is not yet ready.
12. **Land Section 11 (Future possibilities).** What this RFC deliberately does not do. If the verdict is defer or share-behind-a-contract, the trigger for the follow-up RFC.
13. **Land Section 12 (Appendix — matrix delta).** Before / after slices of the blueprint matrix affected by the decision. Any dependency-diagram edits.
14. **Return to Section 1 (Summary).** Write the one-paragraph summary. Include verdict, substrate cell, affected teams, and timeline. Time-box: five minutes and no longer. If the summary takes half an hour, the RFC is not yet compressed enough — usually a signal that some later section is doing summary work that belongs at the top.
15. **Author the paired exec one-pager.** Chapter 5 walked its shape: verdict, business impact, risk at the top; three-to-five bullets in the middle on what changes for which teams and when; the two decisions the exec is being asked to endorse at the bottom. Do not write it as a summary of the RFC; write it as a decision the exec is being asked to endorse.

## Deliverable

Two artifacts:

**Artifact 1 — the RFC.** A single document, 8-14 pages, in the chapter-5 template. All twelve sections landed. Appendix carries the blueprint-matrix delta.

**Artifact 2 — the exec one-pager.** A single-page document paired with the RFC. Reads as a decision request, not a summary. Links to the RFC for supporting detail.

Optional companion: a **short authoring log** — a paragraph noting which sections took the longest to write, which sections revealed thin analysis in exercise-03, and which section you would revise first if a reviewer sent it back for a second draft. This is a private reflection artifact, not part of the RFC, but is high-value for tracking your own growth.

## Starter guidance

- **Author section 1 last.** Repeated because it is the most common process failure. Learners write a summary first, spend the next four hours proving the summary right, and produce an RFC whose body reads as advocacy rather than analysis. Write the summary last so it compresses what the RFC actually decided.
- **Do not delete a section because it feels irrelevant.** Chapter 5 was explicit — if a section is genuinely irrelevant, replace its body with one sentence explaining why. A missing section reads as "the author did not think about this"; a section with a one-sentence "not applicable and here's why" reads as "the author thought about this and decided it was not applicable".
- **The Unresolved-Questions section is not optional.** New authors under-populate this section because it feels like admission of incompleteness. It is not; it is the honest catalogue of what is not yet decided. Reviewers respect a well-populated unresolved section; they distrust an empty one.
- **The exec one-pager is not the summary.** Chapter 5 was explicit: the exec brief reads as a *decision the exec is being asked to endorse*, with the RFC as the supporting document. If your one-pager reads as *"here is what the RFC decided"*, you have written the wrong document.
- **Time-box each section on the first draft.** Section 4 (Detailed design) is the section that will eat all your time if you let it. First-draft target: 30 minutes per section. The polish pass happens after every section is in place.
- **The prior-art section is not decorative.** Section 9's references are the vocabulary the reviewers already speak. Pick two-to-three genuinely load-bearing references and cite them where they actually inform the RFC's design, not as a bibliography at the end.

## Acceptance criteria

- RFC lands 8-14 pages with all twelve template sections present.
- Section 1 (Summary) is one paragraph and includes verdict, substrate cell, affected teams, and timeline.
- Section 3 (Motivation) cites concrete evidence — an incident, a drift measurement, an exec ask, or an explicit `unevidenced` acknowledgement.
- Section 4 (Detailed design) names a specific owner team, not a hand-waved "platform team".
- Section 5 (Alternatives) presents each non-chosen verdict in its strongest form.
- Section 6 (Migration plan) reports per-team schedules with error bars and decommissioning dates.
- Section 7 (Political cost) has a paragraph per consumer team plus a mitigation per team and an explicit escalation contact.
- Section 8 (Rollout and rollback) names a fires-when threshold and a named human for the rollback.
- Section 10 (Unresolved questions) has three-to-six items, each with an owner and a target date.
- The exec one-pager fits on one page and reads as a decision request, not a summary.
- Prior-art references cite at least two real, publicly-linkable sources.

## Stretch goals

- **Take the RFC to a peer for the "pre-write" pass** as chapter 5's authoring cadence suggests. Get a written or verbal reaction from one peer staff engineer and one platform lead. Revise the RFC based on their feedback and note the diffs. Attach the diffs to the authoring log.
- **Author the escalation brief** for the specific escalation path named in section 7 — one page, using the chapter-6 template. You will not send it, but drafting it now surfaces whether the political-cost analysis was actually testable.
- **Author two versions of Section 4** — the verdict you chose and the second-best verdict you shadow-drafted in exercise-03's stretch goal. Compare the two side by side; pick the one that would be defensible in review. This is the highest-signal way to check whether your verdict is the right one.
- **Simulate the architecture review meeting** with two peers. One takes the author seat, one takes the reviewer seat, one observes. Twenty minutes of the meeting, using chapter-6's structure. Track the block / nudge / escalate outputs. This is a preview of project-401's capstone deliverable.
- **Track the RFC through to a real decision.** If your portfolio is your current employer and the substrate cell is real, offer the RFC to your team's staff-plus channel or your peer staff engineers as a "practice RFC" for their feedback. Even without a review meeting, the feedback from three peers is the highest-signal input for the next revision.
