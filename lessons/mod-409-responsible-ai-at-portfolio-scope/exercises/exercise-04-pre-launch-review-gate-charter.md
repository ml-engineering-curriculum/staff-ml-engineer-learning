# exercise-04: Pre-Launch Review Gate Charter

**Estimated effort:** 2 hours

## Objective

Author the **pre-launch review gate charter** for the portfolio you have been building through exercises 01, 02, and 03. The charter is the document that defines the required artifacts, the signoff matrix, the block-vs-flag rule, the freshness thresholds, and the escalation path to the head of AI governance. The deliverable is a two-to-four-page charter document plus a one-page walked-example gate review of one launching system drawn from your portfolio — the artifact that tests whether the charter you have written would actually work when a launch arrives on Monday.

Chapter 5 was explicit that a gate that never blocks anything is decoration; a gate that blocks everything is a bottleneck the org routes around within a quarter. This exercise's most consequential move is drafting the block-vs-flag rule and the escalation path — the two decisions that determine whether the gate is respected or resented. After this exercise the learner holds the fourth of the module's five artifacts (the fifth is chapter 6's delegation contract, addressed as a stretch goal) and the artifact that consumes all three prior exercises' outputs as one packet.

## Prerequisites

- Exercise-01's register for the portfolio. The gate blocks on register-row freshness and completeness.
- Exercise-02's classification table for the same portfolio. The gate proportionality rule keys off the EU AI Act tier per row.
- Exercise-03's model-card and datasheet templates and the populated instance for one system. The gate consumes the populated instance as one of the four always-required artifacts.
- Chapter 05 — the four always-required artifacts, the two conditional artifacts, the signoff matrix by tier, the block-vs-flag rule, the escalation to the head of AI governance, and the Staff-owns-charter vs. analyst-owns-policy split. Read in full.
- Chapter 06 — the red-team artifact schema and the delegation contract. Read enough to understand what the red-team artifact will look like when the gate consumes it (this exercise treats the contract as a given rather than authoring it, unless you take the stretch goal).
- Recommended: skim *Software Engineering at Google* (Winters, Manshreck, Wright; O'Reilly, 2020) chapters on design documents and launch committees for the shape reference of a launch-committee charter in a mature ML-adjacent org.

## Choose the launching system

Reuse the exercise-03 chosen system for the walked-example gate review. Priority order:

1. **The exercise-03 system, whose model card and datasheet you already populated.** This is the whole point of the four-exercise chain — the packet from exercises 01-03 arrives at exercise-04's gate; the walked-example rehearses the flow end-to-end. Do not choose a different system if this one is available.
2. **A different high-risk system from the exercise-02 classification, with a stipulated packet.** If for some reason the exercise-03 system is minimal-risk and you want to exercise the heavy-tier signoff matrix, stipulate the packet contents (register row, classification row, card, datasheet) at the top of the walked-example section and note the stipulation.

## Steps

1. **Draft the charter header.** One paragraph at the top naming the portfolio the charter binds, the charter version and date, the charter owner (Staff ML engineer — you), the governance-analyst counterpart, the head of AI governance for escalation, the deployment tool or launch runbook the gate is integrated with, and the cadence at which the charter is reviewed (annually by default, plus on any material policy change).
2. **Draft Section 1 — Scope of the gate.** Half a page. Which launches go through this gate: every new system, every material re-train (define material), every material product change (define material — a change in input modality, a change in the population served, a change in the automation level of the decision). Which launches are exempt (hotfix roll-forwards under a documented rule; internal-only experiments below a defined traffic threshold). Chapter 5 was explicit that a gate whose scope is not written down loses the argument the first time a team wants to skip it.
3. **Draft Section 2 — Required artifacts.** Chapter 5's four always-required (register row, classification-table row, model card, datasheets) and two conditional (red-team artifact, human-oversight design). For each:
   - **Freshness threshold.** How recent the artifact must be at the gate — register row less than 30 days, classification row less than 90 days or since last material change, model card and datasheets against the current template version, red-team artifact against the cadence chapter 6 sets.
   - **Trigger for the conditionals.** When each conditional artifact becomes required — chapter 5's rule for the red-team artifact (any gen-AI system, any system with adversary-shaped inputs, any high-risk system) and for the human-oversight design (any high-risk system, any system with automated decisions materially affecting users).
   - **Pre-review completeness check.** The Staff-owned quality gate that runs before the reviewers spend attention on the packet — the packet is present, links resolve, the register row is fresh, the classification-table row is legal-reviewed if the system is high-risk. Packets that fail the pre-review check are returned to the launching team with the specific defects named, not just "not ready".
4. **Draft Section 3 — Signoff matrix.** Chapter 5's five-role matrix (product owner, ML tech lead, governance analyst, legal counsel, security partner) with the required-for-tier column filled in for your portfolio. For each role:
   - **Mandate at the gate.** In one sentence, what this role is signing for. A role signing outside its mandate is a role the gate has misused.
   - **Signoff form.** Role, name at signoff, date, one-line rationale for any conditional acceptance. The gate produces a signed packet, not a Slack thumbs-up.
   - **Time-box.** Five business days per role by default. A review that has not moved in the time-box escalates to the head of AI governance, both to prevent quiet blocking and to prevent quiet skipping.
5. **Draft Section 4 — Block-vs-flag rule.** Chapter 5's most consequential move. For each condition that triggers a block, write it as a testable rule: "any always-required artifact missing", "any always-required artifact below its freshness threshold", "classification-table row `unreviewed` on a system whose engineering read is high-risk", "model card intended-use materially disagrees with the launching product surface", "eval evidence in the model card is older than the per-tier freshness threshold or has regressed materially against a named metric", "unresolved security-partner finding of risk from the red-team artifact", "any policy conflict the standing signoffs cannot resolve — block continues through escalation". For each condition that triggers a flag, write it as a rule with an owner and a target date: "section thin but artifact present, owner and date to fill", "conditional artifact recommended follow-up not yet scheduled, owner and date". Name the enforcement — a flag whose target date passes graduates to a block on the next launch of the same system.
6. **Draft Section 5 — Escalation path.** Chapter 5's short, documented process. Write the escalation as three named steps: (1) reviewer identifying the conflict files a one-page memo (name the memo's required fields — conflict, two positions, decision requested, launch-dependent date); (2) head of AI governance receives, resolves directly, chairs a decision meeting, or refers to the policy-waiver process; (3) resolution recorded on the classification-table row and, if it modifies policy, propagates into the analyst's policy document. Name the two anti-patterns to avoid (escalating too early — every disagreement is not escalation-worthy; escalating too late — the five-business-day time-box on standing signoffs is what forces the escalation clock).
7. **Draft Section 6 — Ownership split at the gate.** Quarter-page block. Chapter 5's Staff-owns (charter, operational running, chairing high-risk reviews, gate-health reporting) vs. analyst-owns (the policy, chairing policy-review-shaped launches, waiver signoff, external-audience summaries) split, specialised to *this* portfolio and *this* analyst counterpart.
8. **Draft Section 7 — Gate-health measurement.** Half a page. Chapter 5 was explicit — the gate is measurable. Name the metrics you will report to the head of AI governance on a documented cadence: median review latency by tier, block rate, flag-graduation rate (flags that graduated to blocks because their target date passed), escalation frequency, packet-return rate at the pre-review completeness check (step 3). Name where these numbers are logged and who reviews them.
9. **Walk one example gate review.** One page. For the system chosen above, walk the gate end-to-end:
   - **Packet contents.** Which of the four always-required artifacts are present (from exercises 01, 02, 03), which of the two conditional artifacts are present or waived, and the freshness of each.
   - **Pre-review completeness check.** Pass or fail; if fail, name the defect and the return.
   - **Signoff matrix walk.** For each required role, would they sign, and on what rationale? For any role that would not sign, name the block or the flag.
   - **Verdict.** Block, flag, or clear. If flag, the target-date and owner. If block, the specific defect and the path to unblock.
   - **Escalation.** If the walk surfaces a policy conflict the standing signoffs cannot resolve, name it and walk step 6.
   This is the exercise's most valuable part. A charter that reads well on paper but breaks on the first walked example is a charter you want to revise before Monday.
10. **Score the charter against the "will teams route around this" test.** Re-read the charter asking, for each rule you wrote: would a team lead facing a launch deadline route around this? If the answer is yes for any rule, that rule is either mis-calibrated (too heavy for the tier), missing a proportionality shape (the same rule for high-risk and minimal-risk), or missing the enforcement mechanism (no consequence for skipping). Fix each rule that fails this test. Chapter 5's proportionality is exactly what defends against routing-around.

## Deliverable

Two artifacts in one document (or two sibling documents in a folder):

**Artifact 1 — Gate charter** (2-4 pages):

- **Header.** Portfolio, version, owner, analyst counterpart, escalation destination, launch-tool integration, review cadence (step 1).
- **Section 1 — Scope.** Which launches go through the gate; which are exempt (step 2).
- **Section 2 — Required artifacts.** Four always-required and two conditional, each with freshness thresholds and the pre-review completeness check (step 3).
- **Section 3 — Signoff matrix.** Five roles, mandate per role, signoff form, five-business-day time-box (step 4).
- **Section 4 — Block-vs-flag rule.** Blocks as testable rules; flags with owner and target date; the flag-graduates-to-block enforcement (step 5).
- **Section 5 — Escalation path.** Three-step, one-page-memo escalation to the head of AI governance; two anti-patterns to avoid (step 6).
- **Section 6 — Ownership split at the gate.** Staff-owns vs. analyst-owns, specialised to this portfolio (step 7).
- **Section 7 — Gate-health measurement.** Named metrics, log location, review cadence (step 8).

**Artifact 2 — Walked-example gate review** (1 page):

- Packet contents summary with freshness per artifact (step 9).
- Pre-review completeness check outcome.
- Signoff matrix walk with per-role verdict.
- Final verdict (block / flag / clear) with defect or owner-and-date.
- Escalation (if any) with the memo shape from step 6.

## Starter guidance

- **The block-vs-flag rule is the charter's political move.** Chapter 5 was emphatic — a gate that always blocks becomes hated, a gate that always flags becomes ignored. Write blocks as testable rules a launching team can predict and prepare for; write flags with owner and target date so they are not quietly permanent. The rule of thumb: block on absent artifacts and material regressions; flag on thin sections and scheduled-but-not-yet-happened follow-ups.
- **Proportionality is the charter's most important discipline.** A minimal-risk recommender launch does not survive the high-risk-system signoff matrix — the launching team will route around it within a quarter. Chapter 5's proportionality by tier is what makes the gate landable across the portfolio; write the matrix tier-by-tier.
- **The pre-review completeness check saves everyone's attention.** A packet that arrives at the reviewers with broken links, a missing datasheet, or a stale register row is a packet that wastes the reviewers' time and produces a hostile flag. The Staff-owned pre-review check catches these before the reviewers ever look. It is the single most cost-effective piece of the charter.
- **Signoffs are role, not person; role, not team.** Chapter 5 was explicit. The product-owner-role signs; when the person rotates, the next role-holder signs the next launch, no institutional-memory carry-over. This is what keeps the gate legible across staff turnover.
- **The five-business-day time-box is not a suggestion.** Chapter 5 was explicit that quiet blocking and quiet skipping are the two failure modes of unbounded reviews. The time-box is what forces the escalation clock; if you drop it or extend it silently, both failure modes return.
- **The escalation path is short deliberately.** A one-page memo, a decision-maker, a resolution recorded in a specific place. Escalations that grow into four-week processes are escalations teams learn to fear rather than use.
- **Do not treat the walked example as decoration.** Chapter 5 was explicit — the audit trail is the career signal. The walked example is where the charter meets reality; if the walk surfaces a defect (a rule that would have missed a block, a signoff role missing for a specific tier), fix the charter before shipping it. That is what the walk is for.
- **The gate-health metrics are how the head of AI governance believes the gate exists.** Median review latency, block rate, flag-graduation rate, escalation frequency, packet-return rate. Numbers that get reported are numbers that get taken seriously. If Section 7 is empty at charter v1, the gate is invisible to the roles funding it.
- **This is the charter, not the policy.** Chapter 5 and chapter 1 were both explicit — the policy the gate enforces belongs to the governance analyst. If you find yourself writing "the org's position on fairness is X" inside this charter, extract it out; the charter references the policy by name and does not author it.

## Acceptance criteria

- The charter binds a real portfolio (the exercises-01-through-03 portfolio) named in the header block with version, owner, analyst counterpart, escalation destination, launch-tool integration, and review cadence.
- Section 1 defines the scope of the gate — which launches go through it, what counts as material re-train or material product change, and what is exempt.
- Section 2 lists all four always-required artifacts (register row, classification-table row, model card, datasheet(s)) and both conditional artifacts (red-team, human-oversight design) with freshness thresholds and per-conditional triggers.
- Section 2 defines the pre-review completeness check with the specific things it verifies before the reviewers spend attention.
- Section 3 defines the signoff matrix for all five roles (product owner, ML tech lead, governance analyst, legal counsel, security partner) with a per-role mandate, signoff form, and a five-business-day (or explicitly justified other) time-box.
- Section 4 lists blocks as testable rules and flags with named owner and target date; the flag-graduates-to-block enforcement is stated.
- Section 5 defines a three-step escalation path to the head of AI governance with the memo's required fields; names the two anti-patterns to avoid.
- Section 6 states the Staff-owns vs. analyst-owns split at the gate specialised to this portfolio.
- Section 7 names at least four gate-health metrics with a log location and a review cadence.
- The walked-example gate review is present, uses the chosen system's packet, walks pre-review, signoff, verdict, and escalation, and produces a specific verdict (block / flag / clear) with either a defect or an owner-and-date.
- No rule in the charter would let a team route around the gate on a plausible reading; the routing-around test (step 10) is documented.
- The charter nowhere authors the responsible-AI policy itself — it references the analyst's policy document by name.

## Stretch goals

- **Draft the delegation contract to the peer security track.** Chapter 6's one-page contract with scope, cadence, artifact schema, escalation, signoff, and hand-back triggers. This is the fifth module artifact; landing it here (rather than deferring to a project or a later exercise) closes the module's artifact set. Grounded in `ai-infra-security-learning`'s catalogue if you have access.
- **Populate the gate-health dashboard.** A one-page mock dashboard for the head-of-AI-governance monthly review: median review latency by tier over the last two quarters, block rate, flag-graduation rate, escalation frequency, packet-return rate. This is what the head of AI governance opens; the numbers are what earn the gate its continued sponsorship.
- **Simulate the first three months of the gate.** Half a page. Pick three plausible launches from the portfolio — one high-risk, one limited-risk, one minimal-risk — and walk each through the gate at the level of step 9. Note where the charter would be revised after each. Chapter 5's audit trail is what a promotion committee reads; the simulation is your first draft of it.
- **Draft the exec-audience gate summary.** One page. The version of the charter that goes to the board or the exec team on the annual cadence. Non-technical, focused on the block that mattered, the flag that closed, the escalation that resolved, and the numbers from Section 7. Chapter 5 was explicit that this summary is analyst-owned but Staff-supplied; drafting it here rehearses the supply.
- **Take the draft to a peer for adversarial pre-review.** A Staff-plus peer, ideally one with prior review-body experience. Ask them to identify the rule they would most likely dispute and the walked-example verdict they most disagree with. Absorb the pushback and revise. This is a rehearsal for the analyst's first gate review of your charter.
- **Draft the exec one-liner.** One sentence at the top of the charter: what the gate requires, what it does not, when it starts. "The pre-launch gate requires four artifacts and up to five signoffs per launch, proportional to EU AI Act tier; effective Q3 for every new launch and every material re-train; the head of AI governance signs off on the charter." That sentence is what the head of product reads when the launch team escalates a delay.
