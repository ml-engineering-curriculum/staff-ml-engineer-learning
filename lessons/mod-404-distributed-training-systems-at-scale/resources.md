# Resources for mod-404-distributed-training-systems-at-scale (Distributed-Training Strategy at Scale — Parallelism, MFU, Failure Budgets)

Real, citable references for the topics this module covers. The load-bearing reading is the ZeRO / Megatron-LM / FSDP trio for the parallelism primitives, the roofline paper and the MegaScale / LLaMA-3 program reports for MFU and operational shape, and the Daly checkpoint paper plus the OPT-175B / BLOOM training chronicles for failure-mode and reserve strategy. Read the primary papers before the framework docs.

## Parallelism primitives — the chapter 2 core reading list

- **Rajbhandari, Samyam et al.** *ZeRO: Memory Optimizations Toward Training Trillion Parameter Models.* SC 2020. [arXiv:1910.02054](https://arxiv.org/abs/1910.02054). The three-stage sharding taxonomy (ZeRO-1 optimizer state, ZeRO-2 + gradients, ZeRO-3 + weights) chapter 2 uses.
- **Zhao, Yanli et al.** *PyTorch FSDP: Experiences on Scaling Fully Sharded Data Parallel.* VLDB 2023. [arXiv:2304.11277](https://arxiv.org/abs/2304.11277). The PyTorch-native FSDP paper with sharding-unit and prefetch discussion. PyTorch 2.x introduced FSDP2 which fixes several original-FSDP ergonomics issues; see the [PyTorch FSDP docs](https://pytorch.org/docs/stable/fsdp.html) and the [FSDP2 migration guide](https://pytorch.org/tutorials/intermediate/FSDP_tutorial.html).
- **DeepSpeed team.** *DeepSpeed ZeRO documentation.* [deepspeed.ai/tutorials/zero](https://www.deepspeed.ai/tutorials/zero/). The reference implementation of ZeRO-1/2/3 with the `stage3_module_granularity_threshold` and related tuning knobs chapter 2 mentions.
- **Shoeybi, Mohammad et al.** *Megatron-LM: Training Multi-Billion Parameter Language Models Using Model Parallelism.* arXiv 2019. [arXiv:1909.08053](https://arxiv.org/abs/1909.08053). The tensor-parallel origin text; the column-split-then-row-split pattern for attention and MLP chapter 2 uses verbatim.
- **Narayanan, Deepak et al.** *Efficient Large-Scale Language Model Training on GPU Clusters Using Megatron-LM.* SC 2021. [arXiv:2104.04473](https://arxiv.org/abs/2104.04473). The interleaved-1F1B schedule and the 3D-parallel mesh-nesting rules chapter 3 leans on.
- **Huang, Yanping et al.** *GPipe: Efficient Training of Giant Neural Networks using Pipeline Parallelism.* NeurIPS 2019. [arXiv:1811.06965](https://arxiv.org/abs/1811.06965). The original pipeline-parallel formulation with the (P-1)/M bubble analysis.
- **Narayanan, Deepak et al.** *PipeDream: Generalized Pipeline Parallelism for DNN Training.* SOSP 2019. [PipeDream](https://dl.acm.org/doi/10.1145/3341301.3359646). The 1F1B schedule that lowered activation memory vs. GPipe at the same bubble.
- **Qi, Penghui et al.** *Zero Bubble Pipeline Parallelism.* ICLR 2024. [arXiv:2401.10241](https://arxiv.org/abs/2401.10241). The bubble-elimination scheduling technique chapter 2 lists in the modern-pipeline-schedule section.
- **DeepSeek-AI.** *DeepSeek-V3 Technical Report.* arXiv 2024. [arXiv:2412.19437](https://arxiv.org/abs/2412.19437). The DualPipe schedule and the FP8-training-at-scale operational discussion that anchor the modern-recipe frontier.
- **Korthikanti, Vijay et al.** *Reducing Activation Recomputation in Large Transformer Models.* MLSys 2023 / NVIDIA 2022. [arXiv:2205.05198](https://arxiv.org/abs/2205.05198). The sequence-parallel and selective-activation-recomputation paper chapter 2 cites for SP semantics.
- **Lepikhin, Dmitry et al.** *GShard: Scaling Giant Models with Conditional Computation and Automatic Sharding.* ICLR 2021. [arXiv:2006.16668](https://arxiv.org/abs/2006.16668). The original at-scale MoE recipe with expert-parallel + AllToAll.
- **Fedus, William; Zoph, Barret; Shazeer, Noam.** *Switch Transformers: Scaling to Trillion Parameter Models with Simple and Efficient Sparsity.* JMLR 2022. [arXiv:2101.03961](https://arxiv.org/abs/2101.03961). The K=1 MoE simplification chapter 2 mentions.
- **Gale, Trevor et al.** *MegaBlocks: Efficient Sparse Training with Mixture-of-Experts.* MLSys 2023. [arXiv:2211.15841](https://arxiv.org/abs/2211.15841). Block-sparse-kernel MoE that eliminates the drop-token padding overhead of prior MoE implementations.
- **Zhou, Yanqi et al.** *Mixture-of-Experts with Expert Choice Routing.* NeurIPS 2022. [arXiv:2202.09368](https://arxiv.org/abs/2202.09368). The expert-choice routing chapter 2 lists as a load-balance mitigation.
- **Jacobs, Sam Ade et al.** *DeepSpeed Ulysses: System Optimizations for Enabling Training of Extreme Long Sequence Transformer Models.* arXiv 2023. [arXiv:2309.14509](https://arxiv.org/abs/2309.14509). The context-parallelism technique chapter 3 gestures at for very-long-sequence programs.
- **Liu, Hao et al.** *Ring Attention with Blockwise Transformers for Near-Infinite Context.* ICLR 2024. [arXiv:2310.01889](https://arxiv.org/abs/2310.01889). The ring-attention companion technique for long-context training.
- **Sergeev, Alexander; Del Balso, Mike.** *Horovod: fast and easy distributed deep learning in TensorFlow.* arXiv 2018. [arXiv:1802.05799](https://arxiv.org/abs/1802.05799). The alternative-to-DDP historical reference.
- **Baidu Research.** *Bringing HPC Techniques to Deep Learning.* Baidu blog 2017 (ring-AllReduce). [andrew.gibiansky.com/blog/machine-learning/baidu-allreduce](https://andrew.gibiansky.com/blog/machine-learning/baidu-allreduce/). The ring-AllReduce origin explanation.

## Attention kernels and precision

- **Dao, Tri et al.** *FlashAttention: Fast and Memory-Efficient Exact Attention with IO-Awareness.* NeurIPS 2022. [arXiv:2205.14135](https://arxiv.org/abs/2205.14135). The IO-aware attention algorithm chapter 4's attention row references.
- **Dao, Tri.** *FlashAttention-2: Faster Attention with Better Parallelism and Work Partitioning.* ICLR 2024. [arXiv:2307.08691](https://arxiv.org/abs/2307.08691). The FlashAttention-2 rewrite for higher hardware utilisation on A100 / H100.
- **Shah, Jay et al.** *FlashAttention-3: Fast and Accurate Attention with Asynchrony and Low-precision.* NeurIPS 2024. [arXiv:2407.08608](https://arxiv.org/abs/2407.08608). The H100-optimised warpgroup-async and FP8-native attention chapter 4 recommends when the recipe hits HBM-bound attention.
- **Micikevicius, Paulius et al.** *Mixed Precision Training.* ICLR 2018. [arXiv:1710.03740](https://arxiv.org/abs/1710.03740). The FP16-with-FP32-master-and-loss-scaling paper; foundational for BF16-native modern practice.
- **NVIDIA.** *Transformer Engine documentation.* [docs.nvidia.com/deeplearning/transformer-engine](https://docs.nvidia.com/deeplearning/transformer-engine/user-guide/index.html). The FP8-training library referenced by chapter 4's FP8 MFU-band discussion; per-tensor scaling and delayed scaling algorithms.
- **NVIDIA.** *NVIDIA H100 Tensor Core GPU Architecture Whitepaper.* [resources.nvidia.com/en-us-tensor-core/gtc22-whitepaper-hopper](https://resources.nvidia.com/en-us-tensor-core/gtc22-whitepaper-hopper). Vendor spec chapter 4's roofline arithmetic cites for the 989 TFLOPs BF16 and 3.35 TB/s HBM3 numbers.
- **NVIDIA.** *NVIDIA H200 Tensor Core GPU Datasheet.* [nvidia.com/en-us/data-center/h200](https://www.nvidia.com/en-us/data-center/h200/). The H200 141 GB HBM3e and 4.8 TB/s bandwidth spec.

## Roofline model and performance analysis

- **Williams, Samuel; Waterman, Andrew; Patterson, David.** *Roofline: An Insightful Visual Performance Model for Multicore Architectures.* Communications of the ACM 2009. [DOI 10.1145/1498765.1498785](https://dl.acm.org/doi/10.1145/1498765.1498785). The origin paper for the roofline formulation chapter 4 uses.
- **NVIDIA.** *Nsight Systems documentation.* [developer.nvidia.com/nsight-systems](https://developer.nvidia.com/nsight-systems). The system-level tracing tool chapter 4's per-bottleneck diagnostics reference.
- **NVIDIA.** *Nsight Compute documentation.* [developer.nvidia.com/nsight-compute](https://developer.nvidia.com/nsight-compute). The kernel-level profiler chapter 4's HBM-bound diagnostic points at.
- **NVIDIA.** *NCCL User Guide.* [docs.nvidia.com/deeplearning/nccl/user-guide](https://docs.nvidia.com/deeplearning/nccl/user-guide/docs/index.html). Reference for the collective communication library chapter 4's network-bound diagnostics reference. Includes `NCCL_DEBUG=INFO` and topology-detection details.
- **NVIDIA.** *DCGM (Data Center GPU Manager) documentation.* [developer.nvidia.com/dcgm](https://developer.nvidia.com/dcgm). The GPU telemetry stack chapters 5 and 6 reference for ECC events, HBM utilisation, and per-rank health.

## Reference training programs — the "read the report" list

Every published program's tech report contains an operational-engineering section worth extracting. Read at least one pretraining program's full operational writeup before exercise-03.

- **Llama Team, Meta AI.** *The Llama 3 Herd of Models.* Meta 2024. [arXiv:2407.21783](https://arxiv.org/abs/2407.21783). The 15T-token pretraining, the recipe details across 8B / 70B / 405B, the reliability / operations discussion, and the failure-mode section. Currently the most operationally-detailed contemporary training report.
- **Touvron, Hugo et al.** *Llama 2: Open Foundation and Fine-Tuned Chat Models.* Meta AI 2023. [arXiv:2307.09288](https://arxiv.org/abs/2307.09288). The chapter 4 ~40% MFU reference and the recipe-side discussion of grouped-query attention.
- **Jiang, Ziheng et al.** *MegaScale: Scaling Large Language Model Training to More Than 10,000 GPUs.* ByteDance / NSDI 2024. [arXiv:2402.15627](https://arxiv.org/abs/2402.15627). The load-bearing reference for chapters 4 (55% MFU) and 6 (warm-standby percentage, restart-latency benchmarking). The section on straggler detection is directly aligned with chapter 5 mode 2.
- **Chowdhery, Aakanksha et al.** *PaLM: Scaling Language Modeling with Pathways.* Google Research 2022. [arXiv:2204.02311](https://arxiv.org/abs/2204.02311). The 540B TPUv4 pretraining and the 46% MFU headline; also documents loss-spike-and-recovery behaviours cited in chapter 5.
- **BigScience Workshop.** *BLOOM: A 176B-Parameter Open-Access Multilingual Language Model.* arXiv 2022. [arXiv:2211.05100](https://arxiv.org/abs/2211.05100). Companion to the BLOOM training chronicles; documents the loss-spike and hardware-failure timeline chapter 5's mode-5 weighting is calibrated against.
- **Meta AI.** *OPT-175B Logbook.* [`facebookresearch/metaseq` `OPT175B_Logbook.pdf`](https://github.com/facebookresearch/metaseq/blob/main/projects/OPT/chronicles/OPT175B_Logbook.pdf). The primary "what really happens on a large main run" chronicle. Multiple loss spikes, hardware substitutions, checkpoint corruptions — chapters 5 and 6 are calibrated against this document.
- **Zhang, Susan et al.** *OPT: Open Pre-trained Transformer Language Models.* Meta AI 2022. [arXiv:2205.01068](https://arxiv.org/abs/2205.01068). Companion paper to the logbook.
- **DeepSeek-AI.** *DeepSeek-V3 Technical Report.* [arXiv:2412.19437](https://arxiv.org/abs/2412.19437). The DualPipe schedule, the FP8 training-at-scale, and the operational-engineering discussion — currently one of the strongest published treatments of the modern MoE recipe.
- **DeepSeek-AI.** *DeepSeek-V2: A Strong, Economical, and Efficient Mixture-of-Experts Language Model.* arXiv 2024. [arXiv:2405.04434](https://arxiv.org/abs/2405.04434). The V2 MoE reference chapter 3 cites for MoE recipe sizing.
- **Mistral AI.** *Mixtral of Experts.* arXiv 2024. [arXiv:2401.04088](https://arxiv.org/abs/2401.04088). Reference for the Mixtral 8×7B MoE architecture chapter 3 uses as a worked recipe.
- **Rae, Jack W. et al.** *Scaling Language Models: Methods, Analysis & Insights from Training Gopher.* DeepMind 2021. [arXiv:2112.11446](https://arxiv.org/abs/2112.11446). Gopher's training-side operational writeup.

## Checkpointing and reliability

- **Young, John W.** *A First Order Approximation to the Optimum Checkpoint Interval.* Communications of the ACM 1974. [DOI 10.1145/361147.361115](https://dl.acm.org/doi/10.1145/361147.361115). The origin of the closed-form-optimum checkpoint interval chapter 6 uses.
- **Daly, John.** *A higher order estimate of the optimum checkpoint interval for restart dumps.* Future Generation Computer Systems 2006. [DOI 10.1016/j.future.2004.11.016](https://doi.org/10.1016/j.future.2004.11.016). The second-order refinement and the modern form of `τ* = √(2 · T_c · M)` chapter 6 quotes.
- **Mohan, Jayashree; Phanishayee, Amar; Chidambaram, Vijay.** *CheckFreq: Frequent, Fine-Grained DNN Checkpointing.* FAST 2021. [usenix.org/conference/fast21/presentation/mohan](https://www.usenix.org/conference/fast21/presentation/mohan). The DNN-specific checkpoint-frequency optimisation companion to Daly for the LLM regime.
- **Wang, Zhen et al.** *GEMINI: Fast Failure Recovery in Distributed Training with In-Memory Checkpoints.* SOSP 2023. [dl.acm.org/doi/10.1145/3600006.3613145](https://dl.acm.org/doi/10.1145/3600006.3613145). The in-memory-checkpoint technique that shrinks `T_c` toward the async-checkpoint regime chapter 6 discusses.
- **PyTorch.** *`torch.distributed.checkpoint` (DCP) documentation.* [pytorch.org/docs/stable/distributed.checkpoint](https://pytorch.org/docs/stable/distributed.checkpoint.html). The modern PyTorch async-and-sharded checkpoint API that chapters 5-6 assume as the reference implementation.
- **NVIDIA NeMo.** *NeMo Checkpoint IO documentation.* [docs.nvidia.com/nemo-framework](https://docs.nvidia.com/nemo-framework/user-guide/latest/nemotoolkit/features/checkpointing.html). Reference for asynchronous / distributed checkpoint IO in the Megatron / NeMo stack.
- **PyTorch.** *`torch.distributed.elastic` documentation.* [pytorch.org/docs/stable/distributed.elastic](https://pytorch.org/docs/stable/distributed.elastic.html). The elastic-training support chapter 5 mode 2 cites as an alternative to restart-from-checkpoint on straggler eviction.

## Failure modes and observability

- **Xu, Peng et al.** *Silent Data Corruption in HPC.* IEEE Micro 2021. [DOI 10.1109/MM.2021.3061497](https://ieeexplore.ieee.org/document/9376974). Reference on silent-data-corruption rates at HPC scale that chapter 5 mode 1 cites for the bit-flip rate ceiling.
- **Meta / Facebook Research.** *Silent Data Corruptions at Scale.* arXiv 2021. [arXiv:2102.11245](https://arxiv.org/abs/2102.11245). The Meta report on production-scale silent hardware corruption chapter 5 mode 1 references.
- **Google / SDC report.** *Cores that don't count.* HotOS 2021. [research.google/pubs/pub50230](https://research.google/pubs/pub50230). Google's fleet-scale silent-hardware-corruption report; a widely-cited companion to the Meta report.
- **NVIDIA.** *DCGM ECC event categories.* Part of the [DCGM API reference](https://docs.nvidia.com/datacenter/dcgm/latest/dcgm-api/dcgm-api-field-ids.html). The corrected / uncorrected ECC event codes chapter 5 mode 1's detection cadence references.
- **PyTorch.** *`torch.distributed` collective timeout and watchdog documentation.* [pytorch.org/docs/stable/distributed](https://pytorch.org/docs/stable/distributed.html). The `pg.timeout` and NCCL watchdog primitives chapter 5 mode 3 references.
- **Wu, Yifan et al.** *A Study of BLOOM Training Stability.* HuggingFace BigScience blog 2022. [huggingface.co/blog/bloom-megatron-deepspeed](https://huggingface.co/blog/bloom-megatron-deepspeed). The training-stability discussion behind the BLOOM chronicles.
- **BigScience.** *BLOOM training chronicles.* Model card: [huggingface.co/bigscience/bloom](https://huggingface.co/bigscience/bloom). The community operational-engineering document chapters 5 and 6 are calibrated against for spike, restart, and load-balance realities.

## Cluster capacity, cloud, and hardware

- **AWS.** *EC2 Capacity Reservations documentation.* [docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-capacity-reservations](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-capacity-reservations.html). The AWS-side reserved-capacity model chapter 6 references.
- **AWS.** *EC2 Spot Instances documentation.* [aws.amazon.com/ec2/spot](https://aws.amazon.com/ec2/spot/). The AWS-side spot-capacity model chapter 6 references.
- **GCP.** *TPU reservations.* [cloud.google.com/tpu/docs/reservations](https://cloud.google.com/tpu/docs/reservations). Google Cloud's reserved-capacity model for TPU programs.
- **GCP.** *Spot VMs documentation.* [cloud.google.com/spot-vms](https://cloud.google.com/spot-vms). Google Cloud's spot-capacity model.
- **Azure.** *Spot Virtual Machines documentation.* [learn.microsoft.com/en-us/azure/virtual-machines/spot-vms](https://learn.microsoft.com/en-us/azure/virtual-machines/spot-vms). Azure spot-capacity reference.
- **NVIDIA.** *DGX SuperPOD reference architecture.* [nvidia.com/en-us/data-center/dgx-superpod](https://www.nvidia.com/en-us/data-center/dgx-superpod/). The NVLink-domain-and-InfiniBand topology reference that chapter 3's mesh-nesting rule assumes.
- **NVIDIA.** *NVLink and NVSwitch overview.* [nvidia.com/en-us/data-center/nvlink](https://www.nvidia.com/en-us/data-center/nvlink/). Reference for the NVLink-domain-size number chapter 2 caps TP against.
- **NVIDIA.** *GB200 NVL72 announcement and spec.* [nvidia.com/en-us/data-center/gb200-nvl72](https://www.nvidia.com/en-us/data-center/gb200-nvl72/). The 72-GPU NVLink-domain reference chapter 3 lists as an exception to the standard 8-GPU intra-node domain.
- **Ulmschneider, Robert; Perez, David.** *Kubernetes for Machine Learning Workloads.* Various — see [kubernetes.io / kubeflow](https://kubernetes.io) and [kubeflow.org](https://kubeflow.org). Reference for the scheduler-teardown-latency component chapter 6's restart-latency decomposition cites.
- **Kubernetes.** *`kube-scheduler` documentation.* [kubernetes.io/docs/reference/scheduling](https://kubernetes.io/docs/reference/scheduling/). The scheduler-behaviour reference for pod-teardown and pod-schedule latency chapter 6 mentions.

## Framework and library documentation

- **PyTorch.** *Distributed overview.* [pytorch.org/tutorials/beginner/dist_overview](https://pytorch.org/tutorials/beginner/dist_overview.html). Entry point to PyTorch's distributed primitives.
- **PyTorch.** *`torch.distributed.tensor.parallel` documentation.* [pytorch.org/docs/stable/distributed.tensor.parallel](https://pytorch.org/docs/stable/distributed.tensor.parallel.html). Native PyTorch tensor-parallel support.
- **PyTorch.** *`torch.distributed.pipelining` documentation.* [pytorch.org/docs/stable/distributed.pipelining](https://pytorch.org/docs/stable/distributed.pipelining.html). Native PyTorch pipeline-parallel support with the 1F1B and interleaved-1F1B schedules chapter 2 lists.
- **NVIDIA.** *Megatron-LM (`NVIDIA/Megatron-LM`).* [github.com/NVIDIA/Megatron-LM](https://github.com/NVIDIA/Megatron-LM). Reference implementation of TP + PP + SP for the Megatron pattern.
- **NVIDIA.** *NeMo Framework documentation.* [docs.nvidia.com/nemo-framework](https://docs.nvidia.com/nemo-framework/user-guide/latest/nemotoolkit/index.html). Higher-level framework built on Megatron-LM with training-loop, checkpoint-io, and profiling integrations.
- **Microsoft.** *DeepSpeed (`microsoft/DeepSpeed`).* [github.com/microsoft/DeepSpeed](https://github.com/microsoft/DeepSpeed). Reference implementation of ZeRO-1/2/3 and DeepSpeed-Megatron.
- **HuggingFace.** *`accelerate` documentation.* [huggingface.co/docs/accelerate](https://huggingface.co/docs/accelerate/index). Higher-level distributed-training wrapper that most fine-tune programs use.
- **Google.** *JAX and Flax documentation.* [jax.readthedocs.io](https://jax.readthedocs.io) / [flax.readthedocs.io](https://flax.readthedocs.io). The TPU-primary framework families that most Google-scale training programs use.
- **Google.** *`jax.sharding` and `PartitionSpec` documentation.* [jax.readthedocs.io/en/latest/notebooks/Distributed_arrays_and_automatic_parallelization](https://jax.readthedocs.io/en/latest/notebooks/Distributed_arrays_and_automatic_parallelization.html). The JAX-native parallelism primitives (SPMD, `pjit` / `jit`) that TPU programs use.

## Program-management and hand-off references

- **Larson, Will.** *Staff Engineer: Leadership beyond the management track.* Stripe Press, 2021. [staffeng.com/book](https://staffeng.com/book). The Staff-scope archetypes chapter 1 positions the mod-404 recipe-owner against.
- **Fournier, Camille.** *The Manager's Path.* O'Reilly, 2017. Cross-reference for the peer-track hand-off ceremony chapter 7 opens with.
- **Nygard, Michael.** *Release It! Design and Deploy Production-Ready Software (2nd ed.).* Pragmatic Bookshelf, 2018. The operational-reliability framework chapter 5's failure-mode taxonomy adapts to the training-run regime.
- **Beyer, Betsy et al.** *Site Reliability Engineering.* O'Reilly / Google 2016. [sre.google/sre-book](https://sre.google/sre-book/table-of-contents/). The SLO / SLI vocabulary chapter 5's per-mode "response SLO" language adopts.

## In-track references

- [`../../CURRICULUM.md`](../../CURRICULUM.md) — the module and project plan mod-404 sits inside.
- [`../../PREREQUISITES.md`](../../PREREQUISITES.md) — the assumed L20 / L30 foundation and the full list of peer specialist, peer platform, and peer L40 tracks the chapter 1 and chapter 7 hand-off contracts route to.
- [`../mod-401-staff-ml-role-scope/`](../mod-401-staff-ml-role-scope/) — the altitude vocabulary, the four Staff archetypes, and the hand-off-contract shape chapters 1 and 7 operationalise for recipe scope.
- [`../mod-402-multi-team-ml-architecture/`](../mod-402-multi-team-ml-architecture/) — the RFC template the one-page recipe summary (chapter 7) can pair with when the training program sits inside a portfolio-scope architectural change.
- [`../mod-403-foundation-model-training-programs/`](../mod-403-foundation-model-training-programs/) — the program brief that produces the (N, D, C) target, cluster-hour budget, and risk multiplier chapter 1 lists as inputs; the up-stream artifact chapters 3 and 6 feed revisions back to.
- [`../mod-405-ml-platform-strategy/`](../mod-405-ml-platform-strategy/) — the platform-strategy scope the reserve-and-checkpoint chapter 6's substrate acceptance criteria feed into.
- [`../mod-407-capacity-cost-and-tco/`](../mod-407-capacity-cost-and-tco/) — the org-scope capacity portfolio chapter 6's reserved / burst / spot posture sits inside.
- [`../mod-408-portfolio-reliability-and-incident-command/`](../mod-408-portfolio-reliability-and-incident-command/) — the portfolio-scope incident-command framework chapter 5's failure-mode SLOs and chapter 7's on-call rotation escalate into.
- [`../mod-410-staff-plus-technical-leadership/`](../mod-410-staff-plus-technical-leadership/) — the principal-track co-design skill chapter 3's frontier-scale worked example gestures at for cases where the recipe author sits inside a multi-track design conversation.
