# Technical chapters: pipeline, skeleton, tech stack, AI per block, test & quality (ch. 5–8, 10)

- **Topic:** Technical design of the MVP
- **How decided:** Drafted by Claude in fast mode (Karthi: "yes"), accepted by Karthi 24 Sep 2026. Full detail in `MASTER_PLAN.md` §5–8, §10.
- **Key decisions:**

| # | Decision | Alternatives considered |
|---|---|---|
| 1 | 28 blocks: B04 merged into B03 (one vision call for steps + tools + actions); new B24 Debrief Builder and B25 General Library; Backlog Planner parked | Keep 27 as is |
| 2 | New plugs U9 Debrief pack, U10 Library passage, U0 Slice; U1 gains trust label, debrief / research evidence, resolving flag | — |
| 3 | One app, separate modules; web + worker processes; Postgres job queue (Procrastinate); idempotent jobs; wiring table | Microservices; Redis/SQS queue |
| 4 | Live transport = media segments over HTTP; B01 owns 🏁 (stream end or button) | RTMP / WebRTC / SRT server |
| 5 | AI gateway refuses text without a B08 clearance stamp; no silent provider fallback; cache by input hash | Discipline only |
| 6 | Python 3.12 + uv · FastAPI · Jinja2 + HTMX + Alpine + Tailwind · Pydantic v2 · Postgres + pgvector · SudachiPy for Japanese keyword search · SQLAlchemy 2 + Alembic · pytest · ruff · gitleaks · private GitHub | React front end; PGroonga / pg_bigm |
| 7 | B08 privacy filter local only (rules + GiNZA + safe list); B07 role step runs after B08 | Cloud NER |
| 8 | Version O: GPT-5.4 mini (eye), GPT-5.4 / 5.5 (cards); Version B: Claude Haiku 4.5 / Sonnet 5 (names confirmed at Gate 0a) | — |
| 9 | Speech: OpenAI transcription as a stand-in (synthetic data only) → faster-whisper on GPU later | GPU from day one |
| 10 | Factory (tentative, decided in the flow): Claude via subscription writes scripts (locked cases by a background agent into sealed storage) · obstacle marks from dataset labels + rules + frame check · VOICEVOX voices | Google (Gemini + Cloud TTS) — not feasible; OpenAI factory — same family as Version O |
| 11 | Test layers incl. simulated reviewer, 1× timing runs, ≈ 30-question set, final exam run once | — |

- **Resources:** Claude subscription only (no Claude API key). OpenAI API for Version O, possibly with program X credits (≈ $10k, to confirm).
- **Decided by:** Karthi (accepted Claude's drafts) · 24 Sep 2026
