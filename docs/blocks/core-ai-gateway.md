# AI Gateway (core module)  ✅ agreed 28 Sep 2026

Used by every AI block (B03, B05, B06, B07, B10, B11, B13, B15, B17, B18). **No block ever calls a provider directly.**

## 1. Job
The single door between our blocks and any AI provider. A block asks for a **task**; the gateway picks provider + model, enforces privacy, checks the answer, records cost and caches.

```
 B03 eye ─┐                ┌──────────── AI GATEWAY ────────────┐     ┌─ 🟠 OpenAI      (Version O)
 B06 …   ─┼── task ───────▶│ 🔒 privacy lock → 🧭 route → 💾 cache  │────▶├─ 🟢 Bedrock     (Version B)
 B10 …   ─┤                │ 💰 budget → 📡 call → ✅ check answer   │     ├─ 🧪 Fake        (tests, $0)
 B17 …   ─┘◀── result ─────│ 🧾 stamp + usage log                  │     └─ 🏠 Local       (later)
                           └──────────────────────────────────────┘
```

## 2. In → Out

| In | Out |
|---|---|
| Task name · inputs (text / frames / audio) · expected answer shape · case + slice ID · privacy stamp · data class (synthetic / real) | Checked answer in the expected shape + stamp (provider · model · prompt version · tokens · cost · time · cache hit), or a clear error |

## 3. Tasks

| Task | Input → Output | Used by |
|---|---|---|
| `see_pictures` | 10 frames + dictionary lists → labels per second | B03 |
| `see_clip` | frames of a short clip → yes / no / unclear + confidence | B05 |
| `structured_text` | text + answer shape → JSON | B07 role, B10, B11 judge, B13, checkers |
| `write_text` | evidence pack → text with citations | B17, B18 |
| `embed` | text → vectors | B15, B11 |
| `transcribe` | audio chunk → words + times | B06 (stand-in until GPU) |

## 4. How one call works

```
 ① 🔒 PRIVACY LOCK  case text must carry the privacy filter's stamp (a fingerprint of the cleaned
                    text: edited afterwards → stamp breaks → refused) · switch "frames may leave: yes/no"
                    (MVP: yes) · cloud `transcribe` refused when data class = real
 ② 🧭 ROUTE         active version (O / B / Fake) + task → provider + model (one config file)
 ③ 💾 CACHE         same task + prompt version + model + inputs → stored answer, $0
 ④ 💰 BUDGET        over the cap → refuse + alert
 ⑤ 📡 CALL          timeout · retries with backoff · rate limit per provider
 ⑥ ✅ CHECK         answer matches the expected shape? no → retry once with the error → fail visibly
 ⑦ 🧾 RECORD        tokens, cost, time, model → usage log · stamp the result · audit (IDs only)
```

## 5. Routing (model names confirmed at Gate 0a)

| Task | Version O | Version B |
|---|---|---|
| see_pictures | OpenAI GPT-5.4 mini | Bedrock Claude Haiku 4.5 |
| see_clip (B05) | stronger OpenAI vision model | stronger Bedrock vision model |
| card drafting | OpenAI GPT-5.4 | Bedrock Claude Sonnet 5 |
| embed | OpenAI text-embedding-3 | Bedrock Cohere multilingual or Titan |
| transcribe | OpenAI transcription (synthetic only) | shared GPU Whisper (later) |

## 6. Live vs batch

| | Live | Archive / tuning runs |
|---|---|---|
| Calls | Normal calls, **priority lane** | Provider **Batch API** allowed (cheaper, results in hours) |

Live and timing runs never use the Batch API.

## 7. Defaults

| Setting | Default |
|---|---|
| Randomness | 0 for labels and cards · low for written answers |
| Retries | 3 (network / rate limit) · 1 (malformed answer) |
| Parallel calls | Limited per provider, start at 8 |
| Prompts | Versioned files per task; version is part of the stamp and cache key |
| Keys | `.env` only, never logged |
| Budget | Alerts at 50 / 80 / 100% · **hard stop at 100%**, manual override by Karthi |
| Saving | **Every prompt and answer saved** in the MVP (debugging + reproducibility); review before real data |

## 8. Checks + failures

| Problem | Action |
|---|---|
| Text without a privacy stamp | ❌ Refuse (counted as a hard-rule test) |
| Provider down / rate-limited | Retry, then the job waits — never switch provider silently |
| Malformed answer | Retry once with the error, then fail visibly |
| Budget cap reached | Refuse + alert (manual override possible) |
| Model retired / renamed | Caught by a startup self-test |

## 9. How we test it
- 🧪 Fake provider: canned answers → every block testable with no key and $0 (powers Gate 1)
- Unstamped text → refused · cloud transcription of real data → refused
- Same request twice → second from cache
- Broken answer → retry → fails visibly
- Version O and B return the same answer shape
- Gate 0a smoke test: one tiny real call

## 10. Decisions
- Hard stop at 100% of budget + manual override ✅
- Batch API for tuning runs (never for live / timing) ✅
- Save every prompt and answer in the MVP ✅
