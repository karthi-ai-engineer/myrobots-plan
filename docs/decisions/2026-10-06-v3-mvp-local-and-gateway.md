# v3: MVP runs on the laptops · AI is swappable through the gateway

- **Topic:** Where the MVP runs, and how the product uses AI models during development, the demo and later.
- **Context:** Karthi, 5–6 Oct 2026:
  - No cloud environment for development and the demo; expand to the real environment after the MVP.
  - Local model servers are not available yet, so for faster development the AI should sit behind a **separate gateway module**. The application then runs independently of local or cloud models, and models are easy to exchange.

## Decisions

1. **MVP hosting: the two laptops only.**
   - No cloud hosting for development or the demo (no AWS environment, no Cloudflare demo host).
   - **Surface Laptop 7** (16 GB): screens, backend, Postgres, light jobs.
   - **ASUS Zenbook** (48 GB, Snapdragon X Elite / X2 Elite, ARM64): AI models and heavy speech jobs.
   - After the MVP is approved, the same code moves to the real environment (AWS Tokyo, real-data account) as decided in ch. 9.
2. **All AI goes through the AI Gateway module** (`core-ai-gateway.md`, extended). The application never calls a model directly.
   - **Task menu:** blocks request *tasks* (`transcribe`, `classify_answer`, `draft_card`, `verify_field`, `map_term`, `summarize`, `embed`, `rerank`), never a model.
   - **One contract per task:** the same JSON input and output schema whichever model answers. Models without structured outputs → validate and repair inside the gateway.
   - **Provider adapters** (plug-ins):
     - local (llama.cpp / OpenAI-compatible local server; sherpa-onnx / whisper.cpp for speech; ONNX Runtime for embeddings);
     - Anthropic API · OpenAI API · AWS Bedrock (`jp.` / In-Region Tokyo);
     - fake (tests).
   - **Profiles chosen by one setting** (`AI_PROFILE`):

     | Profile | What it uses | Data allowed |
     |---|---|---|
     | `dev-cloud` | Claude / OpenAI APIs | 🧪 synthetic only |
     | `local` | Open models on the ASUS laptop | 🧪 synthetic or 🏥 real |
     | `pilot-japan` | Japan-region models only (ch. 8) | 🧪 synthetic or 🏥 real |

   - **Hard rule regardless of profile:** data marked 🏥 real is refused by any non-Japan provider (guardrail 🇯🇵, enforced in code; in the pilot also by IAM/SCP).
   - **Per-model prompt variants** live inside the gateway, versioned per task.
   - **Every call is logged:** provider, model ID, prompt version, tokens, cost, latency.
   - **Every swap re-runs the golden test set** and reports scores before the new profile is used.
   - **Embeddings:** one index per embedding model, so switching embedding provider means re-indexing.
3. **Development uses `dev-cloud` for speed.** It is safe because MVP data is synthetic only.
   - Claude Code (coding help) and AI-assisted *synthetic* script writing remain allowed dev tools. No patient data is ever involved.
4. **Demo profile: target `local`** ("nothing leaves this laptop": the strongest privacy story, and the ASUS is the same class of machine as the CEO's planned OR PC).
   - The final choice is made **after the G1 benchmark**, with scores for local vs cloud on the same test set.
   - If local models fail the L1 pass marks, the demo uses `dev-cloud` on synthetic data and this is disclosed in the demo.
5. **Local model candidates** (benchmarked in G1; licences checked before use):
   - drafting: a Japanese-tuned Qwen3-Swallow 30B-A3B class model;
   - verifier: a different family (e.g. GPT-OSS-Swallow 20B or Gemma);
   - embeddings/reranker: ruri-v3 (ONNX);
   - speech: ReazonSpeech-k2 + kotoba-whisper (unchanged).

## Amends

- **ch. 8 (`2026-10-04-v3-ai-per-block.md`):** "dev uses the same Japan-region models; OpenAI only as benchmark" → dev may use `dev-cloud` (any API) on synthetic data; models are chosen per profile. The real-data Japan-only rule is unchanged.
- **ch. 9 (`2026-10-04-v3-architecture.md`):** the "P-cloud-JP synthetic" demo profile is **not used for the MVP**; MVP = P-dev (laptops). The pilot profiles are unchanged.
- **ch. 10 (`2026-10-04-v3-tech-stack.md`):** the database fallback is no longer "small RDS dev DB". It is Postgres x64 on Windows' built-in emulation if native ARM64 Postgres + pgvector fails. Still no cloud.

## Risks

- Local models are weaker than Claude Opus at grounded Japanese extraction, so the demo may need more corrections. G1 measures this.
- Local speed on the ASUS (≈ 1–2 min per card for a 30B-class model is an estimate) → measured in G1.
- Prompts may need tuning per model. Swaps are cheap in code, but not free in quality work.

**Decided by:** Karthi (hosting, gateway approach) + Claude (gateway design details) · 6 Oct 2026
