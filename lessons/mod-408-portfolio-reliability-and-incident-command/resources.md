# Resources for mod-408-portfolio-reliability-and-incident-command

Real, citable references for the topics this module covers. The canonical SRE texts and the ML-monitoring literature are load-bearing — read the SRE Book before chapter 1 and the ML Test Score paper before chapter 2. Company-blog and vendor-doc references are prior art you can point at in the RFCs the module produces.

## Canonical SRE texts

- **Beyer, Betsy; Jones, Chris; Petoff, Jennifer; Murphy, Niall Richard (eds).** *Site Reliability Engineering: How Google Runs Production Systems.* O'Reilly, 2016. Free HTML edition at [sre.google/books/](https://sre.google/books/). Chapters 4 (Service Level Objectives), 5 (Eliminating Toil), 12 (Effective Troubleshooting), 13 (Emergency Response), 14 (Managing Incidents), and 15 (Postmortem Culture) are the load-bearing chapters for this module.
- **Beyer, Betsy; Murphy, Niall Richard; Rensin, David K.; Kawahara, Kent; Thorne, Stephen (eds).** *The Site Reliability Workbook: Practical Ways to Implement SRE.* O'Reilly, 2018. Free HTML edition at [sre.google/workbook/](https://sre.google/workbook/). Chapters on Implementing SLOs, Alerting on SLOs, and Managing Incidents refine the SRE Book's framework with post-2016 lessons; chapter 5 on SLO engineering is the primary reference for the SLO-vs-goal distinction in this module's chapter 2.
- **Alba, Alex; Beyer, Betsy; Rensin, David K.; Robinson, David; Salyards, Kent; Sullivan, Nick.** *Building Secure and Reliable Systems.* O'Reilly, 2020. Free HTML edition at [sre.google/books/](https://sre.google/books/). Chapter 15 (Deploying Code) is the closest formal reference for the retraining-as-deploy framing chapter 6 argues for; the book's postmortem chapter also refines the 2016 SRE Book's blameless-postmortem material.

## Incident-command frameworks

- **PagerDuty.** *PagerDuty Incident Response Documentation.* [response.pagerduty.com](https://response.pagerduty.com/). PagerDuty's open-sourced incident-response playbook — the roles, the tempo, and the response-team structure that a maturing on-call rotation reads against. Chapter 4's role assignments are consistent with the PagerDuty structure.
- **FEMA.** *Introduction to the Incident Command System (ICS-100).* [training.fema.gov](https://training.fema.gov/emiweb/is/icsresource/). The origin of the ICS structure Google SRE borrowed and this module inherits. ICS itself was developed by the FIRESCOPE program in California in the 1970s after the 1970 wildland-fire response failures; the FEMA training course is the canonical modern reference for the structure.
- **Google SRE.** *Incident Management at Google.* Reference chapter within the Site Reliability Workbook (see above) plus the summary at [sre.google/sre-book/managing-incidents/](https://sre.google/sre-book/managing-incidents/). The Incident Commander / Ops Lead / Comms Lead roles from chapter 4 read directly against this reference.

## ML monitoring, drift, and test-score literature

- **Sculley, D. et al.** *Hidden Technical Debt in Machine Learning Systems.* Advances in Neural Information Processing Systems (NeurIPS) 2015. [Paper (PDF)](https://papers.nips.cc/paper/2015/hash/86df7dcfd896fcaf2674f757a2463eba-Abstract.html). The reference behind mod-402 and referenced here for the failure surface that reliability is contending with — CACE, glue code, feedback loops, entangled features.
- **Breck, Eric; Cai, Shanqing; Nielsen, Eric; Salib, Michael; Sculley, D.** *The ML Test Score: A Rubric for ML Production Readiness and Technical Debt Reduction.* IEEE Big Data 2017. [Paper (PDF)](https://research.google/pubs/pub46555/). The closest industry-wide production-readiness rubric; chapter 2's SLI catalogue is a portfolio-standard subset. Read before authoring exercise-01.
- **Klaise, Janis; Van Looveren, Arnaud; Cox, Clive; Vacanti, Giovanni; Coca, Alexandru.** *Monitoring and explainability of models in production.* 2020, arXiv:2007.06299. [Paper](https://arxiv.org/abs/2007.06299). A survey of drift-detection and post-deploy monitoring practices; useful for the vocabulary chapter 2 uses for prediction-quality drift and calibration decay.
- **Alibi Detect.** [alibi-detect documentation](https://docs.seldon.io/projects/alibi-detect/en/stable/). Open-source drift-detection library from Seldon; canonical implementations of KS, MMD, and Chi-Squared drift tests referenced in chapter 2.
- **Evidently AI.** [evidentlyai.com/docs](https://docs.evidentlyai.com/). Open-source production ML monitoring — drift, data quality, model performance dashboards. Useful for triangulating the SLI vocabulary in chapter 2.
- **WhyLabs.** [whylabs.ai/docs](https://docs.whylabs.ai/). Commercial monitoring platform with a well-documented feature-drift, prediction-drift, and data-quality SLI taxonomy that lines up with the chapter-2 catalogue.

<!-- needs-research: locate the primary academic citation for the PSI 0.1 / 0.25 thresholds. The 0.1 "small drift" / 0.25 "material drift" numbers show up in vendor and credit-scoring documentation but are frequently attributed to Karakoulas et al. without a stable primary source. -->

## Feature-pipeline and feature-store reliability

- **Feast.** [feast.dev/documentation](https://docs.feast.dev/). Open-source feature-store; the [Feature freshness](https://docs.feast.dev/reference/feast-cli-commands) and [Feature validation](https://docs.feast.dev/reference/data-quality) documentation is the vocabulary chapter 2's feature-freshness SLI family reads against.
- **Tecton.** *Feature monitoring documentation.* [docs.tecton.ai](https://docs.tecton.ai/). Commercial feature-platform monitoring; the freshness, drift, and skew SLIs are documented in a way that lines up with the chapter-2 catalogue and gives the vocabulary the RFC readers already know.
- **Hopsworks.** [Feature Store documentation](https://docs.hopsworks.ai/). Point-in-time-correctness and feature-monitoring vocabulary that the chapter-2 catalogue re-uses.

## Model-serving reliability

- **NVIDIA Triton Inference Server.** [Triton documentation](https://docs.nvidia.com/deeplearning/triton-inference-server/user-guide/docs/index.html). The Model Configuration and model-repository documentation are the reference implementations chapter 6's canary path reads against.
- **KServe.** [kserve.github.io](https://kserve.github.io/website/). Open-source model-serving on Kubernetes; the Canary Rollout documentation is the direct reference for chapter 6 clause 1.
- **Ray Serve.** [docs.ray.io/en/latest/serve/](https://docs.ray.io/en/latest/serve/index.html). Model serving with deployment strategies and A/B split; chapter 6's serving-substrate framing includes Ray Serve as a valid backing implementation.
- **vLLM.** [docs.vllm.ai](https://docs.vllm.ai/). Production-grade LLM inference server; the LLM-specific canary and rollback discussions in chapter 6 read against vLLM's deployment patterns.

## Postmortem culture and Learning-from-Incidents

- **Allspaw, John.** *Blameless PostMortems and a Just Culture.* Etsy Code as Craft blog, 2012. [codeascraft.com/2012/05/22/blameless-postmortems/](https://www.etsy.com/codeascraft/blameless-postmortems). The short, load-bearing origin post for the "blameless" framing chapter 5 inherits. Read it before authoring exercise-04.
- **Allspaw, John; Robbins, Jesse (eds).** *Web Operations: Keeping the Data on Time.* O'Reilly, 2010. Older but includes the operational-culture and postmortem-review chapters that pre-date and inform the SRE Book's chapter 15.
- **Learning from Incidents in Software.** [learningfromincidents.io](https://www.learningfromincidents.io/). The community around Nora Jones, Lorin Hochstein, John Allspaw et al. that extends the blameless-postmortem practice into human-factors and cognitive-systems-engineering territory. The Rasmussen safety-boundary framework often referenced in this community is worth reading if the org's postmortem practice is maturing.
- **Dekker, Sidney.** *The Field Guide to Understanding 'Human Error'.* Third Edition, CRC Press, 2014. The human-factors reference behind the "systems, not individuals" framing; not required reading for this module but the depth reference for learners who want to push beyond the SRE Book's operational form.

## ML-incident writeups from company engineering blogs

- **Netflix Tech Blog.** [netflixtechblog.com](https://netflixtechblog.com/). Netflix's ML reliability, monitoring, and rollout writeups; useful for triangulating the SLI catalogue in a large consumer-product ML org.
- **Uber Engineering blog.** *Michelangelo model-monitoring writeups.* [www.uber.com/blog/](https://www.uber.com/blog/). Uber's operational writeups on model performance monitoring; useful contrast to the chapter-2 catalogue.
- **LinkedIn Engineering.** [engineering.linkedin.com/blog](https://engineering.linkedin.com/blog). Feature-freshness, model-monitoring, and drift-response posts across the ProML corpus.

<!-- needs-research: identify two or three publicly-published ML incident postmortems (as opposed to platform-writeup posts). Candidates to check first: Shopify, Stripe, Instacart, Doordash engineering blogs; also the Google Bard launch-response and the Amazon recruitment-model retirement writeups where a formal postmortem is public. -->

## Engineering practice adjacent to this module

- **Larson, Will.** *Staff Engineer: Leadership beyond the management track.* Stripe Press, 2021. [staffeng.com/book](https://staffeng.com/book). The Right Hand archetype's incident-command lean is a common Staff ML engineer role during a Sev-0; chapter 4's role assignment reads against the archetype vocabulary from mod-401.
- **Reilly, Tanya.** *The Staff Engineer's Path.* O'Reilly, 2022. The chapters on being on-call as a Staff engineer, and on the postmortem-review culture, complement chapter 5's postmortem template.
- **Winters, Titus; Manshreck, Tom; Wright, Hyrum.** *Software Engineering at Google.* O'Reilly, 2020. Free HTML edition at [abseil.io/resources/swe-book](https://abseil.io/resources/swe-book). Chapter 22 on Large-Scale Changes and chapter 24 on Continuous Delivery are useful references for the retraining-as-deploy framing in chapter 6.

## In-track references

- [`../../CURRICULUM.md`](../../CURRICULUM.md) — the module and project plan this module sits inside.
- [`../../PREREQUISITES.md`](../../PREREQUISITES.md) — the L20 and L30 foundations assumed for this module.
- [`../mod-401-staff-ml-role-scope/`](../mod-401-staff-ml-role-scope/) — introduces the L30 → L40 altitude vocabulary and the peer-EM partnership contract this module's chapter 4 reads against.
- [`../mod-402-multi-team-ml-architecture/`](../mod-402-multi-team-ml-architecture/) — the portfolio substrate this module's reliability contract applies to. Chapter 3's model-registry-fragmentation failure mode is a hard prerequisite to chapter 6's retraining-as-deploy contract.
- [`../mod-405-ml-platform-strategy/`](../mod-405-ml-platform-strategy/) — the peer platform team conversation that turns the reliability contract into a piece of the paved road the platform team ships.
- [`../mod-406-cross-team-eval-and-experimentation/`](../mod-406-cross-team-eval-and-experimentation/) — the progressive-rollout contract across shared traffic sits on top of this module's canary path.
- [`../mod-409-responsible-ai-at-portfolio-scope/`](../mod-409-responsible-ai-at-portfolio-scope/) — the pre-launch review gate reads the reliability contract this module produces.
- [`../../projects/project-403-staff-plus-tech-leadership-simulation/`](../../projects/project-403-staff-plus-tech-leadership-simulation/) — the capstone simulation that includes this module's SLI catalogue, severity matrix, and postmortem template as required artifacts.

<!-- needs-research: locate a canonical primary reference for the wildland-fire FIRESCOPE origin of ICS. The FEMA training course above is the canonical modern reference but a good historical primary source would strengthen chapter 4's citation. -->
