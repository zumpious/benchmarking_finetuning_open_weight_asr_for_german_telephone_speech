# Inference parameters (current 5-model roster)

Effective inference settings for the 5 models the paper reports (Whisper
large-v3, Whisper medium, Parakeet-TDT, Voxtral-Mini, Qwen3-ASR).

## Backend routing

| Model | Backend |
|---|---|
| Whisper large-v3, Whisper medium | Transformers (HF `pipeline`) |
| Parakeet-TDT | NeMo (`ASRModel.transcribe`) |
| Voxtral-Mini, zero-shot | Transformers |
| Voxtral-Mini, fine-tuned (LoRA) | Transformers |
| Qwen3-ASR | vLLM |

Voxtral's zero-shot and fine-tuned requests both route through the
Transformers backend: vLLM has no LoRA support for Voxtral's audio
encoder, so Transformers is used for both, which is able to apply the
adapter the same way training does.

## Parameters common to the Transformers/NeMo client path

| Parameter | Effective value |
|---|---|
| Requested language | `de` |
| Timestamp request | `false` |
| Batch size | 8 |

`language` controls Whisper's generation; the NeMo/Parakeet backend
accepts the field but does not use it.

## Whisper large-v3 / Whisper medium

| Parameter | Effective value |
|---|---|
| Backend | HF `pipeline("automatic-speech-recognition")` |
| Chunk length | 28 s |
| Language | `de` |
| Returned timestamps | `false` |
| Batch size | 8 |
| Model dtype | `float16` (default) |
| Decoding | Beam search, `num_beams=5` (Transformers ASR pipeline default) |

Both models use the identical code path; Whisper medium differs only in
checkpoint size.

## Parakeet-TDT

`nvidia/parakeet-tdt-0.6b-v3`, loaded via NeMo's standard model registry.

| Parameter | Effective value |
|---|---|
| Backend | NeMo `ASRModel.transcribe` |
| Batch size | 8 |
| CUDA memory fraction | 0.4 |
| Device | CUDA when available, else CPU |
| Decoder family | Token-and-Duration Transducer (TDT) |

## Voxtral-Mini (`mistralai/Voxtral-Mini-3B-2507`)

| Parameter | Effective value |
|---|---|
| Backend (zero-shot) | Transformers |
| Backend (fine-tuned) | Transformers |
| Request format | Mistral's structured `TranscriptionRequest` API, not a free-text prompt |
| Language | Passed as a request field, not embedded in text |

## Qwen3-ASR (`Qwen/Qwen3-ASR-1.7B`)

| Parameter | Effective value |
|---|---|
| Backend | vLLM, temperature 0.01, max 4096 tokens |
| Request format | Chat-template string with `<\|audio_start\|><\|audio_pad\|><\|audio_end\|>`; German forced via a `language German<asr_text>` assistant-prefix |
