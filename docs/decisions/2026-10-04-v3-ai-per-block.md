# v3 ch. 8: AI per block and the AI gateway

- **Topic:** Which AI runs each stage, where, and the gateway rules that keep it safe.
- **Inputs:** RS4 §3, §7.5–7.6; RS3 §1–3, §7; and AWS model cards fetched 4 Oct 2026 for Opus 5.5, Sonnet 4.6, Sonnet 5.5, Qwen3-235B, DeepSeek V3.2 and gpt-oss-120b.

## Verified facts (AWS docs, 4 Oct 2026)

| Model | Japan route | Structured outputs | Notes |
|---|---|---|---|
| Claude Opus 5.5 | `jp.anthropic.claude-opus-5-5` (Tokyo↔Osaka) | ❌ | Launched 22 Sep 2026; no batch |
| Claude Sonnet 4.6 | `jp.anthropic.claude-sonnet-4-6` | ✅ | |
| Claude Sonnet 5.5 | **Global only** from Tokyo | ❌ | → synthetic only |
| gpt-oss-120b | In-Region Tokyo | ✅ | "EOL no sooner than" 5 Aug 2026; still Active |
| Qwen3-235B | In-Region Tokyo | ✅ | "EOL no sooner than" 28 Apr 2026; still Active |
| DeepSeek V3.2 | In-Region Tokyo | ✅ | "EOL no sooner than" 1 Dec 2026 |

Lifecycle: AWS gives a legacy period of at least 6 months after an EOL notice, so model swaps are planned changes, not emergencies.

## Gateway rules

- Every call carries a data class (🧪 synthetic / 🏥 real).
- **Real data:** only Japan-region routes (`jp.` / In-Region Tokyo / local), enforced in code **and** by IAM/SCP.
- **Never for real data:** OpenAI US, Sonnet 5/5.5, Opus 5, Gemini Pro (global), Claude-on-Vertex, Azure Data Zone.
- A privacy stamp is required on real-data text.
- Every call records: pinned model ID · prompt version · dictionary version · tokens · cost · latency · cache hit.
- Budget/throughput guard; fake provider for tests; JSON validation + repair for models without structured outputs; no silent provider switch.
- **Any model change → golden-set re-run before activation.** A lifecycle (EOL) watch runs on all models in use.

## Model per stage (starting configuration; G1 picks the winners)

| Stage | Start | Fallback |
|---|---|---|
| B06 ASR | ReazonSpeech-k2 (sherpa-onnx) + kotoba-whisper (whisper.cpp) as two engines; disagreement → review; medical-word biasing; hallucination filter | whisper-large-v3-turbo (MIT) · AWS Transcribe ja-JP Tokyo (comparator) |
| B07 Who spoke | Narration = 1 speaker; prospective = mic channel (console vs room) | pyannote community-1 + human role check; no voiceprints |
| B08 Privacy | Rules + per-case name list (kana readings) + Japanese NER + in-Japan LLM pass + human check | — |
| B09 Moments | ★-anchored (narration); Sonnet 4.6 `jp.` for whole-case (V1) | Opus 5.5 `jp.` |
| B10 Drafting | **A/B in G1:** Opus 5.5 `jp.` vs Sonnet 4.6 `jp.`; k = 3 samples for decision & why | Opus 4.8 In-Region Tokyo |
| B26 Verifier | **Best performer, any origin, from a different family than the drafter.** Candidates: gpt-oss-120b, Qwen3-235B, DeepSeek V3.2 (all In-Region Tokyo) | The next-best candidate |
| Embeddings / reranker | ruri-v3-310m + ruri-v3-reranker (self-hosted in Tokyo) vs Cohere Embed v4 / Rerank 3.5 (Tokyo) | — |
| Summaries / ASK | Sonnet 4.6 `jp.` + cross-family checker | — |

- **Cost:** ≈ $1.3–4.6 per case in LLM calls.

## Karthi's choices

- **Dev models: the same Japan-region models as real data (chosen).** No behaviour drift between dev and pilot. The OpenAI key is used only for labelled side-by-side benchmarks on synthetic data. *Option considered:* any model for synthetic.
- **Verifier: best performer, any origin (chosen).**
  - *Options considered:* non-Claude preferring gpt-oss-120b; Claude-only.
  - Cross-family independence from the drafter is kept as a requirement, since it is the reason a verifier exists.
  - If a Chinese-origin model (Qwen / DeepSeek) wins, this is **disclosed to the partner hospital** before real data. If procurement objects, the next-best candidate is used.
- **Local-LLM hospital-box profile: plan only (chosen).** The gateway can route to local models, which are benchmarked on the 48 GB laptop in dev. The profile is built and operated only if a partner forbids cloud LLMs. *Option considered:* build it in S0.

## Flags

- **Legal opinion needed** before commercial use of ASR models trained on the ReazonSpeech corpus (use limited under Copyright Act Art. 30-4; RS3). Clean fallbacks: whisper-large-v3-turbo (MIT), AWS Transcribe.

**Decided by:** Karthi · 4 Oct 2026


> ⚠ **Amended 6 Oct 2026:** see `2026-10-06-v3-mvp-local-and-gateway.md`. The MVP runs on the two laptops only, and all AI goes through the gateway with switchable profiles (`dev-cloud` · `local` · `pilot-japan`).
