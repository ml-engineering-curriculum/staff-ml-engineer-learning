# Resources for mod-407-capacity-cost-and-tco

Real, citable references for the topics this module covers. Read the FinOps canonical text and the cloud pricing docs before authoring the TCO model; treat the hyperscaler engineering blogs as prior art for the "how big orgs actually price their ML fleet" side; treat MLPerf and the vendor whitepapers as size inputs, not aspirational targets.

## FinOps canonical

- **Storment, J. R.; Fuller, Mike.** *Cloud FinOps: Collaborative, Real-Time Cloud Financial Management.* O'Reilly, 2nd ed. 2023. The canonical text. Chapter 5's *Inform / Optimise / Operate* model is the operating discipline this module runs against; the chapters on chargeback / showback (12), unit economics (14-15), and RIs / commits (24-25) are the most load-bearing for chapters 3 and 5.
- **FinOps Foundation.** *FinOps Framework.* [finops.org/framework](https://www.finops.org/framework/). The public documentation of the same operating model plus the domains, capabilities, and personas. Free reading and the canonical reference the community cites in review meetings.
- **FinOps Foundation.** *State of FinOps* annual report. [finops.org/insights](https://www.finops.org/insights/). Industry-frequency evidence for the practices chapter 5 recommends; useful for justifying the review cadence to leadership.

## Cloud pricing and committed-use programmes

- **AWS.** [Savings Plans documentation](https://docs.aws.amazon.com/savingsplans/latest/userguide/what-is-savings-plans.html). The current Savings Plans programme structure — Compute vs. EC2 Instance vs. SageMaker — and the discount arithmetic chapter 3's reserved-and-committed buckets use.
- **AWS.** [EC2 Spot Instances documentation](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/using-spot-instances.html). The preemption model, interruption notice window, and pricing history that chapter 3's spot-bucket sizing rules of thumb are grounded in.
- **AWS.** [EC2 On-Demand Capacity Reservations](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-capacity-reservations.html). The GPU-specific reserved-capacity primitive that pairs with Savings Plans; important for training programmes that need a specific pod topology.
- **Google Cloud.** [Committed Use Discounts documentation](https://cloud.google.com/docs/cuds). The CUD programme — resource-based vs. spend-based — chapter 3 quotes.
- **Google Cloud.** [Spot VMs documentation](https://cloud.google.com/compute/docs/instances/spot). The Google Cloud spot / preemptible model and pricing.
- **Microsoft Azure.** [Azure Reserved VM Instances documentation](https://learn.microsoft.com/en-us/azure/cost-management-billing/reservations/save-compute-costs-reservations). The Azure equivalent to Savings Plans / CUDs; useful for portfolios with an Azure footprint.
- **Microsoft Azure.** [Azure Spot Virtual Machines documentation](https://learn.microsoft.com/en-us/azure/virtual-machines/spot-vms). The Azure spot model.
- **Cloud provider TCO calculators.** [AWS Pricing Calculator](https://calculator.aws/), [Google Cloud Pricing Calculator](https://cloud.google.com/products/calculator), [Azure Pricing Calculator](https://azure.microsoft.com/en-us/pricing/calculator/). Read the docs behind the calculators (assumptions, storage-tier defaults) rather than trusting the calculator output blindly.

<!-- needs-research: verify each committed-use programme's current discount ranges before authoring a TCO model; the numbers move every year and vendor documentation is authoritative over any second-hand summary. -->

## GPU hardware and benchmarks

- **NVIDIA.** [H100 Tensor Core GPU architecture whitepaper](https://resources.nvidia.com/en-us-tensor-core/gtc22-whitepaper-hopper). The Hopper architecture reference; peak-FLOPs, memory-bandwidth, and NVLink numbers chapter 3 uses for sizing.
- **NVIDIA.** [H200 Tensor Core GPU product page and technical brief](https://www.nvidia.com/en-us/data-center/h200/). The H200 memory-bandwidth and capacity uplift vs. H100 that chapter 3's hardware-generation section quotes.
- **NVIDIA.** [Blackwell architecture whitepaper (B100 / B200 / GB200)](https://resources.nvidia.com/en-us-blackwell-architecture). The Blackwell-generation performance uplift and precision-format changes that chapter 3 and chapter 6 price into three-year commits.
- **MLCommons.** [MLPerf Training benchmark results](https://mlcommons.org/benchmarks/training/). The community-standard training-throughput reference. Chapter 3's sizing uses these as sanity checks against program-brief cluster-hour estimates.
- **MLCommons.** [MLPerf Inference benchmark results](https://mlcommons.org/benchmarks/inference/). The equivalent inference-throughput reference. Chapter 4's utilisation review uses MLPerf-derived per-GPU throughput as the reference for `green / yellow / red` flag calls.

## Inference cost engineering — vLLM, TGI, Triton, KV-cache

- **Kwon, Woosuk et al.** *Efficient Memory Management for Large Language Model Serving with PagedAttention.* SOSP 2023. [Paper (PDF)](https://arxiv.org/abs/2309.06180). The origin paper for vLLM and PagedAttention; the KV-cache-memory arithmetic that chapters 4 and 6 rely on for inference cost modelling.
- **vLLM.** [Documentation](https://docs.vllm.ai/) and [GitHub repo](https://github.com/vllm-project/vllm). The current open-source reference implementation for high-throughput LLM inference; the self-hosted alternative in chapter 4's managed-API ↔ self-hosted migration analysis.
- **Hugging Face.** [Text Generation Inference (TGI) documentation](https://huggingface.co/docs/text-generation-inference/index). Alternative open-source LLM-serving stack; useful for triangulating throughput numbers against vLLM.
- **NVIDIA.** [Triton Inference Server documentation](https://docs.nvidia.com/deeplearning/triton-inference-server/). The industry-standard multi-model serving substrate; the consolidation target in chapter 4's fourth migration type.
- **NVIDIA.** [TensorRT-LLM documentation](https://nvidia.github.io/TensorRT-LLM/). NVIDIA's optimised LLM inference runtime; often the peer-performance-track's recommended kernel path when chapter 4's diagnostic surfaces a throughput gap.
- **Leviathan, Y.; Kalman, M.; Matias, Y.** *Fast Inference from Transformers via Speculative Decoding.* ICML 2023. [Paper (PDF)](https://arxiv.org/abs/2211.17192). The speculative-decoding paper; the draft-and-target-model economics chapter 5's optimise phase surfaces as an option.

## Multi-cloud, spot, and elastic-training frameworks

- **Yang, Zongheng et al.** *SkyPilot: An Intercloud Broker for Sky Computing.* NSDI 2023. [Paper (PDF)](https://www.usenix.org/conference/nsdi23/presentation/yang-zongheng). The reference framework for cross-cloud spot-capacity harvesting that chapter 3's spot bucket references.
- **SkyPilot.** [Documentation](https://skypilot.readthedocs.io/) and [GitHub repo](https://github.com/skypilot-org/skypilot). Current open-source SkyPilot; useful for the multi-cloud and spot-arbitrage plumbing chapter 3 assumes exists somewhere.
- **PyTorch.** [TorchElastic / TorchDistributed documentation](https://pytorch.org/docs/stable/distributed.elastic.html). The elastic-training primitives that make spot-capacity training tolerable at portfolio scope; ties into [`mod-404 chapter 6`](../mod-404-distributed-training-systems-at-scale/) reserve-strategy work.

## TCO framing and build-vs-buy

- **AWS.** [Amazon Aurora price-performance whitepapers](https://aws.amazon.com/rds/aurora/pricing/). Not directly ML-related but a good example of vendor TCO storytelling — the arithmetic Amazon uses to compare Aurora TCO against self-managed alternatives is the same shape chapter 6's inference-marginal-cost section uses.
- **AWS.** [AWS Migration TCO whitepapers and tooling](https://aws.amazon.com/tco-calculator/). Vendor-authored TCO framing; useful as a template for what the CFO expects a TCO doc to look like.
- **Wardley, Simon.** *Wardley Maps.* [Public book at medium.com/wardleymaps](https://medium.com/wardleymaps). The evolution-and-visibility framing for build-vs-buy decisions; chapter 4's managed-API ↔ self-hosted migration is a Wardley-shaped call in disguise, and the mapping vocabulary is useful for the migration doc's "alternatives considered" section.

## Company writeups on ML fleet economics

- **Meta.** *Applied Machine Learning at Facebook: A Datacenter Infrastructure Perspective.* HPCA 2018. [Paper page](https://research.facebook.com/publications/applied-machine-learning-at-facebook-a-datacenter-infrastructure-perspective/). Portfolio-scope ML from a hyperscaler perspective; the numbers are dated but the *shape* of the fleet-economics story is a template for chapter 6's TCO one-pager.
- **Meta AI.** [Building Meta's GenAI Infrastructure](https://engineering.fb.com/2024/03/12/data-center-engineering/building-metas-genai-infrastructure/). Meta's 2024 writeup on the 24k-H100 clusters supporting Llama 3 training; useful for grounding chapter 3's self-hosted-at-scale claims. <!-- needs-research: verify URL and citation stability. -->
- **Google.** [Google Cloud AI Infrastructure blog posts](https://cloud.google.com/blog/products/ai-machine-learning). Google's public writeups on TPU and GPU fleet economics; useful for triangulating the cross-cloud story.
- **Microsoft.** [Microsoft AI supercomputing and infrastructure posts](https://blogs.microsoft.com/blog/tag/ai/). Microsoft / OpenAI supercomputing writeups; grounding for the "very large scale, mixed CPU/GPU / accelerator" fleet-economics conversation. <!-- needs-research: identify a specific canonical Microsoft post that matches the mod's altitude and cite it directly. -->
- **CoreWeave / Lambda / neocloud provider blogs.** The neocloud providers publish cost-per-hour rate cards and (occasionally) TCO comparisons vs. hyperscalers. Useful for the chapter 3 self-hosted-vs-neocloud-vs-hyperscaler math. <!-- needs-research: cite one or two specific neocloud posts with defensible cost-per-hour numbers that the module can quote. -->

## Engineering practice texts adjacent to this module

- **Larson, Will.** *Staff Engineer: Leadership beyond the management track.* Stripe Press, 2021. [staffeng.com/book](https://staffeng.com/book). The exec-framing lens for chapter 6's one-pager; the Right-Hand-and-Architect archetypes that portfolio TCO work often reads against.
- **Fournier, Camille.** *The Manager's Path.* O'Reilly, 2017. Relevant specifically for the peer-EM dynamic in chapters 4 and 5 — the escalation gates cross into engineering-management scope. Read alongside mod-401 chapter 5.
- **Winters, Manshreck, Wright.** *Software Engineering at Google.* O'Reilly, 2020. [abseil.io/resources/swe-book](https://abseil.io/resources/swe-book). The chapters on Estimation and on Metrics support the discipline chapter 2's confidence tiers and chapter 6's sensitivity analysis need.

## In-track references

- [`../../CURRICULUM.md`](../../CURRICULUM.md) — the module and project plan this module sits inside.
- [`../../PREREQUISITES.md`](../../PREREQUISITES.md) — the L20 and L30 foundations assumed for this module.
- [`../mod-401-staff-ml-role-scope/`](../mod-401-staff-ml-role-scope/) — the introductory module whose finance-partner and peer-EM partnership contract this module operationalises.
- [`../mod-402-multi-team-ml-architecture/`](../mod-402-multi-team-ml-architecture/) — the portfolio-scope module whose blueprint the TCO model prices against; the RFC template in mod-402 chapter 5 is the template chapter 4's migration doc adapts.
- [`../mod-403-foundation-model-training-programs/03-cluster-hour-budget-defense.md`](../mod-403-foundation-model-training-programs/03-cluster-hour-budget-defense.md) — the single-program cluster-hour budget defence that chapter 1 explicitly builds on; the portfolio TCO model aggregates many of these.
- [`../mod-404-distributed-training-systems-at-scale/`](../mod-404-distributed-training-systems-at-scale/) — the recipe-scope module whose reserve-and-checkpoint strategy in chapter 6 pairs with chapter 3's spot bucket; the training-side MFU numbers this module cites.
- [`../mod-405-ml-platform-strategy/`](../mod-405-ml-platform-strategy/) — the platform-strategy module that owns build-vs-adopt on the substrate this module prices.
- [`../mod-408-portfolio-reliability-and-incident-command/`](../mod-408-portfolio-reliability-and-incident-command/) — the reliability side of the same portfolio.
- [`../mod-410-staff-plus-technical-leadership/`](../mod-410-staff-plus-technical-leadership/) — the exec-communication track that workshops chapter 6's one-pager.
- [`../../projects/project-401-multi-team-ml-blueprint/`](../../projects/project-401-multi-team-ml-blueprint/) — the capstone project that pairs the mod-402 portfolio blueprint with this module's TCO model.

<!-- needs-research: add one or two published ML-org TCO writeups (public write-ups of a workload-migration or reserved-commit decision that landed at portfolio scope) once the autonomous research loop has WebSearch permissions. Candidates to check first: Shopify, Stripe, Cloudflare, DoorDash, Instacart engineering blogs; also NVIDIA / Databricks / Anyscale customer case studies. -->
