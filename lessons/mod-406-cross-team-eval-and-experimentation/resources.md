# Resources for mod-406-cross-team-eval-and-experimentation

Real, citable references for the topics this module covers. Read the canonical texts before authoring the four contracts; treat the platform-team writeups as prior art you can point at when the review body debates a specific capability; treat the tooling references as templates whose *shape* you can adopt without adopting their specific implementation.

## Canonical trustworthy-experimentation text

- **Kohavi, Ron; Tang, Diane; Xu, Ya.** *Trustworthy Online Controlled Experiments: A Practical Guide to A/B Testing.* Cambridge University Press, 2020. The canonical reference for the eval-metric-conflict failure mode's fix and for chapter 5's platform-consumer contract. The chapters on OEC (Overall Evaluation Criterion), guardrail metrics, SRM, ramp-up, novelty and primacy effects, and interference are the most load-bearing for this module. If you author one and only one document based on this book, make it the metric-registry section of chapter 5's contract.

## Variance reduction and sequential testing (foundational papers)

- **Deng, Alex; Xu, Ya; Kohavi, Ron; Walker, Toby.** *Improving the Sensitivity of Online Controlled Experiments by Utilizing Pre-Experiment Data.* WSDM 2013. [ACM link](https://dl.acm.org/doi/10.1145/2433396.2433413). The origin paper for CUPED, chapter 5's second required platform capability.
- **Johari, Ramesh; Koomen, Pete; Pekelis, Leonid; Walsh, David.** *Peeking at A/B Tests: Why It Matters, and What to Do About It.* KDD 2017. [ACM link](https://dl.acm.org/doi/10.1145/3097983.3097992). The paper that popularised always-valid sequential inference in the experimentation-platform community. Chapter 5's sequential-testing capability reads against this and the group-sequential literature it cites.
- **Fabijan, Aleksander; et al.** *Diagnosing Sample Ratio Mismatch in Online Controlled Experiments: A Taxonomy and Rules of Thumb for Practitioners.* KDD 2019. [ACM link](https://dl.acm.org/doi/10.1145/3292500.3330722). The most-cited SRM-diagnostics reference; chapter 5's SRM capability sits on top of it.

## LLM-as-judge literature

- **Zheng, Lianmin; et al.** *Judging LLM-as-a-Judge with MT-Bench and Chatbot Arena.* NeurIPS 2023. [arXiv:2306.05685](https://arxiv.org/abs/2306.05685). The canonical LLM-as-judge paper. Names position bias, verbosity bias, and self-enhancement bias — the three of the four bias classes chapter 3's dashboards track. Read before authoring the LLM-as-judge policy.
- **LMSYS Chatbot Arena.** [chat.lmsys.org](https://chat.lmsys.org/) and the associated blog / paper set at [lmsys.org/blog](https://lmsys.org/blog/). The largest public preference-collection deployment; useful triangulation for what pairwise-preference at scale actually looks like as an eval instrument.
- **Chiang, Wei-Lin; et al.** *Chatbot Arena: An Open Platform for Evaluating LLMs by Human Preference.* ICML 2024. [arXiv:2403.04132](https://arxiv.org/abs/2403.04132). The methodology paper behind Chatbot Arena; useful reference on the Bradley-Terry-style ranking system and calibration.
- <!-- needs-research: add a specific canonical reference for the rubric-drift bias class chapter 3 lists as the fourth bias — the fifth bias class of Zheng et al. is not "rubric drift" by name. Candidates to check on next research pass: Wang et al. "Large Language Models are not Fair Evaluators" (2023, arXiv:2305.17926), Panickssery et al. "LLM Evaluators Recognize and Favor Their Own Generations" (2024, arXiv:2404.13076) for self-enhancement depth, and follow-on work in the same NeurIPS 2023 evaluation track. -->

## Offline eval frameworks and datasets

- **Liang, Percy; et al.** *Holistic Evaluation of Language Models (HELM).* Stanford CRFM, 2022. [crfm.stanford.edu/helm](https://crfm.stanford.edu/helm/latest/) and [arXiv:2211.09110](https://arxiv.org/abs/2211.09110). The reference implementation of a multi-metric, multi-scenario language-model eval. Chapter 2's five-bucket framing does not adopt HELM's specific taxonomy; it reads against HELM as a peer.
- **BIG-Bench.** [GitHub: google/BIG-bench](https://github.com/google/BIG-bench). A community-authored eval benchmark; useful for understanding what "held-out set" looks like at benchmark scale.
- **Hendrycks, Dan; et al.** *Measuring Massive Multitask Language Understanding (MMLU).* ICLR 2021. [arXiv:2009.03300](https://arxiv.org/abs/2009.03300). The canonical general-knowledge eval; illustrative of both the utility and the contamination pitfalls chapter 2's held-out-set contract addresses.

## ML monitoring, drift, and eval discipline

- **Breck, Eric; Cai, Shanqing; Nielsen, Eric; Salib, Michael; Sculley, D.** *The ML Test Score: A Rubric for ML Production Readiness and Technical Debt Reduction.* IEEE Big Data 2017. [Google Research page](https://research.google/pubs/the-ml-test-score-a-rubric-for-ml-production-readiness-and-technical-debt-reduction/). The origin rubric for treating ML systems as needing tests, not just eval metrics. Chapter 2's pre-deploy review-gate borrows the rubric's altitude.
- **Sculley, D.; et al.** *Hidden Technical Debt in Machine Learning Systems.* NeurIPS 2015. [Paper](https://papers.nips.cc/paper/2015/hash/86df7dcfd896fcaf2674f757a2463eba-Abstract.html). Not eval-specific but the source text mod-402 is grounded in, and the reason the eval-metric-conflict failure mode was named as a portfolio-scope failure at all.
- **TensorFlow Data Validation (TFDV).** [TFX guide: data validation](https://www.tensorflow.org/tfx/guide/tfdv). The reference implementation of feature-distribution drift and schema-validation checks chapter 2's drift-and-freshness bucket requires.

## Experimentation-platform prior art

- **Microsoft Experimentation Platform (ExP).** Kohavi et al.'s work is the definitive reference; the ExP team's papers span 20 years. Start with the Kohavi/Tang/Xu book and follow references from there.
- **LinkedIn Engineering.** *Overlapping Experiment Infrastructure at LinkedIn.* Multiple engineering-blog posts; the XLNT platform is the closest peer to what chapter 5's contract describes. [engineering.linkedin.com](https://engineering.linkedin.com/blog/topic/experimentation) is the entry point.
- **Airbnb Engineering.** [Experimentation Reporting Framework (ERF)](https://medium.com/airbnb-engineering/experimentation-and-measurement-6ea7d5cae9b7) — Airbnb's public writeups on their experimentation stack. Useful for the interference and long-running-experiment framing.
- **Netflix Technology Blog.** [Experimentation Platform at Netflix](https://netflixtechblog.com/tagged/experimentation). Long-running series on multi-team experimentation at consumer scale.
- **Booking.com.** *Booking.com Experimentation and Data Science.* Public talks and engineering-blog posts, e.g. [How Booking.com Increases the Power of Online Experiments with CUPED](https://booking.ai/how-booking-com-increases-the-power-of-online-experiments-with-cuped-995d186fff1d). Useful for the CUPED practical-deployment story.
- <!-- needs-research: add one or two published cross-team experimentation postmortems (public write-ups of a two-team interference incident and how the review body resolved it). Candidates to check next research pass: Uber, Shopify, Stripe, Instacart engineering blogs. -->

## Rollout tooling and progressive-delivery references

- **Argo Rollouts.** [argoproj.github.io/argo-rollouts](https://argoproj.github.io/argo-rollouts/). The open-source progressive-delivery controller on Kubernetes; the canonical open-source reference for chapter 4's rollout contract's mechanical layer. The [canary strategy documentation](https://argoproj.github.io/argo-rollouts/features/canary/) is the shortest ramp-shape reference.
- **Flagger.** [flagger.app](https://flagger.app/) — the alternative open-source canary controller under Flux; useful as triangulation for what "the platform team could build" looks like.
- **LaunchDarkly, Split.io, Optimizely.** Commercial feature-flag / experimentation vendors; useful for orgs whose platform is bought rather than built. Contracts still apply; the vendor becomes the platform team.

## Engineering-practice texts adjacent to this module

- **Larson, Will.** *Staff Engineer: Leadership beyond the management track.* Stripe Press, 2021. [staffeng.com/book](https://staffeng.com/book). The delegation-vs-doing frame in chapter 6 draws directly on Larson's Architect and Tech-Lead archetype discussions.
- **Reilly, Tanya.** *The Staff Engineer's Path.* O'Reilly, 2022. Chapter 5's review body and chapter 6's escalation flow read against Reilly's chapters on writing durable contracts and building influence across teams.
- **Winters, Manshreck, Wright.** *Software Engineering at Google.* O'Reilly, 2020. [abseil.io/resources/swe-book](https://abseil.io/resources/swe-book). The chapters on Testing and Code Review supply the "test as a first-class engineering artifact" altitude chapter 2 needs.

## In-track references

- [`../../CURRICULUM.md`](../../CURRICULUM.md) — the module and project plan this module sits inside.
- [`../../PREREQUISITES.md`](../../PREREQUISITES.md) — the L20 and L30 foundations assumed for this module.
- [`../mod-402-multi-team-ml-architecture/`](../mod-402-multi-team-ml-architecture/) — the portfolio blueprint and RFC track this module's artifacts roll up into; specifically [`03-portfolio-failure-modes.md`](../mod-402-multi-team-ml-architecture/03-portfolio-failure-modes.md) for the eval-metric-conflict failure mode this module systematically fixes.
- [`../mod-405-ml-platform-strategy/`](../mod-405-ml-platform-strategy/) — the platform-team side of chapter 5's consumer contract; where "the platform team builds the experimentation platform" is negotiated.
- [`../mod-408-portfolio-reliability-and-incident-command/`](../mod-408-portfolio-reliability-and-incident-command/) — the reliability side of chapter 4's rollout contract; a guardrail-breach halt becomes an incident under mod-408's vocabulary.
- [`../mod-409-responsible-ai-at-portfolio-scope/`](../mod-409-responsible-ai-at-portfolio-scope/) — where Tier-C systems from chapter 3's judge policy are enumerated and where the responsible-AI safety-eval bar is set.
- [`../mod-410-staff-plus-technical-leadership/`](../mod-410-staff-plus-technical-leadership/) — the exec-comms lens on the eval-program artifacts; how the four contracts are defended in the leadership channel.
- [`../../projects/project-403-staff-plus-tech-leadership-simulation/`](../../projects/project-403-staff-plus-tech-leadership-simulation/) — the capstone that folds this module's artifacts into a Staff-plus leadership simulation.

<!-- needs-research: add one or two published LLM-eval postmortems (public write-ups of a judge-model failure that materially affected a launch decision) once the autonomous research loop has WebSearch permissions. Candidates: OpenAI, Anthropic, Google DeepMind eval-team blog posts, Hugging Face open-eval community writeups. -->
