# WER normalization methodology

The paper reports every WER number under a single normalization profile
(`WER_norm_text_abbr_numbers`, defined in `main.tex`'s Metrics paragraph).
This file describes what that profile does, stage by stage, with worked
examples. No other normalization profile is reported in this repository
(see `sensitivity_to_normalization.md` for the one exception: a
robustness check against the plain, unnormalized metric only).

## WER, corpus-level

$\mathrm{WER} = (S+D+I)/N$ (substitutions, deletions, insertions over
reference word count), computed over the full set of normalized
(reference, hypothesis) pairs for a given model/condition/dataset -- one
corpus-level edit-distance ratio, **not** a mean of per-utterance WER
values. This matters for datasets with very short references: a handful
of one-word utterances can swing a per-utterance mean sharply, but barely
move the corpus-level total.

## The three stages, in application order

The profile applies exactly three stages, in this order: **abbreviations
→ numbers → text**. Each stage's output feeds the next stage's input.
Applied identically to both reference and hypothesis before scoring.

### 1. Abbreviations

Substitutes a fixed table of common German abbreviations and their
spoken-out-loud full forms onto the same short token, e.g. both `z. B.`
and `zum Beispiel` become `zb`; both `bzw.` and `beziehungsweise` become
`bzw`. Case-insensitive matching, applied before lowercasing. Covers
`z.B.`/`zum Beispiel`→`zb`, `d.h.`/`das heißt`→`dh`, `u.a.`/`unter
anderem`→`ua`, `bzw.`/`beziehungsweise`→`bzw`, `usw.`/`und so
weiter`→`usw`, `etc.`/`et cetera`→`etc`, `ca.`/`circa`→`ca`,
`Mio.`/`Million(en)`→`mio`, `Mrd.`/`Milliarde(n)`→`mrd`,
`Kilometer(n)`/`km`→`km`, `Meter(n)`/`m.`→`m`, `Megabit(s)`/`Mbit(s)`→`mbit`,
`Gigabyte(s)`/`GB`→`gb`, and several institutional abbreviations
(`i.T.`→`it`, `s.v.p.`→`svp`, `b.d.p.`→`bdp`, `e.d.u.`→`edu`, `e.U.`→`eu`).

### 2. Numbers

Spells out integer digit sequences in German, e.g. `3` → `drei`, `100` →
`hundert`. Leading-zero sequences are left untouched (phone-fragment/ID
digits, e.g. `0511`, are not spoken-language numbers), as are sequences
longer than 18 digits (typically hallucinated digit runs, not real
numbers).

**Order-dependent limitation, worth knowing:** because the abbreviations
stage runs before the numbers stage, a number spelled out by the numbers
stage cannot retroactively be caught by an abbreviation rule that would
otherwise apply to its spelled-out form. Example: `5000000` becomes
`fünf millionen` (spelled out digit-by-digit-group), not `fünf mio` --
the `mio` abbreviation rule only fires against the literal word
`Millionen` already present in the input text.

### 3. Text

NFKC Unicode normalization, lowercasing, whitespace collapse, and
umlaut/ß folding (`ä`→`ae`, `ö`→`oe`, `ü`→`ue`, `ß`→`ss`).

## What this profile does *not* do

Punctuation is kept, and reference-empty pairs are not filtered -- both
of those are used by an internal `WER_norm_full` profile during
development (see `sensitivity_to_normalization.md`) but are not part of
the paper's canonical profile.

## Worked examples

```
'Das sind z. B. 3 Straßen, oder?'
  -> 'das sind zb drei strassen, oder?'

'Das kostet ca. 5 Millionen Euro, bzw. 5000000 Cent.'
  -> 'das kostet ca fuenf mio euro, bzw fuenf millionen cent.'
  (the order-dependent limitation above: bzw's number is spelled out,
   not abbreviated, because it wasn't the word "Millionen" at input time)

'Ich habe 3 Äpfel und die Straße ist 100 Meter lang.'
  -> 'ich habe drei aepfel und die strasse ist hundert m lang.'

'Wir treffen uns z. B. um 15 Uhr, d.h. pünktlich.'
  -> 'wir treffen uns zb um fuenfzehn uhr, dh puenktlich.'
```

Note punctuation (commas, question marks) survives throughout -- this is
what distinguishes the canonical profile from the internal
`WER_norm_full` variant, which additionally strips it.
