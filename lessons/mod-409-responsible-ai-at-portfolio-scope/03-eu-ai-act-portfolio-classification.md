# EU AI Act Classification at Portfolio Scope

## Motivation

The EU AI Act (Regulation (EU) 2024/1689) is the first horizontal AI regulation in a major jurisdiction, and its risk-tiered structure — unacceptable, high-risk, limited-risk, minimal-risk — is now the reference frame every other major jurisdiction is being compared to. Any ML portfolio that touches EU users, EU data subjects, or an EU-affiliated procurement process needs a written engineering read of where each system sits, even if the org has no lawyers on staff who specialise in the Act. The **portfolio classification table** is that read.

This chapter walks the tier definitions, names the obligation surface each tier implies, sketches how the extraterritorial reach applies to a non-EU-headquartered org, and — critically — draws the line between the Staff engineer's *engineering classification* and the legal team's *binding classification*. A Staff engineer who confuses those two is offering a legal opinion they are not qualified to give; a Staff engineer who refuses to make the engineering read is leaving the org unable to plan compliance work.

## What the classification table is

The **EU AI Act portfolio classification table** in this module is:

> A table with one row per production ML system in the portfolio, columns for the engineering-read tier (unacceptable / high-risk / limited-risk / minimal-risk), the obligation classes that flow from that tier, the extraterritorial-reach argument if the org is not EU-headquartered, and the legal-review escalation status.

The classification is an *engineering read*, not a legal opinion. The Staff engineer's read is what lets the org plan the compliance work. The binding classification — the one filed if a regulator asks — is a legal-team artifact that consumes the engineering read as input. Every row in the table names its legal-review status: `unreviewed`, `in-review`, `legal-confirmed`, `legal-disputed`.

## The four risk tiers

The Act organises AI systems into four tiers with progressively lighter obligations. This module's short forms follow the Act's structure; refer to Regulation (EU) 2024/1689 in the Official Journal for the binding definitions.

**Unacceptable-risk systems.** Systems the Act prohibits — practices deemed to pose an unacceptable risk to fundamental rights. Examples in the Act include specific forms of social scoring by public authorities and certain manipulative or exploitative systems. If the engineering read on any portfolio system lands here, the row's escalation is *immediate legal review*, and the launch gate (chapter 5) blocks unconditionally until legal signs off. In practice a well-run enterprise ML portfolio should have zero rows in this tier; a row that lands here means the org is planning something the Act likely does not allow.

<!-- needs-research: the Act's Article 5 enumerates the specific prohibited-practice categories; the exact list has been amended through the legislative process. Verify the current enumeration against the published Official Journal text (Regulation (EU) 2024/1689) and update the enumeration here and in exercise-02 before shipping. -->

**High-risk systems.** Systems that operate in domains the Act designates as high-risk — including specific uses in employment, education, essential services, law enforcement, migration, and certain product-safety-embedded uses. High-risk systems carry the heaviest obligation surface: documented risk management, data-governance controls, transparency to users, human oversight, accuracy / robustness / cybersecurity requirements, and conformity assessment. A row that lands here triggers the multi-artifact obligation set catalogued below.

**Limited-risk systems.** Systems with a specific transparency obligation — the user has to know they are interacting with an AI system, or that content they are seeing is AI-generated. Chatbots, deepfake-adjacent generators, and emotion-recognition-adjacent systems typically land here. Obligations are lighter than high-risk but non-trivial: the transparency notice must actually reach the user.

**Minimal-risk systems.** Everything else. No mandatory obligations beyond existing horizontal law (data protection, consumer protection, non-discrimination). The Act encourages voluntary codes of conduct at this tier but does not require them. A recommender or a search-ranking system for non-sensitive content typically lands here.

The tiers are cumulative in the sense that a high-risk system does not escape the transparency obligations of the limited-risk tier — it inherits them and adds the high-risk obligations on top.

## The obligation surface for a high-risk system

Chapter 5's gate spends most of its energy on high-risk systems, because that is where the obligations that flow from the Act become launch-blocking. The obligation classes, at the level of column labels for the classification table:

- **Risk management.** A documented, iterative risk-management process across the system's lifecycle. This is where the portfolio risk register (chapter 2) does most of the load-bearing work.
- **Data and data governance.** Training, validation, and test data are governed for relevance, representativeness, and where possible free of errors. This is where the datasheet-for-datasets (chapter 4) is a required artifact rather than an optional one.
- **Technical documentation and record-keeping.** The system's operation is documented in enough detail that a supervisory authority can assess compliance. Automated logging of relevant events. The model card (chapter 4) is a core piece of this.
- **Transparency and provision of information to users.** The deploying user of the system understands its intended purpose, its performance characteristics, and its limitations.
- **Human oversight.** The system is designed to be effectively overseen by humans — the oversight interfaces, the escalation paths, the ability to intervene. This is often the obligation that reshapes the product design most.
- **Accuracy, robustness, and cybersecurity.** The system meets appropriate accuracy targets, is robust to errors and inconsistencies, and is protected against attempts to alter its use, behaviour, or output.

<!-- needs-research: the specific article numbers for the six obligation classes above (Articles 9, 10, 11-12, 13, 14, 15 are the commonly cited mapping) should be verified against the final Official Journal text of Regulation (EU) 2024/1689 before naming them by number in student-facing content. This chapter deliberately cites obligation *classes* by description rather than by article number for that reason; add the article numbers once verified. -->

Each of the six obligation classes maps to specific artifacts the launch gate requires and specific evaluations the register cites. Chapter 5 walks the mapping in gate-charter form.

## Extraterritorial reach

The Act reaches beyond the EU's borders in a way analogous to the GDPR. A non-EU-headquartered organisation is in scope when its AI system is placed on the EU market, when the output produced by the system is used in the EU, or when the system otherwise falls within the enumerated cases in the Act. In practice, for a portfolio review this means:

- **A system serving EU users at any material volume is in scope.** The "material volume" line is not the engineer's to draw; the legal team's guidance sits on top. But the engineering read errs toward *in-scope* rather than *out-of-scope* when there is any EU-user traffic on the system.
- **A system whose output feeds an EU-based downstream user of the model** (a B2B customer, a downstream product surface serving EU users) is typically in scope even if the system itself never sees an EU-user request directly.
- **A system used entirely for internal-to-the-org purposes on non-EU staff** is typically out of scope, but the row still records the reasoning so the classification survives an internal-audit question.

Each row in the classification table records the extraterritorial-reach argument in one line, even for out-of-scope rows. An unrecorded argument is the shape of a defect the next auditor will find.

## Staff owns vs. legal owns

The clean split, again in one sentence per role:

**Staff ML engineer owns.**

- The **engineering-read tier** per system — the best-informed guess from someone who understands what the system actually does.
- The **artifact obligation map** — for each row's tier, the list of artifacts (risk-register row, model card, datasheet, human-oversight design, logging spec) that the gate will require.
- The **escalation** of any row where the engineering read is unclear or contested, to the legal team through the documented escalation path.
- **Version discipline on the table** — the same cadence as the register in chapter 2, refreshed when a system's purpose, deployment surface, or user population changes.

**Legal team owns.**

- The **binding classification** — the tier the org will assert to a regulator.
- **Regulator-facing filings** and any conformity-assessment interaction under the Act.
- The **interpretation of ambiguous clauses** — the tier boundaries have boundary cases and the legal team makes the call, not the engineer.
- **Waiver and derogation decisions** — where the Act allows a controlled deviation, the legal team decides whether to invoke it.

The escalation from Staff to legal is a documented process, not a hallway conversation. Exercise-02 lands the process.

## How the classification table composes with the register

The classification table and the risk register (chapter 2) are two views of the same portfolio. The register has a "tier" column that references the classification table; the classification table has a "risk-register-row" column that references the register. A change on either side triggers a review of the other — the two artifacts are locked at the row level.

A worked example. If a fraud model's engineering-read tier is `high-risk` because the deployment surface serves EU users and the decision materially affects access to a financial service, the register row's Manage column names the human-review queue as the human-oversight implementation, the Measure column names the accuracy-and-robustness eval as the Article-15-adjacent evidence, and the Govern column names the head-of-risk as the accountable owner for the Act's obligation surface. Chapter 5's gate reads across both artifacts as one packet.

## Summary

- The **EU AI Act portfolio classification table** is a one-row-per-system engineering read of tier (unacceptable / high-risk / limited-risk / minimal-risk), obligation surface, extraterritorial-reach argument, and legal-review status.
- **Staff owns** the engineering read and the artifact obligation map; **legal owns** the binding classification and regulator-facing correspondence. Escalation from Staff to legal is documented, not conversational.
- **High-risk systems** carry an obligation surface — risk management, data governance, technical documentation, transparency, human oversight, accuracy / robustness / cybersecurity — that the gate spends most of its energy enforcing.
- **Extraterritorial reach** puts most portfolios with EU user traffic in scope; the engineering read errs toward *in-scope* and records the argument even for out-of-scope rows.
- The classification table and the register (chapter 2) compose. A change on one triggers a review of the other; the launch gate reads both as one packet.

The next chapter walks the two document shapes the gate requires per system — the org-standard model card and the datasheet for datasets.

Exercise-02 lands this chapter: classify each system in your portfolio to the four tiers, note the obligation surface for the high-risk rows, and document the escalation to legal for any contested rows.
