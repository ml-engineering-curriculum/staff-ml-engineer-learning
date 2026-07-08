# The Executive One-Pager: Turning the Scaling-Law Bet Into a Business Case

## Motivation

Six documents into the program brief and the Staff engineer is proud of the artifact. The isoflop bowl is drawn, the risk-adjusted cluster-hour budget is defended, the mixture table is signed off by legal, the ablation cadence is committed to, the post-training plan is in the hands of the fine-tuning-engineer specialist. Twenty-something pages, all correct.

The CFO reads two.

Chapter 8 is the two-page executive one-pager that translates the program into a **business decision leadership can approve**. The one-pager is not a summary of the program brief; it is a *distinct* artifact whose audience is not the ML engineer or even the VP of Engineering but the CFO, the CEO, the head of product, and the board members who will sign off on the compute spend. The one-pager reads left-to-right, top-to-bottom, in the same shape as an investment memo — because functionally that is what it is.

This chapter walks the one-pager template, calls out the sections most often skipped, and shows how the one-pager pairs with the mod-402 multi-team RFC template when the program is being reviewed alongside portfolio-scope decisions.

## Who reads the one-pager

Three primary audiences, in decreasing order of technical depth:

**1. The VP of Engineering / CTO.** Cares about the technical bet — is Chinchilla the right shape, is 15B / 1.1T the right point, is the MFU assumption defensible. This audience reads the linked program brief before the review.

**2. The CFO / VP of Finance.** Cares about the dollars, the alternates, and the risk. Wants to see the cluster-hour-to-dollar conversion, the buy-vs-build defense, and the answer to *"what if it costs 30% more."*

**3. The CEO / board.** Cares about the strategic framing — what does this program let the company do that we cannot do today, and what is the competitive posture if we do not run it.

The one-pager has to serve all three, on two pages. That means every section has to be intelligible to the CFO while remaining defensible to the VP of Engineering. There is no "technical appendix" a one-pager can push details into — the details either fit in the two pages or they live in the linked program brief.

## The one-pager template

Two pages, tight typography, four sections per page. The template:

### Page 1 — The bet and the ask

**Section 1 — The one-sentence framing (top of page).**

Format: *"We propose training a **[N]-parameter, [D]-token base model / large-scale fine-tune of [base]** to serve **[intended downstream product / capability]** for an expected cost of **[$X]** over **[T weeks]**, replacing the current alternative of **[buy-instead-of-train / use base as-is / no capability]** which costs / delivers **[status quo comparison]**."*

Example (deployment-cost-dominated program):

> *"We propose training a **15B-parameter, 1.1T-token** base model to serve our **customer-support automation product** for an expected cost of **$420k in on-demand cloud terms / $210k in reserved-capacity terms** over **10 weeks**, replacing the current alternative of **API-calling a frontier vendor** which costs **$1.4M/year at expected volume** and creates a **critical-path third-party dependency**."*

**One sentence. Numbers in bold.** The framing has to hold together on its own — this is often the sentence forwarded from the review meeting.

**Section 2 — What we get for the money.**

Two bullets: the model, and the learnings.

- **Model**: N parameters, D tokens, expected benchmark deltas vs. the closest baseline (state the specific benchmarks and the specific expected deltas — no hand-waves like "state of the art").
- **Learnings** (chapter 1's point about the program's second output): the isoflop grid for our data distribution, the mixture ablation matrix, the internal red-team payload library, the eval waterfall infrastructure. Each is reusable for the *next* program and reduces its cost.

Naming the learnings explicitly is what turns a one-time bet into a durable capability investment. Leadership prices this.

**Section 3 — The scaling-law bet.**

The chapter-2 bet, restated for a non-technical reader.

- The Chinchilla-optimal point at our compute budget.
- Where we chose relative to it and why (over-trained for inference cost, or Chinchilla-aligned, or under-trained deliberately).
- The isoflop bowl plot (small, embedded).
- One sentence on confidence: *"the extrapolation from small-scale ablations to the main-run scale carries a ±30% band on the target loss; the ablation phase tightens this before the main-run commit."*

The plot is the one visual on page 1. It replaces two paragraphs of prose.

**Section 4 — The cluster-hour budget and dollar cost.**

Two rows: main run and risk-adjusted total.

```
                     | Accelerator-hours | On-demand $ | Reserved $ | Wall-clock (calendar)
---------------------|-------------------|-------------|------------|-----------------------
Main run             |          70,400   |   $210k     |   $105k    |   5.7 days on 512xH100
Risk-adjusted total  |         140,000   |   $420k     |   $210k    |   ~10 weeks including ablations
```

One sentence on the split: *"the risk-adjusted total includes ablations, restarts, post-training, and 15% headroom, per the chapter-3 breakdown in the linked brief."*

### Page 2 — The alternates, the risks, the timeline

**Section 5 — Defense against alternates.**

The chapter-3 alternate calls, each in two-to-three sentences.

- **Smaller model.** *"7B at same compute lands N% below our target on [benchmark]; specifically that misses the launch gate for [downstream product feature]. If the launch gate weakens, 7B becomes the right call."*
- **Larger model.** *"30B at same compute lands X% above on [benchmark] but at 2× the inference cost, adding $[Y]/year to the deployment budget. The isoflop bowl and the deployment-cost curve both prefer 15B for our expected volume."*
- **Buy-instead-of-train.** *"The frontier vendor's API at expected volume costs $[Z]/year, plus [licensing / dependency / latency / sovereignty] constraints not met by external API. The break-even against training is [T months]. Above [V volume threshold], training wins."*
- **Defer this quarter.** *"Deferring six months costs [$W in delayed value from the downstream product]. It saves [$V in program cost due to hardware progression]. Net cost of deferral: $[W - V] to the P&L, plus a competitive-posture cost that is qualitative."*

**Section 6 — Risk table.**

The five-to-eight risks with likelihood, impact, and mitigation. Table format:

```
Risk                              | Likelihood | Impact          | Mitigation
----------------------------------|------------|-----------------|--------------------------------
MFU below 35% plan                | Medium     | +20% program $  | Reserved capacity contract with credits
Main run instability / restart    | High       | +5-15% program $ | 15% headroom in budget; warm standby
Data-quality regression from      | Low        | +2 weeks cal.   | Corpus versioning and rollback plan
   mixture-shift ablation loss    |            |                 |
Post-training safety gate miss    | Medium     | +2-4 weeks cal. | Multi-round red-team budget in ch-7 plan
Base-model competitor ships       | Medium     | Qualitative     | Program second-output learnings retain value
   at scale before we do          |            |                 |
Regulatory changes to training-   | Low        | Program halt    | License and PII pipeline already compliant
   data licensing                 |            |                 |
```

The table is scanning-optimised. Leadership will read the risks and mentally rate them; the mitigations are what tell leadership *the program owner has thought about this*.

**Section 7 — Timeline and go/no-go gates.**

A horizontal timeline with the phase boundaries and the go/no-go gates from chapter 6:

```
Wk 0-2  Proxy runs & isoflop grid          → Gate: scaling-law fit within band
Wk 2-4  Small-scale ablations              → Gate: mixture / dedup / architecture decisions
Wk 4-6  Full-scale ablations               → Gate: scale transfer confirmed
Wk 6-8  Main run (with mid-run halt gate)  → Gate: MMLU@40% > previous-gen baseline
Wk 8-10 Post-training & final eval          → Gate: safety + helpfulness launch thresholds
```

The gates are the mechanism leadership has to steer the program mid-flight — not by second-guessing the ML decisions but by triggering the pre-agreed review at the pre-agreed point.

**Section 8 — Ask.**

Three items:

1. **The compute budget approved.** The dollar number.
2. **The capacity commitment.** *"512 H100-equivalent for the wall-clock window in Weeks 6-8, with reserved-capacity guarantee."*
3. **The go-decision authority.** Who signs off on the pre-main-run go/no-go review at end of week 6.

## Anti-patterns

Three ways one-pagers reliably fail in review, and the fix.

### Anti-pattern 1 — the technical dive that never surfaces

The one-pager starts *"We use a decoder-only transformer with rotary embeddings, SwiGLU activations, and grouped-query attention across 32 layers with hidden dimension 4096..."* and never gets to the business framing.

The CFO does not care about SwiGLU. This is a program brief section, not a one-pager section. **The one-pager surfaces the business case; the linked brief carries the technical depth.**

### Anti-pattern 2 — the aspirational timeline

The one-pager shows a ten-week calendar with all gates green in the base case and no discussion of the failure modes' calendar impact.

Leadership does not believe aspirational timelines and knows to add 30-50%. Beat them to it. **The base case timeline is honest; the risk table quantifies the calendar variance.**

### Anti-pattern 3 — the "we just need to trust the process" ending

The one-pager ends without a specific ask — no dollar number, no capacity commitment, no go-decision authority.

Leadership will not approve what has not been asked for. **Section 8 is required, not optional.**

## When the one-pager pairs with the mod-402 multi-team RFC

The mod-403 program often lands *inside* a mod-402 multi-team RFC that argues for consolidating the org's training capacity, or for a shared inference-serving substrate that the trained model will use, or for a data-pipeline substrate the program depends on. In those cases the one-pager sits alongside the RFC as the program-scope companion.

The relationship:

- **The mod-402 RFC** argues the multi-team architectural change and its migration and political cost. It is the artifact for the architecture review body.
- **The mod-403 one-pager** argues this specific program's business case. It is the artifact for the exec approval body.

Both go to the same review meeting when leadership needs to bless both. The two artifacts share a **cover section** — one paragraph that names the joint decision — and then split into their respective bodies. The cover paragraph looks like: *"We propose training a domain-adapted 15B model on our new shared training substrate. The program brief is in this one-pager; the substrate consolidation RFC is in this document."*

## How the one-pager gets written

The order matters. Do not write page 1 first.

1. **Write the risk table first.** It forces the honest inventory of what can go wrong.
2. **Write the alternates second.** It forces the honest inventory of what else the org could do.
3. **Write the budget table third.** By this point the framing is clear.
4. **Write the scaling-law-bet section fourth.** It reads as a design choice against the alternates rather than an isolated technical decision.
5. **Write the "what we get" section fifth.** After all the trade-offs are stated, this section is what leadership needs to see the value in.
6. **Write the one-sentence framing last.** After all the numbers exist, the framing snaps into shape.
7. **Write the ask.** The specific dollar / capacity / decision-authority requests.

This order is deliberately opposite to the reading order. Writing top-to-bottom produces a one-pager that reads well until the bottom half falls apart under scrutiny; writing bottom-to-top produces a one-pager that survives the review.

## The review meeting

The review that approves the one-pager is typically a 30-60 minute meeting with the CFO, VP of Engineering, VP of Product for the downstream product, and the ML org head. The program owner presents.

**What the meeting is not.** A technical deep-dive. The technical decisions were made in the ML org's review and are documented in the linked program brief. The exec meeting is a *decision-forcing* meeting on approve / defer / reject.

**What the meeting is.** A stress test on the business case. The three-to-five hardest questions leadership can produce, answered by the program owner in the language of the one-pager, not the language of the program brief.

**What a decision at the meeting looks like.**

- **Approve.** Program funded as stated; go/no-go gates committed to; capacity commitment signed.
- **Approve with adjustments.** Approved conditional on specific changes — usually a specific alternate gets folded in ("approved but re-explore whether 20B is the right N once phase-2 ablations are in").
- **Defer.** Not rejected, but re-review after a specific missing piece is addressed. Common causes: MFU assumption not defensible, legal sign-off on pipeline not complete, downstream product commitment not firm.
- **Reject.** Program does not run. Rare on a well-prepared one-pager, common on a rushed one.

Mod-410 covers the review-culture side of these meetings; this chapter's contribution is the artifact.

## Summary

- The executive one-pager is a **distinct artifact**, not a summary of the program brief. It is written for the CFO, CEO, and board; the technical audience reads the linked brief.
- **Two pages, eight sections**: one-sentence framing, what-we-get, scaling-law bet, budget table, defense against alternates, risk table, timeline with gates, and the specific ask.
- The one-pager is written **bottom-to-top** — risk table first, ask last drafted, one-sentence framing snapped into shape after everything else exists.
- The one-pager pairs with the mod-402 multi-team RFC when a program lands inside a portfolio-scope architectural change; the two share a cover section and split into their respective review bodies.
- The review meeting is a **decision-forcing** meeting: approve / approve-with-adjustments / defer / reject. A rejected one-pager teaches the same lesson as a rejected RFC — the artifact was not ready, not that the program is wrong.

The program is scoped, the compute is defended, the data is sourced, the ablations are cadenced, the post-training is planned, and the one-pager is written. The exercises take you through each of the five artifacts on a concrete program brief you choose; the capstone project-402 rolls them into an end-to-end deliverable.
