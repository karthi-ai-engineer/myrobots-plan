# v3: OpenAI for the MVP · model-agnostic by design

- **Topic:** Which AI services the MVP uses during development and the demo, and how undecided model choices are handled.
- **Context (Karthi, 6 Oct 2026):** "We will use Whisper and use OpenAI models." The verifier model is not decided yet; it will be whatever a startup accelerator programme provides credits for. "We will work in a way of not involving the model specific."

## Decisions

1. **Speech-to-text (MVP): OpenAI's transcription API** (Whisper family), through the gateway task `transcribe`, profile `dev-cloud`, synthetic data only.
   - The local engines verified in P0a (sherpa-onnx ReazonSpeech, whisper.cpp) are **off the MVP path**. They remain the swap target for real data (pilot): one gateway setting.
   - B06 supports **1 or 2 engines by config**. If two OpenAI transcription models are available (e.g. whisper-1 + a GPT-4o-class transcribe model), they run as engines A and B for the disagreement policy; otherwise one engine plus critical-term flags. Capabilities (word timestamps, Japanese quality) are verified in P1.
2. **Card drafting (MVP): OpenAI GPT models** via the gateway (`draft_card`, `classify_answer`, `map_term`).
3. **Verifier: not decided.** It will be chosen when accelerator credits are known (OpenAI, AWS Bedrock/Claude, Google or other).
4. **Model-agnostic rule (applies everywhere):**
   - Application code never names a model or vendor. It requests **tasks** with versioned JSON contracts.
   - Provider + model per task live **only in config** (`ai_profiles.yaml`), with per-model prompt variants inside the gateway.
   - Every task has a **fake provider** for tests, so all phases can be built and tested before any model is chosen.
   - Changing a model = editing config, plus a golden-set re-run and activation record before use.
   - Adapters are built as needed: **OpenAI first**, then whichever provider the credits bring.
5. **Cross-family verifier policy:**
   - Kept as the target, but made **policy-driven** while the choice is open.
   - Dev: a same-family verifier is allowed, with a visible warning in the logs and the L1 report.
   - Before the final L1 claim and the demo: cross-family, or an explicit written waiver by Karthi.

## Amends

- `2026-10-06-v3-approval-hybrid-and-api-models.md` §4: drafter/verifier "Claude via Bedrock + OpenAI" → OpenAI now, verifier open. Speech "local" → OpenAI API now, local later.
- `IMPLEMENTATION_PLAN_v1.md` §2 and §4.2 (see the v1.2 note there).

## Still true

- Synthetic data only in the MVP.
- The gateway refuses 🏥 real data to any non-Japan provider, in every profile.

**Decided by:** Karthi · 6 Oct 2026
