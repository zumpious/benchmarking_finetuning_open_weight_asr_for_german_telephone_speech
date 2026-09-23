# Zero-shot degradation sweep: 5 foundation models, 10 conditions

Zero-shot degradation sweep on a 1000-utterance subset of the mIT
telephone corpus (v1, fold 0 only; a single fixed subset, not
cross-validated across folds). Reported under two profiles: the plain,
unnormalized WER (`WER_raw`), and the paper's canonical normalization
profile (lowercased, NFKC/umlaut-folded, whitespace collapsed, German
abbreviations and spelled-out numbers normalized, punctuation kept;
Sec. "Metrics" in `main.tex`), which the paper's own numbers use
throughout.

Conditions: clean baseline (`C`); 4 additive white-noise SNR levels (20,
10, 5, 0 dB -- lower dB is harder); 3 packet-loss rates (5%, 10%, 20%,
no other degradation); and 2 combined conditions (8 kHz band-pass +
resample, G.711 codec, 10% packet loss with jitter, at 10 dB and 5 dB
additive noise respectively). `Combined 10dB` is the condition used for
the regression check (`N` in Tab. 3 of the paper); `Combined 5dB` is the
paper's worst-case condition (`C10`), one SNR step harder, used only for
the zero-shot sweep answering RQ1 -- at 5 dB SNR, "does fine-tuning
help" and "does the model survive at all" become conflated, so the
regression check uses the milder `N` instead.

## Canonical profile (paper's numbers)

| Model | Clean | Noise 20dB | Noise 10dB | Noise 5dB | Noise 0dB | PktLoss 5% | PktLoss 10% | PktLoss 20% | Combined 10dB (N) | Combined 5dB (C10) |
|---|---:|---:|---:|---:|---:|---:|---:|---:|---:|---:|
| Whisper large-v3 | 26.67 | 29.16 | 40.35 | 69.21 | 108.61 | 35.79 | 36.56 | 47.40 | 80.34 | 99.22 |
| Whisper medium | 29.74 | 39.33 | 69.14 | 80.60 | 138.19 | 43.66 | 40.35 | 50.82 | 86.94 | 118.94 |
| Parakeet-TDT | 26.32 | 30.03 | 37.97 | 45.60 | 56.69 | 28.76 | 29.64 | 35.85 | 46.79 | 56.86 |
| Voxtral-Mini | 24.28 | 48.45 | 78.31 | 121.51 | 223.97 | 27.03 | 28.14 | 36.45 | 79.54 | 57.19 |
| Qwen3-ASR | 28.15 | 32.59 | 40.15 | 123.40 | 125.11 | 31.31 | 32.97 | 41.73 | 50.60 | 59.98 |

## WER_raw (plain, unnormalized)

| Model | Clean | Noise 20dB | Noise 10dB | Noise 5dB | Noise 0dB | PktLoss 5% | PktLoss 10% | PktLoss 20% | Combined 10dB (N) | Combined 5dB (C10) |
|---|---:|---:|---:|---:|---:|---:|---:|---:|---:|---:|
| Whisper large-v3 | 29.91 | 32.29 | 43.27 | 72.26 | 111.30 | 39.16 | 40.00 | 50.70 | 83.18 | 101.84 |
| Whisper medium | 33.01 | 42.40 | 71.99 | 83.31 | 140.75 | 46.80 | 43.63 | 53.72 | 89.68 | 121.42 |
| Parakeet-TDT | 29.87 | 33.45 | 41.20 | 48.60 | 59.00 | 32.36 | 33.13 | 39.00 | 49.83 | 59.46 |
| Voxtral-Mini | 27.45 | 51.53 | 81.20 | 124.19 | 226.47 | 30.26 | 31.43 | 39.57 | 82.37 | 59.81 |
| Qwen3-ASR | 33.14 | 37.71 | 45.01 | 127.77 | 128.68 | 36.23 | 38.00 | 46.27 | 54.75 | 63.41 |

CrisperWhisper, Whisper-primeline, Parakeet-primeline, and a German CTC
fine-tune of XLSR-53 exist as established German ASR fine-tunes but are
out of scope for this paper, which focuses on foundation models (Related
Work, "Existing German ASR fine-tunes," `main.tex`); their results are
not reported here.

WER above 100% reflects fabricated insertions by autoregressive decoders
on degraded or silent input, not literal error rates -- see the paper's
Related Work ("Hallucination").

Model ranking is not stable across degradation types. At Noise 5dB
alone, Voxtral-Mini and Qwen3-ASR are the two worst models (121.51%,
123.40%); at Combined 5dB (C10) they're two of the three best (57.19%,
59.98%), and both Whisper checkpoints are worst instead. C10 adds
band-limiting and packet loss on top of noise, so a model's C10 rank
does not predict its rank under noise alone. The paper reports only
Clean and C10.
