# Block designs: Ingest, AI gateway, See, Hear (B06–B07)

- **Topic:** Detailed design of the first blocks, planned block by block and lane by lane (25–28 Sep 2026)
- **Approach:** one template per block (job · in→out · how · AI or rules · live vs batch · defaults · checks · tests · decisions); lanes in data-flow order; pages in `docs/blocks/`
- **Choices (Claude's picks, accepted by Karthi):**

| Block | Agreed decisions | Alternatives considered |
|---|---|---|
| B01 Case Intake | One camera (laparoscope) · required intake form fields · 🏁 only by explicit signal | Room camera too; timeout-based 🏁 |
| B02 Media Prep | 768-px frames · "camera outside the body" flag · first audio track · 480p review video · 30-s audio chunks with 5-s overlap | Larger frames; no out-of-body flag |
| AI Gateway | Hard budget stop at 100% + manual override · Batch API for tuning runs (never live) · save every prompt and answer · cloud transcription refused for real data · fake provider for tests | Soft budget; no batch; log usage only |
| B03 Eye | **B04 stays merged into B03** (one call: steps + tools + action hints) · standard label set (7 steps · 7 tools · 9 actions) · blood/bile hint in the same call · previous slice's step as context | Separate B04 (3× vision cost) |
| B05 Event Check | Bleeding **and** bile · stronger vision model (few calls) · voice keywords can trigger | Bleeding only; small model |
| B06 Speech → Text | OpenAI transcription stand-in (synthetic data only) · debrief answers via the same engine in a priority lane · automatic lock for real data | GPU Whisper from day one |
| B07 Who Spoke | pyannote local voice split · 5 roles · some factory voices deliberately similar | Cloud diarization; fewer roles |

- **Carried to later blocks:** B24 debrief ends with "Was there any other difficult moment we missed?" · build order (archive first vs live together) stays a ch. 11 question; all blocks are slice-shaped so both work with one code path.
- **Decided by:** Karthi (accepted Claude's picks) · 28 Sep 2026
