# mIT corpus: v1 vs. v2 summary (size, duration, speakers, gender)

Aggregate corpus statistics for both revisions used in the paper: v1
(the 28-config LoRA search) and v2 (the 5-fold fine-tuning study and
primary test set). Gender figures are summed across each version's 5
cross-validation folds; fold speaker-sets are disjoint for both versions
(0 shared speakers across every fold pair, for both v1 and v2), so the
per-fold sums below involve no double-counting. The mIT corpus itself
cannot be released
(real, de-identified customer-support calls, GDPR); only these
aggregate statistics are published.

## Summary table

| | v1 (`mIT_telephone_data`) | v2 (`mIT_telephone_data_v2`) |
|---|---:|---:|
| Samples (raw, this profile) | 15,098 | 20,724 |
| Samples (actually used, post-filter) | 15,010† | 20,613‡ |
| Duration | 15.05h (54,190.6s) | 21.95h |
| Calls | 363 | 530 |
| Speakers | 279 | 384 |
| Male | 199 speakers, 11,162 samples (73.9%), 11.24h (74.7%) | 266 speakers, 14,865 samples (71.7%), 15.70h (71.5%) |
| Female | 100 speakers, 3,875 samples (25.7%), 3.74h (24.9%) | 142 speakers, 5,734 samples (27.7%), 6.09h (27.7%) |
| Unknown gender (explicit value) | -- (none) | 2 speakers, 31 samples (0.1%), 0.04h (0.2%) |
| Missing gender field | 61 samples (61 of 15,098 rows have no `voice_gender` at all) | 94 samples |

† v1's 15,010 is the actual train+eval+test sum used by the LoRA
configuration search's 3-fold rotation; this profile's 15,098 is the raw
corpus snapshot before the same clips-beyond-30s/empty-transcript filter
`main.tex` documents for v2 (an 88-sample gap), not separately confirmed
for v1.

‡ v2's 20,613 is `main.tex`'s own stated post-filter count ("clips beyond
30 seconds and empty transcripts"), a 111-sample gap from this profile's
raw 20,724. This profile's raw totals (20,724 segments, 21.95h, 384
speakers, 530 calls) match `main.tex`'s published numbers exactly.

## Notes

- Male speakers dominate both versions (~74% of v1's audio, ~72% of
  v2's), roughly proportional across sample count, duration, and speaker
  count within each version.
- v2 has an explicit "unknown" gender value that v1 doesn't (31 samples,
  2 speakers), separate from the "missing field entirely" count (94
  samples) -- two different things in the source data, not a
  discrepancy.

## Per-fold breakdown (v2, the 5-fold fine-tuning study)

| Fold | Samples | Duration (h) | Speakers | Top speaker (% of samples / % of duration) |
|---|---:|---:|---:|---:|
| 0 | 4,146 | 4.34 | 71 | 61.0% / 55.4% |
| 1 | 4,145 | 4.59 | 74 | 54.1% / 44.8% |
| 2 | 4,145 | 4.23 | 78 | 41.3% / 35.7% |
| 3 | 4,143 | 4.36 | 80 | 14.9% / 23.6% |
| 4 | 4,145 | 4.43 | 81 | 13.8% / 13.9% |
| **Total** | **20,724** | **21.95** | **384** | -- |

**This table explains the paper's fold-0-outlier finding** (Results,
"Fine-tuning gains and regression check": fold 0 scores ~9pp worse than
the other four's mean, across every model x configuration combination in
Tab. 2): fold 0 has by far the most extreme single-speaker concentration
of any fold -- 61.0% of its segments from one speaker, versus 13.8-54.1%
for the other four. Per the research team's manual review, this speaker
uses an unusually strong regional dialect.
