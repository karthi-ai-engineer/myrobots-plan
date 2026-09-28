# B06 · Speech → Text  ✅ agreed 28 Sep 2026

Lane: 👂 Hear · Previous: B02 · Next: B07 Who Spoke → B08 Privacy · **AI (speech model via the gateway) + rules**

```
 🔊 audio chunks ─▶ B06 SPEECH→TEXT ─▶ B07 WHO SPOKE ─▶ B08 PRIVACY ─▶ clean transcript lines (U4)
                    words + times       surgeon / nurse…   names removed
                    ⚠ raw = contains names → SEALED until B08 cleans it
```

## 1. Job
Turn OR speech (and the surgeon's debrief voice answers) into Japanese text with **word-level times** and **confidence**, on the one clock, with medical words right.

## 2. In → Out

| In | Out |
|---|---|
| 30-s audio chunks (B02) · debrief voice answers · medical word list (from the dictionary) | **Raw transcript**: every word with start · end · confidence, stitched across chunks. 🔒 **Sealed** — may contain names; only B07/B08 (and the tester role) read it |

## 3. How it works

```
 ① take a 30-s chunk · skip it if silent (voice detection)
 ② speech engine (via the gateway) + medical word list as a hint
      e.g. 胆嚢管 (cystic duct) · 総胆管 (common bile duct) · カロー三角 (Calot's triangle) · 癒着 (adhesion) · 吸引 (suction)
 ③ word times shifted onto the case clock (chunk start + offset)
 ④ stitch the 5-s overlaps: each word kept once, from the chunk where it's furthest from the edge
 ⑤ flag low-confidence words (< 0.5) → shown later in review
 ⑥ → B07
```

## 4. AI or rules?

| | Engine | Output shape |
|---|---|---|
| MVP | OpenAI transcription via the gateway — **synthetic data only** | same |
| Later / before real data | Whisper large-v3 on our own GPU (AWS or local) | same |

🔒 Safety lock: the gateway refuses cloud transcription when the case's data class is **real**.

## 5. Live vs batch

| | Live | Archive |
|---|---|---|
| When | Every chunk as it completes (every 25 s), target ≤ ~20 s per chunk | All chunks in parallel |
| Debrief answers | Priority lane, text back in seconds | — |

## 6. Defaults

| Setting | Default |
|---|---|
| Language | Japanese (English words inside Japanese speech handled) |
| Chunks | 30 s · 5-s overlap |
| Silence skip | On |
| Word times | On |
| Low-confidence flag | < 0.5 |
| Randomness | 0 |

## 7. Checks + failures

| Problem | Action |
|---|---|
| Made-up text in silence (known Whisper habit, e.g. 「ご視聴ありがとうございました」) | Silence skip + filter for known invented phrases + drop very-low-confidence text over silence |
| Duplicate words at chunk edges | Stitching rule |
| OR noise | Measured per noise level (easy / medium / hard) |
| Real data + cloud engine | ❌ Refused |
| Engine fails on a chunk | Retry; the time range is marked "pending" for later blocks |

## 8. How we test it
- Character error rate (standard for Japanese) vs the factory script, per noise level (measure first)
- Medical terms right (胆嚢管 and 総胆管 never mixed up)
- Word times within ±0.5 s of the script
- No lost or doubled words at chunk edges
- Silence in → no text out
- Planted fake names are transcribed (so B08 can prove it removes them)

## 9. Decisions
- MVP engine = OpenAI transcription until the GPU is ready (synthetic data only) ✅
- Debrief answers through the same engine, priority lane ✅
- Automatic lock: cloud transcription refused for real data ✅
