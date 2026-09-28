# Block-by-block design

> Detailed design of each block, planned lane by lane with Karthi (started 25 Sep 2026).
> These pages refine `docs/MASTER_PLAN.md` §5–8. Where they differ, the newer block page wins.
> Planning only — no code until Karthi's final go.

## Status

| Order | Lane | Block | Page | Status |
|---|---|---|---|---|
| 1 | 📥 Ingest | B01 Case Intake | `B01-case-intake.md` | ✅ agreed 28 Sep |
| | | B02 Media Prep | `B02-media-prep.md` | ✅ agreed 28 Sep |
| 2 | 🔌 Core | AI Gateway | `core-ai-gateway.md` | ✅ agreed 28 Sep |
| 3 | 👁 See | B03 Eye (B04 merged in) | `B03-eye.md` | ✅ agreed 28 Sep |
| | | B05 Event Check | `B05-event-check.md` | ✅ agreed 28 Sep |
| 4 | 👂 Hear | B06 Speech → Text | `B06-speech-to-text.md` | ✅ agreed 28 Sep |
| | | B07 Who Spoke | `B07-who-spoke.md` | ✅ agreed 28 Sep |
| | | B08 Privacy Filter | — | ▶ **next** |
| 5 | 🧠 Understand | B09 Moments · B10 Card Drafter · B11 Dictionary Matcher | — | ⏳ |
| 6 | ✅ Review | B24 Debrief Builder · B12 Review Desk · B13 Learner | — | ⏳ |
| 7 | 📚 Know | B14 Graph · B15 Search · B16 Patterns · B25 Library | — | ⏳ |
| 8 | 💬 Ask | B17 Answers · B18 Pre-case Brief · B19 Dashboard | — | ⏳ |
| 9 | 🛡 Always on | B20 Login · B21 Audit · B22 Dictionary Mgr · B23 Test Harness | — | ⏳ |
| 10 | 🏭 Factory | F1 Scripts · F2 Voices · F3 Mixer · F4 Live Streamer | — | ⏳ (last: depends on the data question) |

## Template (every block page)

1. **Job** (one line) · 2. **In → Out** · 3. **How it works** · 4. **AI or rules?** · 5. **Live vs batch** · 6. **Defaults** · 7. **Checks + failures** · 8. **How we test it** · 9. **Decisions** (agreed answers)

## Notes carried to later blocks

| For | Note (agreed 28 Sep) |
|---|---|
| B24 Debrief Builder | Add one last debrief question: **"Was there any other difficult moment we missed?"** (voice answer → new candidate card). Catches obstacles that were neither seen nor said |
| B09 Moments | Clues include the eye's **blood / bile hints**, tool clues, voice keywords and an unusually long step; look back ~30 s for warning signs |
| F2 Voices | Make some factory voices **deliberately similar**, so "who spoke" is tested realistically |
| Build order | Archive-first vs live-together is a build-order question for ch. 11 (on hold). All blocks are designed **slice-shaped** so both work with the same code |
