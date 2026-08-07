# exercise-05: Vendor and Partnership Judgement Doc

**Estimated effort:** 2 hours

## Objective

Author a **vendor-decision doc** on one live vendor call from chapter 6's four decision classes — **LLM API vendor mix**, **foundation-model provider (self-host vs. API)**, **cloud provider (specialty compute)**, or **MLOps vendor selection** — using chapter 6's five-question framework, the utility-vs-strategic classification, and the non-negotiable written exit contract. After this exercise the learner should hold a three-to-five-page decision doc that names the reversibility class, the TCO shape, the political ownership of the migration, and — most load-bearing — the exit contract with data-portability, contract-termination, migration, fallback, and trigger clauses. The document should be one that Finance and Legal could counter-sign on the material-commitment clauses.

Pairs with chapter 6. Reads against exercise-01's roadmap Section 6 (peer-platform dependencies) and Section 5 (resource asks). Feeder for [`project-403-staff-plus-tech-leadership-simulation`](../../../projects/project-403-staff-plus-tech-leadership-simulation/).

## Prerequisites

- Chapter 06 — the four vendor decision classes, the Bezos Type 1 / Type 2 reversibility axis applied to vendors, the utility-vs-strategic distinction (Fowler / Wardley), the five-question framework, the LLM-vendor routing-layer pattern, the hybrid API + self-host posture, the cloud-provider trade-off patterns, the MLOps-vendor mod-405 boundary, the exit-contract non-negotiable, the Staff / EM / Finance / Legal signatory pattern, and the five common failure modes. Read in full.
- Chapter 03 — the exec one-pager structure; the trade-offs section of the exec one-pager (from exercise-02) is where this doc's reversibility-class and TCO framing lands for the exec audience.
- mod-405 chapter 3 — the build-vs-adopt seven-criterion weighted rubric, if the vendor decision is an MLOps vendor at the mod-405 boundary. The MLOps vendor case in exercise-05 reads against the exercise-01 verdict from mod-405 where applicable.
- mod-407 — the TCO model this exercise's five-question framework question 3 depends on. If the learner has completed mod-407 exercise-01 or exercise-02, that model is the input to this doc's TCO section.
- Recommended: the [1997 Amazon shareholder letter](https://s2.q4cdn.com/299287126/files/doc_financials/annual/Shareholderletter97.pdf) for the Type 1 / Type 2 framing; Martin Fowler's [Utility vs. Strategic Dichotomy](https://martinfowler.com/bliki/UtilityVsStrategicDichotomy.html); a Wardley Maps intro on the utility / product / custom axis. <!-- needs-research: verify a durable Wardley Maps URL once WebSearch is available. -->

## Choose the vendor decision

Pick **one** vendor decision from one of chapter 6's four classes. Priority order:

1. A **live vendor call on the learner's desk** (current employer, current portfolio) that the ML org will actually make in the next quarter. This is the highest-signal option; the exit contract in Section 6 is the load-bearing section chapter 6 named, and it lands best when written against a real commitment.
2. A **vendor call the ML org has recently made** that the learner can write retrospectively — same shape, calibrated against what actually happened.
3. If neither above applies, stipulate a scenario from one of the four classes:
    - **LLM API vendor mix.** *"The ML org currently routes 90% of production LLM calls through Vendor A (frontier vendor). Vendor B cut prices 40% on their frontier model last week and their quality on the org's task suite is now within 5% of Vendor A. Design the routing policy."*
    - **Foundation-model provider.** *"The ML org's batch enrichment pipeline consumes 12M tokens/day through Vendor A at $X. Meta's Llama-3-70B on 4x H100s would run the same workload at estimated $Y with an added ops surface. Decide."*
    - **Cloud provider (specialty compute).** *"The primary cloud is AWS. Training clusters currently run on p5-instances at $Z/hour. CoreWeave quotes 45% cheaper for the same GPU class with 2-quarter reserved capacity. Decide."*
    - **MLOps vendor selection.** *"The peer ML platform team is 4 quarters from a paved-road experiment tracker. The ML org can adopt W&B in the interim for $X/yr. Decide."*

Do not scope this exercise to more than one vendor decision. Chapter 6's five-question framework is per-decision; running it against two decisions in two hours produces two thin docs.

## Steps

1. **State the vendor decision and its class in one paragraph.** Named: the specific decision (with vendor candidates), the chapter-6 class (LLM API mix / foundation-model provider / cloud provider / MLOps vendor), the exercise-01 roadmap dependency (which program, if any, is affected), the current state (what is in place today), the forcing function (why this decision is on the staff engineer's desk now).
2. **Answer Question 1 — Reversibility class.** One-to-two paragraphs. Chapter 6's first move: is this Type 1 (irreversible in the two-year window — deep SDK integration, multi-year committed spend, data-egress trap, substrate-shaping choice) or Type 2 (reversible within a quarter or two)? Chapter 6 was direct: vendors will consistently frame everything as Type 2; the staff engineer names the actual class based on the specific integration shape. If the classification is contested — the vendor says Type 2, the technical shape reads Type 1 — argue it explicitly here. The Type-1 argument becomes the trigger for the deep exit contract in Section 6.
3. **Answer Question 2 — Utility vs. strategic.** One-to-two paragraphs. Where does this capability sit on the Fowler / Wardley axis? Chapter 6's three-way split:
    - **Utility** (commodity — object storage, experiment tracking at low usage, log aggregation): prefer commodity vendor; buy, do not build; optimise for reliability and price.
    - **Strategic** (differentiator — the ranking-model architecture, the proprietary feature layer, the org's specific eval standard): do not outsource; the vendor's roadmap will not match the org's differentiation trajectory.
    - **In-between**: the interesting cases — LLM APIs, foundation-model training compute, inference gateways. May be strategic for one workload and utility for another inside the same portfolio; the routing-layer pattern applies (chapter 6's LLM section).
   If the answer is in-between, explicitly name the workload split that makes it so.
4. **Answer Question 3 — Total cost of ownership.** Two-to-three paragraphs plus a four-row TCO table. The mod-407 TCO model:
    - **Variable spend** — vendor sticker price × usage × horizon.
    - **Engineering hours to integrate** — the initial adoption cost. Named as engineer-quarters or FTE-months.
    - **Engineering hours to maintain the integration** — the year-2/3 ops load. Chapter 6 was direct: for most non-trivial vendor decisions, engineering hours dominate sticker price.
    - **Exit cost** — the engineering hours to leave the vendor if the exit contract is invoked. This row is what the Section 6 exit contract quantifies.
    - **Opportunity cost** — the value of the vendor's rate of feature release outpacing internal build (or the counterfactual — the cost of *not* adopting the vendor, if the internal capability is behind).
   Present as a four-column table: category × cost × confidence × source. Any TBD row is a red flag; either write the number or explicitly note the confidence range.
5. **Answer Question 4 — Political shape.** One-to-two paragraphs. Which teams have to change their workflow to adopt? Which peer platform team's roadmap is affected? Who benefits, who pays the migration cost, and does the migration cost land on the team that benefits? Chapter 6's key trigger: adopting an MLOps vendor without the peer platform team in the loop is a partnership failure — the mod-405 partnership rules apply here. Explicitly name the peer platform team's disposition (aligned, neutral, opposed) if the decision is at the mod-405 boundary.
6. **Answer Question 5 — Exit contract (the load-bearing section).** The single non-negotiable section chapter 6 named. Five sub-clauses, each with named commitments — not aspirational language:
    - **6a — Data portability.** Can we get our data out? In what format? At what cost? Over what timeline? Named: specific export format (Parquet + JSONL + config, etc.), named per-GB cost or per-record cost, named timeline commitment (X days from request).
    - **6b — Contract termination.** Notice period (X months), penalties (dollar amount or waived), any lock-in provisions (early termination fee, minimum-commit clawback, unused-credit forfeiture). If the vendor's standard MSA has terms the ML org will not accept, name them here and note the negotiation target.
    - **6c — Engineering migration.** What internal engineering is needed to move off the vendor? Named as engineer-weeks or FTE-months. Named owner (which team, which L30 tech lead). Named preparation state (is there a routing-layer wrapper today or does the org have to build one at exit time — the wrapper is one of chapter 6's key patterns for LLM APIs).
    - **6d — Fallback.** Which specific vendor or internal capability do we migrate *to*? Do we have a written commitment from that fallback team? If the fallback is a peer platform team's paved road, cite the mod-405 consumer contract; if the fallback is another vendor, name them and note whether their pricing and capacity are actually available at the volumes we would need.
    - **6e — Trigger.** Under what conditions do we invoke the exit? Named: cost thresholds (variable spend > X or unit cost > Y), capability regressions (quality drop > Z on the org's task suite), contract-renewal decisions (do not renew if condition A), strategic pivots (vendor bought by strategic competitor, vendor pivots away from our workload class).
7. **Author the routing-layer commitment (LLM-API-class decisions only).** If the decision is in the LLM API vendor mix class, one paragraph on the routing-layer pattern chapter 6 named as the single most repeatable vendor discipline. Structure:
    - **Interface stability.** The org's internal LLM-call wrapper — one thin abstraction over vendor SDKs. All vendor-specific tuning lives behind it.
    - **Cap enforcement.** The maximum-per-vendor commitment (chapter 6's 60-70% heuristic) is enforced by the routing policy, not by hope.
    - **Renewal cadence.** Quarterly (typical), with the routing policy revisited as first-class staff work.
8. **Author the staff / EM / Finance / Legal signatory block.** Chapter 6's expanded boundary. In one paragraph each:
    - **Staff owns.** Technical shape, reversibility-class assessment, TCO input, routing policy, exit-contract engineering, peer-platform implications. Named signatory.
    - **EM owns.** Delivery framing, team capacity to adopt/migrate, on-call impact, engineering-hours estimate. Named signatory.
    - **Finance owns.** Contract negotiation, spend commitment level, vendor management, MSA sign-off. Named signatory or "*Finance-team lead, TBD*" placeholder for exercise purposes.
    - **Legal owns.** Data-processing agreement, indemnification, IP terms, regulatory review (mod-409 forward pointer). Named signatory or "*Legal-team lead, TBD*" placeholder.
    - **All four sign** on material-commitment clauses (Section 6a, 6b, and the variable-spend commitment in Section 4).
9. **Sanity-check against chapter 6's five failure modes.** In one paragraph:
    - **The vendor's slides became the analysis.** Confirm Section 4's TCO model has a non-vendor source for each row.
    - **Standardisation without a routing layer.** Confirm Section 7's routing-layer commitment exists (for LLM-API-class decisions). For non-LLM decisions, confirm the exit-contract engineering (Section 6c) does not depend on a routing-layer wrapper that does not exist today.
    - **Exit contract in the vendor's language.** Confirm Section 6 is written in the ML org's language with specific numbers, not in the vendor's *"you can leave any time"* framing.
    - **Off-paved-road adoption without the platform team.** Confirm Section 5 (political shape) named the peer platform team's disposition (for MLOps-vendor-class decisions).
    - **Under-commitment to the primary vendor.** Confirm the routing-layer cap in Section 7 is a *cap and floor*, not just a cap.

## Deliverable

A single document, 3-5 pages, in this structure:

- **Section 1 — Vendor decision and class.** One paragraph (step 1).
- **Section 2 — Reversibility class.** Question 1's answer (step 2).
- **Section 3 — Utility vs. strategic.** Question 2's answer (step 3).
- **Section 4 — Total cost of ownership.** Question 3's answer with a four-row × four-column table (step 4).
- **Section 5 — Political shape.** Question 4's answer, including peer platform team disposition where applicable (step 5).
- **Section 6 — Exit contract.** Question 5's answer with all five sub-clauses (6a-6e) named with specific commitments (step 6).
- **Section 7 — Routing-layer commitment.** One paragraph, LLM-API-class only; for other classes, one sentence noting non-applicability (step 7).
- **Section 8 — Signatory block.** Staff / EM / Finance / Legal, with the material-commitment sign-off clause (step 8).
- **Section 9 — Failure-mode audit.** One paragraph confirming chapter 6's five common failure modes are addressed (step 9).
- **Reflection.** Two-to-three sentences: which of the five questions was hardest to answer honestly? Which sub-clause of the exit contract needed a real negotiation the exercise had to hand-wave?

## Starter guidance

- **Name the reversibility class before anything else.** Chapter 6's first move. Vendors frame everything as Type 2. The staff engineer's first job on any vendor decision is to look past the vendor's framing at the actual integration shape and name the class. If the class is contested, argue it in Section 2 — the argument is the value the doc delivers.
- **Do not accept the vendor's TCO as your TCO.** Chapter 6's first failure mode. The vendor's cost comparison is optimised for the vendor's win condition — usually favouring their variable spend against a straw-man internal build. Your Section 4 TCO uses mod-407's discipline: your own engineering-hours estimate, your own operations-hours estimate, your own exit-cost estimate, with the source of each number cited. If any row's source is "vendor materials", the section needs rework.
- **The exit contract is the load-bearing section, not the appendix.** Chapter 6 was emphatic. A vendor decision without a written exit contract is a Type 1 door dressed as a Type 2 door. If any of the five sub-clauses (6a data portability, 6b termination, 6c engineering migration, 6d fallback, 6e trigger) has *"to be negotiated"* or *"depends on vendor terms"* without a numeric target, the section is not review-ready. Either write the number or mark the sub-clause `NEGOTIATE` with a specific target — do not paper over.
- **The routing-layer pattern is the LLM-vendor discipline.** For LLM API vendor mix decisions, chapter 6's routing-layer pattern converts a Type 1 lock-in call into a Type 2 workload-routing call. If your Section 7 does not commit to the routing layer, your decision is either standardising on a single vendor (which is Type 1 and the exit contract needs to reflect it) or leaving the routing implicit (which fails at the next vendor pivot). Choose explicitly.
- **The peer platform team is not optional for MLOps vendors.** Chapter 6's fourth failure mode. If the decision is in the MLOps vendor class and the peer platform team's disposition is not named in Section 5, the doc is not partnership-ready. The mod-405 partnership rules apply — the decision goes to the peer platform team for review before signing. Adopting a vendor that competes with the peer platform team's roadmap without the platform team in the loop is a durable partnership failure.
- **Finance and Legal are not formalisms.** Chapter 6 named Finance and Legal as expanded signatories on any material commitment. If your Section 8 has the Staff engineer and EM signatures but Finance and Legal placeholders, the doc is not ready for the vendor's MSA — the ML org's Legal team must review the DPA and IP terms before signing, and Finance must review the spend commitment. Named the specific negotiations these teams will run.
- **Cross-check against exercise-02's exec one-pager.** If the vendor decision requires exec sign-off, the one-pager in exercise-02 is the exec-audience artifact this doc feeds. The reversibility class named in Section 2 of this doc should match the reversibility class named in the one-pager's trade-offs section. Inconsistency is a trust-erosion event; catch it in draft.
- **Do not confuse the vendor decision with the build-vs-adopt decision.** For MLOps-vendor-class decisions, the mod-405 chapter 3 rubric is the build-vs-adopt call; this exercise's five-question framework is the *adopt-vs-adopt-which-vendor-under-what-terms* call. They are complementary, not redundant. If your exercise-05 doc is really a build-vs-adopt argument, redirect to mod-405 exercise-01 for that shape.

## Acceptance criteria

- Section 1 names the vendor decision, the chapter-6 class, the exercise-01 roadmap dependency, the current state, and the forcing function.
- Section 2 (Reversibility class) explicitly names Type 1 or Type 2 and argues the classification against the vendor's own framing.
- Section 3 (Utility vs. strategic) places the capability on the Fowler / Wardley axis. If the answer is in-between, the workload split that makes it so is named.
- Section 4 (TCO) has a four-column × 4-5-row table: category × cost × confidence × source. No row has "TBD" without an explicit confidence range. The variable spend row is grounded on the mod-407 TCO model (or explicitly notes non-applicability).
- Section 5 (Political shape) names which teams change workflow, whose roadmap is affected, and — for MLOps-vendor-class decisions — the peer platform team's disposition.
- Section 6 (Exit contract) has all five sub-clauses (6a-6e) with specific, numeric, or dated commitments. No sub-clause has "to be negotiated" without a `NEGOTIATE` marker and an appendix note.
- Section 7 (Routing-layer) has a paragraph committing to the interface-stability + cap-enforcement + renewal-cadence pattern for LLM-API-class decisions, or a one-sentence non-applicability note for other classes.
- Section 8 (Signatory block) names Staff, EM, Finance, and Legal signatories (or placeholders) and includes the material-commitment sign-off clause.
- Section 9 audits the doc against chapter 6's five failure modes and names any mitigations.
- The exit contract's reversibility framing is consistent with Section 2's reversibility class.
- If the vendor decision requires exec sign-off, the reversibility class in this doc matches the reversibility class in exercise-02's one-pager.

## Stretch goals

- **Take the exit contract to the peer platform team lead (or a peer role-play).** For MLOps-vendor-class decisions especially: the peer platform team is a signatory partner on the fallback commitment (Section 6d). Ask a peer to read Section 6 and write a one-paragraph disposition on whether they would counter-sign 6d as the fallback team. This is the exercise's highest-signal check on whether the exit contract is real.
- **Draft the vendor RFP (request for proposal) that produced this decision.** In one page: what did the ML org ask each vendor for, on what terms. Compare against the vendor's response and against the actual chapter-6 five-question framework. Which of the five questions the vendor's response *avoided* answering is calibration data for the next RFP.
- **Author the quarterly-renewal review template.** Chapter 6's *renew intentionally* pattern. In one page: what does the quarterly vendor-mix review look like — which metrics are checked, which questions from the five-question framework are re-answered, what triggers a re-decision. The template lives on the staff engineer's ongoing discipline, not on the one-time decision doc.
- **Compare against a public vendor-decision write-up.** If any of the chapter-6 vendor classes has a public write-up (a company's engineering blog on why they adopted or migrated off a specific vendor), read it against this exercise's rubric. Which of the five questions did the public write-up address well? Which did it under-defend? Which of chapter 6's five failure modes did it exhibit? The comparison is calibration data.
- **Draft the "vendor sunset" plan.** If the exit-contract trigger (Section 6e) fires next quarter, what is the actual migration plan — the sequence of engineering work, the interim state during migration, the deprecation communication to internal consumers? Two pages. This is the sub-document Section 6c gestures at; making it real surfaces the engineering-hours estimate you cannot hand-wave.
- **Cross-reference to the mod-409 risk register.** For vendor decisions with data-privacy, IP, or compliance implications: which mod-409 risk register entry does this vendor decision open or close? Note the linkage in the reflection.
- **Take the doc to a Finance-lead role-play.** Chapter 6 named Finance as an expanded signatory. Ask a peer to role-play the Finance lead and write a one-paragraph disposition on Section 4's TCO and Section 6b's contract-termination clause. What would Finance push back on? What information would they ask for? This is a rehearsal for the actual multi-party sign-off chapter 6 described.
