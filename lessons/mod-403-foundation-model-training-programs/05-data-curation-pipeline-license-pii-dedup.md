# The Data Curation Pipeline: License, PII, and Dedup

## Motivation

Chapter 4 designed the mixture. This chapter designs the **pipeline that turns raw sources into the tokens the mixture actually consumes**. In practice this pipeline is where the majority of a training program's engineering weeks are spent — pretraining programs consistently report that data engineering absorbed more human-months than the model or the training loop combined. The Dolma paper (Soldaini et al. 2024) is explicit: the value of the paper is the pipeline documentation, not the model. The RedPajama-v2 release documented its filter cascade with the same emphasis.

The pipeline is a program-scope substrate. It has to run continuously for the duration of the program, produce new outputs between ablation runs, satisfy license and PII constraints that legal must be able to defend, and survive the shift in mixture weights that comes out of chapter 4's ablation. This chapter walks the pipeline stages, calls out the failure modes, and defines the deliverable — the **data curation pipeline design document** — that goes into the program brief.

## The pipeline stages

The high-level shape of a modern pipeline, in order:

```
[raw sources] → [source-level filter] → [document-level quality filter]
             → [license and PII handling] → [near-duplicate dedup]
             → [substring / n-gram dedup] → [safety filter]
             → [mixture-weighted sampler] → [tokenizer] → [training-time consumer]
```

Each stage is a decision. Each stage is also a place where downstream effects are non-local: a bug in the dedup stage manifests as a held-out-perplexity anomaly at the eval waterfall (chapter 6) three weeks later, and the diagnosis requires reproducing the pipeline from a source snapshot.

### 1. Source acquisition

**What.** Pull raw source data from its origin — Common Crawl WARC files, GitHub repository clones, ArXiv full-text dumps, licensed dataset APIs, internal warehouse tables (for domain-adapted fine-tune programs).

**Program-scope decisions.**

- **Cadence.** Common Crawl publishes new dumps monthly; the program needs to pick a source snapshot for each mixture recompute. Pinning a specific Common Crawl dump ID is required for reproducibility.
- **Legal-agreement retention.** Every non-public source needs a license or usage agreement on file, ideally in a machine-readable form the pipeline can enforce.
- **Storage and cost.** Common Crawl WARC files are ~90TB per snapshot, and the derived text (WET files) are ~9TB per snapshot. Programs commonly ingest 4-12 snapshots; petabyte-scale raw storage is a real line item.

**Failure mode to name.** Programs that grab the "latest" Common Crawl each week without pinning a snapshot ID make their pipeline non-reproducible. The eval-waterfall regressions that show up become unattributable — was it the mixture change, the filter change, or a new-crawl content shift? Pin snapshots.

### 2. Source-level filter

**What.** Coarse filters applied before per-document work. Deduplicating URLs, removing hosts on a blocklist, removing content in unsupported languages if the program is language-limited, removing NSFW-flagged hosts, etc.

**Program-scope decisions.**

- **Blocklist ownership.** Blocklists change; the program needs an owner who is responsible for the blocklist and a change-log for provenance. The blocklist is often the least-tested piece of the pipeline and the piece most likely to be modified quietly.
- **Language detection.** For a majority-English program, dropping non-English at the source-level filter saves downstream compute. But language detection at the source level is coarser than per-document detection — expect false positives.

### 3. Document-level quality filter

**What.** The classifier or heuristic-cascade that decides whether a given extracted document is worth training on. This is the stage the Dolma, RedPajama, and DCLM papers most heavily emphasise.

Two common families:

**Heuristic filters.** Rules-based: minimum document length, maximum symbol-to-word ratio, minimum stop-word ratio, maximum boilerplate density, non-English filtering via language-ID (fastText or cld3). Rae et al. (2021, *"Scaling Language Models: Methods, Analysis & Insights from Training Gopher"*) documented the "MassiveWeb" heuristic cascade in detail; the pattern is still the workhorse of modern pipelines.

**Classifier filters.** A binary classifier trained to distinguish "Wikipedia-quality" (positive class) from generic Common Crawl (negative class). The classifier is often a fastText or lightweight transformer, and the filter drops the bottom-K% of the corpus by classifier score. DCLM (Li et al. 2024) explicitly frames the classifier quality as a scaling axis: *better classifiers produce more useful tokens at the same volume*, and the classifier itself is a scaling-law leverage point.

**Program-scope decisions.**

- **Which family.** Heuristic filters are simple, reproducible, and cheap. Classifier filters produce measurably better tokens at scale but require a training set for the classifier, which is itself a research question. Modern programs typically use both — heuristic filters as a coarse first pass and a classifier as a fine second pass.
- **Filter aggression.** Higher filter aggression = fewer, higher-quality tokens = smaller D. The program's chapter-4 mixture design and the D target set the aggression. Test in ablation.
- **Version pinning.** Filter versions matter. The classifier weights, the heuristic thresholds, the language-ID model version — all pinned in the pipeline metadata for reproducibility.

### 4. License and PII handling

**What.** Ensure every document in the training set has a license the program can use, and remove PII where the program's policy requires.

**License handling.**

- **License-known sources** (ArXiv CC-BY, Wikipedia CC-BY-SA, StackExchange CC-BY-SA, licensed dataset API terms, licensed book acquisitions): tag every document with its license identifier at ingest. Rejections happen at the source-acquisition stage, not later.
- **License-unknown sources** (Common Crawl): the program's legal policy decides how to treat these. Some programs treat unlicensed public-web text under the "training-data-is-fair-use" theory that is currently unsettled in US and EU jurisprudence. Some programs apply a robots.txt-and-noai-meta-tag exclusion. Some programs commission license-audit passes.
- **Opt-out signals.** Programs increasingly honour opt-out signals: robots.txt, the ai.robots.txt convention, the C2PA "do-not-train" metadata, the site-level "noai" meta tag. The program brief documents which signals are honoured and how they are enforced.

**PII handling.**

- **PII detection.** Named-entity recognition passes for email addresses, phone numbers, government-ID numbers, physical addresses, and other regionally-defined PII. Presidio (Microsoft) and equivalent open-source tools are the common starting point.
- **PII treatment.** Options are: redact (replace with a placeholder), remove (drop the entire document if any PII is found), or annotate (keep the document but tag PII spans for downstream handling). Redaction is the standard choice; removal is what programs subject to GDPR or HIPAA-adjacent rules do.
- **Regional variation.** GDPR (EU) and CCPA (California) impose different data-subject-rights obligations. A program that ingests EU-origin data owes GDPR compliance. The program's legal-review sign-off is required, not the ML engineer's opinion.

**Program-scope decisions.**

- **What legal has to sign off on.** The pipeline design document is the artifact legal signs off on. If legal has not signed off on the license and PII posture, the program is at material regulatory risk. The EU AI Act's transparency obligations (Article 53) require providers of general-purpose AI models to publish a training-data summary; the pipeline metadata is the input to that summary.
- **The audit trail.** Which documents were included, from which snapshot, with which license tag, having passed which filter versions. The audit trail is a program-lifetime artifact, not a run-time artifact.

**Failure mode to name.** Programs that treat license and PII as a "we'll deal with it at ship time" problem discover at ship time that the pipeline cannot produce the audit trail regulators want, and the program's ship date slips by weeks or months. Do the license and PII work up front; the pipeline metadata cost is much lower than the retrofit cost.

### 5. Near-duplicate dedup

**What.** Remove documents that are near-duplicates of other documents in the corpus.

Why it matters at program scope: **dedup is a scaling axis**. Lee et al. (2022, *"Deduplicating Training Data Makes Language Models Better"*) showed that models trained on deduplicated corpora achieve better perplexity per training token and are less prone to memorising training-set verbatim spans. The effect is large — comparable to a real mixture-quality improvement — and dedup is far cheaper than commissioning new sources.

**The mechanics.**

- **Exact hash dedup.** MD5 or SHA of the document (or normalised document) removes exact-duplicate documents. Cheap, catches a big chunk of duplication.
- **MinHash near-dedup.** MinHash-based Locality-Sensitive Hashing (LSH) approximates Jaccard similarity between documents efficiently. The standard configuration is 128-256 permutations, banded LSH for candidate generation, and a similarity threshold in the 0.7-0.85 range. Documents above the threshold are marked as near-duplicates; the pipeline keeps one representative per cluster.
- **SimHash and other fingerprinting.** Alternatives to MinHash with similar quality; MinHash is the incumbent in the ML-scale literature.
- **Substring / n-gram dedup.** Removes duplicated substrings within documents (e.g., a copy-pasted paragraph appearing in a thousand documents). The Google *"Deduplicating Training Data..."* paper introduced the exact-substring dedup pattern; SlimPajama and Dolma both apply it.

**Program-scope decisions.**

- **Threshold.** MinHash similarity threshold 0.7 aggressively removes near-duplicates and shrinks D materially; threshold 0.85 keeps more D but leaves in some near-duplicates. Test in ablation.
- **Scope.** Dedup within a snapshot, across snapshots, across source classes? The stronger the scope, the higher the compute cost and the more D is removed.
- **Compute cost.** MinHash-LSH at trillion-token scale is a real MapReduce-style job — hundreds of thousands of CPU-hours are typical. This is not free.

**Failure mode to name.** Programs that skip dedup on the theory that "the model will just learn to weight duplicates less" are wrong. Duplicated training data measurably hurts held-out perplexity, inflates the effective epoch count on the duplicated slice, and increases verbatim-memorisation risk (which is a legal and safety problem). Dedup is not optional.

### 6. Safety filter

**What.** Remove or downweight documents whose content is problematic for the program's intended use: CSAM, non-consensual intimate imagery, hate content, self-harm content, and so on.

Two families:

**Classifier filters for known-bad content.** Categories the program's use case cannot tolerate — CSAM being the universal one; hate content, extremism, and violence being commonly filtered depending on use case.

**Downweighting for controversial-but-legitimate content.** Political content, NSFW-but-legal content, and similar. Rather than removing, the program downweights these categories in the sampler.

**Program-scope decisions.**

- **Filter categories and thresholds.** The program brief has to write these down. Legal, safety, and the peer [`mod-409-responsible-ai-at-portfolio-scope`](../mod-409-responsible-ai-at-portfolio-scope/) risk register are the audience.
- **False-positive rate.** Filters are imperfect; over-aggressive safety filters cost D. Test.
- **Ownership.** Who maintains the filter classifiers and the category definitions? This is often a place the ML team and the responsible-AI / policy team have to co-own explicitly.

### 7. Mixture-weighted sampler

**What.** At training time (or at pre-tokenisation time), sample from the filtered, deduped, license-clean, safety-checked corpora according to the mixture weights from chapter 4.

**Program-scope decisions.**

- **Sampling with replacement vs. without.** If the target D fits within the unique tokens for each source class, sample without replacement (one epoch of each). If the mixture requires epoching (chapter 4), sample with replacement and control the effective epoch count per source.
- **Interleaving.** Random interleaving vs. blocked-per-source. Random interleaving is the default; blocked-per-source is what a domain-progressive curriculum uses in a fine-tune program.

### 8. Tokenizer

**What.** Convert deduped, sampled text into token IDs.

**Program-scope decisions.** The tokenizer is a program-level artifact that must be trained once and pinned. Options:

- **Reuse an existing tokenizer.** For a fine-tune program continuing from LLaMA / Mistral / Qwen, use the base's tokenizer. Do not retrain.
- **Train a new tokenizer.** For a pretraining program from scratch, train a BPE or SentencePiece tokenizer on a representative subsample of the final mixture. Vocab size 32k-128k is typical; SentencePiece BPE is the default.
- **Extend an existing tokenizer.** For a domain-adapted fine-tune where domain vocabulary is highly out-of-distribution, extend the base's vocabulary with domain-specific tokens. Requires embedding-initialisation tricks and a warmup phase.

## The pipeline as a program dependency

The pipeline is not a one-shot build. It runs continuously for the duration of the program. Between the small-scale ablations and the full-scale ablations (chapter 6), the mixture weights or filter aggression may change; the pipeline reruns and produces a new corpus version. The training loop consumes a specific corpus version, pinned in the run metadata.

**Corpus versioning.** Every pipeline run produces a versioned corpus artifact with the input-snapshot IDs, filter versions, dedup config, mixture weights, tokenizer version, and audit-trail location. The corpus version ID goes in every run's metadata so ablation results are attributable.

**Sequential vs. parallel pipeline compute.** For a corpus in the trillion-token range, a full pipeline pass on modern data-processing infrastructure is measured in days-of-cluster time, not minutes. Budget it. The pipeline compute is not part of chapter 3's accelerator budget; it is a separate CPU-and-storage line-item and often ends up being 5-15% of the total dollar cost of the program.

**Ownership.** The pipeline should have an owner — one specific engineer whose full-time job is the pipeline. Not the ML engineer moonlighting between ablations. Not the platform team. A named owner. This is a hand-off contract (mod-401 chapter 4) and it is worth writing.

## Dedup as a scaling axis — the point that changes program shape

The single most under-appreciated finding of the last three years is that **dedup gains are on the same scale as model-size or data-quality gains**. Lee et al. (2022) showed the effect on validation perplexity. Muennighoff et al. (2023) tied it to the epoching analysis. Modern programs treat dedup aggressiveness as a mixture-weight-adjacent hyperparameter and test it in ablation.

**Practical implication.** When the program is negotiating between "add more sources" and "dedup more aggressively", the dedup change is often cheaper, more reproducible, and produces a comparable eval gain. This should be reflected in the ablation cadence — chapter 6's ablation grid should include at least one dedup-aggressiveness axis.

## The pipeline design document

The written artifact this chapter produces:

1. **Source manifest.** Every raw source, its snapshot ID or version, its license status, its expected token volume.
2. **Filter cascade.** Each stage, in order, with the version, thresholds, and expected token drop rate.
3. **License and PII posture.** What the program treats as licensable, what opt-out signals are honoured, what PII is redacted / removed / annotated, what the audit trail covers. Signed off by legal.
4. **Dedup config.** The exact-hash / MinHash / substring configuration, thresholds, and scope. Expected token drop rate.
5. **Safety filter categories.** The list of categories, ownership, false-positive-rate targets.
6. **Corpus versioning.** How each corpus version is identified, stored, and referenced from run metadata.
7. **Compute and cost.** Expected CPU-hours per pipeline run; expected calendar time; expected storage cost.
8. **Pipeline owner.** One named person.

Any pipeline design document missing item 3 (legal sign-off), item 4 (dedup config), item 6 (versioning), or item 8 (owner) is not ready for the program review.

## Summary

- The pipeline turns raw sources into training-time tokens. Stages: **source acquisition, source-level filter, document quality filter, license and PII, near-dedup, substring dedup, safety filter, sampler, tokenizer**.
- **License and PII handling** is a legal-sign-off artifact, not a technical afterthought. Regulatory obligations (GDPR, CCPA, EU AI Act Article 53 transparency) require the audit trail.
- **Dedup is a scaling axis**. MinHash-LSH near-dedup plus substring dedup materially improves perplexity per token and reduces memorisation risk. Not optional. Test aggressiveness in ablation.
- The pipeline is a **program-scope dependency** with a named owner, versioned corpus outputs, and its own compute-and-storage line item (typically 5-15% of program dollar cost).
- The pipeline design document has eight sections. Legal sign-off, dedup config, versioning, and named owner are the four sections most often skipped and most often the blockers at review.

The next chapter walks the ablation cadence and the eval waterfall — how the program uses the pipeline output and the compute budget to *learn* between runs, and how it decides whether to launch the main run at all.
