# Outlier-speaker chart

Fold 0's dominant speaker contributes 2,530 of fold 0's 4,146 segments
(61.0%); the remaining 1,616 segments come from the rest of fold 0.
`outlier_speaker_wer_gap.{png,pdf}` shows, for each of the three retained
models (Whisper large-v3, Parakeet-TDT, Voxtral-Mini), base and fine-tuned,
clean and `N` (moderately degraded): the outlier speaker's own WER
compared to the rest of fold 0's WER.

`data.json` has the same 12 rows (3 models x 2 variants x 2 conditions)
under the paper's single canonical normalization profile. WER here is a
corpus-level metric per group (outlier speaker's segments scored
together, rest-of-fold segments scored together), not a mean of
per-utterance rates.

## Caveat

Base Voxtral-Mini's 199.99% WER on the outlier speaker under `N` is
high enough to be a hallucination signature (insertion-heavy output on
degraded audio, see the paper's Related Work) rather than genuine
pronunciation difficulty. We have not run an S/D/I error decomposition
to check this, so the dialect explanation for this speaker is not yet
confirmed against that alternative.
