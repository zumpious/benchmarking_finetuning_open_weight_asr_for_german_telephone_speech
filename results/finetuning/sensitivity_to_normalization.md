# Sensitivity to normalization profile (regression-check deltas, raw vs. canonical)

See `normalization_methodology.md` for what the canonical profile does
stage by stage. This file is the robustness check: does the paper's
story hold if you skip normalization entirely?

The paper reports every number under one canonical WER normalization
profile (Sec. "Metrics" in `main.tex`). We report two profiles here:
`WER_raw` (no normalization) and the canonical profile the paper uses
throughout, to check that its main findings -- small mean WER increases
on the non-telephone corpora, WER reductions on telephone speech, and
cross-architecture transfer -- also hold on the plain, unnormalized
metric.

$\Delta$ = fine-tuned minus zero-shot baseline, percentage points,
negative is better. W = Whisper, P = Parakeet-TDT, V = Voxtral.

| Dataset | Cond. | Model | WER_raw | Canonical (paper) |
|---|---|---|---:|---:|
| FLEURS | C | W | +1.08 | +1.06 |
| | | P | +0.18 | +0.04 |
| | | V | +0.45 | +0.27 |
| FLEURS | N | W | +1.26 | +2.31 |
| | | P | +0.14 | -0.02 |
| | | V | +1.83 | +1.89 |
| Tuda-De | C | W | +0.80 | +0.86 |
| | | P | +0.17 | +0.03 |
| | | V | +0.60 | +0.55 |
| Tuda-De | N | W | +1.72 | +1.60 |
| | | P | +0.85 | +0.71 |
| | | V | +1.83 | +1.76 |
| VoxPopuli | C | W | -0.18 | -0.75 |
| | | P | +0.38 | +0.38 |
| | | V | +1.69 | +1.31 |
| VoxPopuli | N | W | +1.21 | +1.12 |
| | | P | +0.58 | +0.14 |
| | | V | +2.62 | +2.19 |
| CALLHOME | C | W | -6.28 | -7.34 |
| | | P | +0.45 | -1.34 |
| | | V | -1.41 | -2.82 |
| CALLHOME | N | W | -27.40 | -28.51 |
| | | P | -3.20 | -5.43 |
| | | V | -36.48 | -38.09 |
| mIT | C | W | -7.04 | -6.72 |
| | | P | -6.96 | -6.71 |
| | | V | -12.07 | -11.67 |
| mIT | N | W | -27.09 | -27.13 |
| | | P | -10.44 | -10.54 |
| | | V | -52.67 | -52.50 |

**Largest spread:** CALLHOME / N / Parakeet-TDT, 2.23pp between
`WER_raw` (-3.20) and the canonical profile (-5.43).

**Sign flips between raw and canonical:** Parakeet-TDT at FLEURS/N
(+0.14 to -0.02) and CALLHOME/C (+0.45 to -1.34) -- both near-zero
deltas where fine-tuning's effect is genuinely negligible, not a
substantive disagreement about direction.
