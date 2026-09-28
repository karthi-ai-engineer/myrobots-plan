# B07 · Who Spoke  ✅ agreed 28 Sep 2026

Lane: 👂 Hear · Previous: B06 · Works on both sides of B08 Privacy · Next: B09 Moments · **Local open model (voice split) + rules + small cloud model on cleaned text (roles)**

## 1. Job
Split the transcript into **speaker turns** (one line = one person speaking) and give each turn a **role** — surgeon · assistant · nurse · anesthetist · other — with a confidence.

## 2. Two parts, on either side of the privacy filter

```
 B06 raw words ─▶ B07-A VOICE SPLIT (audio, local) ─▶ turns ─▶ B08 PRIVACY ─▶ B07-B ROLE (text) ─▶ U4 lines
                  "Speaker 1, Speaker 2 …"                     names removed    "Speaker 1 = surgeon"
                  no text leaves the machine                                    may use cloud AI (text is clean)
```

## 3. In → Out

| In | Out (U4 transcript lines) |
|---|---|
| Audio chunks (B02) · raw words (B06) · then clean lines (B08) | line ID · start · end · voice (Speaker N) · role + confidence · clean text · words · flags (context-only · low-confidence words · hidden name · overlap) |

## 4. How it works

```
 PART A — VOICE SPLIT (before privacy, local)
 ① voice-split engine (pyannote, open model) → who spoke when
 ② a "voice fingerprint" per speaker → the same person stays "Speaker 2" across chunks
 ③ each word → its speaker (by time) → grouped into turns; long turns split at pauses > 1.5 s
      → B08 privacy filter
 PART B — ROLE (after privacy)
 ④ rules first: instructions 「クリップ」「吸引して」 → surgeon-like · confirming / handing
    「はい、どうぞ」 → nurse-like · vitals 「血圧…」 → anesthetist-like
 ⑤ small AI model (via the gateway) reads a sample of each speaker's lines → role + confidence
 ⑥ one role per voice per case · confidence < 0.6 → "unknown" → review
 ⑦ nurse / anesthetist lines → "context only" (knowledge comes from surgeon + assistant)
```

**Debrief answers:** speaker already known (the operating surgeon) → no split; role = surgeon, source = debrief.

## 5. Live vs batch

| | Archive | Live |
|---|---|---|
| Voice split | Whole audio at once (most accurate) | Per chunk + voice fingerprints to keep IDs stable |
| Roles | Once | Updated as lines arrive; final pass at 🏁 |

Live is somewhat less accurate than archive; the Test Harness measures the difference.

## 6. Defaults

| Setting | Default |
|---|---|
| Max speakers | 6 |
| Roles | Surgeon · assistant · nurse · anesthetist · other / unknown |
| New line after a pause of | 1.5 s |
| "Unknown" below | 0.6 confidence |
| Engine | pyannote (check model terms first) · laptop CPU for archive, GPU later for live |

## 7. Checks + failures

| Problem | Action |
|---|---|
| Two people talking at once | Both lines kept, flagged "overlap" |
| Two voices merged into one | Role confidence drops → review |
| Voice split fails on a chunk | Lines kept as "Speaker ?", role unknown; they can still open moments with lower strength |
| Synthetic voices too different from each other (test too easy) | Factory makes some voices deliberately similar |

## 8. How we test it
- Who-spoke accuracy vs the factory answer key (measure first)
- Role accuracy — especially: are the surgeon's lines found?
- Same person keeps the same ID across chunks (live)
- Debrief answers auto-tagged "surgeon"
- "Context only" flag correct

## 9. Decisions
- Voice-split engine = pyannote, local ✅
- 5 roles ✅
- Some factory voices deliberately similar ✅
