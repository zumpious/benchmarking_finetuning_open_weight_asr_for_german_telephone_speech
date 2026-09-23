# LoRA target-module mapping (Whisper -> Parakeet-TDT / Voxtral-Mini)

The paper's Method section ("Fine-tuning protocol") states that
target-module names are "mapped to the closest architecture-specific
equivalents" when the three carried-forward LoRA configurations
(searched on Whisper large-v3) are applied to Parakeet-TDT and
Voxtral-Mini. This file gives the literal module names used for each
architecture.

## Mapping table

| Whisper | Parakeet-TDT | Voxtral-Mini |
|---|---|---|
| `q_proj` | `linear_q` | `q_proj` |
| `v_proj` | `linear_v` | `v_proj` |
| `out_proj` | `linear_out` | `out_proj` (audio encoder), `o_proj` (language decoder) |
| `fc1` | `linear1` | `fc1` (audio encoder), `gate_proj` + `up_proj` (language decoder) |
| `fc2` | `linear2` | `fc2` (audio encoder), `down_proj` (language decoder) |

**Parakeet-TDT note:** the names above are the configured aliases passed
to the training pipeline; the actual resolved target modules (as
recorded by the training framework's own input configuration for each
run) are `linear_q`, `linear_v`, `linear_out`, `linear1`, and `linear2`.

**Voxtral-Mini note:** the complete target-module list actually applied
is `q_proj`, `v_proj`, `out_proj`, `o_proj`, `fc1`, `fc2`, `gate_proj`,
`up_proj`, `down_proj` -- nine names, not five. Voxtral-Mini combines a
Whisper-like audio encoder with a Mistral language decoder, so `out_proj`,
`fc1`, and `fc2` address the audio-encoder tower while `o_proj`,
`gate_proj`, `up_proj`, and `down_proj` address the Mistral decoder
tower; `q_proj` and `v_proj` occur in both towers and are targeted in
both.

## Why the target set differs in size across architectures

Whisper and Parakeet-TDT each have one attention-output projection and
one two-layer FFN per targeted block, so five Whisper-side names map to
five architecture-specific names apiece. Voxtral-Mini's language
decoder uses a gated FFN (SwiGLU-style, three projections instead of
Whisper's two) and has a separate attention-output projection from its
audio encoder, so its equivalent of "target `out`, `fc1`, `fc2`" expands
to seven names once both towers are covered, plus the two names
(`q_proj`, `v_proj`) shared with the audio encoder.
