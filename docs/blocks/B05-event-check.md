# B05 · Event Check (the second look)  ✅ agreed 28 Sep 2026

Lane: 👁 See · Triggered by B03 clues and voice keywords · Output to B09 · **AI (stronger vision model via the gateway) + rules**

## 1. Job
When a clue suggests **bleeding or bile**, take a focused second look at a short clip and confirm: is it really there, and how sure are we? It checks only suspicious moments, never the whole video. The output is a **hint**, never an obstacle by itself.

## 2. In → Out

| In | Out (U3 video fact, kind = event check) |
|---|---|
| A trigger at time t · 8 frames from a 16-s window around t (from B02) | Event (bleeding / bile) · start–end · **yes / no / unclear** · confidence · amount hint (small / moderate / large) · which trigger fired · labelled HINT |

## 3. How it works

```
 TRIGGERS (any one fires)
   👁 eye's "blood / bile visible?" hint (B03)
   🔧 tool clue: irrigator / suction appears · burst of coagulation
   🎙 voice keyword: 「出血」「吸引」「胆汁」 "bleeding" "suction"   (after the privacy filter)
        │
 ① merge triggers within 30 s → one check
 ② B02 cuts the window: 10 s before + 6 s after → 8 frames
 ③ gateway task see_clip → yes / no / unclear + confidence + amount hint
 ④ "yes" only if confidence ≥ 0.6, otherwise "unclear" (never guess)
 ⑤ save → B09: the moment gets a 👁 video witness
 ⑥ cooldown: after a "yes", no new check for 60 s unless a different trigger fires
```

## 4. AI or rules?
AI for the look + rules for triggers, merging and cooldown. Only ~10–40 checks per case, so the **stronger vision model** is used here (better accuracy, small cost).

## 5. Live vs batch

| | Live | Archive |
|---|---|---|
| When | ~6 s after the trigger (waits for "after" frames) | All triggers at once |

## 6. Defaults

| Setting | Default |
|---|---|
| Events checked | **Bleeding + bile** |
| Window | 16 s (10 before · 6 after) · 8 frames |
| Merge / cooldown | 30 s / 60 s |
| "Yes" threshold | Confidence ≥ 0.6 |
| Cost guard | Max ~60 checks per case, then flag |
| Randomness | 0 |

## 7. Checks + failures

| Problem | Action |
|---|---|
| Frames out of body, smoke, blurry lens | → "unclear" |
| Too many triggers | Cap + flag the case |
| Provider fails | Retry; the moment can still open from voice or tool clues |
| A "yes" alone | Still only a hint — B09/B10 decide |

## 8. How we test it
- Planted bleeding / bile in factory cases → found? (measure first)
- False alarms in routine cases
- Merge + cooldown rules
- "Unclear" rate and cost per case

## 9. Decisions
- Check bleeding **and bile** ✅
- Stronger vision model for this block ✅
- Voice keywords can trigger the check ✅
