# exercise-04: Contribute-Back RFC with Ownership Contract

**Estimated effort:** 3 hours

## Objective

Author a **contribute-back RFC** offering an ML-org-built component (or a feature the ML org built and now wants the platform team to own long-term) to the peer platform team, using chapter 5's ten-section template. The load-bearing section is Section 6 — the **ownership contract** — covering code ownership, on-call, API-stability commitment, migration support for existing homegrown users, and feature-request routing. Six-to-ten pages.

The RFC's success criterion is not that the platform team accepts (as with exercise-03, that is out of your hands). The criterion is: *the platform team's staff-plus IC could counter-sign the ownership contract without renegotiating the substantive clauses*, or reject the RFC cleanly with a specific reason tied to one of the ten sections. Chapter 5's most common failure mode is the RFC-as-hand-off: offering the code but not the operational contract that makes the platform team's acceptance rational. Avoid it.

If the exercise-01 verdict was `adopt-managed` or `adopt-OSS` and the ML org has not built anything to contribute back, this exercise pivots: pick a plausibly contribute-back-shaped component from a public write-up (LinkedIn Feathr, Netflix Metaflow, Uber Michelangelo sub-components) and treat the RFC as if you were the ML-org Staff engineer at that company at the moment the contribute-back conversation started.

## Prerequisites

- Exercise-01 complete — the component's build-vs-adopt verdict is set. If the verdict was `build-inside-the-ML-org`, this RFC is the natural next artifact.
- Exercise-02 complete — the consumer contract in force. The ownership contract in Section 6 of this RFC reads against the same producer-commitment vocabulary chapter 4 named.
- Chapter 05 — the contribute-back RFC's ten sections, the ownership-contract section's five sub-clauses, the four common failure modes, and the "contribute-back vs. fork vs. adopt-OSS-and-sunset" decision test. Read in full.
- Recommended: read one contribute-back-shaped public write-up end-to-end — [LinkedIn open-sourcing Feathr](https://engineering.linkedin.com/blog/2022/open-sourcing-feathr---linkedin-s-feature-store-for-productive-m) or [Netflix open-sourcing Metaflow](https://netflixtechblog.com/open-sourcing-metaflow-a-human-friendly-lib-for-data-science-fa72e04a5d9). The pattern is the same shape at OSS scale as at internal-platform-team scale.
- Sanity-check: chapter 5's decision test named three cases where the answer is not contribute-back. Confirm your candidate is not: not deeply-coupled-to-the-ML-org-product-model (that is `fork`), not made-redundant-by-a-new-OSS-alternative (that is `adopt-OSS-and-sunset`), not being-used-to-route-around-a-rejected-gap-RFC (that is a political misfire). Chapter 5's decision test is upstream of writing the RFC.

## Choose the component

Pick **one** ML-org-built component or feature to offer to the platform team. Priority order:

1. If exercise-01's verdict was `build`, and the built component has now proven itself with two or more adopting ML-org teams: that is the natural candidate.
2. If your current employer (or a past employer) has an internal component that would be a contribute-back candidate: use it, with details anonymised if required.
3. Otherwise: pick from the public-write-up prior art (Feathr, Metaflow, Chronon, Michelangelo sub-components). Stipulate the setup at the top of the RFC — org shape, component, why the ML org built it originally.

Do not try to contribute back a component with only one internal adopter. Chapter 5's failure mode of *adoption case too narrow* is caught here: the RFC would really be a *transfer to platform ownership* proposal, and the platform team's evaluation criteria differ.

## Steps

1. **Walk chapter 5's decision test explicitly at the top of the doc.** For each of the three verdicts (contribute-back / fork / adopt-OSS-and-sunset), one paragraph: does this apply here, and why not (for the two you rejected)? The RFC's first job is to argue why contribute-back is the right verdict; if the RFC starts with the ownership contract, reviewers will (correctly) ask whether the verdict was ever considered.
2. **Author Section 1 — Summary.** One paragraph: component, current adopters inside the ML org, proposed hand-off quarter, ML-org sponsor. Save the final rewrite for the end.
3. **Author Section 2 — Motivation.** Why contribute back rather than keep ML-org-owned. Named: multiple non-ML-org consumers could benefit; ML-org's continued ownership carries the chapter-1 *silently-becoming-the-platform-team* scope-drift risk; the platform team is better positioned to sustain and evolve. One-to-two paragraphs.
4. **Author Section 3 — Component description and current state.** What it does, who currently uses it (specific ML-org teams and use cases), current SLO, documentation state, technical-debt items. Chapter 5 was emphatic: honest about limitations. The undisclosed-debt failure mode is caught here — if the component has three critical open bugs the ML org has been managing informally, name them.
5. **Author Section 4 — Adoption case beyond the ML org.** Named: which non-ML-org consumers benefit (analytics org, fraud org, whichever apply); what interviews have been done to validate this (the ML org must actually have done them); whether any consumer has counter-signed as a pilot adopter. Without this, the RFC is really a fork-the-ML-org's-code proposal and the platform team should reject it.
6. **Author Section 5 — Proposed API and technical shape.** Current API, proposed post-hand-off API (which may include the platform team's suggested changes on acceptance), and the migration story from the current API for existing ML-org adopters. One-to-two pages including a code-fence or two.
7. **Author Section 6 — Ownership contract.** The load-bearing section. Five sub-clauses, each named:
    - **6a — Code ownership.** Named repo (post-hand-off), CODEOWNERS entries, review-approval requirements.
    - **6b — On-call.** The platform team's on-call rotation covers the component. Named transition period (typically 2 quarters) during which ML-org engineers remain in a secondary escalation role. Rotation-hand-off date named.
    - **6c — API-stability commitment.** The platform team applies its paved-road stability window (semver, breaking-change deprecation months, backward-compat for supported versions). Numbers named — 6-month deprecation window, backward-compat for current + one prior minor version.
    - **6d — Migration support for existing homegrown users.** ML-org teams that were consumers of the pre-hand-off version get a migration plan and tooling support. Migration tooling named, migration deadline named.
    - **6e — Feature-request routing.** Post-acceptance, feature requests from any consumer become gap RFCs (chapter 5's shape) against the platform team's roadmap, not direct implementation asks against the ML-org engineers who originally wrote the code. This clause prevents the *founding authors get permanent support tickets* failure mode.
8. **Author Section 7 — Transition plan.** Named: what happens in the two-to-three quarters after acceptance. Repo transfer date, on-call transition date, staff-time budget from both sides during the transition (typically N hours per week from ML-org for M months, N hours per week from platform team for M months), exit criteria for calling the transition complete (e.g., three consecutive months with zero ML-org-side pages, one full deprecation cycle executed under platform-team ownership).
9. **Author Section 8 — Sunset alternative.** What happens if the platform team declines. Three options with the ML org's chosen path: keep-and-maintain (current state), fork-and-freeze (stop investing, migrate off to an OSS or vendor equivalent), or shut-down-and-adopt-alternative. Chapter 5 was explicit: naming this section is what makes the RFC bilateral. Without it, the RFC reads as ultimatum — a scope smell that platform teams reject on procedural grounds.
10. **Author Section 9 — Risks.** Three-to-five risks for the platform team to evaluate. Operational risk (SLO history), scope creep (feature-request stream the platform team inherits), maintenance backlog (technical debt), knowledge-transfer risk (the founding authors leaving the ML org before transition completes), consumer-migration risk (ML-org adopters missing the migration window).
11. **Author Section 10 — Signatures and review triggers.** ML-org signers (Staff engineer, ML-org EM if operational commitments cross team lines). Requested platform-team review timeline (typically 4-6 weeks per chapter 5). Escalation path if the review overruns.
12. **Return to Section 1 (Summary).** Rewrite the summary now that the RFC is drafted. One paragraph, four elements: component, adopters, hand-off quarter, sponsor.
13. **Sanity-check against chapter 5's four common failure modes.** Ownership contract too thin (Section 6 with "TBD" clauses) — go back and finish. Adoption case too narrow (Section 4 names only ML-org consumers) — either widen with interviews or relabel the RFC as *transfer to platform ownership*. Undisclosed debt (Section 3 skips the technical-debt list) — disclose it. No sunset alternative (Section 8 missing) — write it. All four are pre-review fatal.

## Deliverable

A single document, 6-10 pages, in the chapter 5 ten-section template with the exercise wrapper:

- **Section 0 — Verdict argument.** The decision-test walk-through from step 1: why contribute-back and not fork or adopt-OSS-and-sunset.
- **Section 1 — Summary.** One paragraph — component, current adopters, hand-off quarter, sponsor (step 2, rewritten in step 12).
- **Section 2 — Motivation.** Why contribute back (step 3).
- **Section 3 — Component description and current state.** Function, adopters, SLO, docs, technical debt (step 4).
- **Section 4 — Adoption case beyond the ML org.** Named non-ML-org consumers, interview evidence, counter-signed pilots (step 5).
- **Section 5 — Proposed API and technical shape.** Current API, post-hand-off API, migration story (step 6).
- **Section 6 — Ownership contract.** Five named sub-clauses — code, on-call, API stability, migration support, feature-request routing (step 7).
- **Section 7 — Transition plan.** Repo transfer, on-call transition, staff-time budgets, exit criteria (step 8).
- **Section 8 — Sunset alternative.** Three options with the ML-org chosen path (step 9).
- **Section 9 — Risks.** Three-to-five named risks (step 10).
- **Section 10 — Signatures and review triggers.** Named signers, requested timeline, escalation (step 11).
- **Appendix — Failure-mode audit.** One paragraph confirming none of chapter 5's four common failure modes apply (step 13).

## Starter guidance

- **Section 6 is what the RFC lives or dies on.** Chapter 5 was explicit: an ownership contract with "to be worked out" clauses is a rejected RFC. If you cannot name a specific deprecation window in months, a specific on-call transition date, or a specific migration-support commitment, either negotiate it with a peer or mark it `NEGOTIATE` and note in the appendix that the RFC is not yet review-ready. Do not paper over.
- **Section 3's debt disclosure is a trust move.** Chapter 5 named the undisclosed-debt failure mode: the platform team discovers the debt post-acceptance and the trust between the two staff-plus ICs takes damage that outlasts the specific incident. If the ML org has been managing three critical bugs informally, name them; the platform team's acceptance decision reads more favourably on an honest RFC than on a clean-looking RFC that surfaces problems in month 2.
- **Section 4 without interviews is fatal.** Chapter 5 was emphatic: the RFC must have actually interviewed candidate non-ML-org consumers. Guessing that the analytics org would want the component is not adoption evidence. If you cannot conduct the interviews (this is an exercise, after all), simulate them: for each candidate consumer, one paragraph on what a plausible interview would surface — and note in the appendix that these are simulated, not conducted. Do not present simulated interviews as real.
- **Section 8 makes the RFC bilateral.** Chapter 5: naming the sunset alternative is what turns the RFC from an ultimatum into an offer. The platform team's decision-making is markedly different when they know the ML org has a Plan B — the rejection cost is lower and their willingness to engage is higher.
- **Do not smuggle the fork verdict into a contribute-back wrapper.** Chapter 5's decision test named fork as the right verdict when the component is intrinsically ML-org-coupled or when the platform team has already declined a related gap RFC. If Section 0's decision walk-through reveals fork is actually the right verdict, stop and pivot: write a two-page fork memo instead of the contribute-back RFC. The fork memo has different sections (code ownership stays with ML org; deprecation is a fork-freeze-migrate plan; the platform team is not in scope).
- **The ownership-contract vocabulary reads against chapter 4.** Sub-clauses 6b (on-call), 6c (API stability), 6d (migration support) are the producer-commitment categories from chapter 4's consumer contract. Consistency across the module is the deliverable at project scope; if your ownership contract uses different vocabulary from your consumer contract, harmonise.

## Acceptance criteria

- Section 0 walks chapter 5's three-verdict decision test with a paragraph each on why contribute-back is preferred to fork and adopt-OSS-and-sunset.
- All ten chapter-5 sections are present. The summary in Section 1 is one paragraph with four named elements (component, adopters, hand-off quarter, sponsor).
- Section 3 (Current state) names specific ML-org adopting teams and use cases, current SLO, documentation state, and at least one technical-debt item (or an explicit statement that no material debt exists, with justification).
- Section 4 (Adoption case beyond the ML org) names at least one specific non-ML-org candidate consumer and includes either interview evidence or an appendix note that interviews are simulated.
- Section 5 (Proposed API and technical shape) includes at least one code-fence showing current API and one showing proposed post-hand-off API.
- Section 6 (Ownership contract) has **all five** named sub-clauses — code ownership, on-call, API stability, migration support, feature-request routing — each with specific, numeric, or dated commitments. No "TBD" or "to be worked out" without a `NEGOTIATE` marker and an appendix note.
- Section 7 (Transition plan) names repo transfer date, on-call transition date, staff-time budgets from both sides, and exit criteria for the transition.
- Section 8 (Sunset alternative) names all three options (keep-and-maintain, fork-and-freeze, shut-down-and-adopt-alternative) with the ML-org's chosen fallback path.
- Section 9 (Risks) has 3-5 named risks including at least one operational risk and one knowledge-transfer or maintenance-backlog risk.
- Section 10 (Signatures and review triggers) names both-side signers, requested platform-team review timeline, and an escalation path.
- Appendix confirms none of chapter 5's four common failure modes (ownership contract too thin, adoption case too narrow, undisclosed debt, no sunset alternative) apply.
- The ownership-contract vocabulary in Section 6 is consistent with the producer-commitment vocabulary in exercise-02's Section 5.

## Stretch goals

- **Take the draft to a peer role-playing the platform-team staff engineer.** Ask them to write a one-paragraph disposition on the ownership contract in Section 6, sub-clause by sub-clause. Which clauses would they counter-sign? Which would they push back on? The exercise is the single highest-signal check on whether Section 6 is real or aspirational.
- **Draft the fork-memo alternative.** In two pages, write the fork verdict as if it had won Section 0's decision test. Code stays with the ML org; the on-call stays with the ML org; the deprecation plan is a fork-freeze-migrate schedule the ML org executes internally. Compare against the contribute-back RFC: which was easier to write? Which was more honest? Sometimes the exercise surfaces that fork was the right verdict all along.
- **Author the "founding author retention plan" appendix.** Section 9 named knowledge-transfer risk. Section 6e (feature-request routing) prevents founding authors from becoming permanent support. But during the transition, the founding authors are the platform team's most valuable asset. In one page: what does the retention plan look like for the two-to-three quarters of the transition? Who else can carry the knowledge? What is the *bus-factor mitigation*? Chapter 5 gestures at this; the appendix fills it in.
- **Simulate the platform-team review meeting.** Write a 20-minute agenda: Section 0's decision-test walk-through (ML-org author leads), Section 6's ownership-contract sub-clause-by-sub-clause negotiation (both staff-plus ICs), Section 8's sunset alternative check (platform-team EM), sign-off or defer. Include the specific numbers each side must confirm.
- **Cross-reference to project-401.** Note which sections of this RFC will land in project-401's platform-strategy artifact and which are RFC-specific detail. As with exercise-03, consistency across the module's four artifacts is the capstone deliverable; catching drift early is worth an hour.
- **Track the RFC through to a real disposition.** If the component is a real component at your current employer, offer the RFC to your platform-team counter-part as a "practice contribute-back RFC" for their feedback. Even without a formal review, feedback from the actual counter-party is the highest-signal input available — and occasionally the RFC lands and the contribute-back happens for real. That outcome is the whole point of the module.
