# Online Appendix -- ICASSP 2027

[![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.22857382.svg)](https://doi.org/10.5281/zenodo.22857382)

Supplementary material for *Benchmarking and Fine-Tuning Open-Weight ASR
Foundation Models for Real-World German Telephone Speech*.

## How to cite

This repository is archived on Zenodo: [10.5281/zenodo.22857382](https://doi.org/10.5281/zenodo.22857382).
See [`CITATION.cff`](CITATION.cff) for full citation metadata (or use
GitHub's "Cite this repository" button above the file list).

## What this paper does

Five open-weight foundation ASR models from three architecture families
(encoder-decoder: Whisper large-v3, Whisper medium; Token-and-Duration
Transducer: Parakeet-TDT; speech-LLM: Voxtral-Mini, Qwen3-ASR) are
evaluated under nine synthetic telephone-channel degradations
(band-limitation, additive noise, packet loss), then adapted with LoRA
on a newly collected German telephone corpus (`mIT`). A 28-configuration
LoRA search on `mIT v1` selects three configurations, carried forward to
a five-fold fine-tuning and regression-check study on `mIT v2` across
five German speech corpora (three public, plus `mIT` and CALLHOME
German). Three research questions structure the paper: RQ1 (degradation
robustness across model families/sizes), RQ2 (which LoRA configurations
work, and do they transfer across architectures), RQ3 (does fine-tuning
help under degradation without regressing on clean speech elsewhere).

This repository holds the material that did not fit the paper's 5-page
limit: full per-fold breakdowns, the complete 28-config search, the full
10-condition degradation matrix, normalization methodology, and
inference settings. It does not repeat what is already reasonably
covered in the paper's own tables and text.

## Statement-to-evidence index

One row per citable claim in `main.tex`, in the paper's own section
order, linked to the file that backs it.

**Data and Degradation Conditions**

| Paper claim | Evidence |
|---|---|
| `mIT` corpus: `v1`/`v2` call/segment/duration/speaker/gender counts | [`datasets/mit_v1_v2_summary.md`](datasets/mit_v1_v2_summary.md) |
| Degradation matrix: clean baseline, 4 noise levels, 3 packet-loss rates, 2 combined conditions (`N`, `C10`) -- "full matrix: online appendix" | [`results/degradation/degradation_conditions.md`](results/degradation/degradation_conditions.md) |

**Method, "Baseline selection"**

| Paper claim | Evidence |
|---|---|
| Adaptation-base clean-condition WER: Whisper large-v3 26.67%, Parakeet-TDT 26.32%, Voxtral-Mini 24.28% | [`results/degradation/degradation_conditions.md`](results/degradation/degradation_conditions.md) |

**Method, "LoRA configuration search"**

| Paper claim | Evidence |
|---|---|
| 28 LoRA configurations searched, three fold rotations, Whisper large-v3 only, `mIT v1` -- "all presented in the online appendix" | [`results/lora_search/lora_search_28_configs.md`](results/lora_search/lora_search_28_configs.md) |
| Three configurations carried into the five-fold study | Same file, "Selected for 5-fold fine-tuning" |

**Method, "Metrics"**

| Paper claim | Evidence |
|---|---|
| "Raw ... normalization results are provided in the online appendix" | [`results/finetuning/normalization_methodology.md`](results/finetuning/normalization_methodology.md) (what the canonical profile does, stage by stage, with worked examples) and [`results/finetuning/sensitivity_to_normalization.md`](results/finetuning/sensitivity_to_normalization.md) (every regression-check cell under raw vs. canonical WER) |

**Results, "RQ1: Degradation and architecture"**

| Paper claim | Evidence |
|---|---|
| Clean-condition WER range (24.28-29.74%) and `C10` values per model, on the fixed 1,000-sample subset -- "complete condition sweep is provided in the online appendix" | [`results/degradation/degradation_conditions.md`](results/degradation/degradation_conditions.md) |

**Results, "RQ2a: Which configurations are effective"**

| Paper claim | Evidence |
|---|---|
| All 28 configurations, sorted by mean held-out WER; the attention-output-projection and effective-scaling comparisons; the rank-vs-scaling check | [`results/lora_search/lora_search_28_configs.md`](results/lora_search/lora_search_28_configs.md) |

**Results, "RQ2b: Cross-architecture transfer" (Tab. 2)**

| Paper claim | Evidence |
|---|---|
| Tab. 2 caption: "Full per-fold results: online appendix" | [`results/finetuning/transfer_table.md`](results/finetuning/transfer_table.md) |
| Standard-LoRA vs. rsLoRA pp gaps are small (≤0.36pp) on every architecture | [`results/finetuning/transfer_table.md`](results/finetuning/transfer_table.md) |

**Results, "RQ3a: Fine-tuning gains and fold-0 sensitivity" / "RQ3b: External-corpus regression check" (Tab. 3)**

| Paper claim | Evidence |
|---|---|
| Tab. 3 caption: "Extended table: online appendix" | [`results/finetuning/regressioncheck_table.md`](results/finetuning/regressioncheck_table.md) |
| Per-model relative gain ranges, external-corpora zero-shot WER ranges, CALLHOME/`mIT` clean and `N` ranges | [`results/finetuning/regressioncheck_table.md`](results/finetuning/regressioncheck_table.md) |
| Fold 0 vs. folds 1-4 spread (7.2-10.2pp, mean 9.0pp); excluding fold 0 (1.4-2.0pp mean-WER change, ~4pp to 0.3-1.5pp SD change) | [`results/finetuning/transfer_table.md`](results/finetuning/transfer_table.md) |
| Outlier speaker: 61.0% of fold 0's segments, WER gap vs. rest of fold 0 -- "chart: online appendix" | [`results/finetuning/outlier_speaker/`](results/finetuning/outlier_speaker/) |
| `mIT` per-fold results (clean and `N`) -- "per-fold results: online appendix" | [`results/finetuning/regressioncheck_table.md`](results/finetuning/regressioncheck_table.md), "mIT" section |

## Supporting material (not tied to one specific number in the paper)

| Topic | Evidence |
|---|---|
| Effective inference settings, all 5 models (backend routing, batch size, decoding parameters) | [`results/finetuning/inference_parameters.md`](results/finetuning/inference_parameters.md) |
| Raw-vs-canonical WER normalization sensitivity check, every regression-check cell | [`results/finetuning/sensitivity_to_normalization.md`](results/finetuning/sensitivity_to_normalization.md) |

## Data availability

- **The `mIT` telephone corpus cannot be released.** Real, de-identified
  customer-support calls; only aggregate statistics appear here.
