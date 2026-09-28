# B02 · Media Prep  ✅ agreed 28 Sep 2026

Lane: 📥 Ingest · Previous: B01 · Next: B03 Eye (frames) and B06 Speech → Text (audio) · **Rules only (FFmpeg), no AI**

## 1. Job
Turn the stored original into the raw material every AI block needs — frames, speech-ready audio, a review video and an index — all on **one clock**, working on a **time slice**.

## 2. In → Out

| In | Out |
|---|---|
| Case record + original (or the new live piece) + slice `[start → end]` | 🖼 frames 1/sec · 🔊 audio chunks · 🎞 review video · ✂️ clips on request · 🗂 manifest · 📣 events to B03 (frames ready) and B06 (audio ready) |

It does **not** make the transcript (B06), speaker labels (B07) or step/tool labels (B03).

## 3. How it works (per slice)

```
 ① read the slice from the original (by timestamp, not frame count)
 ② 🖼 FRAMES   one per second → resize to 768 px → JPEG → file name = its second
 ③ 🔊 AUDIO    first audio track → 16 kHz mono WAV → 30-s chunks, a new chunk every 25 s
 ④ 🎞 REVIEW   480p MP4 copy for the review screen (finished at 🏁 in live mode)
 ⑤ 🫥 BLUR     empty slot in the MVP; must be filled before real data
 ⑥ 🗂 update the manifest · stamp the version · send events → B03 + B06
 ✂️ CLIPS      made on request: 20 s around a moment (bleeding check + review)
```

**Output layout (one case):**
```
cases/CASE-0007/
├── original/video.mp4                 ← untouched
└── derived/ingest/v1/
    ├── frames/000001.jpg … 003600.jpg ← file name = second
    ├── audio/full.wav + chunks/0000-0030.wav, 0025-0055.wav, …
    ├── review.mp4                     ← 480p
    └── manifest.json                  ← index: source info, flags, frames, audio chunks, made_by
```

**One clock:** time = seconds from the first video frame, taken from the video's own timestamps.

## 4. AI or rules?
Rules only (FFmpeg).

## 5. Live vs batch

| | Archive | Live |
|---|---|---|
| Frames | All at once | 10 per new 10-s piece, immediately |
| Audio chunks | All at once (≈ 144 for 60 min) | Each as soon as its 30 s have arrived |
| Review video | One file | Grows during surgery, finished at 🏁 |

## 6. Defaults

| Setting | Default |
|---|---|
| Frames | 1/sec · 768 px long side · JPEG quality 85 |
| Audio | 16 kHz · mono · WAV · 30-s chunks · 5-s overlap |
| Review video | 480p · H.264 MP4 · fast start |
| Clips | 20 s (10 s before + 10 s after) |
| Versions | `derived/ingest/v1/…`, never overwritten |

**Size of one 60-min case (approx.):** original 1–4 GB · frames ~250–400 MB · audio ~115 MB · review video ~300–500 MB.

## 7. Checks + failures

| Problem | Action |
|---|---|
| **Camera outside the body** (lens cleaning — may show the room or faces 🔒) | ⚠ Flag those frames (simple brightness/colour rule); the eye skips them. A better detector before real data |
| Black / blank frames | ⚠ Flag and skip |
| Audio silent, too quiet or clipped | ⚠ Flag |
| Several audio tracks | Use the first, flag the rest |
| Frame count ≠ length | ⚠ Flag |
| Audio/video drift > 0.5 s | ⚠ Flag |
| A slice fails | Retry that slice only; others continue |

## 8. How we test it
- 60-min video → exactly 3,600 frames, named by second
- Frame at 30:00 = the original's picture at 30:00
- 144 audio chunks, correct overlaps, total length = video length (±0.1 s)
- Same input twice → identical output
- 360 small 10-s slices = one big slice (proves the live path)
- Odd frame rate → frames still on the right seconds
- Speed for a 60-min case on the laptop (measure first)

## 9. Decisions
- Frame size 768 px (tune later by testing) ✅
- "Camera outside the body" flag in the MVP ✅
- Several audio tracks → use the first ✅
