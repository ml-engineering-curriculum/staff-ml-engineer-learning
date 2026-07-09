# The Org-Standard Model Card and Datasheet-for-Datasets

## Motivation

A governance analyst reviewing launch packets across four ML teams will, absent an org standard, receive four different documents in four different shapes. Team A's model card is a Markdown file with headings borrowed from Hugging Face. Team B's is a Notion page with a fairness section. Team C's is a slide deck. Team D refers you to a Confluence page that has not been updated since the pre-training run. The analyst cannot review four different shapes fairly; they end up defaulting to whichever team writes the tightest document, which is not the same team as the one that ships the highest-risk system.

The **org-standard model card** and the **org-standard datasheet for datasets** are the two document shapes every team's launch packet uses. They are grounded in Mitchell et al. 2019 (*Model Cards for Model Reporting*, FAT\* / FAccT) and Gebru et al. 2021 (*Datasheets for Datasets*, CACM). This chapter walks each shape, the org-specific extensions the pre-launch gate depends on, and — importantly — what to leave out so the templates stay authorable in a week rather than a quarter.

## What the model card is

The **model card**, following Mitchell et al. 2019, is a short structured document — a page, sometimes two — that describes a trained model to the human deciding whether to use it, deploy it, or ship a change to it. The Mitchell et al. shape has sections along the lines of *Model Details, Intended Use, Factors, Metrics, Evaluation Data, Training Data, Quantitative Analyses, Ethical Considerations, Caveats and Recommendations*.

The org-standard model card in this module extends the Mitchell et al. shape with three additions the portfolio gate depends on:

1. **A pointer to the register row.** Every card names the risk-register row and the EU AI Act classification-table row it corresponds to. A card that cannot name its register row is a card the register does not know about — a gate defect.
2. **A pointer to the datasheet.** Every card names the datasheets for its training, validation, and (where distinct) evaluation datasets. A card whose datasheets do not exist is a gate defect.
3. **A signoff footer.** Product owner, ML tech lead, and — for launch — the governance analyst's signoff line with a date. Chapter 5 walks the routing.

The card is a *decision-support* document, not a research paper. If the ethical-considerations section is longer than the intended-use section, the card has drifted into essay writing and the reviewer will not read it. If the quantitative-analyses section is a link to an eval dashboard with the last three quarters' numbers, the card is doing its job.

## What the datasheet for datasets is

The **datasheet**, following Gebru et al. 2021, is a short structured document that describes a dataset to the humans consuming it — for training, for evaluation, or for a future audit. The Gebru et al. structure walks a set of prompted questions across categories along the lines of *Motivation, Composition, Collection Process, Preprocessing/Cleaning/Labeling, Uses, Distribution, Maintenance*.

The org-standard datasheet in this module keeps the Gebru et al. shape and extends it with:

1. **A licensing and provenance section.** Where the data came from, under what licence, with what attribution. Where synthetic or model-generated data was mixed in, and how much. This is the section EU AI Act Article-10-adjacent obligations lean on and the section a regulator's data-provenance question lands on. It is also the section that catches the *hidden dataset* — the one someone downloaded from a repo three years ago and no one currently on the team can name the licence for.
2. **A PII and sensitive-attribute section.** What personal or sensitive information is in the dataset, how it is protected, what the retention policy is, how a data-subject-deletion request is honoured. This links to the org's privacy-review artifact and does not replace it.
3. **A pointer back to model cards.** Every model card that uses the dataset should be reachable from the datasheet — bidirectional linking. If a dataset is used by two systems in the portfolio, the datasheet's *Uses* section names both, and a change to the dataset triggers a review of both cards.

## Grounding claims in the canonical references

Both formats are grounded in canonical peer-reviewed references. Mitchell, Margaret et al., "Model Cards for Model Reporting", published at the FAT\* / FAccT conference in 2019, is the origin document; the paper's Section 3 is the section-by-section template every downstream implementation (Hugging Face's `README.md` convention, Google Cloud's Model Cards for AI, Anthropic's system cards, Meta's system cards) traces back to. Gebru, Timnit et al., "Datasheets for Datasets", published in Communications of the ACM in December 2021, is the origin document for the datasheet shape; the paper's Appendix contains the full question set the org-standard datasheet condenses.

The org standard is not a re-invention of the two papers. It is a *conservative extension* — the Mitchell et al. and Gebru et al. shapes verbatim, plus three portfolio-specific fields on each. This matters for two reasons. First, external reviewers (auditors, potential customers, regulators) recognise the canonical shapes; a card that reads like a Mitchell et al. card is legible to them without translation. Second, keeping the extensions small keeps the templates authorable; a template that has grown to eighteen sections is one that a team lead will delegate to their junior engineer to fill in, which produces the fill-in-the-blank artifact the gate is trying to avoid.

## What Staff owns vs. what the model author owns

The templates are org artifacts. The instances are per-model artifacts. The clean split:

**Staff ML engineer owns.**

- The **templates** themselves, their versioning, their evolution. Introducing a new required field is a portfolio-scope decision that goes through the same review body as the gate (chapter 5).
- The **conformance check** that every launched system has a card and datasheet against the current template version. This is a pre-launch-gate check.
- The **template-training** of new ML tech leads — the walkthrough that the template is authorable in a week if the eval evidence is already sitting in the eval report.
- The **cross-linking discipline** — register row ↔ classification-table row ↔ model card ↔ datasheet. Broken links are a Staff-owned defect.

**Model author (Senior tech lead / model-team engineer) owns.**

- The **instances** — the actual model card and datasheet for their model. The content, the eval numbers, the intended-use description, the caveats.
- **Keeping the card current** — a re-trained model has a re-authored card, not a hand-waved "same as last quarter".
- **Escalating template ambiguity** — if a section on the template does not fit their model, they escalate to the Staff engineer for a template clarification rather than filling it in with a shrug.

**Governance analyst owns.**

- **Reviewing** the card and datasheet as part of the launch gate. This is the point of the standard; the analyst reads across four teams' packets in the same shape.
- **The transparency-obligation determination** — for a limited-risk EU AI Act system, whether the card's user-facing summary meets the transparency notice standard.

## The temptation to over-specify

Every Staff engineer who has read the Mitchell et al. paper wants to add sections. Robustness. Calibration. Distribution shift. Environmental impact. Adversarial-eval results. Failure modes at deployment. Every one is defensible. Every one is *also* another section a launching team has to fill in, and each additional section grows the template's completion-time by more than its length — a five-section template is filled in a day, an eighteen-section template becomes a quarter-long project no team lead volunteers for.

Two disciplines defend against over-specification.

**Add sections only when they are load-bearing for the gate.** A section that the pre-launch-review gate would not block on is a section the org can live without. A section that only some models need can be a *conditional* — "if the system is EU-AI-Act high-risk, complete section X; otherwise write N/A and state why". Conditionals are cheap; unconditional new sections are expensive.

**Keep the template versioned and deprecate loudly.** When a section is added or removed, the template version bumps and the change is announced. Systems launched under the old version are grandfathered until their next re-train; systems launching after the bump must adopt the new version. A template that grows silently is one the org will diverge on within a quarter.

## Cross-linking as a first-class artifact

The four documents — register row, classification-table row, model card, datasheet — should be *linked bidirectionally*. Concretely:

- The **register row** points at the classification-table row, the model card path, and each datasheet path.
- The **classification-table row** points at the register row, the model card path, and the legal-review artifact (if any).
- The **model card** points at the register row, the classification-table row, and each datasheet used.
- The **datasheet** points at each model card that consumes it and — for shared datasets across the portfolio, the mod-402 concept — at every model card in the portfolio that uses it.

Cross-linking is the property that makes the packet *reviewable* in one sitting. A governance analyst opening the model card should be one click from every artifact the launch depends on. A broken link is chapter 5's gate defect and is one of the two most common failure modes exercise-04 catches (the other being the stale row-owner from chapter 2).

## Summary

- The **org-standard model card** and **org-standard datasheet** are the two document shapes every team's launch packet uses. Their shapes are grounded in Mitchell et al. 2019 (model cards) and Gebru et al. 2021 (datasheets); the org-standard adds three portfolio-specific extensions on each and no more.
- **Staff owns** the templates and the cross-linking; the **model author owns** the instance content; the **governance analyst reviews** in one shape across teams. That split is what makes multi-team review fair.
- **Over-specifying** the template is the main failure mode of an ambitious Staff engineer at this chapter. Add a section only when the gate would block on it, and version deprecations loudly.
- **Cross-linking** the register row ↔ classification-table row ↔ model card ↔ datasheet is a first-class artifact discipline. A broken link is a gate defect.

The next chapter walks the pre-launch review gate itself — the required artifacts, the signoff routing, and the block-vs-flag rule.

Exercise-03 lands this chapter: adapt the Mitchell et al. and Gebru et al. shapes to a two-page org-standard template each, apply them to one system in your portfolio, and have a peer reviewer walk them cold.
