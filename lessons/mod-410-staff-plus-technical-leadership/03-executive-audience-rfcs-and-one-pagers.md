# Executive-Audience RFCs, One-Pagers, and Working-Backwards

## Motivation

The technical RFCs the staff engineer learned to author in mod-402 (multi-team RFC) and mod-405 (platform-gap RFC and contribute-back RFC) are written for a *peer technical audience* — other staff engineers, platform-team leads, senior tech leads inside the ML org. They can assume shared vocabulary about registries, feature stores, and eval contracts, and they can run ten-plus pages long because the reader is paid to read them slowly.

An **executive-audience** artifact is a different genre. The reader is a Director, VP Engineering, CTO, CFO, occasionally CEO, sometimes a board member. They are technically literate but not paid to read ML architecture. They have between five and thirty minutes with the document, and their attention is competing with every other capital allocation decision the exec team makes that week. If the artifact does not land in the first paragraph, it does not land.

This chapter names the executive-audience genres — the one-pager, the exec-audience RFC, the working-backwards PR/FAQ — and the discipline that separates a decision-quality exec doc from a status report. Pairs with exercise-02. Grounds heavily on Colin Bryar and Bill Carr *Working Backwards* (St. Martin's Press, 2021), Jeff Bezos's shareholder letters (canonical examples of the compressed exec-audience writing style), and Barbara Minto's *The Pyramid Principle* on top-down structure.

## First separate "needs a decision" from "needs an FYI"

The single most valuable move on any executive-audience artifact is deciding *what shape it is* before writing a word. Three shapes cover almost all of them:

1. **Decision doc.** The executive has to make a specific call — approve a $10M capacity ask, pick vendor A or vendor B, approve or block a merge/split RFC's business impact. There is exactly one decision on the page and the doc's structure is built to make that decision easy.
2. **FYI / heads-up.** The executive should know something is happening — a shift in the pretraining program's schedule, a compliance escalation on a specific model, a peer team's regression that will surface in the next board deck. No decision needed today; the doc exists so the exec is not surprised later.
3. **Options paper.** The executive will make a decision but not today. The doc lays out two or three real options, defends each, and names the *forced move* — the thing the exec team will have to choose between at the next review.

Mixing these three is the single most common way exec docs land badly. A decision doc dressed as an FYI does not get decided. An FYI dressed as a decision doc gets pushed back to "come back with more analysis". An options paper written as a decision doc reads as a staff engineer trying to get the exec to make the call the staff engineer should have made themselves.

Reilly makes a related point in *The Staff Engineer's Path* on communication: the writer's first job is to signal what the reader is being asked to do. The genre labelling in the first sentence of the summary — "This is a decision doc requesting approval to..." vs. "This is an FYI on..." — does most of the work.

## The one-page structure

The exec one-pager is the workhorse artifact of staff-plus technical leadership. Every roadmap exec review, every capacity ask, every vendor choice, every escalation eventually needs one. The structure that reliably works:

```
Title: [Decision or FYI], one line, no jargon
Requested action: [what the exec is being asked to do, or explicitly "FYI, no action requested"]
Deadline: [when the decision is needed by, or "no deadline"]

Background (3-5 sentences): the situation and why it is on the exec's desk now.

Options considered (bullet list, one line each): what alternatives are on the table
  - Option A: [one-line description] — recommended
  - Option B: [one-line description]
  - Option C: [one-line description]

Recommendation (2-3 sentences): what the staff engineer thinks and why.

Trade-offs (bullet list): what we give up under the recommendation.
  - Cost:
  - Risk:
  - What we defer:

Ask (bullet list): what the exec is being asked to do concretely.
  - [Approve X]
  - [Unblock Y with peer team]
  - [Communicate Z to the board]

Contacts: [staff engineer, peer EM, sponsor director]
Related docs: [links to the full RFC, roadmap, or prior briefing]
```

Two structural rules make the one-pager work:

**The recommendation lives above the fold.** Barbara Minto's *pyramid principle* — start with the answer, then support it — is the shape the exec-audience one-pager takes. If the recommendation is in the last paragraph, the reader who stops halfway through has not gotten the point of the doc. The Amazon operating tradition frames this as *"put the conclusion first"*; Bezos's shareholder letters consistently open with the point they intend to make.

**The trade-offs are honest.** An exec who reads a one-pager where the trade-offs section is empty or hedged (*"minimal risk with careful implementation"*) discounts the whole doc. The trade-offs section is where the staff engineer's judgement shows. Naming a real cost or a real deferred program in one sentence buys more exec trust than three pages of optimistic framing.

## Working backwards — Amazon PR/FAQ, briefly

Bryar and Carr's *Working Backwards* documents the Amazon PR/FAQ style: for a proposed program, write the *press release* the org would issue when it ships, and the *frequently asked questions* the reader would have. It forces the author to state what the finished thing looks like, in customer- or business-facing terms, before authorising the engineering that gets there.

The PR/FAQ genre is *not* the default staff-plus exec-audience doc, but it is the right genre for a specific subset of exec artifacts:

- The exec framing of a *new program* — a pretraining program, a new customer-facing ML capability, a portfolio-wide platform pivot. Programs where the outcome is what the exec team needs to visualise before authorising the roadmap slot.
- The exec framing of a *build-vs-adopt* call where the "adopt" option means committing to a vendor and the exec needs to see what the org looks like after adoption.
- The exec framing of a *portfolio-wide change* — a new eval standard, a new risk register discipline — where the shape of the future is more important than the technical path.

The PR/FAQ is *not* the right genre for capacity asks, staffing asks, or infrastructure trade-offs where the outcome is not customer-facing. For those, the one-pager is stronger.

A minimal PR/FAQ structure that adapts well to ML staff-plus scope:

- **Press release (1 page).** Headline, subhead, one-paragraph summary, a customer or business quote, two-to-three key capabilities, one line on availability.
- **Internal FAQ (2-4 pages).** The eight-to-fifteen questions the internal exec team would ask: *what does it cost, what does it break, what happens if we do not do it, which teams own which pieces, what does the roadmap look like, what are the risks, what is the fallback.*
- **External FAQ (1-2 pages), if relevant.** What customers or regulators would ask.

The PR/FAQ pairs with the roadmap from chapter 2, not against it. The roadmap answers *what the ML org is doing over four quarters*; the PR/FAQ answers *what one specific program looks like when it ships*.

## Trade-off framing that lands

Executives read trade-offs first because trade-offs are where the recommendation is testable. Three patterns for trade-off framing that lands:

**Frame in the exec's units.** Trade-offs stated in ML-engineering vocabulary — *"we accept 2% MFU regression to unlock FSDP"* — do not land. The same trade-off in exec vocabulary — *"we spend an extra $180k in Q3 training cost to keep the training program on the 400B-parameter schedule the board approved"* — does. mod-407 (TCO) is where the exec vocabulary comes from.

**Name the reversibility class.** Bezos's 1997 shareholder letter frames this as *Type 1 vs. Type 2 decisions* — one-way doors versus two-way doors. A staff-plus exec doc names the class of every recommended action: *"Type 2 — we can renegotiate the vendor contract at the annual renewal"* vs. *"Type 1 — once we migrate the model registry the previous registry is decommissioned within two quarters"*. Type 1 decisions warrant more debate and a written exit condition; Type 2 decisions warrant a faster call.

**State the counterfactual cost.** *"If we do not do X, we spend Y on Z instead."* The exec is not choosing X vs. nothing; they are choosing X vs. the next-best allocation of the same dollars, engineers, or quarters. Naming that alternative explicitly is what separates a decision doc from a request.

## The exec-audience RFC — when to author one, and when not

The full exec-audience RFC — a five-to-fifteen-page document intended for the Director or VP — is the right artifact when *the technical detail materially affects the decision* and *the exec is willing to spend thirty minutes with the doc*. If either is false, the one-pager is stronger and the technical detail lives in the linked peer RFC.

An exec-audience RFC keeps the peer-RFC bones from mod-402 and mod-405 but re-shapes three sections:

- **Summary (1 page).** The one-pager collapsed into the front of the RFC. If the exec reads only page one, they get the decision, the trade-offs, and the ask.
- **Background (1-2 pages).** Business framing, not architectural framing. Why is this problem on the exec's desk now?
- **Recommendation and defence (2-3 pages).** The core of the doc. Peer RFC-style: options considered, recommendation, trade-offs, migration cost, political cost (using mod-402 chapter 5's political-cost model where relevant).
- **Technical appendix (open-ended).** Everything a peer staff engineer would need to review — assumptions, benchmarks, diagrams. Explicitly labelled *appendix* so the exec knows they can skip.

The exec-audience RFC is the artifact that gets sent to the peer EMs, the peer platform-team leads, and the director *at the same time* — the wider audience is part of the point. The narrower peer-RFC is for the multi-team review body from mod-402 chapter 6.

## When an RFC is the wrong artifact for the audience

Three specific situations where a staff engineer writing an RFC has misread the audience:

1. **The exec asked a question, not for a decision.** The Director asks *"why do we have three model registries"* in a staff meeting. The right response is a one-pager, not a fifteen-page RFC. The RFC is the follow-up if the exec agrees the answer requires org action.
2. **The decision is inside the ML org's control.** No exec approval is needed; the merge/split verdict is a mod-402 peer RFC and the exec sees it only as an FYI. Pushing it up to exec attention wastes the exec's time and dilutes the escalation budget for decisions that actually need it.
3. **The audience is customers, regulators, or partners.** Then the artifact is a PR/FAQ, a compliance filing, or a partner-facing brief — not an internal RFC. mod-409 covers the compliance side; the vendor side comes back in chapter 6.

The mod-402 chapter 6 review body handles the peer technical audience. The one-pager and PR/FAQ handle the exec audience. The exec-audience RFC is the bridge when both audiences need the same document.

## What Staff owns vs. what the peer L40 EM owns

The mod-401 chapter 5 boundary-6 split (exec comms) applies to every artifact in this chapter:

- **Staff owns.** The technical framing — the recommendation, the technical trade-offs, the reversibility class, the technical defence of the option, the RFC or PR/FAQ body.
- **EM owns.** The delivery framing — team capacity to absorb the decision, hiring impact, on-call impact, quarterly-commit implication, org-health signal.
- **Both.** Sign the exec-facing doc together. Stand side-by-side in the exec review. A staff-authored exec doc that has not been reviewed by the peer EM is a staff engineer speaking for the team without the team's delivery buy-in.

## Common failure modes on the exec-audience artifact

Four failure modes worth naming:

- **Bloated summary.** The summary tries to include everything and reads as a wall of text. Executives skim summaries in under a minute; if the recommendation is buried, it does not land. Cut ruthlessly.
- **Hedged recommendation.** *"We could do A or B; there are trade-offs either way."* The staff engineer's job is to *pick*. Presenting an options paper as a decision doc invites the exec to make the call the staff engineer should have made.
- **Missing forcing function.** Executives defer decisions when no deadline is stated. The **deadline** line at the top of the one-pager exists for this reason. If no deadline is real, say so — otherwise pick one.
- **Technical detail leaking into the summary.** *"We recommend adopting FSDP with ZeRO-3 partitioning and CPU-offload for optimiser state"* is not an exec-facing sentence. *"We recommend switching the pretraining training recipe to reduce Q3 GPU cost by $2M"* is. The engineering detail belongs in the RFC body.

## Summary

- Exec-audience artifacts are a distinct genre from peer RFCs — the reader has 5-30 minutes, is technically literate but not paid to read ML architecture, and needs the recommendation in the first paragraph.
- Separate **decision doc**, **FYI**, and **options paper** genres before authoring. Label the genre in the first sentence.
- The **one-pager** is the workhorse artifact: title / requested action / deadline at the top; background, options, recommendation, trade-offs, ask, contacts, related docs below. Recommendation above the fold; trade-offs honest.
- **Amazon-style PR/FAQ** working-backwards is the right genre for new-program framing and portfolio-wide change; not for capacity asks or infrastructure trade-offs.
- Frame trade-offs in the **exec's units** (dollars, quarters, business metrics — not MFU); name the **reversibility class** (Type 1 / Type 2); state the **counterfactual cost**.
- The **exec-audience RFC** is the right artifact only when the technical detail materially affects the decision and the exec will spend 30 minutes with it. Otherwise the one-pager is stronger.
- The **Staff/EM boundary** on exec comms is *technical framing vs. delivery framing*; both sign, both stand in the room.
- Common failure modes: **bloated summary, hedged recommendation, missing deadline, technical detail leaking into the summary**.

The next chapter walks the staff-plus hiring loop — running rubric-based ML interviews and calibrating senior-vs-staff at bar.
