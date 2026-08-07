# exercise-05: Executive One-Pager for the Training Program

**Estimated effort:** 3 hours

## Objective

Roll the four previous exercises — the scaling-law bet (exercise-01), the data plan (exercise-02), the ablation and eval plan (exercise-03), and the post-training plan (exercise-04) — into the **executive one-pager** that leadership will actually read and approve. The document produced here is the artifact that sits at the top of the program brief for the CFO, CEO, VP of Product, and board; it is the deliverable the review meeting decides on.

The exercise is deliberately about the *distinct* one-pager artifact, not a summary of the brief. Chapter 8's opening line is the framing: the CFO reads two pages. Everything the CFO needs to decide has to fit on those two pages.

## Prerequisites

- Exercises 01-04 completed against the *same* program brief. If you swapped briefs mid-way, this exercise will not compose.
- Chapter 08 — The Executive One-Pager: Turning the Scaling-Law Bet Into a Business Case (audience, template, section order, anti-patterns, review-meeting shape).
- Chapters 01-07 skim for cross-references — the one-pager cites numbers from all four earlier exercises.

## Steps

1. **Gather the numbers from exercises 01-04.** Before writing, pull into a scratchpad: (N, D, C) from exercise-01, the mixture posture and pipeline-cost estimate from exercise-02, the phase-by-phase compute breakdown and mid-run halt trigger from exercise-03, the post-training posture and launch-gate thresholds from exercise-04. If any of these are missing or vague in the earlier exercises, fix them before proceeding — the one-pager cannot invent numbers the brief does not defend.
2. **Write the risk table first.** Section 6 of the template. Five to eight rows: MFU shortfall, main-run restarts, mixture-shift data regression, post-training safety-gate miss, competitor-lands-first, regulatory shift, capacity contention. For each: likelihood (Low / Medium / High), impact (dollar or calendar or qualitative), mitigation (with pointer to the program-brief section). This is the section that most changes what the framing looks like — write it first so the rest of the one-pager knows what it is defending against.
3. **Write the defense-against-alternates section.** Section 5. Four sub-sections in two-to-three sentences each: smaller model, larger model, buy-instead-of-train, defer-this-quarter. Each with the specific numeric answer from your exercises 01-04 work. Do not hand-wave — the numbers came out of exercise-01's isoflop bowl and exercise-03's compute breakdown; use them.
4. **Write the budget table.** Section 4. Two rows: main run and risk-adjusted total, with columns for accelerator-hours, on-demand dollars, reserved-capacity dollars, wall-clock in calendar time. Cross-check the numbers against exercise-03's phase breakdown — they should match.
5. **Write the scaling-law-bet section.** Section 3. One paragraph plus the embedded isoflop-bowl plot from exercise-01. State the Chinchilla-optimal point, the picked point, and one sentence on the confidence band. A non-technical reader has to be able to read this and understand why the picked (N, D) is not obvious.
6. **Write the "what we get for the money" section.** Section 2. Two bullets: the model (N, D, expected benchmark deltas vs. the closest baseline — specific benchmarks and specific deltas, no "state of the art" hand-waves) and the learnings (isoflop grid for your data distribution, mixture ablation matrix, internal red-team payload library, eval-waterfall infrastructure). The learnings bullet is what turns a one-time bet into a durable capability investment; skip it and the CFO prices the program as a one-shot.
7. **Write the timeline.** Section 7. Horizontal timeline with the four (or five) phases from exercise-03, the compute-fraction breakdown, and the go/no-go gate at each phase boundary. Include the mid-run halt trigger from exercise-03 explicitly.
8. **Write the one-sentence framing.** Section 1. Format: *"We propose training a [N]-parameter, [D]-token base model / large-scale fine-tune of [base] to serve [downstream product] for an expected cost of [$X] over [T weeks], replacing the current alternative of [buy-vs-train answer] which costs / delivers [status quo]."* Numbers in bold. One sentence. This is the sentence forwarded from the review meeting; write it last so all the numbers exist first.
9. **Write the ask.** Section 8. Three items: dollar budget approved, capacity commitment (specific accelerator-count for the specific window), go-decision authority (who signs off on the pre-main-run go/no-go review). Do not skip this — a one-pager without an ask does not close in the review meeting.
10. **Lay it out on two pages.** Real typography, real page breaks. Page 1 is sections 1-4 (framing, what-we-get, scaling-law bet, budget). Page 2 is sections 5-8 (alternates, risks, timeline, ask). If it spills to three pages, cut — the constraint is load-bearing.

## Deliverable

A two-page document (PDF, Google Doc, or Markdown that would render to two pages), containing:

- **Page 1**
  - Section 1 — One-sentence framing, numbers in bold.
  - Section 2 — What we get: model bullet + learnings bullet.
  - Section 3 — Scaling-law bet, one paragraph, isoflop bowl plot embedded.
  - Section 4 — Budget table: main run + risk-adjusted total, four columns.
- **Page 2**
  - Section 5 — Defense against alternates: smaller / larger / buy / defer, two-to-three sentences each.
  - Section 6 — Risk table: 5-8 rows, columns likelihood / impact / mitigation.
  - Section 7 — Timeline with go/no-go gates and mid-run halt trigger.
  - Section 8 — Ask: dollar budget, capacity commitment, go-decision authority.

Plus a **short covering note** (not on the two pages, submitted alongside):

- Three-sentence reflection on which of the four earlier exercises' numbers moved the most when you had to defend them on one page in a non-technical audience's language.
- One-sentence answer to *"if leadership pushes back on the ask, which of the three ask items would you concede first, and why?"*

## Starter guidance

- **Write the risk table before you write the framing.** Chapter 8 is explicit: the bottom-to-top order is what produces a one-pager that survives the review. Writing top-down produces a one-pager that reads well until the CFO opens the risk table and finds it thin.
- **The one-sentence framing is the hardest single sentence in the module.** Draft it three times. The first two drafts will be too technical or too vague. The third draft should read like an investment memo — a specific ask against a specific status quo with specific numbers.
- **Do not embed the program-brief content on the one-pager.** No architecture details, no tokenizer choice, no MFU explanation. Everything technical is on the linked brief. The one-pager surfaces the *business case*.
- **Numbers in the budget table have to match exercise-03.** If the risk-adjusted total on the one-pager is different from the total in exercise-03, one of the two is wrong. Reconcile before submitting.
- **The learnings bullet is often skipped.** Do not skip it. The isoflop grid for your data distribution, the mixture ablation matrix, and the eval-waterfall infrastructure are reusable assets that reduce the *next* program's cost. Naming them turns the one-shot bet into a durable investment story.
- **The mid-run halt trigger goes on page 2, not in a footnote.** Leadership needs to see it. Sunk-cost programs are what happen when the halt trigger is hidden.
- **Read the whole thing aloud before submitting.** If it takes more than four minutes to read aloud, it is too long for two pages of tight typography.

## Acceptance criteria

- The deliverable fits on two pages.
- The one-sentence framing has all six variables in the template (N, D, base or from-scratch, downstream product, dollar cost, timeline, buy-vs-train alternate, status quo comparison), with the dollar and timeline numbers in bold.
- The "what we get for the money" section has both bullets — the model and the learnings — and the learnings bullet names at least three specific reusable artifacts.
- The scaling-law-bet section has the embedded isoflop-bowl plot from exercise-01 and states the confidence band in one sentence.
- The budget table has both main-run and risk-adjusted-total rows and reconciles numerically with exercise-03's phase breakdown.
- The defense-against-alternates section has all four sub-sections (smaller, larger, buy, defer) with specific numbers.
- The risk table has 5-8 rows, each with likelihood / impact / mitigation columns, and no `TBD` cells.
- The timeline has the phase boundaries from exercise-03, the compute-fraction per phase, the go/no-go gate at each boundary, and the mid-run halt trigger.
- The ask names a specific dollar number, a specific accelerator-count-and-window, and a specific decision-authority role.
- The three-sentence reflection identifies one specific number that changed when translated to a one-pager audience.

## Stretch goals

- **Present the one-pager to a non-technical reader.** Recruit a colleague from finance, product, or legal. Give them the two pages, no context, and ask them to summarise the program in three sentences and state one question they would ask leadership about it. Where their summary diverges from your framing is where the one-pager needs another revision.
- **Draft the cover section for the mod-402 pairing.** Chapter 8 covers the case where the program lives inside a mod-402 multi-team RFC (e.g., a shared training substrate consolidation, a shared inference-serving substrate that the trained model will use). Write the one-paragraph cover section that would sit atop *both* artifacts and name the joint decision.
- **Anticipate the three hardest questions.** Chapter 8's review-meeting section: the three-to-five hardest questions leadership can produce. Write out the questions you expect and draft two-sentence answers in the language of the one-pager, not the language of the brief. Attach the Q&A as a page-3 appendix that stays *off* the delivered one-pager but is in your pocket at the review.
- **Compare with a published program's approval framing.** Read the LLaMA-3 paper, DeepSeek-V2 report, or Falcon technical report and reverse-engineer what the internal one-pager for that program might have said — the Chinchilla vs. over-train call, the alternate-defense, the mixture and post-training posture — from the paper's justifications. Where your one-pager for your program lands *differently* on the same trade-offs is the interesting content.
- **The reject-and-redo drill.** Assume the review meeting rejected the one-pager and asked for a redo in a week. From chapter 8's reject vs. defer vs. approve-with-adjustments taxonomy, guess which category the rejection would land in for your program, and draft the two changes to the one-pager that would flip a reject to an approve-with-adjustments on the next round.
