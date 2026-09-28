# B03 · Eye (steps + tools + action hints)  ✅ agreed 28 Sep 2026

Lane: 👁 See · Previous: B02 · Next: B05 Event Check, B09 Moments · **AI (vision via the gateway) + rules**

> **B04 (Tools) is merged into B03.** One AI call per 10 frames returns steps, tools and action hints together: ≈ 360 calls per 60-min case instead of ≈ 1,080, and the answers can't contradict each other. The number B04 stays empty so older documents still match.

## 1. Job
Look at the frames and say, second by second, which **surgical step** is happening, which **tools** are visible and which **action** is likely — as hints with confidence, smoothed into time segments. The eye **never decides obstacles** (B09/B10 do).

## 2. In → Out

| In | Out (U3 video facts) |
|---|---|
| 10 frames (one 10-s slice) from B02 · dictionary ID lists · previous slice's step | **Raw** per second: step · tools · action hint · **blood / bile hint** · confidence · out-of-body · **Segments** ("Calot dissection 12:10–24:55", "clipper 24:20–24:50") · **Clues** for B05 + B09 |

## 3. How it works

```
 ① skip frames B02 flagged "out of body / blank"   (privacy + cost)
 ② gateway task see_pictures: 10 frames + fixed instructions + dictionary ID lists
      → per frame: step ID · tool IDs · action hint · blood/bile visible? · confidence  ("unknown" allowed)
 ③ confidence < 0.5 → "unknown"
 ④ SMOOTHING (rules): step min 20 s (short blips → neighbours) · tools: join gaps ≤ 3 s
 ⑤ CLUE RULES: blood/bile hint · irrigator/suction appears · repeated coagulation · clipper after long dissection
 ⑥ save raw + segments + clues → B09 (moments) · B05 (second look)
```

**MVP label set:**

| Kind | Labels |
|---|---|
| Steps (7) | Preparation · Calot triangle dissection · Clipping & cutting · Gallbladder dissection · Gallbladder packaging · Cleaning & coagulation · Gallbladder retraction |
| Tools (7) | Grasper · Bipolar · Hook · Scissors · Clipper · Irrigator · Specimen bag |
| Action hints (9) | Grasp · Retract · Dissect · Coagulate · Clip · Cut · Aspirate · Irrigate · Pack |

## 4. Steps vs obstacles (how an obstacle during a step is caught)

```
 time     20:00          23:00    23:30          25:10          30:00
 STEPS    [ 3 · Clipping & cutting ─────────────────────────────][ 4 · GB dissection …
 CLUES                     👁 blood visible? ✔ · 🔧 irrigator appears · 🎙 「出血… 吸引して」
 B09      ◀── look-back 30 s ──▶ MOMENT opens 23:30 ──────────── closes 25:10
 B10      ⚡ CARD: obstacle = bleeding · step = 3 Clipping & cutting · response = suction → clip · …
```

| Situation | What happens |
|---|---|
| Obstacle continues into the next step | Card stores the start step + "continued into step N" |
| Two obstacles in one step | Two cards |
| Eye unsure of the step | Step = the one said aloud, or "unknown"; the surgeon picks from a list in the debrief |
| Return to an earlier step | Allowed, flagged |
| Obstacle neither seen nor said | Can be missed → caught by the debrief question "anything we missed?" (B24) |

## 5. Live vs batch

| | Live | Archive |
|---|---|---|
| Calls | One per 10-s slice, as it arrives | Same; Batch API allowed |
| Smoothing | Provisional at slice edges, final at 🏁 | Once, over the whole case |

## 6. Defaults

| Setting | Default |
|---|---|
| Frames per call | 10 (≈ 360 calls per 60-min case) |
| Model | O: GPT-5.4 mini · B: Claude Haiku 4.5 |
| Randomness | 0 |
| "Unknown" below | 0.5 confidence |
| Min step length · tool gap join | 20 s · 3 s |
| Prompt caching | Fixed instructions + ID lists first |
| Answers | Dictionary IDs only |

## 7. Checks + failures

| Problem | Action |
|---|---|
| Label not in the dictionary | Invalid → retry → "unknown" |
| Step goes backwards | Allowed, flagged |
| Provider fails for a slice | That slice retries; moments can still open from voice |
| Two AI eyes agree | Still **one** witness (video + voice = two) |

## 8. How we test it
- Step + tool accuracy vs the dataset's own labels, if available (measure first)
- "Unknown" rate per case
- Smoothing removes injected blips
- Only valid dictionary IDs
- Cost + time per case
- Same slice twice → same labels

## 9. Decisions
- Standard label set (7 steps · 7 tools · 9 actions) ✅
- "Blood / bile visible?" hint in the same call ✅
- Previous slice's step given as context ✅
- B04 stays merged into B03 ✅
