# LoRA configuration transfer: Whisper, Parakeet-TDT, Voxtral (Tab. 2)

Full per-fold breakdown behind the paper's Tab. 2: zero-shot baseline and
the three carried-forward LoRA configurations, mIT v2 clean condition,
5-fold cross-validation, fold 0 shown separately from folds 1-4 (fold 0
carries an outlier speaker -- see the paper's Results, "Fine-tuning gains
and regression check"). Reported under the paper's canonical WER
normalization profile throughout (Sec. "Metrics" in `main.tex`).

**Bold** marks the lowest WER in each fold-group column, among the 3 LoRA
configuration rows (zero-shot baseline excluded).

| Configuration | Whisper fold 0 | Whisper fold 1-4 | Whisper all 5 | Parakeet fold 0 | Parakeet fold 1-4 | Parakeet all 5 | Voxtral fold 0 | Voxtral fold 1-4 | Voxtral all 5 |
|---|---:|---:|---:|---:|---:|---:|---:|---:|---:|
| Zero-shot baseline | 37.03 | 26.75±2.11 | 28.81±4.95 | 36.38 | 26.49±0.57 | 28.47±4.45 | 57.18 | 27.51±3.31 | 33.45±13.57 |
| LoRA, normal scaling | **27.87** | 20.67±0.70 | 22.11±3.27 | 29.82 | 20.82±0.31 | 22.62±4.03 | 31.27 | 21.12±1.52 | 23.15±4.73 |
| LoRA, higher scaling (carried forward) | 29.21 | 20.31±0.58 | 22.09±4.02 | 28.65 | **20.04±0.33** | **21.76±3.86** | **29.85** | **19.76±1.34** | **21.78±4.65** |
| rsLoRA, higher scaling | 28.83 | **20.00±1.07** | **21.77±4.06** | **28.64** | 20.05±0.35 | 21.77±3.85 | 30.02 | 20.16±0.85 | 22.14±4.47 |
| **$\Delta$ carried-forward config vs. baseline (all 5)** | -- | -- | **-6.72** | -- | -- | **-6.71** | -- | -- | **-11.67** |

The two higher-scaling variants (plain LoRA and rsLoRA) differ only
slightly on any architecture: rsLoRA is marginally lower on Whisper (by
0.32pp), plain higher scaling is marginally lower on Parakeet and
Voxtral (by 0.01 and 0.36pp) -- both share the same high-effective-
scaling, output-projection-targeting regime and can be treated as one
setting reached two ways.

## Individual per-fold values

The table above collapses folds 1-4 into one mean±std. The tables below
give each of the 5 individual fold values, per model.

### Whisper large-v3

| Configuration | fold 0 | fold 1 | fold 2 | fold 3 | fold 4 | mean (all 5) | std (all 5) | mean (excl. fold 0) |
|---|--:|--:|--:|--:|--:|--:|--:|--:|
| Zero-shot baseline | 37.03 | 28.57 | 23.87 | 28.05 | 26.50 | **28.81** | 4.95 | 26.75 |
| LoRA, normal scaling | 27.87 | 21.44 | 19.90 | 20.30 | 21.05 | **22.11** | 3.27 | 20.67 |
| LoRA, higher scaling (carried forward) | 29.21 | 20.51 | 19.64 | 20.08 | 21.00 | **22.09** | 4.02 | 20.31 |
| rsLoRA, higher scaling | 28.83 | 18.49 | 20.45 | 20.11 | 20.95 | **21.77** | 4.06 | 20.00 |

### Parakeet-TDT

| Configuration | fold 0 | fold 1 | fold 2 | fold 3 | fold 4 | mean (all 5) | std (all 5) | mean (excl. fold 0) |
|---|--:|--:|--:|--:|--:|--:|--:|--:|
| Zero-shot baseline | 36.38 | 26.51 | 26.11 | 26.06 | 27.28 | **28.47** | 4.45 | 26.49 |
| LoRA, normal scaling | 29.82 | 20.93 | 20.48 | 20.68 | 21.18 | **22.62** | 4.03 | 20.82 |
| LoRA, higher scaling (carried forward) | 28.65 | 19.97 | 19.68 | 20.05 | 20.47 | **21.76** | 3.86 | 20.04 |
| rsLoRA, higher scaling | 28.64 | 20.07 | 19.62 | 20.06 | 20.46 | **21.77** | 3.85 | 20.05 |

### Voxtral-Mini

| Configuration | fold 0 | fold 1 | fold 2 | fold 3 | fold 4 | mean (all 5) | std (all 5) | mean (excl. fold 0) |
|---|--:|--:|--:|--:|--:|--:|--:|--:|
| Zero-shot baseline | 57.18 | 32.45 | 25.64 | 25.62 | 26.35 | **33.45** | 13.57 | 27.51 |
| LoRA, normal scaling | 31.27 | 22.00 | 18.95 | 21.21 | 22.34 | **23.15** | 4.73 | 21.12 |
| LoRA, higher scaling (carried forward) | 29.85 | 20.08 | 17.96 | 19.84 | 21.17 | **21.78** | 4.65 | 19.76 |
| rsLoRA, higher scaling | 30.02 | 20.89 | 18.94 | 20.27 | 20.54 | **22.14** | 4.47 | 20.16 |

All values under the paper's canonical WER normalization profile only;
no other profile is reported here.
