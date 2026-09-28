# HANDOVER — read this first

> **Purpose:** anyone (a new Claude Code session, another terminal, another laptop) can pull this repo and continue exactly where we stopped.
> **Last updated:** 28 Sep 2026 · **Phase:** planning — block-by-block design in progress (no code yet)
> **Update rule:** at the end of every working session, update §2, §3 and §5 (and `docs/NEXT_STEPS.md`).

---

## 1. What this repository is

The planning workspace for the **MY ROBOTS Surgical Knowledge MVP**: surgery video + OR speech + a short post-op debrief → surgeon-approved "episode cards" → knowledge that grows with every case. Procedure: laparoscopic cholecystectomy.

Only two people work on it: **Karthi** (decides + builds) and **Claude** (senior advisor + developer, via Claude Code).

---

## 2. Current status (one screen)

| Item | Status |
|---|---|
| Master plan (`docs/MASTER_PLAN.md`) | **Almost approved** — 12 of 13 chapters ✅ · ch. 11 Roadmap & budget ⏸ on hold until Karthi's discussion · "a few more changes later" |
| CEO document (`docs/CEO_Explanation.md`) | ✅ Written (no cost figures, by request). Karthi to review and share |
| Block-by-block design (`docs/blocks/`) | 📥 Ingest ✅ · 🔌 AI gateway ✅ · 👁 See ✅ · 👂 Hear: B06 ✅ B07 ✅ · **next: B08 Privacy Filter** |
| Code | ❌ None yet — **no code until Karthi's final go** |
| Data source | 🟡 Open — handled as a separate process (public datasets, real clinical videos, or other) |
| Repo | ✅ `karthi-ai-engineer/myrobots-plan` · **public** (Karthi's choice, 24 Sep) · commits only as karthi-ai-engineer (see `CLAUDE.md` §1) |

---

## 3. What happened so far (session log)

### 24 Sep 2026 — master plan session (rethink from scratch)

| # | Decision | File |
|---|---|---|
| 1 | Create a master plan before any code; rethink the earlier plan (PLAN_v2) from scratch; outside-in order | `docs/decisions/2026-09-24-master-plan-approach.md` |
| 2 | ❌ Track B (surgeon think-aloud narration) dropped — no surgeon time | `…-drop-track-b.md` |
| 3 | Two equal use cases: 🆕 new surgery (live, 2–3 min post-op debrief by the operating surgeon, voice + tap) and 🗄 archive (batch, 👁 reviewer-checked). Audience: CEO + surgeons + investors. No clinician available. Web app first, phone-friendly | `…-use-cases-and-review.md` |
| 4 | MVP claim = full loop (proven by tests) + value (shown by a research-grounded **stand-in surgeon = Karthi**, blind on locked cases) | `…-mvp-claim.md` |
| 5 | Success metrics: hard rules, pass marks, "shown", measure-first | `…-success-metrics.md` |
| 6 | Guardrails: 22 rules (12 kept, 6 changed, 4 new) | `…-guardrails.md` |
| 7 | Scope: 13 cases (3 dev + 7 tuning + 3 locked); Japanese voice only, 🇯🇵/🇬🇧 display; Eye v2 parked | `…-scope.md` |
| 8 | Knowledge core (delegated to Claude): cards = truth, IDs-first RAG, rules-only patterns, growth replay | `…-knowledge-core.md` |
| 9 | Pipeline (28 blocks), skeleton, tech stack, AI per block, test & quality (Claude drafts, accepted) | `…-technical-chapters.md` |
| 10 | Factory (tentative): Claude via subscription writes scripts (locked cases by a background agent), VOICEVOX voices, obstacle marks from dataset labels; OpenAI API runs Version O | `…-technical-chapters.md` |
| 11 | Data plan: data source open, plan is data-agnostic, intake requirements | `…-data-plan.md` |
| 12 | Ch. 11 on hold; ch. 12 risks + ch. 13 how-we-work accepted "for now"; GPU = AWS (vast.ai fallback only, never real data); final consistency review | `…-review-and-remaining.md` |
| 13 | Procedural Graphs paper (arXiv 2609.09153) read and **parked** for later versions | `docs/research/2026-09-24-procedural-graphs-paper.md` |
| 14 | CEO explanation document written | `docs/CEO_Explanation.md` |
| 15 | GitHub set up: repo `karthi-ai-engineer/myrobots-plan` (public, Karthi's choice), karthi-ai-engineer as the only contributor, no assistant attribution (enforced by settings + hooks); mentions of Claude inside documents kept | `CLAUDE.md` §1 |

### 25–28 Sep 2026 — block-by-block design (lane by lane)

| # | Decision | File |
|---|---|---|
| 16 | Plan every block with one template (job · in→out · how · AI or rules · live vs batch · defaults · checks · tests · decisions), lane by lane in data-flow order | `docs/blocks/README.md` |
| 17 | B01 Case Intake: one camera · required intake form · 🏁 only by explicit signal, never by timeout | `docs/blocks/B01-case-intake.md` |
| 18 | B02 Media Prep: rules only (FFmpeg) · 768-px frames · 16 kHz audio in 30-s chunks · 480p review video · "camera outside the body" flag · first audio track | `docs/blocks/B02-media-prep.md` |
| 19 | AI Gateway: task menu · privacy lock · fake provider for tests · hard budget stop + manual override · Batch API for tuning · save every prompt + answer · cloud transcription refused for real data | `docs/blocks/core-ai-gateway.md` |
| 20 | B03 Eye: B04 stays merged in · standard labels (7 steps · 7 tools · 9 actions) · blood/bile hint · previous step as context | `docs/blocks/B03-eye.md` |
| 21 | B05 Event Check: bleeding + bile · stronger vision model · voice keywords can trigger | `docs/blocks/B05-event-check.md` |
| 22 | B06 Speech → Text: OpenAI transcription stand-in (synthetic only) · debrief answers in a priority lane · automatic lock for real data | `docs/blocks/B06-speech-to-text.md` |
| 23 | B07 Who Spoke: pyannote local voice split · 5 roles · some factory voices deliberately similar | `docs/blocks/B07-who-spoke.md` |
| 24 | Carried to later blocks: debrief question "anything we missed?" (B24) · archive-first vs live-together stays a ch. 11 build-order question | `docs/blocks/README.md` |
| — | Record | `docs/decisions/2026-09-28-block-designs-ingest-see-hear.md` |

---

## 4. Where everything is

| File | Read when |
|---|---|
| `CLAUDE.md` | Every session (how to work + strict git rules) |
| `docs/MASTER_PLAN.md` | Every session — the plan; starts with a one-page summary |
| `docs/NEXT_STEPS.md` | To pick the next task |
| `docs/CEO_Explanation.md` | When talking to / preparing for the CEO |
| `docs/blocks/` | Detailed design of each block (status table in its README) |
| `docs/decisions/` | To see why something was decided |
| `docs/PLAN_v2.md` · `docs/history/` | History only (superseded plan and old briefing) |

---

## 5. Next steps (in order)

1. **Karthi + Claude:** continue block-by-block design → **B08 Privacy Filter**, then 🧠 Understand lane (B09 · B10 · B11) — see `docs/blocks/README.md`
2. **Karthi:** review `docs/CEO_Explanation.md` → share with the CEO
3. **Karthi + CEO:** discuss **ch. 11 Roadmap & budget** (draft in the master plan) → finalise
4. **Karthi:** final approval of the master plan (a few changes still expected)
5. **CEO asks:** OpenAI API key + spending limit · OpenAI credits programme · data (any source) + license / legal check · AWS credits + Bedrock + GPU quota
6. **After the final go:** Gate 0a (dev start) — see `docs/NEXT_STEPS.md`

**Open questions waiting for Karthi:**
- Are the company's local GPU machines available for this project? (could run speech-to-text on-premises)

---

## 6. Context that lives outside the plan

- **Dev machine (current):** Windows 11, 32 GB RAM, no NVIDIA GPU. Git, Python 3.13 (+3.12 via uv), uv, FFmpeg, Docker, Node installed. (The older docs wrongly assumed a MacBook Air with 8 GB.)
- **Accounts:** Claude **subscription** only (no Claude API key) · OpenAI API (company; credits programme pending) · AWS pending · GitHub: karthi-ai-engineer.
- **Claude's local memory** (per machine, not in git) held these working notes — they are now in `CLAUDE.md` §0–1, so nothing is lost on a new machine.

---

## 7. Set up on a new machine

```bash
# 1. Log in to GitHub as karthi-ai-engineer (or switch to it if already logged in)
gh auth login                          # choose github.com, HTTPS, log in as karthi-ai-engineer
gh auth switch --user karthi-ai-engineer
gh auth status                         # active account must be karthi-ai-engineer

# 2. Clone with the username in the URL (so the right credentials are used)
git clone https://karthi-ai-engineer@github.com/karthi-ai-engineer/myrobots-plan.git
cd myrobots-plan

# 3. Repo-local identity + hooks (required once per clone)
git config user.name  "karthi-ai-engineer"
git config user.email "296384397+karthi-ai-engineer@users.noreply.github.com"
git config core.hooksPath .githooks

# 4. Start Claude Code in this folder and say:
#    "Read HANDOVER.md, CLAUDE.md and docs/MASTER_PLAN.md, then continue from HANDOVER §5."
```
