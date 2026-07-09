# The Portfolio AI Risk Register — NIST AI RMF as the Column Set

## Motivation

The first question a governance analyst, an incoming head of AI, or a Fortune-500 auditor asks about an ML portfolio is a version of *"what are your ML systems, and what could go wrong with them?"*. If the answer is a slack thread, a Confluence page from 2023, a spreadsheet one engineer maintains, and the org's collective memory, the questioner has already learned something bad about the org — and the Staff ML engineer's next quarter will be spent reconstructing the artifact they should have had from the beginning.

The **portfolio AI risk register** is that artifact. This chapter walks its column set — grounded in NIST AI RMF 1.0's four core functions and the Gen-AI Profile overlay — and the discipline that keeps the register a *living* document rather than a compliance snapshot that is stale ninety days after it is written.

## What the risk register is

The **portfolio AI risk register** in this module is:

> A single document — usually a table plus per-row supporting notes — that lists every ML system in the portfolio, columns aligned to NIST AI RMF (Govern, Map, Measure, Manage) and the Gen-AI Profile overlay for generative systems, refreshed on a written cadence, owned by the Staff ML engineer and reviewed by the governance analyst.

Three properties are load-bearing.

**One row per system.** Not one row per model version. Not one row per incident. Not one row per risk. *One row per production ML system* — the same system granularity the mod-402 portfolio blueprint uses. If the register has forty rows on five systems, it has drifted into a defect tracker; if it has one row that says "our ML systems", it has drifted into a marketing document. Five to fifteen rows is the healthy shape.

**Columns aligned to a public framework.** In this module the framework is the NIST AI Risk Management Framework 1.0 (NIST AI 100-1, January 2023), whose four core functions — Govern, Map, Measure, Manage — are the column groups. Aligning to a public framework is what makes the register portable across leaders, auditable by outsiders, and defensible in a regulatory conversation. A register whose columns are the Staff engineer's own invention will not survive their next role change.

**A cadence, not a snapshot.** The register has a "last reviewed" date per row and a "next review by" date per row. If those dates are the same across every row, the register has drifted into a snapshot; if the next-review dates are all in the past, the register has drifted into decoration. Chapter 5's gate blocks on cadence freshness.

## NIST AI RMF 1.0 as the column set

NIST AI RMF 1.0 organises AI risk management around four functions. This module treats them as column groups on the register.

**Govern.** The organisational-side controls that make the other three functions possible: policy applicability to this system, accountable owner, escalation path, review cadence, evidence of documented decisions. In the register, the Govern columns answer *"who is on the hook for this system, and under what policy?"*.

**Map.** The context and characterisation of the system: intended purpose, populations affected, upstream data sources, downstream consumers, adjacent systems in the portfolio, business objective, deployment environment. In the register, the Map columns answer *"what is this system doing, for whom, in what setting?"*. This is where the mod-402 blueprint's per-system paragraph flows in.

**Measure.** The evaluation posture: what has been measured (accuracy on which populations, fairness against which attributes, robustness against which perturbations, drift on which signals), when, with what result, and what is scheduled next. Cite the eval artifact. In the register, the Measure columns answer *"what do we actually know about how this system behaves?"*.

**Manage.** The active-response posture: known risks that are being mitigated (with the mitigation named), residual risks that are accepted (with the acceptance signed), monitoring in production, incident history relevant to this system, and the mod-408 SLO / incident linkage. In the register, the Manage columns answer *"what are we doing about the risks we know about, and what are we accepting as residual?"*.

The four groups are not a checklist. They are a way to make sure the row's contents cover every altitude of risk management — organisational, contextual, evaluative, operational — rather than the one the author happens to think about most. NIST AI 100-1 is the primary reference; the RMF Playbook (a companion resource NIST publishes alongside) is a useful expansion but not required reading before this chapter.

## The Gen-AI Profile overlay

Generative AI systems — LLM-backed features, image generators, embedding-based retrieval with generative post-processing, agent systems — carry risks that supervised classifiers do not. NIST published the **Generative AI Profile** (NIST AI 600-1, July 2024) as an overlay on the RMF's four functions, cataloguing risk categories specific to gen-AI. This module treats the Gen-AI Profile as an *additional column group* that applies to gen-AI systems and is empty for classical-ML systems.

The overlay column group covers, at minimum:

- **Confabulation / hallucination risk.** What the system produces that is not grounded in its inputs; how bounded; what evaluation runs against it.
- **Harmful-content risk.** What the org considers harmful output for this system; the filter or refusal layer; the residual leakage rate.
- **Data-provenance risk.** What is in the training data (or in the fine-tune data, or in the retrieval corpus). Includes copyrighted material, PII, and unlicensed content.
- **Prompt-injection and jailbreak risk.** Whether the system takes attacker-controllable text as input; the mitigations; the red-team evidence.
- **Human-AI configuration risk.** How the system's output is presented to humans; how automation bias is countered; how the human oversight from EU AI Act Article 14 (chapter 3) is actually implemented.

<!-- needs-research: NIST AI 600-1 enumerates a specific set of gen-AI risk categories with formal names (roughly 12-13 categories in the published profile); the list above is a working subset. Verify the exact enumeration against the published NIST AI 600-1 document and update chapter 2 and exercise-01 with the canonical category names before shipping. -->

A row on a classical-ML system (a fraud model, a ranking model) has the Gen-AI Profile column group empty and states that in one line. A row on a gen-AI system has it filled in. The register's cover paragraph names which columns apply where so a reviewer does not read the absence of a value as a gap.

## What Staff owns vs. what the analyst owns on the register

The register is a shared artifact and its ownership is the most common misfire in the module. The clean split:

**Staff ML engineer owns.**

- Register **existence, structure, and cadence**. The template. The column set. The refresh calendar.
- The **Map and Measure columns** — the ML-engineering evidence. Per-system descriptions, eval results, monitoring signals, incident linkage.
- **Row freshness** — chasing team owners for updates when a row is stale; escalating to the analyst when a team is not responding.
- **The pointer** from the register to the underlying evidence — the eval reports, the model cards, the dashboards. If a register row cites an eval result and the eval report has been moved, it is the Staff engineer's job to keep the pointer alive.

**Governance analyst owns.**

- The **Govern columns** — policy applicability, accountable owner, escalation path. These are policy statements and belong to the analyst.
- **Sign-off** on the register as a whole on the documented cadence — typically quarterly.
- The **Manage-column policy** — what qualifies as an accepted residual risk vs. a risk that must be mitigated. The Staff engineer authors the individual Manage entries; the analyst authors the acceptance policy those entries are measured against.
- **External-audience versions** of the register — the regulator-facing summary, the board-facing brief. The Staff engineer supplies the underlying data.

The split matters because the promotion packet and the audit trail both depend on it. If the Staff engineer is signing "policy applies to this system", they are doing the analyst's job and their promotion packet will not be able to show it. If the analyst is filling in the eval-result cells, they are doing the Staff engineer's job and the analyst's function has been misused. Chapter 5's gate depends on the split being clean.

## What a healthy row looks like

A row on the register is a paragraph plus a small table, not a novella. For a hypothetical fraud model:

- **System.** `fraud-risk-scoring-v3` — real-time transaction risk score.
- **Govern.** Applies under org policy AI-2 (medium-risk internal-decision-support). Accountable owner: fraud team lead. Escalation to head of risk. Next review 2026-Q4.
- **Map.** Scores every merchant-checkout transaction; input features include transaction attributes and a customer-graph embedding shared with three other systems (see mod-402 blueprint). Populations affected: all customers globally, disproportionately impacts high-volume small merchants.
- **Measure.** Precision-at-review-threshold measured monthly on a held-out slice; last result 2026-06-01 at 0.83 precision. Fairness eval against merchant-size decile last run 2026-05-15; drift across deciles within 3 percentage points. Robustness eval against adversarial transaction sequences not yet run — scheduled 2026-Q3 with security partner.
- **Manage.** Mitigation for false-decline risk: a human-review queue with a 2-hour SLA for flagged transactions. Residual risk of missed fraud on unseen patterns accepted by the fraud committee 2026-04-10. Two Sev-2 incidents in the last 90 days, both linked to the mod-408 incident register.
- **Gen-AI Profile.** N/A — classical model.

A row on an LLM-backed feature would have the same shape with the Gen-AI Profile group filled in and typically longer entries under Measure and Manage.

## Cadence and the drift problem

A register goes stale in three ways: the row-owner leaves the org, the system's eval posture drifts without the row being updated, or the register itself is not on anyone's calendar. Three practices fix the three failure modes.

**Row-owner rotation is a launch-gate criterion.** When a team lead changes, the register row's row-owner update is one of the launch-adjacent tasks the mod-408 on-call handoff and the mod-410 org-change process trigger. A stale row-owner is a register defect the gate rejects.

**The register cites live eval artifacts, not copied numbers.** The Measure entry links to the eval dashboard or eval report path. When the dashboard shows a number the row does not, the row is stale and the review process catches it. Copied numbers in the register drift silently.

**The register has a standing calendar.** Quarterly cross-portfolio review with the governance analyst is the default; systems classified as high-risk (chapter 3) escalate to monthly. Systems in a launch quarter are refreshed at launch and again at the 30-day post-launch checkpoint. The calendar is chapter 5's gate cadence.

## Summary

- The **portfolio AI risk register** is one document, one row per production ML system, columns aligned to NIST AI RMF 1.0's four functions (Govern, Map, Measure, Manage) plus the Gen-AI Profile overlay for generative systems.
- **Staff ML engineer owns** structure, cadence, and the Map/Measure columns; the **governance analyst owns** the Govern columns, sign-off, and external-audience versions. Chapter 5's gate depends on the split being clean.
- A **healthy row** has a paragraph per column group with live pointers to eval artifacts and mitigations; a healthy register has a standing calendar and rejects stale row-owners as defects.
- The **Gen-AI Profile** column group is empty for classical-ML systems and filled in for gen-AI systems, with the cover paragraph naming which is which so absence is not read as a gap.

The next chapter walks the second per-system artifact — the EU AI Act engineering classification — and how it composes with the register.

Exercise-01 lands this chapter: pick a real three-to-five-system portfolio, draft one row per system to the NIST AI RMF column set, and take the draft to a peer governance-adjacent reviewer.
