# Resources for mod-409-responsible-ai-at-portfolio-scope

Real, citable references for the topics this module covers. Read the canonical frameworks and papers before the module; treat the company writeups as prior art to point at in a gate charter or a delegation contract; treat the adjacent-governance frameworks as material the analyst partner will bring into the conversation, so knowing the vocabulary keeps the partnership clean.

## NIST AI Risk Management Framework

- **NIST.** *AI Risk Management Framework 1.0.* NIST AI 100-1, January 2023. [Framework page](https://www.nist.gov/itl/ai-risk-management-framework); [PDF via NIST](https://nvlpubs.nist.gov/nistpubs/ai/NIST.AI.100-1.pdf). The primary reference for chapter 2's four core functions — Govern, Map, Measure, Manage — used as the register's column groups. The RMF Playbook page on the same site is a useful expansion.
- **NIST.** *Artificial Intelligence Risk Management Framework: Generative AI Profile.* NIST AI 600-1, July 2024. [Profile page](https://www.nist.gov/itl/ai-risk-management-framework/nist-ai-600-1-artificial-intelligence-risk-management-framework); PDF linked from the same page. The overlay for gen-AI systems chapter 2 layers on top of AI RMF 1.0. <!-- needs-research: the profile enumerates specific gen-AI risk categories with canonical names (roughly a dozen); verify the exact enumeration in the published document and update chapter 2 and exercise-01 to name them precisely. -->
- **NIST.** *Adversarial Machine Learning: A Taxonomy and Terminology of Attacks and Mitigations.* NIST AI 100-2 (E2023 and revisions). [Publication page](https://csrc.nist.gov/publications/detail/nistir/8269/draft) and successor documents at nist.gov. The classical-ML attack taxonomy chapter 6 aligns the red-team artifact schema against. <!-- needs-research: NIST has published multiple iterations of the adversarial-ML taxonomy under different report numbers (NISTIR 8269 draft, NIST AI 100-2 E2023, later editions); verify the current stable citation. -->

## EU AI Act

- **Regulation (EU) 2024/1689** of the European Parliament and of the Council on artificial intelligence (Artificial Intelligence Act). Published in the Official Journal of the European Union, 2024. Search [EUR-Lex](https://eur-lex.europa.eu/) for the canonical text. The binding text of the Act. <!-- needs-research: cite the specific EUR-Lex CELEX number and Official Journal reference once verified; ensure the reference matches the entered-into-force version rather than an earlier compromise text. -->
- **European Commission.** *AI Act* summary and staged-application pages at [digital-strategy.ec.europa.eu/en/policies/regulatory-framework-ai](https://digital-strategy.ec.europa.eu/en/policies/regulatory-framework-ai). The Commission's plain-language summary of the tier structure and obligation surface referenced throughout chapter 3.
- **European Commission.** *AI Act — Application Timeline* pages on the same site. Useful for the phased-application dates chapter 3 alludes to (prohibited practices, GPAI obligations, high-risk obligations each have their own application dates). <!-- needs-research: name the specific application dates once verified against the Official Journal text; the phased dates were subject to legislative revision through 2024. -->

## Model cards and datasheets — canonical papers

- **Mitchell, Margaret et al.** *Model Cards for Model Reporting.* Proceedings of the Conference on Fairness, Accountability, and Transparency (FAT\* / FAccT), 2019. [ACM Digital Library](https://dl.acm.org/doi/10.1145/3287560.3287596); [arXiv:1810.03993](https://arxiv.org/abs/1810.03993). The origin paper for chapter 4's model-card template. Section 3 is the section-by-section shape.
- **Gebru, Timnit et al.** *Datasheets for Datasets.* Communications of the ACM, Vol. 64, No. 12, December 2021. [ACM Digital Library](https://dl.acm.org/doi/10.1145/3458723); [arXiv:1803.09010](https://arxiv.org/abs/1803.09010). The origin paper for chapter 4's datasheet template. The Appendix carries the full prompted-question set.
- **Hugging Face.** [Model Card Guidebook](https://huggingface.co/docs/hub/model-cards). An operationalisation of the Mitchell et al. shape widely used in open-model releases; useful as an example of a template extension that stayed disciplined.
- **Google Cloud.** [Model Cards](https://modelcards.withgoogle.com/about) — worked examples on the same shape, published for illustrative Google models. Useful for triangulating the org-standard template.

## Fairness, bias, and evaluation

- **Barocas, Solon; Hardt, Moritz; Narayanan, Arvind.** *Fairness and Machine Learning: Limitations and Opportunities.* Free at [fairmlbook.org](https://fairmlbook.org/). The canonical textbook. Chapters on group-fairness definitions, causal fairness, and the impossibility results are the vocabulary the register's Measure column uses when it says "fairness eval".
- **Aequitas.** [Bias and Fairness Audit Toolkit](http://aequitas.dssg.io/) from the Center for Data Science and Public Policy at Carnegie Mellon University. An open-source tool for fairness audits at the granularity a register row cites.
- **Fairlearn.** [fairlearn.org](https://fairlearn.org/). Microsoft-originated open-source fairness-assessment and mitigation toolkit; the disaggregated-metric shape it emits is roughly what the register's Measure entries want.

## Red teaming, adversarial ML, and security

- **OWASP Foundation.** *OWASP Top 10 for Large Language Model Applications.* [owasp.org/www-project-top-10-for-large-language-model-applications](https://owasp.org/www-project-top-10-for-large-language-model-applications/). The shared vocabulary chapter 6 requires gen-AI red-team artifacts to cover.
- **NIST.** *Adversarial Machine Learning: A Taxonomy and Terminology of Attacks and Mitigations* — cross-listed under the NIST section above.
- **Anthropic.** [Responsible Scaling Policy](https://www.anthropic.com/rsp) — a public example of a foundation-model provider's tier-and-commitment framework; useful as prior art the head of AI governance may reference in escalation.
- **OpenAI.** *GPT-4 System Card* and successor system cards. Search [openai.com/research](https://openai.com/research). Public examples of system-card documents at the shape a governance analyst can review.
- **Anthropic.** *Model Card* / *System Card* documents accompanying Claude releases; see the Anthropic model page for the current model version. Useful for triangulating the org-standard template against a foundation-model provider's approach.

## Governance frameworks adjacent to this module

- **ISO/IEC 42001:2023** — *Information technology — Artificial intelligence — Management system.* Available for purchase at [iso.org/standard/81230.html](https://www.iso.org/standard/81230.html). The first ISO management-system standard specific to AI; the org's certification path if it pursues one runs through this document.
- **Singapore Infocomm Media Development Authority (IMDA).** *Model AI Governance Framework* (2nd Edition, 2020) and *Model AI Governance Framework for Generative AI* (2024). Both linked from [imda.gov.sg](https://www.imda.gov.sg/). One of the earliest sector-agnostic government-published governance frameworks; useful as an outside-the-EU perspective the register's Govern column can reference.
- **UK AI Safety Institute.** Publications at [aisi.gov.uk](https://www.aisi.gov.uk/). Frontier-model safety evaluations; relevant for orgs whose portfolio includes foundation-model-provider or foundation-model-consumer positioning.

## Company writeups worth reading as prior art

- **Google.** [Responsible AI Practices](https://ai.google/responsibility/responsible-ai-practices/). Google's public documentation of its responsible-AI approach; useful as prior art for the model-card and eval-cadence sections of the register.
- **Microsoft.** [Responsible AI Standard v2](https://www.microsoft.com/en-us/ai/responsible-ai). Microsoft's public framework, which pairs closely with their internal impact-assessment process; useful as an example of a launched-and-maintained internal standard.
- **Meta.** [System Cards](https://ai.meta.com/tools/system-cards/). Meta's product-surface documentation shape — a slightly different granularity from Mitchell et al. model cards (system-level rather than model-level) that many product orgs adopt in parallel.
- **Anthropic.** [Responsible Scaling Policy](https://www.anthropic.com/rsp) — cross-listed under Red teaming.

## Engineering practice texts adjacent to this module

- **Larson, Will.** *Staff Engineer: Leadership beyond the management track.* Stripe Press, 2021. [staffeng.com/book](https://staffeng.com/book). The **Right Hand** archetype chapter 1 anchors on; the archetype's centre of gravity is exactly this module's artifact set. See also mod-401's chapter 2.
- **Reilly, Tanya.** *The Staff Engineer's Path.* O'Reilly, 2022. Chapter 5's "gate as a career signal" section shares its framing with Reilly's chapters on influence, boundary-setting, and doing consequential work others do not want to touch.
- **Winters, Manshreck, Wright.** *Software Engineering at Google.* O'Reilly, 2020. Free HTML edition at [abseil.io/resources/swe-book](https://abseil.io/resources/swe-book). The Design Documents and Launch Committees chapters are relevant to the pre-launch-gate charter shape in chapter 5.

## In-track references

- [`../../CURRICULUM.md`](../../CURRICULUM.md) — the module and project plan this module sits inside.
- [`../../PREREQUISITES.md`](../../PREREQUISITES.md) — the L20 and L30 foundations assumed for this module.
- [`../mod-401-staff-ml-role-scope/`](../mod-401-staff-ml-role-scope/) — the introductory module whose Right Hand archetype, altitude vocabulary, and hand-off-contract framing this module builds on.
- [`../mod-402-multi-team-ml-architecture/`](../mod-402-multi-team-ml-architecture/) — the module whose portfolio blueprint's per-system paragraph flows straight into the register's Map column.
- [`../mod-408-portfolio-reliability-and-incident-command/`](../mod-408-portfolio-reliability-and-incident-command/) — the reliability-and-incident module whose severity vocabulary the register's Manage column and chapter 6's red-team finding severities reuse. Where the reliability boundary meets the responsible-AI boundary.
- [`../mod-410-staff-plus-technical-leadership/`](../mod-410-staff-plus-technical-leadership/) — the module whose executive-comms craft the gate charter's political defence and the exec-audience gate summary draw on.
- [`../../projects/project-402-foundation-model-training-program/`](../../projects/project-402-foundation-model-training-program/) — the training-program capstone; a register entry for the trained model is one of its deliverables.
- [`../../projects/project-403-staff-plus-tech-leadership-simulation/`](../../projects/project-403-staff-plus-tech-leadership-simulation/) — the leadership-simulation capstone; the gate charter and its executive framing are integrated deliverables.

## Partner tracks

- [`ai-infra-security-learning`](https://github.com/ai-infra-curriculum/ai-infra-security-learning) — the peer security track chapter 6's delegation contract points at. Read their catalogue to understand what "deep" work is available before authoring the delegation contract.
- [`ai-governance-analyst`](https://github.com/ai-infra-curriculum/ai-governance-analyst) — the peer governance track whose analysts own the policy the register documents and the gate enforces. Read their catalogue to know what the analyst is being trained to bring into the partnership.
- [`head-of-ai-governance`](https://github.com/ai-infra-curriculum/head-of-ai-governance) — the peer governance-leadership track that chapter 5's escalation path routes to and chapter 6's high-risk delegation contract is co-signed with.

<!-- needs-research: add one or two published post-hoc analyses of an EU AI Act enforcement action (fines, corrective orders, or supervisory-authority findings) once the autonomous research loop has WebSearch permissions and once enforcement history exists — as of the module's authoring date the Act's high-risk obligations were still in their phased-application window and enforcement precedent was thin. -->

<!-- needs-research: verify the current OWASP LLM Top 10 category list against the published document at owasp.org; the ten-category enumeration in chapter 6 tracks the initial published version but the project revises annually. -->
