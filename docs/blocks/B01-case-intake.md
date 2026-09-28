# B01 · Case Intake  ✅ agreed 28 Sep 2026

Lane: 📥 Ingest · Next block: B02 Media Prep · **Rules only, no AI**

## 1. Job
Receive a surgery video (live pieces or a whole file), give it an ID, store the original safely, check it, and track the case's status — so every later block works from one trusted source.

## 2. In → Out

| | Archive | Live |
|---|---|---|
| **In** | Video file + short intake form | "Start case" + 10-s pieces + "End case" 🏁 |
| **Out** | Case record (U2) · original stored · event to B02 · audit entry | Same, one event per piece |

**Intake form (required):** procedure · case date · surgeon (pseudonym, never a real name) · use case (live / archive) · case set (dev / tuning / locked) · source · notes (optional).

## 3. How it works

```
 ARCHIVE                                   LIVE
 ① form + file (or bulk folder import)     ① "start" → case created, recording opened
 ② fingerprint while saving                ② each 10-s piece: check its number
 ③ duplicate? → stop, show existing case      (no gaps, no repeats) → save → append
 ④ save original (write-once, read-only)      → event "slice ready" → B02
 ⑤ probe: length, fps, size, audio tracks  ③ missing piece → wait for resend
 ⑥ quality checks → flags                  ④ "end" 🏁 → status received → final pass
 ⑦ status "received" → event → B02         ⑤ fingerprint the full recording
```

**Case status:**
```
 created → receiving / uploaded → 🏁 received → processing → ready for review → reviewed
                                         ↘ failed (with reason)
```

## 4. AI or rules?
Rules only. No AI, no API keys.

## 5. Live vs batch
Same code. Archive = one whole file; live = pieces added as they arrive.

## 6. Defaults

| Setting | Default |
|---|---|
| Accepted formats | MP4 · MKV · MOV · AVI · TS |
| Live piece length | 10 s (= one video slice) |
| Case ID | `CASE-0001`, `CASE-0002` … |
| Max file size | 20 GB |
| 🏁 end signal | **Only explicit** (stream-end message or "End surgery" button). A timeout never ends a case, because 🏁 unlocks the debrief |
| Video streams | **One** (laparoscope) in the MVP |

## 7. Checks + failures

| Problem | Action |
|---|---|
| Same video added twice | ❌ Reject, link to the existing case |
| Corrupted / unreadable | Status "failed", keep original, notify |
| No audio | ⚠ Flag "video-only" (fine for archive) |
| Very short (< 10 min) | ⚠ Flag, don't reject |
| Odd frame rate | ⚠ Flag (B02 uses timestamps) |
| Live: piece missing | Wait + ask for resend; still missing → mark a **gap** on the timeline (the clock stays true) |
| Live: no pieces for 5 min | ⚠ Flag "stream lost" — **no** automatic 🏁 |
| Disk full | Stop intake, alert |
| Every action | → Audit log (IDs only) |

## 8. How we test it
- Same file twice → second rejected
- Corrupted file → correct flag + status
- Live replay at 10× → reassembled length = source (±0.1 s), no gaps
- Network cut mid-stream → resumes, nothing lost
- Status only moves in allowed steps

## 9. Decisions
- One video stream (laparoscope) only in the MVP ✅
- Required form fields as listed ✅
- 🏁 only by explicit signal, never by timeout ✅
