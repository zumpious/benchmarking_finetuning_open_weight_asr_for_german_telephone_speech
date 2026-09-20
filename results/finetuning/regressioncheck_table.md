# Regression check: Whisper, Parakeet-TDT, Voxtral (Tab. 3)

Full per-fold breakdown behind the paper's Tab. 3: the carried-forward
higher-scaling LoRA adapter evaluated against 4 external corpora
(clean and moderately-degraded/`N`) plus CALLHOME and mIT, mean and
sample standard deviation (ddof=1) across the 5 fold-adapters. Reported
under the paper's canonical WER normalization profile throughout
(Sec. "Metrics" in `main.tex`).

**Bold** marks the lower of Base/FT for each model per row -- whether
fine-tuning helped or regressed on that dataset/condition.

| Dataset | Cond. | Whisper Base | Whisper FT (all 5) | Whisper $\Delta$ | Parakeet Base | Parakeet FT (all 5) | Parakeet $\Delta$ | Voxtral Base | Voxtral FT (all 5) | Voxtral $\Delta$ |
|---|---|---:|---:|---:|---:|---:|---:|---:|---:|---:|
| FLEURS | C | **16.41** | 17.47±0.52 | +1.06 | **17.18** | 17.22±0.02 | +0.04 | **17.13** | 17.40±0.56 | +0.27 |
|  | N | **25.69** | 28.00±2.07 | +2.31 | 26.45 | **26.43±0.13** | -0.02 | **25.64** | 27.53±1.02 | +1.89 |
| Tuda-De | C | **20.09** | 20.95±0.75 | +0.86 | **13.05** | 13.08±0.04 | +0.03 | **21.01** | 21.56±0.71 | +0.55 |
|  | N | **26.79** | 28.39±1.78 | +1.60 | **18.82** | 19.53±0.10 | +0.71 | **27.66** | 29.42±1.26 | +1.76 |
| VoxPopuli | C | 21.62 | **20.87±0.16** | -0.75 | **17.54** | 17.92±0.02 | +0.38 | **20.26** | 21.57±0.90 | +1.31 |
|  | N | **23.10** | 24.22±0.72 | +1.12 | **22.65** | 22.79±0.11 | +0.14 | **24.30** | 26.49±1.84 | +2.19 |
| CALLHOME | C | 45.24 | **37.90±1.09** | -7.34 | 46.24 | **44.90±0.10** | -1.34 | 40.44 | **37.62±0.72** | -2.82 |
|  | N | 87.85 | **59.35±2.77** | -28.51 | 61.84 | **56.41±0.35** | -5.43 | 97.86 | **59.78±3.51** | -38.09 |
| mIT | C | 28.81±4.95 | **22.09±4.02** | -6.72 | 28.47±4.45 | **21.76±3.86** | -6.71 | 33.45±13.57 | **21.78±4.65** | -11.67 |
|  | N | 79.90±12.19 | **52.77±7.95** | -27.13 | 53.61±6.91 | **43.07±6.29** | -10.54 | 108.74±33.80 | **56.24±7.50** | -52.50 |

## Individual per-fold values

The table above collapses the 5 fold-trained LoRA checkpoints into one
mean±std per dataset/condition. For the four external corpora, the
zero-shot base model does not vary by fold (no LoRA checkpoint applied),
so only "FT" varies below; for mIT, evaluation is fold-matched (each
checkpoint scored on its own held-out fold), so both Base and FT vary.

All values under the paper's canonical WER normalization profile only;
no other profile is reported here.


### FLEURS

**Clean** -- base WER is a single value (no fold-trained checkpoint on FLEURS at zero-shot); FT WER varies by which fold's checkpoint is applied:

| Model | Base | FT fold 0 | FT fold 1 | FT fold 2 | FT fold 3 | FT fold 4 | FT mean | FT std |
|---|--:|--:|--:|--:|--:|--:|--:|--:|
| Whisper | 16.41 | 17.18 | 17.26 | 17.31 | 17.20 | 18.40 | **17.47** | 0.52 |
| Parakeet | 17.18 | 17.21 | 17.23 | 17.19 | 17.24 | 17.22 | **17.22** | 0.02 |
| Voxtral | 17.13 | 17.19 | 16.97 | 18.39 | 17.18 | 17.29 | **17.40** | 0.56 |

**N (moderately degraded)** -- base WER is a single value (no fold-trained checkpoint on FLEURS at zero-shot); FT WER varies by which fold's checkpoint is applied:

| Model | Base | FT fold 0 | FT fold 1 | FT fold 2 | FT fold 3 | FT fold 4 | FT mean | FT std |
|---|--:|--:|--:|--:|--:|--:|--:|--:|
| Whisper | 25.69 | 26.49 | 27.07 | 27.05 | 27.79 | 31.61 | **28.00** | 2.07 |
| Parakeet | 26.45 | 26.38 | 26.33 | 26.45 | 26.32 | 26.65 | **26.43** | 0.13 |
| Voxtral | 25.64 | 26.22 | 26.66 | 28.31 | 28.39 | 28.09 | **27.53** | 1.02 |

### Tuda-De

**Clean** -- base WER is a single value (no fold-trained checkpoint on Tuda-De at zero-shot); FT WER varies by which fold's checkpoint is applied:

| Model | Base | FT fold 0 | FT fold 1 | FT fold 2 | FT fold 3 | FT fold 4 | FT mean | FT std |
|---|--:|--:|--:|--:|--:|--:|--:|--:|
| Whisper | 20.09 | 20.58 | 20.50 | 20.96 | 20.49 | 22.25 | **20.95** | 0.75 |
| Parakeet | 13.05 | 13.10 | 13.05 | 13.04 | 13.07 | 13.14 | **13.08** | 0.04 |
| Voxtral | 21.01 | 20.87 | 20.86 | 22.54 | 21.74 | 21.82 | **21.56** | 0.71 |

**N (moderately degraded)** -- base WER is a single value (no fold-trained checkpoint on Tuda-De at zero-shot); FT WER varies by which fold's checkpoint is applied:

| Model | Base | FT fold 0 | FT fold 1 | FT fold 2 | FT fold 3 | FT fold 4 | FT mean | FT std |
|---|--:|--:|--:|--:|--:|--:|--:|--:|
| Whisper | 26.79 | 27.76 | 27.16 | 27.75 | 27.75 | 31.53 | **28.39** | 1.78 |
| Parakeet | 18.82 | 19.51 | 19.40 | 19.53 | 19.54 | 19.68 | **19.53** | 0.10 |
| Voxtral | 27.66 | 28.74 | 28.07 | 29.13 | 31.37 | 29.80 | **29.42** | 1.26 |

### VoxPopuli

**Clean** -- base WER is a single value (no fold-trained checkpoint on VoxPopuli at zero-shot); FT WER varies by which fold's checkpoint is applied:

| Model | Base | FT fold 0 | FT fold 1 | FT fold 2 | FT fold 3 | FT fold 4 | FT mean | FT std |
|---|--:|--:|--:|--:|--:|--:|--:|--:|
| Whisper | 21.62 | 21.04 | 21.01 | 20.86 | 20.71 | 20.72 | **20.87** | 0.16 |
| Parakeet | 17.54 | 17.93 | 17.91 | 17.95 | 17.90 | 17.92 | **17.92** | 0.02 |
| Voxtral | 20.26 | 20.64 | 20.66 | 22.12 | 22.67 | 21.77 | **21.57** | 0.90 |

**N (moderately degraded)** -- base WER is a single value (no fold-trained checkpoint on VoxPopuli at zero-shot); FT WER varies by which fold's checkpoint is applied:

| Model | Base | FT fold 0 | FT fold 1 | FT fold 2 | FT fold 3 | FT fold 4 | FT mean | FT std |
|---|--:|--:|--:|--:|--:|--:|--:|--:|
| Whisper | 23.10 | 23.97 | 24.00 | 23.79 | 23.83 | 25.49 | **24.22** | 0.72 |
| Parakeet | 22.65 | 22.67 | 22.82 | 22.92 | 22.68 | 22.87 | **22.79** | 0.11 |
| Voxtral | 24.30 | 25.05 | 24.81 | 28.23 | 28.70 | 25.65 | **26.49** | 1.84 |

### CALLHOME

**Clean** -- base WER is a single value (no fold-trained checkpoint on CALLHOME at zero-shot); FT WER varies by which fold's checkpoint is applied:

| Model | Base | FT fold 0 | FT fold 1 | FT fold 2 | FT fold 3 | FT fold 4 | FT mean | FT std |
|---|--:|--:|--:|--:|--:|--:|--:|--:|
| Whisper | 45.24 | 39.33 | 36.43 | 37.93 | 38.42 | 37.39 | **37.90** | 1.09 |
| Parakeet | 46.24 | 44.88 | 45.05 | 44.95 | 44.76 | 44.88 | **44.90** | 0.10 |
| Voxtral | 40.44 | 37.03 | 38.14 | 36.94 | 37.40 | 38.60 | **37.62** | 0.72 |

**N (moderately degraded)** -- base WER is a single value (no fold-trained checkpoint on CALLHOME at zero-shot); FT WER varies by which fold's checkpoint is applied:

| Model | Base | FT fold 0 | FT fold 1 | FT fold 2 | FT fold 3 | FT fold 4 | FT mean | FT std |
|---|--:|--:|--:|--:|--:|--:|--:|--:|
| Whisper | 87.85 | 60.96 | 56.10 | 61.20 | 61.89 | 56.57 | **59.35** | 2.77 |
| Parakeet | 61.84 | 56.22 | 55.96 | 56.37 | 56.66 | 56.85 | **56.41** | 0.35 |
| Voxtral | 97.86 | 59.14 | 54.13 | 60.68 | 63.35 | 61.60 | **59.78** | 3.51 |

### mIT (fold-matched: each checkpoint evaluated on its own held-out fold)

**Clean**

| Model | Base fold 0 | Base f1-4 mean | FT fold 0 | FT fold 1 | FT fold 2 | FT fold 3 | FT fold 4 | FT mean | FT std |
|---|--:|--:|--:|--:|--:|--:|--:|--:|--:|
| Whisper | 37.03 | 26.75 | 29.21 | 20.51 | 19.64 | 20.08 | 21.00 | **22.09** | 4.02 |
| Parakeet | 36.38 | 26.49 | 28.65 | 19.97 | 19.68 | 20.05 | 20.47 | **21.76** | 3.86 |
| Voxtral | 57.18 | 27.51 | 29.85 | 20.08 | 17.96 | 19.84 | 21.17 | **21.78** | 4.65 |

**N (moderately degraded)**

| Model | Base fold 0 | Base f1-4 mean | FT fold 0 | FT fold 1 | FT fold 2 | FT fold 3 | FT fold 4 | FT mean | FT std |
|---|--:|--:|--:|--:|--:|--:|--:|--:|--:|
| Whisper | 99.65 | 74.96 | 65.76 | 45.98 | 52.91 | 46.60 | 52.59 | **52.77** | 7.95 |
| Parakeet | 65.60 | 50.61 | 53.90 | 37.57 | 40.34 | 41.77 | 41.76 | **43.07** | 6.29 |
| Voxtral | 162.01 | 95.42 | 68.36 | 51.34 | 49.13 | 57.55 | 54.82 | **56.24** | 7.50 |
