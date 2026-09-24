# MY ROBOTS · MVP Master Plan

> Started 24 Sep 2026 · Karthi + Claude · **Status: almost approved** (24 Sep; a few changes later, ch. 11 on hold) · no code until final approval · CEO version: `CEO_Explanation.md`
> Mode: **rethink from scratch.** Each chapter reviews the matching part of `PLAN_v2.md` → ✅ keep · ✏️ change · ❌ drop.
> Order: **outside-in** (what we must prove → knowledge → pipeline → tech → roadmap).
> When a chapter is agreed, it gets ✅ and a decision file in `docs/decisions/`.

---

## One-page summary

**Product:** video shows *what* the surgeon did; OR speech + a 2–3 min post-op debrief capture *why*. Both go on one timeline and become **surgeon-approved episode cards** that add up to patterns and pre-case briefs.

**Two use cases, one pipeline:** 🆕 new surgery (processed **live** in slices; debrief ready ≤ 5 min after 🏁) · 🗄 archive video (batch; 👁 reviewer-checked).

**The MVP claim:** finds obstacles · proves every field · fast · few fixes · knowledge grows. ✅ **Proven** by tests on simulated surgeries · 🎭 **shown** by a research-grounded stand-in surgeon (Karthi) · 🔜 real speech, clinicians and real OR in a **pilot**.

**Scope:** lap chole · 13 cases (3 dev + 7 tuning + 3 locked) · Japanese voice (factory-made), 🇯🇵/🇬🇧 display · cloud eye only · Version O (OpenAI) now, B (Bedrock) later · **data source open** (separate process; plan is data-agnostic).

**Knowledge core:** approved cards are the truth · rule book / diary / textbook kept apart · graph in Postgres · IDs-first search · the AI writes words, the database writes numbers · growth replay.

**Build:** 28 blocks, one Python app, Postgres, web screens that work on a phone · privacy filter enforced in code · factory = Claude (subscription) + VOICEVOX · OpenAI API for Version O.

**Guardrails (22):** human approves (✅ / 👁 / 🎭) · evidence for every field · privacy before cloud · nothing before 🏁 · approved = frozen · honest labels · license before use · no model training.

**Open:** ch. 11 roadmap & budget (after discussion) · data source · license / legal check · OpenAI credits · AWS.

---

## Chapters

| # | Chapter | Question it answers | Status |
|---|---|---|---|
| 1 | North star | Who is the MVP for, what must it prove, how do we measure it? | ✅ 24 Sep |
| 2 | Guardrails | Which rules are never broken (safety, privacy, legal)? | ✅ 24 Sep |
| 3 | Scope | Procedure, cases, languages, live vs batch, AI versions | ✅ 24 Sep |
| 4 | Knowledge core ⭐ | Ontology, graph schema, search/RAG, patterns, briefs | ✅ 24 Sep (Claude, delegated) |
| 5 | Pipeline | Which blocks/lanes we really need, and their plugs (U-units) | ✅ 24 Sep (Claude, fast mode) |
| 6 | Skeleton | Modules, job queue, AI gateway, storage, slice clock | ✅ 24 Sep (Claude, fast mode) |
| 7 | Tech stack | Language, frameworks, database, UI, infra | ✅ 24 Sep (Claude, fast mode) |
| 8 | AI per block | Model, prompt contract, fallback, cost per block | ✅ 24 Sep (factory details decided in the flow) |
| 9 | Data plan | Datasets, licenses, factory, answer keys | ✅ 24 Sep (data source open, separate process) |
| 10 | Test & quality | Hard rules, pass marks, harness, final exam | ✅ 24 Sep (Claude, fast mode) |
| 11 | Roadmap & budget | Gates → work packages → critical path → cost | ⏸ on hold (after discussion; draft kept) |
| 12 | Risks & dependencies | Risk register; CEO / surgeon / license / AWS asks with dates needed | ✅ 24 Sep (for now) |
| 13 | How we work | Sessions, decision log, git, definition of done | ✅ 24 Sep (for now) |

---

## Decisions so far

| Date | Decision | File |
|---|---|---|
| 24 Sep | Rethink from scratch · plan in this file · outside-in order | `decisions/2026-09-24-master-plan-approach.md` |
| 24 Sep | ❌ Track B (surgeon narration) dropped | `decisions/2026-09-24-drop-track-b.md` |
| 24 Sep | Two use cases, equal · post-op debrief 2–3 min · audience A+B+C · no clinician · archive trust label · web-first UI · voice + tap answers | `decisions/2026-09-24-use-cases-and-review.md` |
| 24 Sep | MVP claim = B (full loop) + C (value, shown by a research-grounded stand-in surgeon) | `decisions/2026-09-24-mvp-claim.md` |
| 24 Sep | Success metrics: hard rules, pass marks, shown, measure-first (ch. 1.3) | `decisions/2026-09-24-success-metrics.md` |
| 24 Sep | Guardrails: 12 kept · 6 changed · 4 new; license = build internally, external demos after clearance; drafts hidden from all clinical users before 🏁 | `decisions/2026-09-24-guardrails.md` |
| 24 Sep | Scope: 13 factory cases (3 dev + 7 tuning + 3 locked) · Japanese voice only, 🇯🇵/🇬🇧 display via IDs · Eye v2 parked · B off critical path | `decisions/2026-09-24-scope.md` |
| 24 Sep | Knowledge core (delegated to Claude): cards = source of truth · 4 stores kept apart · dictionary design · graph in Postgres · IDs-first RAG · rules-only patterns · growth replay | `decisions/2026-09-24-knowledge-core.md` |
| 24 Sep | Pipeline, skeleton, tech stack, AI per block, test & quality (Claude fast mode, accepted) · factory = Claude subscription + VOICEVOX + dataset-label marks (tentative) · OpenAI API for Version O | `decisions/2026-09-24-technical-chapters.md` |
| 24 Sep | Data plan: data source open (separate process, CEO) · plan is data-agnostic · intake requirements · obstacles · factory · sealing · research pack | `decisions/2026-09-24-data-plan.md` |
| 24 Sep | Ch. 11 on hold (after discussion) · ch. 12 risks + ch. 13 how we work accepted "for now" · GPU = AWS (vast.ai fallback only) · final consistency review | `decisions/2026-09-24-review-and-remaining.md` |

## For the CEO update (collect, send once)

- Track B dropped → CEO ask #8 (surgeon narration time) no longer needed
- 📦 **Data source is open** (separate process): public datasets, real clinical videos, or other. Whatever comes must meet the intake requirements (§9.2)
- 🔴 If public datasets: Cholec80 + CholecT50 are **CC BY-NC-SA 4.0 (non-commercial)** → ask CAMMA for commercial permission, or get a legal opinion
- 🔴 If real clinical data: the "before real data" checklist must be done first, and cloud AI choice (OpenAI vs in-region Bedrock / hospital box) must be reviewed
- 📜 Our rule: we build internally now; **no external demo (investors, hospitals) with these videos until the license is cleared**. Needs a quick legal check that internal R&D use is OK
- No clinician available → all medical content is guideline-grounded and labelled 🧪 synthetic
- OpenAI credits via program X (≈ $10k?) → apply; check commercial-use and data-usage terms. Used for Version O
- ☁️ AWS: credits + Bedrock access + **GPU quota** (ask early; slow to approve). AWS GPU replaces vast.ai

## Parked for later versions (not in this plan)

- Procedural Graphs paper (arXiv 2609.09153) → `research/2026-09-24-procedural-graphs-paper.md`

---

## 1. North star
✅ Agreed 24 Sep 2026 · 1.1 ✅ · 1.2 ✅ · 1.3 ✅

### 1.1 Who it's for and how it's used ✅

**Audience: all three** (the demo must serve each):

| | Audience | What they must see |
|---|---|---|
| A | CEO (go / no-go) | Honest test scores, cost per case |
| B | Surgeons / hospital | Fast post-op debrief, useful briefs, privacy story |
| C | Investors | Polished demo, "knowledge grows with every case" |

**Two use cases, equal priority, one pipeline:**

| | 🆕 New surgery | 🗄 Archive video |
|---|---|---|
| Processing | **Live**, in slices during surgery | Batch |
| Who reviews | **Operating surgeon** | Person assigned by the hospital |
| When | **Within minutes after the end** (2–3 min budget) | Any time |
| Voice / "why" | OR talk + targeted "why" questions after surgery | Usually none → "why" = unknown |
| Trust label | ✅ surgeon-approved | 👁 reviewer-checked |
| MVP demo | Factory voices + F4 live stream (simulated OR) | Source videos as they are (no audio) |

```
 🆕 NEW SURGERY
 🕐 surgery (60–90 min)                      🏁 end    ~3–5 min          next case
 ──────────────────────────────────────────┼────────┼────────────────┼──────────▶
 🎥🎙 recorded + processed LIVE (slices)     │ final  │ 📱 debrief      │
 🤖 draft cards build up (🚫 hidden in OR)   │ check  │ 2–3 min: tap +  │
                                            │ (light)│ 🎙 short "why"  │
```

**Post-op debrief rules:**
- Drafts must be ready ≈ 3–5 min after the end → processing must run live (batch after the end ≈ 15–45 min)
- Review unlocks only at 🏁. Nothing is shown during surgery
- The app asks targeted questions only where a card's "why" or key field is unknown
- Answers: **tap** (yes/no, pick from list) + **voice** (short "why", ~30 s)
- Final check at the end is light (minutes). Nothing changes a card after approval
- **Debrief skipped or late:** drafts wait in the surgeon's queue with reminders; late review is allowed (the "why" questions are still asked); **never auto-approved**

**Screens:** one web app, built for desktop first; the debrief screen made to work well in a phone browser. A dedicated phone app comes later.

**Medical content:** no clinician is available → scripts and "why" are grounded in published guidelines + dataset labels, and labelled 🧪 synthetic. A research-grounded **stand-in surgeon** plays the surgeon role (see 1.2).

### 1.2 The claim ✅

**Scope: B (full loop) + C (value), with C shown by a stand-in surgeon, not proven.**

> **While a surgery is running, the system turns video + OR speech into evidence-backed obstacle cards. They're ready within minutes of the end, need few corrections, and add up across cases into knowledge that grows with every case. The same pipeline turns archive videos into reviewer-checked cards, and the cards, briefs and answers are useful to a surgeon preparing a case.**
> *Tested on simulated surgeries against exact answer keys.*

**Five provable parts:**

| # | Part | CEO req. |
|---|---|---|
| ① | **Finds** the real obstacles, invents none | 1 |
| ② | **Proves** every field (transcript line or video time) | 1 |
| ③ | **Fast**: drafts ready soon after 🏁 (live mode) | new |
| ④ | **Few fixes**: few edits needed, debrief fits 2–3 min | 2 |
| ⑤ | **Grows**: cases add up to correct patterns and better briefs | 3, 4 |
| + | 🔴 all hard rules at zero | — |

**Three levels of proof (said openly in every demo):**

| Level | What | How |
|---|---|---|
| ✅ **Proven** | ①–⑤ + hard rules | Test Harness vs sealed answer keys; the answer key also plays a "perfect reviewer" to count edits |
| 🎭 **Shown** | Usefulness (C) · debrief in 2–3 min · archive review | Stand-in surgeon, research-grounded, reviewing blind |
| 🔜 **Pilot** | Real OR teams say the "why" · clinicians confirm medical correctness · real surgeon time · real OR network | First hospital pilot |

**Stand-in surgeon rules:**
- Plays the operating surgeon (debrief, approvals, "why" answers) and the archive reviewer
- Works from a **research pack** of published sources (e.g. Tokyo Guidelines 2018, SAGES safe cholecystectomy); every medical statement cites a source
- Reviews the **locked cases blind**: never sees their scripts or answer keys (sealed envelope). Dev and tuning cases may be seen
- Everything they approve carries a 🎭 stand-in label; the demo says so clearly

**Keeping the test fair:** script writer (Claude) = different AI family from the Version O extractor (OpenAI); Version B results labelled ⚠ same-family · trap rules (off-timed, vague, unspoken, chatter, fake names) · answer keys sealed · locked exam cases never used for tuning · some obstacles only in video, some only in speech.

**Who + blind review (confirmed 24 Sep):**
- 🎭 Stand-in = **Karthi** (taps + voice answers, the human in the loop) · **Claude** builds the research pack with citations
- 🔒 **Blind:** locked-case scripts and answer keys are made by a background agent and sealed. Karthi never reads scripts or keys for the locked cases. The numbers (edit rate, found/invented) come from the simulated reviewer (answer key); Karthi's debrief shows the human flow and review time

### 1.3 Success metrics ✅

Starting values; revisited only in writing. Final-exam numbers come **only** from the locked cases. All results reported as counts first ("4 of 5 (80%) ⚠ small sample").

**🔴 Hard rules (must be 0):**
- Leaked names · low-confidence danger-zone auto-maps · uncited claims · unapproved cards in knowledge base / patterns / answers · wrongly hidden eponyms
- Anything shown to clinical users before 🏁 (during surgery)
- Reviewer access to a sealed answer key (checked via the Audit Log)

**🟡 Pass marks:**

| Part | Metric | Target |
|---|---|---|
| ① Finds | Planted obstacles found | ≥ 80% |
| | Invented obstacles in routine cases | ≤ 1 total |
| ② Proves | Filled fields with a working evidence link | 100% |
| | Evidence points to the right moment (±10 s of the key) | ≥ 90% |
| ③ Fast | Debrief ready after 🏁 (live mode) | ≤ 5 min |
| | Draft card after the event (live) | ≤ 3 min |
| ④ Few fixes | Cards approved unedited (simulated reviewer) | ≥ 70% |
| | Debrief time per case (stand-in, blind) | ≤ 3 min |
| | "Why" questions asked per case | ≤ 3 |
| ⑤ Grows | Pattern counts match answer keys | 100% |
| | Each pattern built from 2+ cases · "seen together" correct | 100% |
| | Edit rate falls from first to last tuning case | falls |
| | Brief questions answered with cited cases, or an honest "no evidence" | ≥ 90% |
| ~~🌐~~ | ~~Same cards in 🇯🇵 and 🇬🇧~~ | dropped in ch. 3 (Japanese voice only) |

**🎭 Shown (recorded, no pass mark):** stand-in usefulness rating (1–5) for briefs and answers · archive review time + edits per case.

**🔵 Measure first:** speech errors (per noise level) · who spoke · steps/tools accuracy · moments matched · "not enough evidence" correctness · cost per case.

## 2. Guardrails ✅

Agreed 24 Sep 2026. These replace `CLAUDE.md` §2 once the plan is approved. 22 rules: 12 kept · 6 changed · 4 new.

| Rule | Meaning | |
|---|---|---|
| ✅ Human approves | AI proposes; nothing becomes knowledge without a **human** approval. The label always shows who: ✅ surgeon (operating surgeon) · 👁 reviewer (archive) · 🎭 stand-in (MVP) | ✏️ |
| 📚 Approved only | Only human-approved cards enter the knowledge base, patterns, search and answers. Patterns show the mix (e.g. "2 ✅ + 1 👁") | ✏️ |
| ⚡ Unit = one obstacle episode | The basic unit is an episode card (U1), not a video | = |
| 👁👂 Two witnesses | Video + voice agree = strong; one only = flagged. Two AI "eyes" never count as two witnesses | = |
| 📎 No evidence → no field | Every card field points to a transcript line, a video time, a **debrief answer** (surgeon's voice/tap after 🏁) or a **cited research source** | ✏️ |
| ❓ Unknown allowed, guessing not | If not said or seen → "unknown" | = |
| 🎯 A moment is a candidate | The extractor may answer "no obstacle here" | = |
| ⏱ One clock | Every timestamp = seconds from video/stream start | = |
| 🔒 Original never touched | Blocks work on copies | = |
| 🔒 Privacy before cloud | The privacy filter runs before ANY cloud AI call: OR speech, user questions **and debrief voice answers** | ✏️ |
| 🔒 Sealed envelope | Answer keys + raw transcripts visible only to the Test Harness / tester role. **The reviewer never opens keys** (checked in the Audit Log) | ✏️ |
| 🫀 Danger zone | Danger-zone anatomy (e.g. cystic duct vs common bile duct) is never auto-mapped at low confidence → review | = |
| 🔢 Small numbers | "2 of 3 (67%) ⚠ small sample"; counts first. Applies to our own test reports too | = |
| ⚖️ Disagreement | Different surgeons' approaches shown side by side, never averaged | = |
| 🚫 Nothing before 🏁 | Live drafts are hidden from **all clinical users** (surgeon, resident, admin) until the end-of-surgery signal. Only the tester role sees drafts, in simulation only. The debrief unlocks after 🏁 | ✏️ |
| 🔑 Secrets | API keys only in `.env` (git-ignored) or a secret manager. Never in code, logs or chat | = |
| 🏥 No real patient data | Public/fake data only until the "before real data" checklist is complete | = |
| 🧪 No model training | Ready-made models only (cloud AI or downloaded, run-only) | = |
| 🏷 Honest labels | 🧪 synthetic · 🎭 stand-in · 👁 reviewer-checked · 🧠 general knowledge, visible on every card, screen and demo | 🆕 |
| 🔏 Approved = frozen | Nothing changes an approved card (final check, re-run, new AI version). A change = new version + re-approval; history kept | 🆕 |
| 📜 License before use | Data used only within its license. **Build internally now; external demos only after the dataset license is cleared** (legal check on internal R&D use) | 🆕 |
| ⚕️ No treatment advice without a regulatory check | The 💡 suggestion layer stays behind the admin switch until the MHLW SaMD check | 🆕 |

## 3. Scope ✅

Agreed 24 Sep 2026.

| Item | Scope | |
|---|---|---|
| Procedure | Laparoscopic cholecystectomy (a plug-in: new dictionary + videos, not the product) | ✅ |
| Use cases | 🆕 new surgery (live, slices) + 🗄 archive (batch), equal priority — see ch. 1 | ✏️ |
| Voice | Factory-made (AI scripts + synthetic voices), **🇯🇵 Japanese only**; real OR audio used instead if it arrives with the data (§9.1) | ✏️ |
| Languages shown | Cards, UI, answers in 🇯🇵 + 🇬🇧 via dictionary IDs + translation. **No twin extraction, no twin merge** | ✏️ |
| Video AI | Cloud eye (v1) only: L1 steps · L2 tools · L3 action hints · 1 picture/s | ✏️ |
| Obstacles | 3–4 types (general level), each in 2–3 cases, ≥ 2 paired cases; must be visible in the video; factory adds speech around them | ✅ |
| Noise | Medium = main version; easy + hard = speech stress test only | ✅ |
| AI versions | O (OpenAI) now; B (Bedrock) built when AWS is ready, off the critical path | ✏️ |
| Archive | Video-only draft cards → 👁 reviewer-checked → knowledge base (labelled; one witness → flagged) | ✏️ |
| Roles | Surgeon · resident · admin · tester · **archive reviewer** | ✏️ |

**Cases: 13 in total** (factory-made voice unless the data brings real OR audio)

| Set | Count | Who sees answer keys | Used for |
|---|---|---|---|
| 🔧 Dev | 3 | Everyone | Building and debugging |
| 🎯 Tuning | 7 (5 hard + 2 routine) | Test Harness + Claude (Karthi may see) | Tuning; simulated reviewer scores |
| 🔒 Locked exam | 3 (2 hard + 1 routine) | Nobody until the exam | Final exam + Karthi's blind debrief |

Demo knowledge base = approved cards from the 10 tuning + locked cases.

**🅿️ Parked (not in MVP):** Eye v2 research video model (trained on Cholec80 + non-commercial weights + GPU setup) · twin-language extraction · archive Tier 2 surgeon narration · Backlog Planner · Procedural Graphs paper · real capture box · native phone app.

## 4. Knowledge core ✅

Decided by Claude (delegated by Karthi, 24 Sep 2026) for a rapid MVP. Karthi may veto any point.

### 4.1 Knowledge model

```
 📖 DICTIONARY          📚 OUR CASES              🧠 GENERAL LIBRARY
 "the rule book"        "the diary"               "the textbook shelf"
 what things ARE        what HAPPENED             what the literature SAYS
        │                      │                          │
        └──── IDs ─────────────┤                          │ cited, never counted
                               ▼                          │
                 🕸 KNOWLEDGE GRAPH (derived: diary + rule book)
                               ▼                          ▼
          📊 patterns · 🔍 search · 📋 briefs · 💬 answers (3 layers)
```

- **Source of truth = approved card versions** (+ the dictionary version they used). The graph, patterns and search indexes are **derived** and can be rebuilt at any time.
- Drafts stay in the pipeline area. The knowledge base reads only human-approved versions (✅ / 👁 / 🎭).
- A case observation never becomes a dictionary fact. Frequency lives only in counted patterns.
- The general library (research pack, guidelines) is cited in the 🧠 layer and grounds the stand-in, but is never counted as our experience.

### 4.2 Dictionary (ontology)

| Aspect | Decision |
|---|---|
| Kinds | step · anatomy · tool · action · obstacle · warning sign · outcome · safety check (CVS items) · severity grade |
| Two levels | general ↔ specific via `is_a`. Counts roll up to general. A card stores the most specific term its evidence supports |
| Labels | 🇯🇵 + 🇬🇧 label, synonyms in both languages including OR slang (e.g. 「血が出てる」 → bleeding), definition + source |
| Flags | `danger_zone` · `safe_name` (eponyms the privacy filter must not hide: Calot, Rouvière, Hartmann, Mirizzi, Luschka, Strasberg) |
| Rule-book relations | `is_a` · `part_of` · `confusable_with` (e.g. cystic duct ↔ common bile duct; drives danger-zone review) |
| External links | OntoSPM IRI where a match exists · SNOMED slot empty |
| Seed sources | (Generic surgical vocabulary; if these datasets aren't used, the same terms come from the research pack) Cholec80 phases → steps (+ Tokyo Guidelines 2018 safe-step checkpoints) · CholecT50 instrument / verb / target words → tools / actions / anatomy · CVS criteria → safety checks · obstacles (3–4 general + specific children), warning signs and severity scale from the research pack (severity grounded in a published intraoperative adverse-event classification, e.g. ClassIntra, to verify) |
| Growth | New word → exact / synonym match → attach; else **pending zone** (usable, labelled pending, not counted) → **weekly approval by the stand-in** with the research pack → new dictionary version. Merge = alias (old ID points to survivor; cards re-pointed in a new version; history kept). Never overwrite |
| Size | ≈ 150 terms in v1 |
| Storage | Seed + approved versions as files in git (reviewable diffs); pending zone in the database |

### 4.3 Graph schema

Built from approved card versions + the dictionary. **Postgres, no separate graph database in the MVP.**

```
        Case 07 ──has──▶ Episode E07-1 ◀──approved_by── 🎭 stand-in (card v2)
                          │
     ┌──────────┬─────────┼───────────┬────────────┬───────────┐
  obstacle   in_step   at_anatomy   warning     response ①②③   outcome
     ▼          ▼          ▼           ▼             ▼            ▼
  bleeding   Calot     cystic      fat hides    pressure →     worked
  (specific) dissect.  artery 🫀   structures    suction → clip★
     │is_a                                         (★ = resolving action)
     ▼
  bleeding (general)     every link ──evidence──▶ 📎 line L-112 · video 1418–1450 s · debrief Q2
```

- **Nodes:** Case · Episode · ResponseStep · Evidence · Person (surgeon persona / reviewer / stand-in) · Term (dictionary)
- **Episode properties:** severity · why · confidence per field · witness · evidence source · trust label · card version · dictionary version · AI version
- **Case-side edges:** has_episode · obstacle · in_step · at_anatomy · warning · response (ordered) → action / tool / target · resolving · outcome · paired_with · approved_by · evidence
- **Rule-book edges:** is_a · part_of · confusable_with
- **Walks needed:** roll-up via is_a · neighbours of an obstacle (steps, warnings, responses, pairs) · danger-zone confusables

### 4.4 Search + answers (RAG)

```
 question → 🔒 privacy filter → map to IDs (exact → synonym → meaning → AI judge)
    → retrieve: ① IDs + graph walk (primary) ② meaning search (per-AI-version index) ③ keyword (Japanese-capable)
    → merge (rank fusion) → evidence pack: approved cards + pattern numbers + 🧠 passages
    → write 3 layers → checker → save with snapshot → one-tap rating
```

- **IDs first:** a question about "bleeding" finds every bleeding episode through IDs, whatever the words or language.
- **The LLM writes words, the database writes numbers:** every count and percentage comes from the pattern engine and is inserted, never generated.
- **Every 📚 sentence cites ≥ 1 approved episode.** The checker removes unsupported sentences and counts them (hard rule: 0 uncited in the final answer).
- **Gaps first:** say what our cases don't cover before what they do.
- 🧠 layer cites the library, labelled unverified; conflicts with our cases are flagged. 💡 only when the admin switch is on.
- **Snapshot:** each answer stores card versions + dictionary version + AI version, so it can be reproduced.
- Keyword search must handle Japanese (tool picked in ch. 7).

### 4.5 Patterns + pre-case briefs

**Pattern card** (per general obstacle; drill down to specific). **Rules only, no AI.** Recomputed on every approval or correction.
- Counts first: "3 of 7 cases (43%) ⚠ small sample". ⚠ shown while total cases < 20 (always, in the MVP)
- Breakdown by specific obstacle · step · anatomy
- Warning signs seen before (counts)
- Responses grouped by resolving action · common sequences · outcomes · severity mix
- By surgeon, side by side · trust mix (✅ / 👁 / 🎭)
- Seen together (pairs) · gaps (unknown-heavy fields, no successful response, few cases) · proof (episode IDs + clips)

**Pre-case brief:**
- Input: pick expected obstacles (and/or a step), or free text → IDs
- Per obstacle: warning signs to watch · what worked (by surgeon) · what failed · seen together · gaps · 🧠 guideline reminders (e.g. CVS before clipping) · links to source clips
- One screen, printable. No prediction from patient data.

### 4.6 How knowledge grows

- **On every approval:** card version → graph update → patterns recount → search indexes update → briefs change.
- **Growth dashboard:** cases · approved episodes · obstacle coverage (types with ≥ 2 cases) · brief-question coverage · edit-rate trend · pending words · rejections.
- **Growth replay** (Test Harness + demo): add cases one by one (1 → 10), run the question set after each, plot coverage and edit rate. Proves claim ⑤ and doubles as the investor demo.
- **Feedback learner:** best approved cards → examples for the extractor · the same fix seen ≥ 3 times → proposed rule → tested on tuning cases → adopted only if scores don't drop · learn forward only (approved cards are never re-edited).

## 5. Pipeline ✅

Drafted by Claude (fast mode), accepted by Karthi 24 Sep 2026.

### 5.1 Blocks after the rethink: 28

Old numbers are kept so older documents still match. 26 of the old 27 kept (many changed) · B04 merged into B03 · 2 new (B24, B25) · Backlog Planner parked.

| Lane | Block | Job | Change |
|---|---|---|---|
| 🏭 Factory | F1 Script Drafter | 🇯🇵 OR scripts grounded in research pack + dataset labels; trap rules. MVP: scripts authored with Claude (subscription), F1 imports, validates, seals | ✏️ Japanese only; factory AI ≠ extractor AI |
| | F2 Voice Maker | One synthetic voice per role (surgeon, assistant, nurse, anesthetist) | ✏️ Japanese only |
| | F3 OR Mixer | Voices + OR noise into the video; easy / medium / hard; seal answer keys | ✏️ 3 versions per case (was 6) |
| | F4 Live Streamer | Push a finished case as live media segments at 1× / 10× / max | = |
| 📥 Ingest | B01 Case Intake | ID, fingerprint, store original, **case status incl. 🏁 end signal**, case set (dev / tuning / locked), use case (live / archive) | ✏️ owns 🏁 |
| | B02 Media Prep | Pictures 1/s · audio chunks (30 s, 5 s overlap) · one clock · blur slot (empty) | = |
| 👁 See | B03 Eye | **Steps + tools + action hints in one call** per 10 pictures → video facts | ✏️ B04 merged in (one call instead of three) |
| | B05 Event Check | Bleeding yes/no on a ~15 s clip, only when a clue fires | ✏️ hints moved to B03 |
| 👂 Hear | B06 Speech → Text | Japanese words + times + confidence; medical word list; **also debrief voice answers** | ✏️ |
| | B07 Who Spoke | Voice split (before privacy) → role from content (**after** B08) | ✏️ order fixed for "privacy before cloud" |
| | B08 Privacy Filter | Local only: names / IDs / dates → [PATIENT]; eponym safe list; **also debrief answers** | ✏️ |
| 🧠 Understand | B09 Timeline Merger | Moments: any clue opens; look-back; join overlaps; strength tag. Rules only | = |
| | B10 Episode Extractor | Moment → draft card or "nothing here"; evidence per field | ✏️ no twins |
| | B11 Ontology Mapper | Words → IDs; pending zone; danger-zone review | ✏️ twin match removed |
| ✅ Review | **B24 Debrief Builder** | After 🏁: pick cards, ≤ 3 targeted "why" questions, clips → debrief pack → notify | 🆕 |
| | B12 Review Desk | Two modes: 🆕 **debrief** (phone-friendly, 2–3 min, operating surgeon / stand-in) · 🗄 **archive review** (desktop, reviewer) | ✏️ |
| | B13 Feedback Learner | Examples + repeated fixes (≥ 3) → rules, gated by tuning scores | ✏️ gate added |
| 📚 Know | B14 Knowledge Graph | Derived from approved card versions (ch. 4) | ✏️ derived |
| | B15 Search Index | IDs + meaning + Japanese keyword; one index per AI version | = |
| | B16 Pattern Builder | Rules only; recount on every approval | = |
| | **B25 General Library** | Research pack + guidelines: sources, passages, citations; feeds 🧠 layer and the stand-in | 🆕 |
| 💬 Ask | B17 Answer Engine | 3 layers, IDs-first retrieval, citation checker, snapshot, rating | = |
| | B18 Case Prep Brief | Expected obstacles → patterns, pairs, gaps, 🧠 reminders, clips | = |
| | B19 Growth Dashboard | Growth metrics + **growth replay** | ✏️ |
| 🛡 Always On | B20 Login & Roles | Surgeon · resident · admin · tester · archive reviewer | ✏️ |
| | B21 Audit Log | Append-only; IDs not content; AI versions; **sealed-key access** | ✏️ |
| | B22 Ontology Manager | Terms, pending zone, weekly approval, versions, aliases, 2 levels | = |
| | B23 Test Harness | Scores vs sealed keys; simulated reviewer; growth replay; O vs B | ✏️ |

### 5.2 Two flows, one set of blocks

```
 🆕 LIVE (new surgery)                                     🗄 ARCHIVE (recorded video)
 F4 / capture box ──segments──▶ B01 (save as it arrives)    upload ──▶ B01
        │ slices every 10 s (video) / 30 s (audio)                │ one big slice
        ▼                                                         ▼
 B02 ─┬─▶ B03 Eye ──▶ B05 (on clue)                         B02 ─▶ B03 ─▶ B05
      └─▶ B06 ─▶ B07 split ─▶ B08 ─▶ B07 role                (no audio → Hear lane empty)
        ▼                                                         ▼
 B09 moments ─▶ B10 drafts ─▶ B11 IDs   (🚫 hidden)          B09 ─▶ B10 ─▶ B11
        │ 🏁 end signal                                           ▼
        ▼                                                  B12 archive review (👁)
 close moments ─▶ light final check ─▶ B24 debrief pack             │
        ▼                                                         │
 B12 debrief (✅ / 🎭) ◀── 📱 notify                                 │
        └──────────────▶ approved card version ◀──────────────────┘
                              ▼
            B14 graph · B16 patterns · B15 index · B17/B18 answers + briefs
```

**Live timing:** pictures every 10 s (10 per request) · audio 30 s chunks, 5 s overlap · moments close after 60 s of quiet (5 min max) · drafts ≈ 2–3 min after an event.
**At 🏁:** close open moments → draft them → light final check (dedupe, pairs, danger-zone re-check, pending terms) → debrief pack → notify. Target ≤ 5 min.

### 5.3 Plugs (data units) after the rethink

| Unit | Change |
|---|---|
| U1 Episode card | − languages / twin agreement · + **trust label** (✅ / 👁 / 🎭) + approved_by role · + evidence types (line · video · **debrief answer** · **research source**) · + `resolving` flag on response items · + card version |
| U2 Case package | + case set (dev / tuning / locked) · + use case (live / archive) · − twin version role |
| U3 Video fact | source = eye v1 or tool rule (no v2) |
| U4 Transcript line | + source (OR speech / debrief answer) · language = ja |
| U5 Moment | − twin |
| U6 Dictionary term | + `confusable_with` · kinds as in ch. 4.2 |
| U7 Pattern card | + trust mix |
| U8 Answer / brief | = |
| 🆕 U9 Debrief pack | case · cards in review order · ≤ 3 questions (card, field, question text, clip) · ready-at time · answers (tap values, voice → U4 line IDs) |
| 🆕 U10 Library passage | source (title, organisation, year, link / DOI) · section · text · language · license note · term IDs |
| 🆕 U0 Slice | case · kind (video / audio / window) · start · end · sequence. The unit every job works on |

## 6. Skeleton ✅

Drafted by Claude (fast mode), accepted by Karthi 24 Sep 2026.

```
            🛎 FRONT DESK (web)                        🏭 CONVEYOR BELT (workers)
  debrief · archive review · ask · brief ·      block jobs pulled from the queue
  dashboard · dictionary · test reports          (CPU workers on the laptop/server;
            │                                     GPU worker only when needed)
            ▼                                                  ▲
  ┌──────────────────────────── one Python app (modules) ─────┴──────────────┐
  │ blocks/ (one module per block: run(slice) → outputs · fake mode · README) │
  │ core/: contracts · queue · wiring · clock · ai_gateway · storage · stamps │
  └──────────────┬──────────────────────────────┬─────────────────────────────┘
                 ▼                              ▼
        🐘 PostgreSQL                     📦 file storage (local → S3)
  records · jobs · graph · vectors ·     originals · derived outputs · 🔒 sealed/
  keyword index · audit log
```

| Part | Decision |
|---|---|
| App shape | **One app, separate modules** (one per block). Two process types: web (front desk) + workers (conveyor). GPU worker is a separate remote process |
| Block plug | Every block: `run(slice, inputs) → outputs`, input/output contracts, `fake` mode, one-screen README (job · in · out) |
| Job queue | **Postgres-based queue**. A job = (block, block version, case, slice, input hashes). Workers claim jobs safely; retries; **idempotent** (same inputs + versions → same output, skip if it exists). Any block can re-run alone |
| Wiring | A small wiring table: when a block saves an output for a slice, the downstream jobs whose inputs are ready are queued |
| Live transport | **Media segments over HTTP** (2–10 s video+audio files, like HLS). F4 pushes them; a real capture box can push the same with FFmpeg later. B01 appends to the saved recording (record first, process second) |
| Slice clock | Virtual clock: 1× (demo), 10× (tests), max (batch). Archive = one big slice |
| 🏁 end signal | Owned by B01. Sources: stream end, or an "End surgery" button. Real OR detection later |
| AI gateway | Tasks: see pictures · structured text · embed · transcribe · speak (factory) · judge. Adapters: OpenAI (O) · Bedrock (B) · VOICEVOX (local, factory voices). Factory scripts are written offline with Claude (subscription) and imported, not called through the gateway. Validates every reply against its contract · logs tokens + cost · caches by input hash · rate limits |
| Privacy in code | The gateway **refuses any text without a B08 clearance stamp**, so "privacy before cloud" is enforced by the code, not by discipline |
| No silent fallback | If a provider fails, the job waits and retries. It never switches provider mid-run (that would mix versions) |
| Storage | Adapter: local folder (dev) → S3 (AWS). `cases/{id}/original/` (never touched) · `derived/{block}/{version}/` · `sealed/` (tester role only, every access audit-logged) |
| Save + stamp | Every output saved as a new version, never overwritten; stamped with block + version · provider · model · prompt version · dictionary version · input hashes · time |
| Audit log | Append-only table; IDs not content; includes sealed-key access |
| Backups | Daily database dump + storage copy (local disk → S3 later) |
| Notifications | MVP: in-app banner + debrief link. Push / email / LINE later |
| Environments | E1 laptop (Windows + Docker Postgres) = dev and first demos · E3 GPU worker when the speech engine moves off the cloud stand-in: **AWS GPU (credits, Tokyo) preferred**; vast.ai only as a fallback if AWS is late, and **never with real data** · E4 cloud AI · E5 local disk → S3. A cloud app server (E2) only when external demos are allowed |

## 7. Tech stack ✅

Drafted by Claude (fast mode), accepted by Karthi 24 Sep 2026.

| # | Area | Pick | Why |
|---|---|---|---|
| 1 | Language | **Python 3.12** (managed by **uv**) | One language for app + AI + media; 3.12 is safest for speech / audio libraries |
| 2 | Web backend | **FastAPI** | Typed, async, simple |
| 3 | Screens | **Server-rendered: Jinja2 + HTMX + Alpine.js + Tailwind** | Least code for 2 people; debrief page works in a phone browser (voice via the browser's recorder) |
| 4 | Contracts | **Pydantic v2** + exported JSON Schemas, versioned | The plugs become checkable code |
| 5 | Database | **PostgreSQL** (Docker) + **pgvector** | One database for records, queue, graph, vectors, keyword |
| 6 | Japanese keyword search | **SudachiPy** splits Japanese into words → stored in Postgres full-text ("simple" config) | Postgres can't split Japanese by itself; this avoids exotic extensions |
| 7 | ORM + migrations | **SQLAlchemy 2** + **Alembic** | Standard |
| 8 | Job queue | **Procrastinate** (Postgres-based task queue) | Retries, locking, workers for free; no Redis |
| 9 | Graph | Postgres tables + recursive SQL | ch. 4 |
| 10 | Media | **FFmpeg** (installed) | Standard |
| 11 | Storage | Own small adapter: local ↔ S3 (boto3) | Same code, two backends |
| 12 | AI SDKs | `openai` (O) · `boto3` bedrock-runtime (B) · VOICEVOX engine (local HTTP, factory voices) | Only inside the AI gateway |
| 13 | Speech → text | **faster-whisper** (Whisper large-v3) on GPU; **OpenAI transcription as a stand-in** until the GPU worker is set up (synthetic data only) | Run-only, open source; stand-in keeps us fast |
| 14 | Who spoke | **pyannote** on GPU (check model terms first) | Ready-made |
| 15 | Privacy filter | **Local only:** rules (さん / 先生 / 様 cues, ID / date patterns, name lists) + **GiNZA** Japanese name detection + eponym safe list | Must run before any cloud call |
| 16 | Login | Built-in accounts + roles (argon2 passwords) for the MVP; Google sign-in later | Fast; no Workspace dependency |
| 17 | Charts | Chart.js | Dashboard + growth replay |
| 18 | Tests | **pytest** + the Test Harness (B23) | Scores from day one |
| 19 | Code quality | **ruff** (lint + format) · **gitleaks** in a pre-commit hook | Keeps secrets out of git automatically |
| 20 | Local services | **Docker Compose: Postgres only** | Laptop stays light |
| 21 | Git hosting | GitHub **`karthi-ai-engineer/myrobots-plan`**; karthi-ai-engineer is the only contributor | Done 24 Sep |

⚠ Windows dev machine: all project scripts in Python (no bash-only scripts), Docker Desktop on WSL2.

## 8. AI per block ✅

Drafted by Claude (fast mode), accepted by Karthi 24 Sep 2026. Model names follow the PLAN_v2 cost sheet and are **confirmed at Gate 0a**.

| Block | Task | 🟠 Version O | 🟢 Version B | Calls per 60-min case | Notes |
|---|---|---|---|---|---|
| B03 Eye | see 10 pictures → steps / tools / action hints | GPT-5.4 mini | Claude Haiku 4.5 | ≈ 360 | Dictionary ID lists in the prompt; "unknown" allowed; smoothing by rules after |
| B05 Event Check | see ~15 s clip → bleeding yes/no + confidence | GPT-5.4 mini | Claude Haiku 4.5 | ≈ 10–40 | Only when a clue fires |
| B06 Speech → Text | transcribe Japanese | faster-whisper (GPU) · stand-in: OpenAI transcription | same (shared GPU part) | per 30 s chunk | Medical word list; word times |
| B07 Who Spoke | split voices → role | pyannote (GPU) → role: rules + GPT-5.4 mini | pyannote → Claude Haiku 4.5 | per chunk | Role step runs **after** B08 |
| B08 Privacy | find names / IDs | **no cloud AI** | **no cloud AI** | — | Local rules + GiNZA |
| B09 Merger | moments | **no AI** (rules) | **no AI** | — | |
| B10 Extractor | moment → draft card (JSON) | GPT-5.4 (GPT-5.5 for the final check) | Claude Sonnet 5 | ≈ 20–40 | Evidence IDs per field; "nothing here" allowed; few-shot examples from B13 |
| B11 Mapper | words → IDs | rules → embeddings → GPT-5.4 mini judge | rules → embeddings → Haiku 4.5 judge | ≈ 20–60 | Danger zone never auto-mapped at low confidence |
| B24 Debrief | pick cards + questions | **no AI** (rules + Japanese templates) | same | — | LLM phrasing only if templates feel stiff |
| B13 Learner | propose rules from repeated fixes | GPT-5.4 | Claude Sonnet 5 | few per week | Gated by tuning scores |
| B15 Index | embeddings | OpenAI text-embedding-3 | Cohere Embed Multilingual or Titan v2 on Bedrock (pick by a Japanese test) | per card / passage | One index per version |
| B17 / B18 | write answer / brief · check citations | GPT-5.4 (writer) + GPT-5.4 mini (checker) | Sonnet 5 + Haiku 4.5 | per question | Numbers inserted from the database |
| F1 Scripts + obstacle marks | write scripts; suggest obstacle marks | **Claude (subscription, offline)** + dataset labels / rules for marks | same | per case | Not via the gateway; see factory setup below |
| F2 Voices | Japanese text → speech | **VOICEVOX (local)** | same | per line | Several natural male + female Japanese voices |

**Prompt contracts:** every AI task has a versioned prompt file + output schema + golden tests on the dev cases. A reply that fails the schema is retried once, then the job fails visibly.
**Cost (estimate, measure first):** ≈ $3–6 per case for Version O, so ≈ $40–80 per full run of 13 cases; the gateway cache makes re-runs of unchanged inputs free. Budget guard in ch. 11.

**Factory setup (tentative, 24 Sep; details decided in the flow):**

| Job | How | Cost |
|---|---|---|
| ✍️ Scripts (🇯🇵) | **Claude via the subscription**: Claude writes dev + tuning scripts in Claude Code sessions (files); **locked-case scripts are written by a background agent straight into sealed storage** (Karthi never sees them). F1 imports, validates and seals | $0 |
| 🎯 Obstacle marks | **Dataset labels + rules** (e.g. dissect adhesion · aspirate fluid · coagulate blood vessel) → frame check by Claude (locked) or Claude + Karthi (dev, tuning). No video AI needed | $0 |
| 🔊 Voices | **VOICEVOX** on the laptop (free commercial use with credit "VOICEVOX: name"; check each voice's terms; pick natural voices) | $0 |
| 🟠 Pipeline | **OpenAI API** (program X credits if granted) | credits |

- No Claude API key (subscription only). Google factory dropped (not feasible).
- Fairness: Claude factory ≠ OpenAI extractor, so Version O numbers are fair. Version B (Claude) results are labelled ⚠ same-family factory.

## 9. Data plan ✅ (data source open)

Agreed 24 Sep 2026. **The data source is not decided and is handled as a separate process** (CEO). It could be public datasets, real clinical videos from a hospital, or something else. The plan is **data-agnostic**: the procedure + videos are a plug-in, and the same pipeline works with any source.

### 9.1 What changes with each kind of data

| Source | What we can do | Must happen first |
|---|---|---|
| Public dataset (e.g. Cholec80 / CholecT50) | Internal build + tests; factory adds voices | 📜 License rule: external demos only after clearance |
| Real clinical videos (no audio) | Same, plus realistic archive review | 🔴 "Before real data" checklist (PLAN_v2 §8): blur, consent, in-hospital or approved setup, … Likely in-region AI (Bedrock Tokyo) or a hospital box instead of OpenAI |
| Real clinical videos **with OR audio** | Real speech: some 🔜 pilot items become testable ("do teams say the why?") | Same + audio name muting, cloud speech off; answer keys need human annotation |
| Anything else | Check against the intake requirements (9.2) | License / permission check |

### 9.2 Intake requirements (any source)

- Laparoscopic cholecystectomy, full case (first incision view → specimen out), laparoscope view
- ≥ 13 cases: 3 dev + 7 tuning (5 hard + 2 routine) + 3 locked (2 hard + 1 routine)
- Common video format, ≥ 25 fps, readable resolution (we sample 1 picture/s anyway)
- No patient identifiers visible, or blurred first (real data)
- Nice to have: phase / tool labels (speed up obstacle marks), case date, pseudonymised surgeon ID
- License or permission in the company's name

### 9.3 Picking the cases

- Hardness signals: long Calot's triangle dissection · many irrigate / aspirate actions · long cleaning and coagulation · long total time → rank → review the top ~15 → pick
- Without labels: rank by duration + a quick frame review
- Assign to dev / tuning / locked at random within hard / routine groups; the locked cases' answer keys are made by a background agent (Karthi never sees them)

### 9.4 Obstacles

- 3–4 general types: **bleeding · dense adhesions · unclear anatomy at Calot's triangle · gallbladder tear / bile spill**; each in 2–3 cases; ≥ 2 paired cases; must be visible in the video
- Marks: dataset labels + rules when labels exist → frame check; no labels → frame check only (slower). Each mark tagged rule-found / Claude-checked / human-found

### 9.5 Factory (per case, from ch. 8)

- **Master script:** time · speaker role · 🇯🇵 line · tags (obstacle / why / trap / chatter / fake name)
- **2–3 fictional surgeon personas with different styles**, so "side by side, never averaged" can be shown
- **Trap rules:** speech 1–5 s before/after actions · many actions unspoken · vague lines · off-topic chatter · a planted fake name · sometimes a word not in the dictionary
- Voices: VOICEVOX, one voice per role · OR noise from CC0 sources, 3 levels · mixer → video + OR audio + sealed keys
- If real OR audio arrives, those cases skip the factory; their answer keys are annotated by hand

### 9.6 Answer keys + sealing

- **Contents:** transcript truth · speaker roles · line times · planted obstacles (type, specific term, start / end, step, anatomy, warning signs, responses + resolving action, outcome, severity, why, pairs) · fake names · question-set answers
- **Sealed:** `sealed/` in storage; only the Test Harness + tester role; every access audit-logged; Karthi never opens locked-case keys

### 9.7 Research pack (B25 General Library)

- **Sources:** Tokyo Guidelines 2018 (safe steps) · SAGES Safe Cholecystectomy · critical view of safety literature (Strasberg) · an intraoperative adverse-event classification for the severity scale (e.g. ClassIntra) · anatomy references · Japanese society guidance where available
- **Format:** U10 passages = short quotes or summaries + full citation (respect copyright; link to the original)
- Built and versioned by Claude; used by the factory scripts, the stand-in surgeon and the 🧠 answer layer

## 10. Test & quality ✅

Drafted by Claude (fast mode), accepted by Karthi 24 Sep 2026. Targets are in ch. 1.3; this chapter says how they are measured.

### 10.1 Test layers

| Layer | What | When |
|---|---|---|
| Unit | Each block with fake inputs (pytest) | Every change |
| Contract | Every output validates against its U-unit schema; O and B must return the same shapes | Every change |
| Golden | Each AI prompt on the 3 dev cases vs expected outputs | Prompt change |
| End-to-end fake | One fake case through all 28 blocks (Gate 1) | Every change |
| Scored run | 7 tuning cases at 10×, scored by the Test Harness | After prompt / rule changes (cost-capped) |
| Timing run | At **1×** (real speed) to measure "debrief ready ≤ 5 min" and "draft ≤ 3 min after event" | Per gate, dev case + locked exam |
| Final exam | 3 locked cases, **run once** at Gate 6. A broken run may be repeated only with a written reason | Gate 6 |
| O vs B | Same cases through both versions, side by side | Each gate once B exists |

### 10.2 How the key numbers are computed

| Metric | Method |
|---|---|
| Obstacle found | A draft episode matches a planted obstacle if the general obstacle ID is the same **and** times overlap (or lie within ±30 s) |
| Invented | A draft episode in a routine case with no planted match |
| Evidence timing | The cited line / video time lies within ±10 s of the answer key's time for that field |
| **Simulated reviewer** | Compares each draft card field-by-field with the answer key → approve unedited / edit (which fields) / reject (reason). Produces the edit rate without a human |
| Debrief time | Timed in the app: debrief opened → last card done (Karthi, blind, locked cases) |
| Pattern counts | Pattern card numbers vs counts computed from the answer keys |
| Leaked names | Planted fake names found in any text after B08, or in any gateway request log |
| Hidden eponyms | Safe-list words wrongly replaced |
| Uncited claims | 📚 sentences without a valid approved-episode citation in final answers |
| Before-🏁 exposure | Any clinical-role request that returned a draft before the case's 🏁 time (from access logs) |
| Growth replay | Add cases 1 → 10 in a fixed order; after each, run the question set; record coverage + edit rate |

### 10.3 Question set

≈ 30 pre-case questions (Japanese, some English): what happened · how it was handled · how often · warning signs · surgeon comparison · questions with **no** answer in our cases (to test honest "no evidence"). Drafted by AI from the answer keys, edited by Claude, locked before tuning ends.

### 10.4 Reports

- Counts first, with ⚠ small sample; a one-page report per run (stamped with all versions)
- Hard-rule failures shown in red at the top; a gate cannot close with any hard-rule failure
- Every run kept, so trends (edit rate, cost per case) are visible over time

## 11. Roadmap & budget ⏸ on hold

**On hold until Karthi's discussion (24 Sep).** The draft below is kept only as material for that discussion. Nothing here is decided.

**Draft build order (option A, "no-video work first"):**

```
 Gate 0a  Dev start            env · repo · OpenAI key · gateway smoke test
 Gate 1   Walking skeleton     28 blocks, FAKE outputs · live + archive flows · save + stamp
                               · Test Harness + Audit Log · minimal screens
 Gate 2   Knowledge side ⭐     dictionary v1 · research pack · graph · patterns · search ·
          (no video needed)    answers · briefs · debrief + review screens → on fake cards
             ⏳ videos arrive (separate process) → first real case, end to end, rough
 Gate 3   Factory              13 cases: scripts · voices · noise · sealed keys
 Gate 4   Senses               real media prep, eye, speech, who spoke, privacy · works live
 Gate 5   Brain                real moments, cards, mapping, debrief builder, learner
 Gate 6   Final exam + demo    3 locked cases · all rules + pass marks · CEO demo
 Parallel: Gate 0b AWS → Version B (off the critical path)
```

Alternatives: B old order (senses → brain → knowledge; blocked until videos arrive) · C real case first (needs a video now).

**Draft budget guard:** OpenAI limit ≈ $350/month until credits arrive · gateway alerts at 50 / 80 / 100% · cost cap per run · cache for unchanged re-runs · expected ≈ $5–30/month in Gates 0–2, ≈ $100–250/month in Gates 3–6 · GPU on AWS credits when needed.

## 12. Risks & dependencies ✅

Drafted by Claude, accepted by Karthi "for now" (24 Sep 2026). Gate numbers follow the ch. 11 draft and may change after that discussion.

### 12.1 Risk register

| # | Risk | Level | Mitigation | Owner |
|---|---|---|---|---|
| R1 | **Data source unknown or late** | 🔴 | Data-agnostic plan (§9); no-video work first (§11 draft); intake requirements ready | CEO |
| R2 | **Public-dataset license is non-commercial** (if Cholec80 / CholecT50) | 🔴 | Build internally; external demos only after clearance; CAMMA permission or legal opinion | CEO |
| R3 | **All voice is synthetic**: can't prove real OR speech carries the "why" | 🔴 | 3 proof levels said openly (✅ / 🎭 / 🔜); pilot proves the rest | Karthi |
| R4 | **No clinician**: medical content could be wrong | 🔴 | Research pack with citations; 🧪 / 🎭 labels; stand-in reviews | Claude |
| R5 | **Real clinical data arrives** → privacy law, hospital rules, OpenAI may not be allowed | 🟡 | "Before real data" checklist; in-region Bedrock / hospital box; AWS GPU only | CEO + Karthi |
| R6 | Same-family bias (Claude factory vs Version B) | 🟡 | Version O numbers are the fair ones; B labelled | Claude |
| R7 | Cloud eye weak on surgical video (steps / tools) | 🟡 | Hints only; smoothing rules; measure first | Claude |
| R8 | Japanese speech errors (medical words, noise) | 🟡 | Medical word list; measure per noise level; privacy filter tested on errors too | Claude |
| R9 | **Live timing** (debrief ≤ 5 min after 🏁) missed due to API latency / rate limits | 🟡 | Parallel jobs; small models for hints; 1× timing runs from Gate 4 | Claude |
| R10 | Small numbers (13 cases) → noisy metrics | 🟡 | Counts first, ⚠ small sample; decisions on field-level scores | Both |
| R11 | Too much work for 2 people (28 blocks, 3 audiences, 2 use cases) | 🟡 | Fake-first skeleton; parked list; investor polish last; cut scope in writing if stuck | Karthi |
| R12 | OpenAI spend before credits | 🟡 | Gateway budget guard; cache; cost cap per run | Karthi |
| R13 | AWS late (credits, Bedrock, GPU quota) | 🟡 | Version B off the critical path; OpenAI speech stand-in; ask for GPU quota early | CEO |
| R14 | Regulatory (MHLW SaMD) for answers / suggestions | 🟡 | 💡 suggestion switch off for clinical use; regulatory check before hospital use | CEO |
| R15 | Blind review broken by accident (Karthi sees locked content) | 🟡 | Background agent writes locked material; `sealed/` access audit-logged | Both |
| R16 | Claude subscription usage limits (factory scripts + research pack are heavy) | 🟡 | Spread work over sessions; scripts in batches | Karthi |
| R17 | VOICEVOX voices sound unnatural / per-voice terms | 🟡 | Pick natural voices; check each voice's terms; credit shown | Claude |
| R18 | Model / library terms (pyannote, GiNZA models, speech models) | 🟡 | Check terms before first use; record in decisions | Claude |
| R19 | Model names / prices change | 🟢 | All calls through the gateway; confirm models at Gate 0a; stamps on every output | Claude |
| R20 | Windows dev machine, no NVIDIA GPU | 🟢 | Python-only scripts; Docker on WSL2; GPU remote (AWS) | Claude |

### 12.2 Dependencies (asks)

| Ask | From | Needed by |
|---|---|---|
| OpenAI API key (company) + monthly spending limit | Karthi / CEO | Gate 0a |
| ~~GitHub repository~~ ✅ `karthi-ai-engineer/myrobots-plan` (24 Sep) | Karthi | done |
| Claude subscription | Karthi | now |
| OpenAI credits via program X (check commercial + data terms) | CEO | before tuning runs (Gate 3–5) |
| Surgery videos meeting the intake requirements (§9.2) | CEO (separate process) | Gate 3 (first real case as soon as possible) |
| License / permission for the chosen data (if public datasets) | CEO | before any external demo |
| Legal check: internal R&D use of licensed data | CEO | before Gate 3 |
| AWS account + credits + Bedrock access + **GPU quota** | CEO | Version B; GPU by Gate 4 (optional) |
| Hospital data agreement (if real clinical data) | CEO | when that path starts |
| Regulatory specialist (SaMD) | CEO | before any hospital use (after MVP) |

## 13. How we work ✅

Drafted by Claude, accepted by Karthi "for now" (24 Sep 2026).

| Area | Rule |
|---|---|
| Roles | Karthi decides (product, scope, people, money) and builds. Claude advises and develops; decides delegated technical items, marked "decided by Claude (delegated)" |
| Communication | Short, visual, one topic at a time; options with a pick for real choices; open items at the end |
| Session start | Read `CLAUDE.md` → `docs/MASTER_PLAN.md` status → `docs/NEXT_STEPS.md`; agree the session goal |
| Session end | Update `NEXT_STEPS.md` (done / next); log any decision; short summary with open items |
| Decisions | One file per decision in `docs/decisions/` (topic · options · choice · reason · decided by). Changes to agreed items only in writing |
| Planning source of truth | `docs/MASTER_PLAN.md` in the repo. Notion gets a short summary when a gate closes |
| Git | Repo `karthi-ai-engineer/myrobots-plan` · **only karthi-ai-engineer** pushes / pulls / commits (identity + remote rules in `CLAUDE.md` §1) · **no assistant attribution ever** (enforced by `.claude/settings.json` + `.githooks/`) · `main` always works · one short branch per task · small commits · commit / push only when Karthi asks |
| Checks before commit | ruff (lint + format) · gitleaks (no secrets) · tests pass |
| Definition of done (block / task) | Contract + fake mode + one-screen README · tests pass · outputs saved + stamped · audit entries written · decision logged if any · `NEXT_STEPS.md` updated |
| Gates | Close by checklist, never by date · stuck → fix or cut scope in writing · each closed gate = short CEO update |
| Secrets | Keys only in `.env` or a secret manager; never pasted in chat |
| Blind review | Locked-case scripts / keys made by a background agent; Karthi never opens `sealed/` |
| Cost | Check gateway spend weekly |
