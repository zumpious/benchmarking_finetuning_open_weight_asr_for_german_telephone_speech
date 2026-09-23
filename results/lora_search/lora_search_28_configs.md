# 28-config LoRA search (Whisper large-v3, mIT v1)

The 28 configurations searched in the LoRA configuration search
(paper Sec. "LoRA configuration search"), three fold rotations on the
mIT corpus's earlier, smaller v1 revision (n=15,010), Whisper large-v3
only. Values are the training framework's own `test_wer` percentage
per held-out test fold -- a search/selection artifact reported as-is,
not recomputed under the paper's canonical WER normalization profile
(which all other tables in this appendix use).

**Effective scaling** is the factor multiplying the low-rank update:
`alpha / r` for standard LoRA, `alpha / sqrt(r)` for rsLoRA. It is the
column that explains the ranking, and without it two rows that look
different are in fact the same setting.

Sorted by mean WER, best first.

| Run | r | alpha | rsLoRA | Eff. scaling | Targets | fold 0 (n=3,004) | fold 1 (n=3,008) | fold 2 (n=3,000) | **mean** |
|---|--:|--:|:--:|--:|---|--:|--:|--:|--:|
| `mitlora_r8_a16_targets_q_v_out_fc1_fc2_rslora` | 8 | 16 | yes | **5.66** | q, v, out, fc1, fc2 | 13.29 | 24.99 | 13.86 | **17.38** |
| `mitlora_stage3_r16_a12_qv_out_fc1_fc2_rslora` | 16 | 12 | yes | **3.00** | q, v, out, fc1, fc2 | 12.84 | 25.71 | 14.29 | **17.61** |
| `mitlora_stage3_r8_a45_qv_out_fc1_fc2` | 8 | 45 | no | **5.62** | q, v, out, fc1, fc2 | 13.64 | 25.10 | 14.26 | **17.67** |
| `mitlora_stage3_r16_a16_qv_out_fc1_fc2_rslora` | 16 | 16 | yes | **4.00** | q, v, out, fc1, fc2 | 14.81 | 25.13 | 14.43 | **18.12** |
| `mitlora_r16_a32_targets_q_v_out_fc1_fc2` | 16 | 32 | no | **2.00** | q, v, out, fc1, fc2 | 15.21 | 25.66 | 14.00 | **18.29** |
| `mitlora_targets_q_v_fc1_fc2` | 8 | 16 | no | **2.00** | q, v, fc1, fc2 | 14.71 | 26.47 | 14.63 | **18.60** |
| `mitlora_stage3_r8_a16_qv_fc2` | 8 | 16 | no | **2.00** | q, v, fc2 | 15.09 | 26.34 | 14.65 | **18.69** |
| `mitlora_stage3_r8_a6_qv_out_fc1_fc2_rslora` | 8 | 6 | yes | **2.12** | q, v, out, fc1, fc2 | 15.05 | 26.95 | 14.27 | **18.76** |
| `mitlora_stage3_r16_a8_qv_out_fc1_fc2_rslora` | 16 | 8 | yes | **2.00** | q, v, out, fc1, fc2 | 14.54 | 26.08 | 16.02 | **18.88** |
| `mitlora_targets_q_v_out_fc1_fc2` | 8 | 16 | no | **2.00** | q, v, out, fc1, fc2 | 15.14 | 27.37 | 14.54 | **19.02** |
| `mitlora_stage3_r8_a16_qv_fc1` | 8 | 16 | no | **2.00** | q, v, fc1 | 15.34 | 27.36 | 14.87 | **19.19** |
| `mitlora_targets_q_k_v_out` | 8 | 16 | no | **2.00** | q, k, v, out | 15.47 | 27.00 | 15.15 | **19.21** |
| `mitlora_stage3_r8_a16_qv_out` | 8 | 16 | no | **2.00** | q, v, out | 15.52 | 26.77 | 15.34 | **19.21** |
| `mitlora_r16_a32_targets_q_k_v_out` | 16 | 32 | no | **2.00** | q, k, v, out | 15.46 | 26.86 | 15.39 | **19.24** |
| `mitlora_r16_a32_targets_v_out` | 16 | 32 | no | **2.00** | v, out | 15.61 | 27.09 | 15.20 | **19.30** |
| `mitlora_r16_a32_targets_q_v_fc1_fc2` | 16 | 32 | no | **2.00** | q, v, fc1, fc2 | 15.40 | 27.49 | 15.51 | **19.47** |
| `mitlora_targets_v_out` | 8 | 16 | no | **2.00** | v, out | 15.56 | 27.39 | 15.63 | **19.53** |
| `mitlora_stage2_s18_r32_a11_qv_rslora` | 32 | 11 | yes | **1.94** | q, v | 16.10 | 28.19 | 15.41 | **19.90** |
| `mitlora_lr3e5_cosine_q_v` | 8 | 16 | no | **2.00** | q, v | 13.71 | 29.59 | 16.70 | **20.00** |
| `mitlora_r16_a32_q_v` | 16 | 32 | no | **2.00** | q, v | 16.03 | 28.44 | 15.87 | **20.11** |
| `mitlora_r32_a64_q_v` | 32 | 64 | no | **2.00** | q, v | 16.03 | 28.88 | 16.07 | **20.33** |
| `mitlora_stage3_r8_a6_qv_rslora` | 8 | 6 | yes | **2.12** | q, v | 16.37 | 28.33 | 16.61 | **20.44** |
| `mitlora_dropout0_q_v` | 8 | 16 | no | **2.00** | q, v | 16.36 | 28.47 | 16.69 | **20.51** |
| `mitlora_dropout0p15_q_v` | 8 | 16 | no | **2.00** | q, v | 16.47 | 28.45 | 16.75 | **20.56** |
| `mitlora_dropout0p1_q_v` | 8 | 16 | no | **2.00** | q, v | 16.47 | 28.43 | 16.77 | **20.56** |
| `mitlora_lr1e5_cosine_q_v` | 8 | 16 | no | **2.00** | q, v | 16.83 | 28.63 | 17.31 | **20.92** |
| `mitlora_r4_a8_q_v` | 4 | 8 | no | **2.00** | q, v | 17.22 | 29.08 | 17.19 | **21.16** |
| `mitlora_lr3e6_cosine_q_v` | 8 | 16 | no | **2.00** | q, v | 17.59 | 30.28 | 18.27 | **22.05** |

## What the search shows

Two factors account for almost all of the spread.

| Factor | Group | n | Mean WER |
|---|---|--:|--:|
| Attention output projection | targets include `out_proj` | 13 | **18.63** |
| | targets exclude `out_proj` | 15 | 20.17 |
| Effective scaling | > 4 | 2 | **17.52** |
| | <= 2.5 | 24 | 19.75 |

The five best configurations all target `q, v, out, fc1, fc2`. The four worst all
target `q, v` only at effective scaling 2.0. Learning rate and dropout sweeps
moved the mean by less than either factor.

Rank on its own does **not** predict quality: r4, r8, r16 and r32 at scaling 2.0
all land between 18.3 and 22.1. What matters is the scaling the rank is paired
with.

## Selected for 5-fold fine-tuning

| Plot name | Configuration | Eff. scaling |
|---|---|--:|
| LoRA, normal scaling | r=8, alpha=16, `q, v, fc1, fc2` | 2.000 |
| LoRA, higher scaling | r=8, alpha=45, `q, v, out, fc1, fc2` | 5.625 |
| RSLoRA, higher scaling | rsLoRA r=8, alpha=16, `q, v, out, fc1, fc2` | 5.657 |

The last two differ by 0.6% in effective scaling; on the 5-fold study
(`../finetuning/transfer_table.md`) their results differ by 0.01-0.36pp
across the three architectures. They are one setting reached two ways.

## Caveats

- The search ran on mIT v1 with three fold rotations, kept smaller for
  compute reasons (28 candidate configs x 3 folds is already 84 training
  runs); the 5-fold fine-tuning study evaluates on mIT v2 with five folds.
  Configurations were selected on one corpus revision and validated on
  another -- see `../../datasets/mit_v1_v2_summary.md` for how the two
  revisions differ.
- Fold 1 is the hard fold in mIT v1. In mIT v2 the hard fold is fold 0
  instead, driven by a single high-frequency, markedly-dialectal speaker
  (see the paper's Results, "Fine-tuning gains and regression check").
  Both revisions land on the same kind of hard fold: one dialectal
  speaker driving most of the spread, not the fold's position in the
  split. The paper only discusses the mIT v2 case.
