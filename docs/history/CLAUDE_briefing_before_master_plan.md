> ⚠ **HISTORY ONLY.** The Claude Code briefing as it was before the 24 Sep 2026 master-plan rethink. Superseded by `CLAUDE.md` + `docs/MASTER_PLAN.md`.

# MY ROBOTS · Surgical Knowledge MVP — Claude Code briefing

> Read this file first, every session. Detailed plan: `docs/PLAN_v2.md`. What to do next: `docs/NEXT_STEPS.md`.
> Plan status: Black box ✅ · Outline ✅ · Blueprint Part 1 (data units) ✅ · **Blueprint Part 2 (skeleton + tech picks) = NEXT** · Build = Gate 0a.
> Last planning update: 21 Sep 2026 (claude.ai planning chat).
> ⚠ **24 Sep 2026: master plan rethink in progress** → `docs/MASTER_PLAN.md` + `docs/decisions/`. Decisions there override this file (e.g. Track B is dropped). No code until the master plan is approved.

---

## 0. How to work with Karthi (read first)

- **Roles.** Karthi decides and builds. You are the senior advisor AND developer: software engineer, AI engineer, knowledge-graph and ontology engineer, multimedia-processing expert, with surgical domain knowledge.
- **Think critically.** Don't just agree. If a request conflicts with a locked decision or a rule below, say so, explain why, and offer options with a recommendation. Karthi makes the final call.
- **Decisions are Karthi's.** When something is a real choice, present 2–4 options, mark your pick, explain the trade-off briefly, and ask. Don't silently decide product questions.
- **Communication style (important):**
  - Short, visual, readable. Headings, small tables, ASCII diagrams, symbols (✅ ⚠ 🔒 ▶).
  - No heavy paragraphs. One topic at a time. Don't overload.
  - Explain difficult terms simply, with a concrete example.
  - End with reminders of open items when relevant.
- **Working model.** Only Karthi + Claude work on this. Don't split work between people. There is **no time to learn ML from scratch** → the MVP **uses ready-made AI only; it never trains models**.
- **Pace.** No fixed deadline, but as soon as possible. Progress is measured by gates (checklists), not dates.

---

## 1. The product in one line

Other systems record **what** the surgeon did (video). We also capture **why** (what the OR team says), join both on one timeline, and turn important moments into **surgeon-approved knowledge** that others can search and use to prepare for their next case.

**Core loop:** record → process (video + voice) → find moments → draft episode cards → surgeon approves → knowledge grows → answers + pre-case briefs.

**The CEO's 4 requirements (everything must serve at least one):**
1. Pulls surgical knowledge out automatically
2. The surgeon has to fix very little
3. Knowledge piles up per procedure
4. Gets more valuable with every new case

---

## 2. Rules that are never broken

| Rule | Meaning |
|---|---|
| ✅ AI proposes, surgeon approves | Nothing becomes knowledge without the operating surgeon's approval |
| ⚡ Unit = one obstacle episode | The basic unit is an episode card (U1), not a video |
| 👁👂 Two witnesses | Video + voice agree = strong; one only = flagged. Two AI "eyes" never count as two witnesses |
| 📎 No evidence → no field | Every card field points to a transcript line or a video time |
| ❓ Unknown allowed, guessing not | If not said or seen → "unknown" |
| 🎯 A moment is a candidate | B10 may answer "no obstacle here" |
| 📚 Approved only | Only approved cards enter the knowledge base and patterns |
| ⏱ One clock | Every timestamp = seconds from video/stream start |
| 🔒 Original never touched | Blocks work on copies |
| 🔒 Privacy before cloud | The privacy filter (B08) runs before ANY cloud AI call; user questions are filtered too |
| 🔒 Sealed envelope | Answer keys + raw transcripts are visible only to the Test Harness / tester role |
| 🫀 Danger zone | Danger-zone anatomy (e.g. cystic duct vs common bile duct) is never auto-mapped at low confidence → review |
| 🔢 Small numbers | Show "2 of 3 (67%) ⚠ small sample"; counts first |
| ⚖️ Disagreement | Different surgeons' approaches shown side by side, never averaged |
| 🚫 Nothing to the operating surgeon | Live drafts are never shown in the OR during surgery (medical-device line) |
| 🔑 Secrets | API keys only in `.env` (git-ignored) or a secret manager. Never in code, logs, or chat |
| 🏥 No real patient data | MVP uses public/fake data only, until the "before real data" checklist is complete |
| 🧪 No model training | Use ready-made models only (cloud AI or downloaded, run-only) |

---

## 3. Scope (locked)

- **Procedure:** laparoscopic gallbladder removal (cholecystectomy). The procedure is a plug-in (new dictionary + videos), not the product.
- **Knowledge cases:** 10 public videos (Cholec80 ∩ CholecT50): 7 hard + 3 routine. None may have been seen by any model we run.
- **Test cases (Track A):** 3 extra public videos with AI-drafted scripts + synthetic voices (exact answer keys).
- **Real voice (Track B):** a surgeon watches each of the 10 videos and explains aloud (think-aloud), in Japanese. Also the product's archive feature.
- **Languages:** Japanese + English. The English twin comes from the corrected Japanese transcript. Twins are **extracted separately, then merged** into one card (agree / partial / disagree).
- **Video AI levels:** L1 steps · L2 tools · L3 actions as hints only.
- **Obstacles:** 3–4 types (general level), each in 2–3 cases, at least 2 paired cases.
- **Pictures:** 1 per second.
- **Test split:** 7 tuning cases + 3 locked final-exam cases (from Track B).

---

## 4. Architecture: 27 blocks in 9 lanes

Every block = one job, fixed input, fixed output (a "plug"). Swap the inside, keep the plugs.

| Lane | Block | Job | Runs in |
|---|---|---|---|
| 🏭 Factory | F1 Script Drafter | Scripts for Track A test cases (🇯🇵+🇬🇧) | E4 |
| | F2 Voice Maker | Synthetic voices (Track A; side lines in Track B) | E4 / E3 |
| | F3 OR Mixer | Mix voices + OR noise into the video; 3 noise levels; seal answer keys | E1 / E3 |
| | F4 Live Streamer | Play a case at 1× as a live camera+mic; replay at 10× for tests | E1 / E2 |
| 📥 Ingest | B01 Case Intake | ID, fingerprint, store original, status; source live/archive; case date | E2 + E5 |
| | B02 Media Prep | Pictures 1/sec, speech-ready audio, one clock; blur slot (empty in MVP) | E2 / E3 |
| 👁 See | B03 Steps (L1) | Which step, second by second → segments (+ raw kept) | E4 (v1) / E3 (v2) |
| | B04 Tools (L2) | Which tools on screen | E4 / E3 |
| | B05 Actions + bleeding | Action hints; bleeding check on ~15-s clips only | E4 |
| 👂 Hear | B06 Speech → Text | Words + times + confidence; medical word list | E3 (+ E4 benchmark) |
| | B07 Who Spoke | Voice split + role from content; one line = one speaker turn | E3 |
| | B08 Privacy Filter | Hide names; eponym safe list (Calot, Rouvière, Mirizzi) | E2 (+ E4) |
| 🧠 Understand | B09 Timeline Merger | Moments: any clue opens; look-back; join overlaps; strength tag. Rules only, no AI | E2 |
| | B10 Episode Extractor | Moment → draft card or "nothing here"; evidence per field | E4 |
| | B11 Ontology Mapper | Words → IDs; pending zone; danger-zone review; twin match | E2 + E4 |
| ✅ Review | B12 Review Desk | Operating surgeon approves/edits (from lists)/rejects (one tap); guard; why note | E2 |
| | B13 Feedback Learner | Examples + repeated fixes → rules; learn forward only | E2 |
| 📚 Know | B14 Knowledge Graph | Approved cards as connected facts; history | E2 |
| | B15 Search Index | Keyword + meaning + graph; one index PER AI version | E2 + E4 |
| | B16 Pattern Builder | Pattern cards; instant; resolving-action grouping | E2 |
| 💬 Ask | B17 Answer Engine | 3 labelled layers; checker removes uncited claims; saved + snapshot + rating | E4 + E2 |
| | B18 Case Prep Brief | Expected obstacles → patterns, pairs, gaps | E4 + E2 |
| | B19 Growth Dashboard | Cases, edit-rate trend, review time, coverage | E2 |
| 🛡 Always On | B20 Login & Roles | Surgeon / resident / admin / tester | E2 |
| | B21 Audit Log | Append-only; IDs not content; AI versions | E2 + E5 |
| | B22 Ontology Manager | Terms, synonyms, pending zone, weekly approval, versions, aliases, 2 levels | E2 |
| | B23 Test Harness | Scores vs answer keys; auto re-run; O vs B comparison | E2 (+E3/E4) |

Plus a **Backlog Planner** (archive priorities + weekly review cap).

**Environments:** E1 laptop · E2 app server · E3 GPU worker (on only when needed) · E4 cloud AI services · E5 file storage · E6 hospital box (later).

---

## 5. Wiring + live mode (locked)

- **One app, separate modules** (one per block), a **job queue** for background work, **GPU workers** switched on only when needed.
- Two kinds of traffic: 🏭 conveyor belt (background processing) and 🛎 front desk (review, ask, dashboard).
- **Contracts:** blocks exchange only the data units (U1–U8). **Save every block's output. Stamp every output** with block version + AI provider + model + dictionary version.
- Automatic by default; **any single block can re-run alone**.
- **Live mode = batch in small slices (ONE code path).** Every block works on a time slice [start → end]. Batch = one slice for the whole case.
  - Pictures every 10 s (10 per request) · audio in 30-s chunks with 5-s overlap · moments close after 60 s of quiet (5 min max) · draft cards ≈ 2–3 min after an event · final check pass at case end.
  - **Record first, process second:** the stream is saved in full as it arrives; processing reads the saved copy.
  - Two passes: fast draft live, final check at the end.

---

## 6. Data units (the contracts) — details in docs/PLAN_v2.md

| Unit | What it is |
|---|---|
| U1 Episode card | One obstacle; step; anatomy (+danger flag); warning signs; ordered response list; outcome; severity (AI proposes, surgeon confirms); why; paired-with links; evidence per field; witness strength; confidence; evidence source (🎙 live / 🗣 narrated / 👁 video-only); languages + twin agreement; status; made-by versions; history |
| U2 Case package | Video with audio; fingerprint; case ID; procedure; persona/surgeon; language; noise level; source (factory/recorder, live/archive); case date; version role (primary pair / stress test). Never: answer keys, patient info |
| U3 Video fact | Case; start/end; kind (step/tool/action hint/event check); ontology ID; confidence; source (eye v1/v2/tool rule); segments + raw kept |
| U4 Transcript line | One speaker turn; case; language; start/end; voice → role + confidence; clean text; word times; flags (context-only, low-confidence, hidden name) |
| U5 Moment | Case; twin; start/end; evidence (U3 + U4); clues that opened it; strength |
| U6 Dictionary term | ID; kind; 🇯🇵/🇬🇧 labels; synonyms; definition; OntoSPM link; SNOMED slot; parent/children; flags (danger zone, safe name); status; source; version; history |
| U7 Pattern card | Obstacle (general ID); N of M cases + % + ⚠; breakdown; steps; warning signs; responses by resolving action; disagreement; pairs; gaps; proof links |
| U8 Answer / brief | Question; 📚 your cases (cited, gaps first) · 🧠 general (labelled) · 💡 suggestion (switchable); checks; made-by; knowledge snapshot; rating |

---

## 7. AI providers: two separate versions (locked)

- **Development runs on OpenAI now** (company account, monthly spending limit ≈ $350).
- Keep **two versions, switchable as a whole**: 🟠 **Version O** = all cloud-AI blocks on OpenAI (cash) · 🟢 **Version B** = all cloud-AI blocks on AWS Bedrock (AWS credits; Claude Haiku 4.5 eye, Claude Sonnet 5 text).
- **One AI gateway module**: every cloud-AI call goes through it (tasks: see pictures · structured text · embeddings · later speech/voices). Blocks never call providers directly.
- Both versions must return the same data units; wrong shape = test failure.
- **Separate search index per version** (embeddings from different providers can't be mixed).
- Test Harness compares O vs B on the same cases at each gate.
- The factory's obstacle-mark helper AI must differ from both versions' eyes (Amazon Nova; Version B never uses Nova for See).
- GPU parts (open-source speech engine, ready-made research models) are shared by both versions.

---

## 8. Build gates (current: Gate 0a)

| Gate | Done when |
|---|---|
| 0a Dev start | OpenAI key in `.env` · laptop env works · (vast.ai GPU when needed) |
| 0b AWS setup (parallel, CEO) | Company AWS account, credits, GPU quota, Bedrock access |
| 1 Walking skeleton | All 27 blocks exist with plugs + FAKE outputs · slice-based · replay at any speed · one fake case flows end to end · every output saved + stamped · Test Harness + Audit Log from day 1 |
| 2a Track A factory | 3 scripted test cases + answer keys |
| 2b Track B data | 10 surgeon-narrated cases + answer keys (needs the surgeon) |
| 3 Senses | Real B02–B08 · privacy hard rules pass · baselines recorded · works live |
| 4 Brain | Real B09–B13 · promise ①② pass marks on tuning cases · danger-zone rule passes · draft cards live |
| 5 Knowledge + answers | Real B14–B20 · zero uncited claims · promise ③④ pass |
| 6 Final exam + demo | 3 locked cases · all hard rules + pass marks hold · CEO demo |

Gate rules: close by checklist, never by date · stuck → fix or cut scope in writing · each closed gate = short update to the CEO.

**Quality:** 🔴 hard rules (0 leaked names, 0 low-confidence danger-zone mappings, 0 uncited claims, 0 unapproved cards, 0 hidden eponyms) · 🟡 pass marks (≥80% planted obstacles found, ≤1 invented in routine cases, review < 2 min/case, ≥70% approved unedited, ≥90% same cards across languages, patterns from 2+ cases, edit rate falls, "seen together" 100%) · 🔵 measure-first (speech errors, roles, steps/tools accuracy, moments matched, "not enough evidence").

---

## 9. What NOT to do

- ❌ Train or fine-tune models (MVP uses ready-made AI only)
- ❌ Show anything to the operating surgeon during surgery
- ❌ Put real patient data anywhere (MVP = public/fake data)
- ❌ Call OpenAI/Bedrock directly from a block (use the AI gateway)
- ❌ Mix embeddings from different providers in one index
- ❌ Let unapproved cards into patterns, search results or answers
- ❌ Commit `.env`, keys, datasets, or large media to git
- ❌ Make product decisions silently — ask Karthi

---

## 10. Open work (start here)

1. **Blueprint Part 2 — the skeleton + tech picks.** Decide with Karthi, one topic at a time (suggested defaults in `docs/NEXT_STEPS.md`).
2. **Gate 0a** setup, then **Gate 1** walking skeleton.
3. Optional: fold the change log into the Notion pages ("Step 2").

## 11. References

- Notion plan (source of truth for planning): "MY ROBOTS · MVP Plan (Black Box + Outline)", incl. "Change log · decisions after v1.0" and "Resource & Cost Plan (v2)".
- PDF guide v1.0 (`MY_ROBOTS_MVP_Plan_Complete_Guide.pdf`) — accurate except changes listed in `docs/PLAN_v2.md` §0.
- Visual explainers: "MY ROBOTS MVP: how the project flows" and "MY ROBOTS · Block by Block" (claude.ai artifacts).
