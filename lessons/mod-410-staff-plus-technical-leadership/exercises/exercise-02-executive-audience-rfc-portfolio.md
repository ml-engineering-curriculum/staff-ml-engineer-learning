# exercise-02: Executive-Audience RFC Portfolio

**Estimated effort:** 3 hours

## Objective

Author **two paired executive-audience artifacts** on a real portfolio decision from exercise-01's roadmap:

1. A **decision one-pager** rewriting a portfolio decision (a program pivot, a resource ask, a vendor call, an escalated non-goal, or a risk-register escalation from mod-409) for a CTO / CFO / VP-Engineering audience, in chapter 3's one-page structure.
2. An **Amazon-style PR/FAQ working-backwards** doc for **one new program** on the roadmap — a pretraining program, a new customer-facing ML capability, a portfolio-wide platform pivot, or a portfolio-wide eval-standard rollout.

After this exercise the learner should hold two artifacts an exec could read: a one-pager with the recommendation above the fold and honest trade-offs in the exec's units, and a PR/FAQ that makes the shape of a not-yet-shipped program *visible* to a leader who has to authorise it.

Pairs with chapter 3. Reads against exercise-01's roadmap. Feeder for [`project-403-staff-plus-tech-leadership-simulation`](../../../projects/project-403-staff-plus-tech-leadership-simulation/).

## Prerequisites

- Chapter 03 — the decision-doc / FYI / options-paper distinction, the one-page structure (title / requested action / deadline / background / options / recommendation / trade-offs / ask / contacts / related docs), the recommendation-above-the-fold rule (Minto), Amazon PR/FAQ working-backwards (Bryar & Carr), the reversibility-class framing (Bezos Type 1 / Type 2), counterfactual cost, and the four common failure modes (bloated summary, hedged recommendation, missing deadline, technical detail leaking into the summary). Read in full.
- Exercise-01 — the roadmap this exercise reads against. If exercise-01 is not complete, do it first; the portfolio decision this exercise reframes is one of the decisions the roadmap embeds.
- Recommended: skim Colin Bryar and Bill Carr, *Working Backwards* (St. Martin's Press, 2021), chapter 4 ("Working Backwards"), specifically the PR/FAQ structure and the internal-FAQ pattern. Optional but grounding: read the [1997 Amazon shareholder letter](https://s2.q4cdn.com/299287126/files/doc_financials/annual/Shareholderletter97.pdf) for the reversibility-class framing (Type 1 / Type 2) and Barbara Minto, *The Pyramid Principle*, on top-down structure.
- If the learner has one prior exec one-pager they authored — even at Senior scope — read it once before drafting. The most common finding is that the recommendation was buried on page two. Chapter 3's diagnosis is easy to see in your own back-catalog.

## Choose the two decisions

**For the one-pager:** pick one portfolio decision that is *live* on the roadmap and *requires* exec sign-off. Priority order:

1. A **resource ask** from exercise-01's Section 5 — a hire, a capacity increment, a budget commitment — that leadership must approve.
2. A **non-goal** from Section 4 that the ML org expects leadership to push back on (the "no to a program leadership asked for" case from chapter 2).
3. A **program pivot** — a scope change on one of the Section 2 programs the exec should know about.
4. A **vendor call** — forward-pointer to exercise-05, if the vendor decision requires exec sign-off on the commit level.
5. A **risk register escalation** — an unresolved AI-risk item from mod-409 the ML org needs the exec to acknowledge.

Do not pick a decision the ML org can resolve internally without exec involvement. Chapter 3's failure mode of *pushing an internal decision up to the exec* is caught here — if the ML org has authority, the artifact is an FYI, not a decision doc.

**For the PR/FAQ:** pick one **new program** from exercise-01's Section 2 — one that has *not shipped yet* and whose outcome the exec team needs to visualise. Chapter 3 named the shape: pretraining programs, new customer-facing ML capabilities, portfolio-wide platform pivots, portfolio-wide eval or risk-register discipline rollouts. Do not pick a program that is halfway shipped — the PR/FAQ is a *working-backwards* artifact for a not-yet-authorised program. Do not pick a capacity ask or infrastructure trade-off for the PR/FAQ — for those, the one-pager is the right genre.

## Steps

### Part A — Author the decision one-pager (1.5 hours)

1. **Declare the genre in one sentence.** Before drafting a word: is this a **decision doc**, an **FYI**, or an **options paper**? Chapter 3 was direct — mixing the three is the single most common way exec docs land badly. Write the genre label at the top of your working draft. If the decision could be resolved without the exec, downgrade the artifact to an FYI and reconsider whether it belongs on the exec's desk at all.
2. **Draft the top-of-page metadata.** Title (one line, no jargon, in the exec's units); Requested action (what the exec is being asked to do, or explicitly *"FYI, no action requested"*); Deadline (when the decision is needed by, or *"no deadline"* if honestly none). Chapter 3's *missing forcing function* failure mode is caught by the deadline line — if no real deadline exists, either name a self-imposed one or explicitly note none.
3. **Draft Background (3-5 sentences).** The situation and why it is on the exec's desk *now*. Not the history of the ML org; not the theory of the technical space; the situation, the constraint, and the forcing function. Cite exercise-01's roadmap Section 1 (Diagnosis) where the exec has already been primed.
4. **Draft Options considered (bullet list, one line each).** Two-to-four options. For each: one line naming the option and its shape. Mark the recommended option with *"— recommended"*. Options that were not seriously considered do not belong on this list; chapter 3's *hedged recommendation* failure mode is caught by keeping the list to the real alternatives.
5. **Draft Recommendation (2-3 sentences).** What the staff engineer thinks and why. The two-to-three sentences after the metadata block are what the exec reads first; if the recommendation is in the last paragraph, the reader who stops halfway has not gotten the point. Minto's pyramid principle: start with the answer, then support it.
6. **Draft Trade-offs (bullet list).** Chapter 3's rule: the trade-offs section is where the staff engineer's judgement shows. Three-to-five bullets covering:
    - **Cost.** In dollars, quarter-slips, or headcount — the exec's units. Grounded on mod-407's TCO model where applicable.
    - **Risk.** In the exec's risk vocabulary — deliverability risk, reputational risk, regulatory risk, capacity risk. Grounded on mod-409's risk register where applicable.
    - **What we defer.** Explicit named other-work the recommendation displaces. Counterfactual cost — chapter 3's *"if we do not do X, we spend Y on Z instead"* framing.
    - **Reversibility class.** Type 1 (one-way door) or Type 2 (two-way door), from Bezos's 1997 letter. Type 1 decisions warrant a written exit condition in the same bullet.
7. **Draft Ask (bullet list).** What the exec is being asked to do concretely — approve X, unblock Y with peer team, communicate Z to the board. Each ask is one line; each is actionable in a single meeting. Vague asks (*"support us"*, *"provide guidance"*) are caught here — either name what the exec should do or remove the ask.
8. **Draft Contacts and Related docs.** Contacts: staff engineer, peer EM, sponsor director. Related docs: linked pointer to exercise-01's roadmap (for context), any relevant peer-audience RFC (from mod-402, mod-405, or mod-406), and — if the recommendation invokes a vendor or budget commitment — a forward pointer to exercise-05.
9. **Sanity-check against chapter 3's four failure modes.** In one final pass: is the summary bloated? Is the recommendation hedged? Is the deadline missing or unreal? Is technical jargon leaking into the summary? If any of the four apply, revise and re-run this pass.

### Part B — Author the working-backwards PR/FAQ (1.5 hours)

10. **Confirm the PR/FAQ is the right genre for this program.** Chapter 3 was explicit: PR/FAQ is for new-program framing, build-vs-adopt calls where adoption reshapes the org, and portfolio-wide change where the shape of the future matters more than the technical path. If the program is a capacity ask or an infrastructure trade-off, stop — the one-pager is the right genre for it and you have two one-pagers, not a paired portfolio.
11. **Draft the Press Release (one page).** Structure:
    - **Headline.** One line naming the program's outcome in customer- or business-facing terms. Not *"we shipped the 200B pretraining run"*; rather *"our recommendation quality now beats the incumbent frontier vendor on N tasks the product depends on"*.
    - **Subhead.** One line on who this affects and why.
    - **Summary paragraph.** Three-to-five sentences describing the shipped program in the language a customer or a board member would use.
    - **Quote (from a stakeholder).** One-to-two sentences, attributed to a plausible internal or external stakeholder — the head of product, a lead customer, a peer engineering leader. The quote makes the outcome *specific* rather than abstract.
    - **Two-to-three key capabilities.** What the program *enables* the org to do that it could not do before.
    - **One line on availability.** When the program is live and who can use it.
12. **Draft the Internal FAQ (2-4 pages).** Eight to fifteen questions in the shape chapter 3 named: *what does it cost, what does it break, what happens if we do not do it, which teams own which pieces, what does the roadmap look like, what are the risks, what is the fallback*. For each question:
    - The question, in one line.
    - The answer, in one-to-three sentences. Honest — the internal FAQ is where the *cost*, *scope*, and *risk* live in a form the internal exec team can absorb before the PR is external-facing.
    - Cite the exercise-01 roadmap section for questions the roadmap already addresses (sequencing, resource asks, peer-platform dependencies).
    Suggested questions to include (adapt to the program):
    - *What is the cost, all-in, over the horizon?*
    - *What are we not doing to do this?*
    - *Who owns this on the ML side? Who owns it on peer platform teams? Who is the exec sponsor?*
    - *What is the risk this doesn't work? What is the fallback?*
    - *What changes for customers if we do this? What changes if we don't?*
    - *What is the compliance / risk posture? (mod-409 forward-pointer.)*
    - *When does the exec review the program and against what metric?*
13. **Draft the External FAQ (1-2 pages) — only if the program is external-facing.** If the program will not be visible to customers or regulators, skip this section and note explicitly *"no external FAQ; program is internal-facing"*. If it is external, five-to-ten customer-audience questions: *what is this, when is it available, what does it cost me, is my data safe, what happens if it goes down, how is this different from vendor X*.
14. **Cross-check the PR/FAQ against the one-pager and the roadmap.** Three sanity checks:
    - The program named in the PR/FAQ is one of exercise-01's Section 2 programs (not a program the ML org has not committed to).
    - The cost, timeline, and resource asks in the internal FAQ match Section 5's resource asks and Section 6's peer-platform dependencies.
    - The one-pager's recommendation — if it touches this program — is consistent with the PR/FAQ's framing. Inconsistency between the two exec artifacts is a trust-erosion event; catch it in draft.

## Deliverable

Two documents in one packet:

- **Document A — Decision one-pager.** One page (or two if the trade-offs section absolutely requires it — chapter 3's rule is *ruthless cutting first*). Structure per steps 2-8: metadata block, background, options, recommendation, trade-offs (including reversibility class), ask, contacts, related docs. Genre labelled at the top.
- **Document B — Working-backwards PR/FAQ.** Three-to-seven pages:
    - **Press release** (1 page).
    - **Internal FAQ** (2-4 pages, 8-15 questions).
    - **External FAQ** (1-2 pages, 5-10 questions) — if the program is external-facing; otherwise a one-line note explaining why omitted.
- **Cross-check note.** Half a page: the three sanity checks from step 14, each with one sentence confirming the check passed (or naming what was reconciled).
- **Reflection.** Two-to-three sentences: which was harder to compress into exec-audience language, the recommendation or the trade-offs? Which internal-FAQ question was hardest to answer honestly?

## Starter guidance

- **Label the genre in the first sentence, always.** Chapter 3's most-referenced rule. *"This is a decision doc requesting approval to..."* vs. *"This is an FYI on..."* vs. *"This is an options paper for the Q3 review."* Executives read the first sentence to calibrate their attention; label mismatches waste both the exec's time and the staff engineer's credibility. Do not skip the label because *"it will be obvious"*.
- **Cut the recommendation until it fits above the fold.** If the recommendation is not readable in the first 15 seconds — everything above the *Trade-offs* section — the one-pager has failed regardless of what is below. Compress. Move context to the appendix. Do not add background to soften the recommendation.
- **Trade-offs are where the exec tests you.** The section that reads *"minimal risk with careful implementation"* is the section that erodes exec trust. Even for a strong recommendation, name one real cost and one real deferred program. If the trade-offs list is empty, the recommendation is not real yet.
- **Frame trade-offs in the exec's units.** *"We accept 2% MFU regression to unlock FSDP"* does not land. *"We spend an extra $180k in Q3 training cost to keep the training program on the board-approved schedule"* does. mod-407's TCO vocabulary is the source; if the trade-off cannot be phrased in dollars, quarters, or business metrics, it is engineering discussion and belongs in the linked RFC.
- **Name the reversibility class explicitly.** Chapter 3 named this as a load-bearing move. *"Type 1 — once we migrate the model registry the previous registry is decommissioned within two quarters"* vs. *"Type 2 — we can renegotiate the vendor contract at annual renewal."* Type 1 decisions get a written exit condition in the same bullet. The reader (or a peer staff engineer reviewing) will look for the class label; missing it is a signal the doc is not review-ready.
- **PR/FAQ is not marketing copy.** The Press Release is a working-backwards visualisation tool — its job is to make the shape of the finished program *visible* to a leader who has to authorise the work now. Overselling in the PR ("*revolutionary new capability*") undermines the internal FAQ's honesty. Match tone: matter-of-fact in both, with the PR describing the outcome in business terms and the internal FAQ describing the cost in engineering terms.
- **The internal FAQ is the load-bearing section of the PR/FAQ.** Bryar & Carr are clear on this. The PR is the visualisation; the internal FAQ is the honesty check. If any of the eight-to-fifteen questions has an unwritten or hand-waved answer, the program is not ready to be authorised.
- **Cross-check for consistency between the one-pager and the PR/FAQ.** Both artifacts read against exercise-01's roadmap; both artifacts touch the same portfolio; both artifacts will be read by the same exec. Inconsistency (different cost numbers, different timelines, different owning teams) is trust-erosion. The step-14 cross-check is not a formality.

## Acceptance criteria

- **Document A (one-pager):**
  - The genre (decision doc / FYI / options paper) is labelled in the first sentence.
  - The top-of-page metadata includes Title, Requested action, Deadline. The deadline is a real date (or an explicit "no deadline" statement — not a missing field).
  - The Options section names 2-4 options, marks the recommended one, and does not list unserious alternatives.
  - The Recommendation is 2-3 sentences and sits above the trade-offs.
  - The Trade-offs section names at least: Cost (in exec units — dollars, quarters, or business metrics), Risk, What we defer (counterfactual cost), and Reversibility class (Type 1 or Type 2). Type 1 trade-offs include a written exit condition.
  - The Ask is a bullet list of concrete, actionable items — no vague "provide guidance" asks.
  - Contacts (staff engineer, peer EM, sponsor director) and Related docs (linked pointer to exercise-01's roadmap, at minimum) are present.
  - The one-pager fits on one page (or two if unavoidable) — no bloated summary; no technical jargon leaking into the summary.
- **Document B (PR/FAQ):**
  - The Press Release fits on one page and includes: headline, subhead, summary paragraph, one attributed quote, 2-3 key capabilities, and one line on availability.
  - The headline is in customer- or business-facing terms — not ML-engineering vocabulary.
  - The Internal FAQ has 8-15 questions, each with a 1-3 sentence answer. Answers cite exercise-01 roadmap sections where the roadmap already addresses the question.
  - The internal FAQ includes at least: cost, what-we-are-not-doing (counterfactual), ownership (ML side + peer platform team + exec sponsor), risk, fallback, compliance forward-pointer, and exec-review cadence.
  - The External FAQ is present with 5-10 questions if the program is external-facing; otherwise a one-line note explains the omission.
  - The program named in the PR/FAQ is one of exercise-01's Section 2 programs (not a new invention).
- **Cross-check note:** All three sanity checks from step 14 pass with a one-sentence attestation each (or a one-sentence note of what was reconciled to make them pass).
- **Reflection:** Two-to-three sentences identifying at least one honest compression cost from each document.

## Stretch goals

- **Rehearse the exec review out loud.** Time yourself reading the one-pager. Under 90 seconds should get you through everything above the trade-offs. If the exec asks the standard three follow-ups (*"what does this cost the roadmap"*, *"why not option B"*, *"what happens if we defer"*), can you answer from the doc without hand-waving? If not, the trade-offs section needs sharpening.
- **Author the FYI variant.** Draft the same portfolio decision as an FYI in half a page — the same background, the same recommendation, but explicitly labelled *"no action requested, informational."* Compare against the decision-doc version: what changed? What did the FYI version *lose* by not asking for a decision? This is the calibration for the "when to escalate to the exec" question chapter 3 named.
- **Simulate the counter-argument.** Have a peer role-play the CFO or CTO reading the one-pager, and ask them to write a one-paragraph rebuttal — either a rejection or a counter-offer. Absorb the rebuttal as a revision. Chapter 3's honest-trade-offs rule is easy to violate silently; a hostile reader catches it.
- **Author the Bezos-style narrative version.** In two pages, rewrite the recommendation as a memo the Amazon S-team format would recognise — six-page narrative with the recommendation on page one, arguments on pages two-three, alternatives on page four, risks on page five, appendix on page six. Compare against the one-pager: which was easier to write? Which is more honest? Neither is universally right; the calibration between the two is the staff-plus judgement.
- **Draft the follow-up FYI for a "yes" outcome.** If the exec approves the recommendation, what is the follow-up FYI to peer teams, the broader ML org, and the board (via the director) that operationalises the decision? Half a page. This is the *decision → org motion* artifact chapter 3 named as the follow-on to the exec sign-off.
- **Cross-reference to exercise-05.** If the one-pager touches a vendor commitment, cross-reference forward to exercise-05's five-question framework and confirm the reversibility class named in the trade-offs section matches the class exercise-05 would assign. Inconsistency between the exec-facing and vendor-decision framings is a common staff-plus authorship failure; catching it here is worth the ten minutes.
- **Compare against a real exec doc you have seen.** If you have read an exec one-pager or a PR/FAQ from a mature ML org (yours or a public write-up), read it against this exercise's rubric. Which of chapter 3's failure modes did the real doc exhibit? Which did it avoid? The comparison is calibration data for the next quarter's exec cycle.
