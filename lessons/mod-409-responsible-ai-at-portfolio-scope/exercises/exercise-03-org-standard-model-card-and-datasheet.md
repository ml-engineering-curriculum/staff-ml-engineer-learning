# exercise-03: Org-Standard Model Card and Datasheet-for-Datasets

**Estimated effort:** 3 hours

## Objective

Adapt the Mitchell et al. 2019 (*Model Cards for Model Reporting*) and Gebru et al. 2021 (*Datasheets for Datasets*) shapes into **two-page org-standard templates**, then apply the templates to one system in the exercise-01 / 02 portfolio to produce a fully-populated model card and a fully-populated datasheet. The deliverable is four artifacts in one document: the blank model-card template, the blank datasheet template, the populated model card for the chosen system, and the populated datasheet for that system's primary training dataset.

Chapter 4 was explicit that the templates are org artifacts and the instances are per-model artifacts; the Staff engineer owns the shapes and the cross-linking discipline, and the model author owns the instance content. This exercise rehearses both roles — you author the templates (the Staff move) and populate them (the model-author move) so you know first-hand where the templates leak, where the per-model detail overflows, and where the cross-links break. Exercise-04's gate blocks on the presence and freshness of both artifacts against the current template version.

## Prerequisites

- Exercise-01's register for the portfolio. The model card cross-links to a specific register row; the datasheet cross-links back to the model card and forward into the datasheet's *Uses* section.
- Exercise-02's classification table for the same portfolio. The classification tier drives which optional model-card sections are required (a high-risk row's card carries the human-oversight description and the accuracy / robustness section as required, not optional).
- Chapter 04 — the Mitchell et al. shape, the Gebru et al. shape, the three portfolio-specific extensions to each, the Staff-owns-templates vs. model-author-owns-instance split, the over-specification failure mode, and the cross-linking discipline. Read in full.
- Mitchell, Margaret et al., *Model Cards for Model Reporting*, FAT\* / FAccT 2019. Section 3 (the section-by-section template) is required reading; if you have not read the paper, do so before drafting the template.
- Gebru, Timnit et al., *Datasheets for Datasets*, Communications of the ACM, December 2021. The Appendix (the full prompted-question set) is required reading; the org-standard datasheet is a condensation of that Appendix, not a re-invention.
- Recommended: skim the [Hugging Face Model Card Guidebook](https://huggingface.co/docs/hub/model-cards) and one published foundation-model system card (Anthropic's Claude model page, OpenAI's GPT-4 System Card, or Meta's Llama system card) to triangulate what a disciplined external example of each shape looks like.

## Choose the system

Pick **one** system from the exercise-01 portfolio for the populated card and datasheet. Priority order:

1. **A high-risk row from exercise-02.** The high-risk tier is where the model card and datasheet carry the heaviest obligation surface and where the template's design choices are most exercised. Chapter 4's over-specification temptation is loudest here; land a high-risk instance to see which sections earn their place.
2. **A gen-AI row (of any tier).** Gen-AI systems stress the datasheet's licensing and PII sections and stress the model card's intended-use bounds; both are useful pressure tests.
3. **Only if neither of the above is in the portfolio:** any minimal-risk classical model where you have first-hand access to eval numbers and training-data provenance. Do not fabricate numbers.

Pick a system whose eval evidence you can cite honestly. The populated card is what teaches the tone of every card that follows; a card populated with plausible-sounding but invented numbers teaches the wrong tone.

## Steps

1. **Draft the blank model-card template.** Two pages max. Follow the Mitchell et al. Section 3 shape (Model Details, Intended Use, Factors, Metrics, Evaluation Data, Training Data, Quantitative Analyses, Ethical Considerations, Caveats and Recommendations). For each section, include:
   - **The section title** in the Mitchell et al. wording.
   - **One-sentence purpose.** What this section is for. The prompt the card author reads first.
   - **Prompt questions.** Two to five questions per section in the vocabulary of the paper. Do not paraphrase to invisibility — a template whose prompts drift from the paper's is a template that will not be recognised by an external reviewer.
   - **Length guidance.** A short line on how long each section should be. Model Details and Intended Use are read-in-30-seconds; Quantitative Analyses is a link to the eval dashboard with the last three quarters' numbers, not a re-typed table; Ethical Considerations is bounded (a two-paragraph limit, chapter 4's essay-writing failure mode).
2. **Add the three portfolio-specific model-card extensions.** Chapter 4's three additions to the Mitchell et al. shape:
   - **Register-row pointer.** A required field at the top of the card naming the exercise-01 register-row identifier and the exercise-02 classification-table-row identifier for this system.
   - **Datasheet pointer(s).** A required field naming the datasheets for training, validation, and (where distinct) evaluation datasets. A card whose datasheets do not exist is a gate defect.
   - **Signoff footer.** Product owner, ML tech lead, governance analyst — role, name (populated at signoff), and date. Chapter 5's gate signoff routing lands here.
3. **Draft the blank datasheet-for-datasets template.** Two pages max. Follow the Gebru et al. Appendix shape (Motivation, Composition, Collection Process, Preprocessing / Cleaning / Labeling, Uses, Distribution, Maintenance). For each section:
   - Section title, one-sentence purpose, prompt questions (two to five, drawn from the Appendix), length guidance.
4. **Add the three portfolio-specific datasheet extensions.** Chapter 4's three additions to the Gebru et al. shape:
   - **Licensing and provenance section.** Where the data came from, under what licence, with what attribution. Where synthetic or model-generated data was mixed in, and how much. Include a prompt for the *hidden dataset* case — the dataset with no traceable licence — with the required next action (`escalate to legal within 5 business days`).
   - **PII and sensitive-attribute section.** What personal or sensitive information is in the dataset, how it is protected, what the retention policy is, how a data-subject-deletion request is honoured. Cross-references the org's privacy-review artifact by name; does not replace it.
   - **Model-card back-pointers.** A required field naming every model card that consumes this dataset — bidirectional link with the model card's datasheet pointer.
5. **Populate the model card for the chosen system.** All Mitchell et al. sections and all three extensions. Length guidance holds — the card is a page, sometimes two, not four. Intended-use bounds are specific (what the model is for, what it is not for, and which classes of use are explicitly prohibited). Quantitative Analyses is a link to the eval dashboard with the last three quarters' numbers if that dashboard exists in your portfolio; otherwise cite the eval report path. The signoff footer is present but empty of dates — this is a *pre-launch* card, awaiting the exercise-04 gate signoffs.
6. **Populate the datasheet for the chosen system's primary training dataset.** All Gebru et al. sections and all three extensions. The Composition section names the dataset's rows, features, split sizes, label distribution, and any known biases. The Collection Process section names where the data came from (event stream, third-party feed, scraped source) and the collection window. The Licensing section is specific about origin, licence, attribution requirements, and any synthetic-mix note. The PII section is specific about what personal data is present and what protection applies. The Model-card back-pointers name the model card from step 5 and any other portfolio cards that consume the same dataset.
7. **Draft the cross-linking check.** A quarter-page block. Chapter 4 was explicit that cross-linking is a first-class artifact discipline. For the system you populated, list every artifact and check the bidirectional links:
   - Register row ↔ classification-table row.
   - Register row ↔ model card path.
   - Register row ↔ datasheet path.
   - Classification-table row ↔ model card path.
   - Model card ↔ datasheet.
   - Datasheet ↔ every model card that uses it (for shared datasets, name each).
   Any link that would 404 or point at a not-yet-created artifact is a defect — note it and the target date to fix.
8. **Score the templates against a "governance-analyst cold read" test.** After drafting, put the doc down for at least an hour, then re-read the templates and the populated card and datasheet as if you were the analyst reviewing your first launch packet from this team. Can you tell what the model does, what the training data was, what evidence supports the eval claims, what the residual risks are, and what the signoff dependencies are — without asking a follow-up question? Mark the sections that fail the test and revise.
9. **Score the templates against the over-specification pressure test.** Re-read the two template drafts asking, for each section: would exercise-04's gate block a launch if this section were missing or thin? If the answer is no, mark the section as *optional* rather than required, or (for a section the gate would neither block nor flag on) delete it. Chapter 4 was explicit that over-specification is the failure mode of an ambitious Staff engineer here; ship the tightest template that still supports the gate.

## Deliverable

A single document (or four sibling documents in a folder), containing:

**Artifact 1 — Blank model-card template** (2 pages):

- All Mitchell et al. Section 3 sections with title, purpose, prompt questions, length guidance (step 1).
- The three portfolio-specific extensions inline: register-row pointer, datasheet pointer, signoff footer (step 2).
- Explicit note on which sections are always required and which are conditional on classification tier (from exercise-02).

**Artifact 2 — Blank datasheet template** (2 pages):

- All Gebru et al. Appendix sections condensed with title, purpose, prompt questions, length guidance (step 3).
- The three portfolio-specific extensions: licensing and provenance, PII and sensitive attribute, model-card back-pointers (step 4).

**Artifact 3 — Populated model card for the chosen system** (1-2 pages, following the template):

- All sections populated with real, defensible content for the chosen system (step 5).
- Cross-links to the register row, classification-table row, and datasheet(s) present and correct.
- Signoff footer present with role labels; dates empty (pre-launch, awaiting gate).

**Artifact 4 — Populated datasheet for the chosen dataset** (1-2 pages, following the template):

- All sections populated with real, defensible content (step 6).
- Licensing section honest — no `TBD` on a dataset the org actually uses in production. If the origin is unclear, mark `needs-research — legal escalation pending` and name the target date.
- Model-card back-pointers list every card that consumes this dataset.

**Appendix — Cross-linking check** (step 7) and cold-read audit (step 8).

## Starter guidance

- **The templates are extensions, not re-inventions.** Chapter 4 was explicit — the org-standard is the Mitchell et al. shape verbatim plus three portfolio-specific fields, and the Gebru et al. shape condensed plus three fields. If your template is longer than two pages before the extensions, you have re-invented and diverged; walk it back.
- **Two pages is the ceiling on each template, not the floor.** A tighter template is better than a padded one. Chapter 4's over-specification pressure test in step 9 exists because the temptation is real; a template with an eighteenth section is one team leads will delegate to their most junior engineer to fill in.
- **The populated card is what teams read the first time.** The blank template is what the review body approves; the populated card is what team leads open when they write their first card. The tone of the populated card sets the tone of every card that follows.
- **Cite eval dashboards, not typed numbers.** Chapter 2 said this for the register; it is doubly true for the card. A card with a `precision = 0.83` number typed inline is a card that will disagree with the dashboard within two weeks. A card with a link to the eval dashboard is a card that stays current.
- **Ethical Considerations is bounded.** Chapter 4 was explicit — if Ethical Considerations is longer than Intended Use, the card has drifted into essay writing and the reviewer will skim it. Two paragraphs max on residual risks accepted plus known limitations.
- **The datasheet's Licensing section is where honesty is hardest and most valuable.** The hidden-dataset case — the dataset with no traceable licence — is common in orgs that have been shipping ML for more than three years. Mark `needs-research — legal escalation pending` on any such dataset rather than fabricating a licence claim. The escalation itself is chapter 4's discipline; the mark blocks the gate on the missing evidence.
- **Cross-linking is a first-class artifact.** Every artifact points at every artifact it depends on and every artifact that depends on it. Chapter 4 was explicit — a broken link is chapter 5's gate defect. If any link in your step-7 check does not resolve, the launch does not go through the gate as-is.
- **Do not skip either the template or the population step.** Some learners will be tempted to write only the templates because "the population is per-model author's work". That misses the point; you cannot know where the template leaks without populating it. Some will be tempted to write only the population because "the template is decorative". That also misses the point; the template is the shape every future card conforms to.
- **The signoff footer is present but empty of dates on the populated card.** The card is a *pre-launch* card. The gate signoffs (exercise-04) are what populate the dates. A populated card with dates in the signoff footer is a card that has jumped the gate.

## Acceptance criteria

- Artifact 1 (blank model-card template) is 2 pages max and covers all Mitchell et al. Section 3 sections with title, purpose, prompt questions, and length guidance.
- Artifact 1 carries the three portfolio-specific extensions inline: register-row pointer, datasheet pointer, signoff footer.
- Artifact 2 (blank datasheet template) is 2 pages max and covers all Gebru et al. Appendix sections (Motivation, Composition, Collection Process, Preprocessing, Uses, Distribution, Maintenance).
- Artifact 2 carries the three portfolio-specific extensions: licensing and provenance section, PII and sensitive-attribute section, model-card back-pointers.
- Artifact 3 (populated model card) is 1-2 pages, populated for a real (or defensibly stipulated) system from the exercise-01 portfolio, following the template exactly.
- Artifact 3 cross-links to the correct register row and classification-table row from exercises 01 and 02.
- Artifact 3's Quantitative Analyses section links to an eval dashboard or eval-report path, not a re-typed number.
- Artifact 3's signoff footer is present with role labels; dates are empty (pre-launch).
- Artifact 4 (populated datasheet) is 1-2 pages, populated for the training dataset of the same chosen system.
- Artifact 4's Licensing section is honest — no `TBD` on a production dataset. `needs-research` with a target date is acceptable; silence is not.
- Artifact 4's Model-card back-pointers list every card that consumes this dataset.
- The cross-linking check in the appendix confirms bidirectional links across register row, classification-table row, model card, and datasheet(s); any broken link is noted with a target date to fix.
- The cold-read audit is present and names at least one section that was revised as a result.
- Neither template exceeds two pages before the portfolio-specific extensions.

## Stretch goals

- **Populate a second card and datasheet on a shared dataset.** Pick a second model in the portfolio that consumes the same dataset as the one you populated. Populate its card and update the datasheet's Uses / back-pointer section to name both cards. Chapter 4's shared-dataset case is where the cross-linking discipline pays off; populating two cards on one datasheet is the exercise that surfaces the discipline concretely.
- **Diff your template against Hugging Face's Model Card Guidebook.** Note where your section-by-section vocabulary matches and where it diverges. An outside anchor is what makes the template defensible when a new team lead pushes back on adopting it.
- **Populate the pre-signoff review checklist.** A one-page appendix — the checklist the ML tech lead walks before pre-signing the card to send it to the gate. Ten to fifteen checkboxes, each pointing back to a section in the template. This is what turns "does this pass the gate?" into an answerable question ahead of the meeting.
- **Draft the template-version deprecation note.** Half a page on how the org handles template versioning — when a section is added or removed, how models on the old version are grandfathered, and when they must adopt the new version. Chapter 4 was explicit that a template that grows silently is one the org diverges on within a quarter.
- **Take the templates and the populated instances to a peer for adversarial pre-review.** A Staff-plus peer or (ideally) a governance-analyst-adjacent reviewer. Ask them to name the section they would ask you to remove and the section they would ask you to add. Absorb the pushback and update the templates.
- **Draft the exec one-liner.** One sentence at the top: what the templates require, what they intentionally do not, when they take effect. "The org-standard model card and datasheet templates extend Mitchell et al. 2019 and Gebru et al. 2021 with three portfolio-specific fields each; effective Q3 for every new launch, with existing launches grandfathered to their next re-train." That sentence is what a director reads before opening the templates.
