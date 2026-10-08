# MY ROBOTS · MVP Implementation Plan: v1

> **Status:** v1.1, 6 Oct 2026, after edge-case review **and Karthi's answers** (`decisions/2026-10-06-v3-approval-hybrid-and-api-models.md`):
> - hybrid approval;
> - bilingual quotes;
> - overdue cards stay visible but are excluded from patterns;
> - **MVP AI runs through API keys on one laptop**.
>
> **Approved by Karthi on 6 Oct 2026. Phase P0a is in progress.**
>
> **v1.2 note (6 Oct, `decisions/2026-10-06-v3-openai-and-model-agnostic.md`):**
> - MVP speech-to-text = **OpenAI transcription API** (local engines later, for real data);
> - MVP drafting = **OpenAI GPT models**;
> - the **verifier is not yet chosen** (it depends on accelerator credits);
> - **all code is model-agnostic**: tasks + config + fake providers; the OpenAI adapter comes first.
>
> Where the text below names Claude/Bedrock or local speech engines for the MVP, read it as "configured provider".
> **What changed from v0:**
> - three reviewers found **262 edge cases** and **75 plan defects**: `C:\Users\karth\MyRobots\analysis\edge-cases\EC1/EC2/EC3*.md`;
> - their 30 ranked edits are folded in;
> - the full catalogues become the **test backlog**: every edge case tagged for a phase must have its acceptance test before that phase's gate can pass.
> **Source of truth for *what*:** `MASTER_PLAN_v3_DRAFT.md` + `decisions/`. This file is the *how* and the *order*.
> **Rule:** gates, never dates.

---

## 0. The ten most important changes from v0 (plain words)

| # | v0 problem | v1 fix |
|---|---|---|
| 1 | The quality gate **could not fail**: an AI answering "unknown" everywhere would pass | Hard rules **plus** pass marks (recall, over-abstention, verifier accuracy); a "null drafter" must fail |
| 2 | **One clock**, but narration has two (the paused video and the answer audio) | Two time domains, never mixed; evidence has a 🗣 *said* anchor (audio) and a 🎥 *seen* anchor (video) |
| 3 | Raw transcript stored **next to** the cleaned text | A sealed raw zone; the app only ever reads redacted text; a leak scanner checks everywhere |
| 4 | "Approved = frozen" **blocked** release and withdraw | Card *content* is frozen; *governance* (privacy, release, withdraw) lives in its own audited history |
| 5 | Review friction was **only in the UI** | Server-side: approval of a pitfall needs a logged evidence view by that user on that version |
| 6 | Practice data was **circular** (an LLM writes the scripts, an LLM extracts them) and only left-sided | Sealed splits; a different model family writes the scripts; human readers who partly improvise; ≥ 2 surgeons, a right-side case |
| 7 | P0 **could not pass** on this laptop (line endings, code page 1252, no `make`) | P0 is split into a **platform spike (P0a)** and a repo setup (P0b); LF, UTF-8 and a cross-platform `tools/check` |
| 8 | The demo **was not designed** | A minute-by-minute storyboard in P1; two demo checkpoints (D1 with fake AI, D2 with real AI and a pulled cable) |
| 9 | The gateway could not prove reproducibility | Every call records model hash, seed and versions; a config must **pass the golden set** before use |
| 10 | Dictionary terms carried side ("left vein") | Side-neutral terms with side on each mention; laterality rules know frame of reference (右側臥位 is correct for a *left* case) |

---

## 1. What "done" means for the MVP

The MVP is done when **all three** hold:

### 1.1 L1 quality on the sealed synthetic set (scored on gold transcripts; ASR transcripts reported alongside)

- **Hard rules** (checked by code, zero tolerance):
  - displayed evidence IDs valid 100%;
  - displayed quotes are exact source slices 100%;
  - numbers not in the cited utterances: 0;
  - planted-PII leaks: 0. The leak scan covers the DB, logs, the job table, file names and browser storage, with ≥ 300 plants per class.
  - negation or counterfactual errors in surviving fields: 0 (≥ 60 traps);
  - **fabricated cards** (a ★ with no stated decision): 0;
  - laterality contradictions surviving at band `high`: 0;
  - silent critical ASR errors: 0.
- **Pass marks:**
  - over-abstention (gold present → `unknown`) ≤ 0.20;
  - fabricated "why" ≤ 2%;
  - card recall ≥ 0.85;
  - band-high precision ≥ 0.95;
  - verifier sensitivity ≥ 0.90 and specificity ≥ 0.85 (≥ 200 planted field–span pairs);
  - debrief CER ≤ 10%.
- **A null drafter (all `unknown`) must fail L1.** This is a test in CI.

### 1.2 The demo (storyboard from P1)

- Runs on **one laptop**, with AI through API keys (profile `dev-cloud`, shown on screen).
- Runs 3× in a row with a ≤ 60 s reset between runs.
- One run with the **internet cut mid-demo** must complete via the prepared lane (⏱ labelled), with honest banners.
- Phone tethering is the backup link.

### 1.3 The runbook

Someone else (target: the CEO) can start the demo from a cold boot using only the runbook.

**Scope:** as v0 §1 (S0 Build). **Out of scope:** Prepare (S2) · ASK (V1) · prospective OR capture (V1) · video AI · hospital SSO · cloud hosting · real patient data.

---

## 2. System on the two laptops

```
 ONE LAPTOP (Surface Laptop 7, 16 GB, ARM64) — the whole MVP
┌──────────────────────────────────────────────────────────────┐
│ Browser (Edge): React app                                     │
│ API (FastAPI) + job worker                                    │
│ Postgres 18 (UTF-8) + pgvector                                │
│ Speech engines (local): sherpa-onnx ReazonSpeech + whisper.cpp │
│ Embeddings + reranker (local): ruri-v3 via ONNX Runtime        │
│ Media store (content-addressed, ASCII paths)                  │
│ AI GATEWAY (in-process) + circuit breaker ──────────────────────┼──▶ ☁️ dev-cloud API keys (synthetic only):
└──────────────────────────────────────────────────────────────┘      Claude via AWS Bedrock (jp. route) · OpenAI
 ASUS Zenbook 48 GB: spare demo machine (same install recipe); `local` profile later (off the MVP path)
```

- **Degradation matrix:** the gateway circuit breaker uses a 2 s connect timeout and fails fast.

  | If… | Then… |
  |---|---|
  | The API is unreachable or slow | Drafting falls back to the **prepared lane** (pre-drafted cards for the demo case, labelled ⏱) |
  | The cloud embedding model is unavailable | Search stays on graph + lexical + local dense |

- **Config fails closed:** there is no default `AI_PROFILE`; real data is refused by every non-Japan provider; API keys live only in `.env`.

---

## 3. Codebase layout (new private monorepo `myrobots`)

As in v0 §3, plus:

- `tools/check`: a cross-platform Python entry point (replaces `make`). Modes: `--platform`, `--fast`, `--full`, `--leak-scan`.
- `tools/demo`: `preflight`, `reset`, `storyboard` and `ai-host` commands.
- `models.lock`: every model file with sha256, licence and source. Downloads are verified against it.
- `fixtures/`: manifests and generators only. Media lives in a hash-addressed local store outside git. `fixtures/media.lock` lists the only importable files.
- `eval/`: the scorer. **Sealed scripts and keys live outside the repo, encrypted**; only their sha256 is committed.
- `.gitattributes`: `* text=auto eol=lf`. `PYTHONUTF8=1` is set in the env template.
- `.claude/settings.json`: **denies read access to the sealed-set path**. The coding assistant never sees the answer keys.
- `docs/edge-cases/`: the EC1–EC3 catalogues, copied in as the test backlog.

---

## 4. Contracts (designed before the code that uses them)

### 4.1 Data model (Postgres 18, UTF-8)

| Group | Tables (v1) |
|---|---|
| **People** | `institution` · `app_user` (one per real person) · `membership` (multi-role) · `session` (per tab: `active_role`, `auth_method`). Roles: surgeon · clinical_owner · junior · admin · tester |
| **Case and media** | `procedure` · `case` (procedure, **side**, approach, platform, technique_version, surgeon_pid, **data_class**, safety_signal (no downgrade), safety_review_state, status) · `media_asset` (kind ∈ original / proxy / storyboard / clip / answer_audio / asr_16k; parent; recipe_version; **content-addressed storage_key**; sha256; duration_ms; codecs; fps_mode; time_base; start_pts_ms; audio_tracks; mask_set_id; oob_version; metadata_stripped; ffmpeg_version; retention_class) · `case_video_segment` (multi-file cases) · `video_flag` (OOB / overlay, human-confirmed) · **sealed** `media_asset_sealed` (original filename, probe JSON) |
| **Narration** | `step_template` (per surgeon × procedure, versioned; **every step maps to a dictionary step term**) · `narration_session` (pins a dictionary release) · `marker` (raw_tap_ms, playback_rate, presented_frame_ms, proposed_stop_ms, **confirmed_stop_ms**, clip_start_ms, step_id, reason_code; ★ within 10 s merged) · `question` (template_id, slots from ratified step names only) · `answer` (marker, question, take_no, superseded_by, status ∈ recording / partial / complete / truncated / withdrawn, no_speech, speech_ms, mime, device info, **playhead_trace[]** (audio_ms ↔ video_ms, rate, paused)) |
| **Speech** | **Sealed** `utterance_raw` (per engine raw text, tokens with times, VAD segments, engine provenance) · `utterance` (redacted canonical text, marker_id, audio_ms_start/end, canonical_source, disagreement spans + severity, redaction_stamp {filter_version, name_list_version, text_hash}, transcript_version) · **sealed** `case_name_list` (name, furigana NOT NULL, role, version) |
| **Runs** | `pipeline_run` · `stage_run` · `ai_call` (provider, residency, model_id, **weights_sha256**, runtime build, config_id, prompt/schema/task/dictionary/transcript versions, sampling params, **seed**, tokens incl. thinking, cache hit, finish_reason, repair count, validation status, cost, latency, data_class, payload ref) · `model_registry` (family **lineage**) · `ai_config_activation` (config_id, golden_run_id, passed, approved_by) |
| **Cards: content** | `card` · `card_version` (origin_marker_id, template_step_id, step_term_id, draft/approval dictionary versions, ai_profile, video_anchor + confirmed_by/at, content_hash, supersedes, approved_by/at; **content immutable once approved**) · `card_field` (registry-typed; status; value; polarity; occurrence; certainty; voice; evidence_kind ∈ stated_narration / stated_debrief / stated_or / **surgeon_edit** / **code_derived** / inferred; verifier verdict; band) · `evidence_anchor` (🗣 said {audio_asset, audio_ms_start/end, contiguous utt_ids, quote exact/prefix/suffix, char offsets} · 🎥 seen {video_ms_start/end, source ∈ confirmed_stop / playhead_trace}) · `card_field_term` (mention surface, offsets, **side**, term_id, method, similarity, margin, needs_review, dictionary_version) · `field_registry` (per type: claim class, allowed evidence kinds) · `evidence_view` (user, card_version, field, at) |
| **Cards: governance** | `card_governance` (current state, privacy_state, release_purposes, review_due, withdrawn_at) · `card_governance_event` (**append-only** history of every transition) |
| **Dictionary** | `term` (**side-neutral**; status official / pending / deprecated / deprecated_split; ltree path; danger_zone; safe_name; `replaced_by` acyclic; `split_into[]`) · `term_label` (normalised + kana-folded key; may carry a side: 左腎静脈 → (腎静脈, left)) · `term_rel` (part_of; confusable_with and sounds_like symmetric) · `term_candidate` (never written by an LLM) · `term_xref` · `laterality_rule` (procedure, case_side, term pattern, mention_side, **frame** ∈ patient / position / hand / screen, verdict, approved_by) · `dictionary_release` (semver, immutable snapshot sha256) |
| **Knowledge** | `edge` (rule-derived from approved content, with provenance; full rebuild = incremental) · `card_lexical` (tokens, bigrams, tokenizer_id) · `card_embedding` (**one per model**: model_id, weights_sha256, dims, vector; atomic active-index switch) |
| **Audit and settings** | `audit_event` (append-only, hash-chained; user, active_role, session; **IDs only**) · `institution_setting` (raw-audio retention: **intake and recording refused until it is set**) |

**Invariants (enforced in the database):**
1. Approved content is immutable. Triggers cover `card_version` content and its `card_field`, `evidence_anchor` and `card_field_term` rows.
2. Governance changes only through `card_governance_event`; the current state is derived.
3. At most one current approved version and one in-review version per card.
4. **Hybrid approval** (decision 6 Oct). Allowed `approval_path`s:
   - `surgeon`: the case's surgeon;
   - `delegated`: a named owner with an active `approval_delegation` for that case;
   - `reviewer`: an owner, for archive cases whose surgeon has left;
   - `department`: an owner, for procedure-level cards.

   The label (✅ / 👁 / 🏥) is derived from the path and is never stored as free text.
5. Clinical-claim fields accept only `stated_*` or `surgeon_edit` evidence; `code_derived` is allowed only for structure fields.
6. A filled field must have ≥ 1 anchor (deferred constraint).
7. The `body` is a derived render; `content_hash` = sha256 of the canonical field JSON.
8. Non-sealed schemas never hold raw text. Job arguments carry IDs only.
9. The data_class of child rows equals the case's data_class.
10. `safety_signal` cannot be downgraded.
11. The audit log is insert-only.

### 4.2 Gateway task contracts (the same JSON for every provider)

| Task | Input → Output (essentials) |
|---|---|
| `transcribe` | 16 kHz mono PCM, shared VAD segments, dictionary hints, fixed decode params (temperature fallback **off**) → per engine: model sha, segments with token times, no_speech, dropped ranges |
| `detect_pii` | **rules-redacted** text, data_class → exact spans only `[{substring, occurrence, category}]`. It never returns rewritten text and never receives the name list |
| `classify_answer` | question, answer → elements (cue / expectancy / option / basis / threshold), `dont_know ∈ none / partial / full`, `refers_to`, `self_repairs[]`, language. **Duration never decides** 「わからない」 |
| `draft_card` | case variant · marker · **question text (cannot be cited)** · answer utterances as aliases u1…un · closer utterances · step candidates · dictionary slice → `{cards: [0..3], no_card_reason?}`. Each field: status, value, polarity, occurrence, certainty, mentions, evidence `[{contiguous utt_ids, quote, voice}]`, evidence_kind. **Scope, side and term IDs are set by code / B11, never by the drafter** |
| `verify_card` | each filled field + only its cited span ± 2 utterances → per field: verdict (supported / partial / unsupported / contradicts), polarity_ok, voice_ok, stated_vs_inferred, added_content |
| `map_term` | surface + candidates → term_id \| none, side, method, similarity, margin, needs_review. **Confidence is computed, never stated by the model** |
| `embed` / `rerank` | as v0, with model_id + weights_sha256 |

**Gateway rules:**
- Only `transcribe` and `detect_pii` may receive un-stamped content, and only on providers allowed for that data class.
- The drafter and verifier must come from **different lineages**; `model_registry` enforces this.
- An app task runs only on an **activated** config (one that passed the golden set).
- Timeouts are set per task × profile.
- Residency is checked by host for `local`.
- Dev-cloud Claude uses the same tool-JSON mechanism as the pilot.

### 4.3 API contracts

- **Auth:**
  - local login, one account per human;
  - the active role is chosen per tab, sent on every request and audited;
  - **DEMO_MODE refuses to start** if any non-synthetic row exists or the profile is `pilot-japan`;
  - approval labels always show the real account (🧪 demo operator).
- **Answers (chunked and resumable):**
  - `POST /answers` → id (**409 while retention is unset**);
  - `PUT /answers/{id}/chunks/{seq}`;
  - `POST /answers/{id}/complete`.
- **Every mutation** carries the expected `card_version_id` + an `Idempotency-Key`. A stale request gets **409 with a diff**.
- **Friction is server-side:**
  - approving a pitfall, recovery or danger-zone card requires `evidence_view` rows for that version by that user;
  - batch approval returns a result per card and never batch-approves those classes.
- **While a narration session is open,** the narrator receives no draft bodies (elicit before show).
- **Other endpoints:** as in v0 §4.3.

### 4.4 Job model

- Each block is a pure stage function with an idempotency key (a hash of its inputs and versions). The functions are portable.
- **In-session fast path** (never waits for drafting): engine A → rules + name-list redaction → rules classifier → next template probe. Target: **p95 ≤ 5 s** from stop to the next question.
- **Drafting runs once per ★**, debounced (★ closed or 20 s idle), over all current takes.
- **Session-end reconciliation** re-drafts any ★ the closer refers to.
- **Re-runs replace only unopened drafts.** They never touch opened, in-review or approved versions.
- **Priority:** in-session ASR > classify > drafting.

### 4.5 Governance and authorisation contract (designed before P2 code)

- **Card states:** draft → in_review → approved → (withdrawn | superseded); in_review → rejected; draft → **purged**.
  - "Don't keep" (残さない) = file deletion + tombstone.
  - Expiry jobs remove unapproved drafts.
- **Three separate gates:** privacy ≠ approval ≠ release. Release is blocked while privacy is pending, review is overdue, or a serious event is unapproved.
- **Serious events:** the case-level `safety_signal` excludes the case by default.
- **Retention:** chosen explicitly at setup. A purge job follows the chosen rule.
- **Review-due:** after the due date the card stays visible with ⚠ "review overdue" and is **excluded from patterns and summaries** until re-reviewed.
- **Role × action matrix:**
  - a surgeon approves only their own case's cards, and can grant or revoke a per-case delegation;
  - the owner releases cards; approves with a delegation (👁 delegated), for departed-surgeon archive cases (👁 reviewer) or procedure-level cards (🏥);
  - a junior reads released cards only;
  - an admin sees no clinical content;
  - a tester sees sealed data only, in a separate DB.

  Authorisation checks the **active role only**.

---

## 5. Phases and gates (new order)

```
 P0a Platform spike ─▶ P0b Repo · CI · skeleton ─▶ P1 Gateway + G1 benchmark + DEMO STORYBOARD
        ─▶ P2 Knowledge core + governance/authz (§4.5)
        ─▶ ┌ P3 Narration pipeline (CLI) ┐
           └ P4a Narrate + Review UI on mocks ┘ (in parallel)
        ─▶ D1 Walking-skeleton demo (fake AI, network off, both laptops)
        ─▶ ┌ P4b Manual · Case · Admin · dictionary view ┐
           └ P5 Knowledge graph + search                ┘ (in parallel)
        ─▶ D2 Real-model demo + failure injection ─▶ P7 Demo hardening ─▶ final L1 on the sealed set ─▶ MVP done
 P6 Evaluation runs alongside P1–P5
```

| Phase | Deliverables | Gate (must pass) |
|---|---|---|
| **P0a Platform spike** | Prove every component on the Surface (and the same recipe on the ASUS as the spare): **one bitness** (all-ARM64 or all-x64); the Postgres route (native ARM64 build or x64 under emulation) with `initdb -E UTF8` + pgvector; psycopg; FFmpeg pinned; sherpa-onnx + Silero VAD; whisper.cpp (fallback off); Janome/Lindera; ONNX Runtime; OCR choice; API reachability (Bedrock `jp.`, OpenAI) with keys from `.env`; keep-awake; the Smart App Control / Defender policy | `tools/check --platform` green on the Surface · ADRs written: Postgres route · bitness · SAC/Defender |
| **P0b Repo · CI · skeleton** | Private monorepo · `.gitattributes` LF · `PYTHONUTF8` · fail-closed config · data-class guard · media allow-list · audit (hash chain) · health · SPA shell with the CEO's design tokens + offline JA fonts · CI: Linux full + Windows smoke | `tools/check --fast` green on both laptops + CI · data-class test (real data refused by non-Japan providers) · audit insert-only + concurrency test · offline run with 0 failed requests |
| **P1 Gateway + G1 + storyboard** | Gateway with fake (incl. adversarial outputs) and dev-cloud adapters (Bedrock Claude `jp.`, OpenAI); `local` adapter stubbed for later · **validators, `jp_normalize`, number normaliser** · **frozen dictionary seed file** · golden splits (dev / tune / sealed) + generators (third model family) · `models.lock` (speech + embedding models) · **demo storyboard v0** · latency and cost budgets · prepared-lane fixtures | **Pass/fail:** the chosen drafter + verifier pair (different families) meets the L1 hard rules on the dev set, **p95 ≤ 90 s per card**, with cost per case recorded. The G1 report and the model-choice decision record are written. |
| **P2 Knowledge core + governance** | Schema + migrations + all §4.1 invariants · §4.5 state machine and authz matrix · server-side friction · dictionary v0 (adrenalectomy, side-neutral, laterality rules with frame) · card / dictionary / governance APIs · OpenAPI frozen for P4a | Invariant tests · **table-driven authz tests** · friction tests (no evidence view → 409) · purge/withdraw cascade tests · DEMO_MODE guard test · dictionary snapshot rebuild is byte-identical · 右側臥位 in a left case → no contradiction |
| **P3 Narration pipeline** | B01 (refuses duplicate sha) · B02 (no-audio proxy and clips, metadata stripped, masks + OOB blanking burned in) · N1 · N2 (stop accuracy) · N3 fast path · B06 (two engines + **disagreement policy** with critical classes) · B07 · B08 (rules + reading-based names + `detect_pii` on both engines' raw output) · B09 · B10 · B26 (`verify_card` + policy table) · B11 · bands · per-★ jobs | On the dev set: L1 hard rules + pass marks · **null drafter fails** · 0 silent critical ASR errors · OCR sweep finds 0 planted overlay strings · 5 reruns byte-identical per engine · reruns idempotent |
| **P4a Narrate + Review** (∥ P3, on mocks) | Narrate studio (MediaRecorder 1 s slices, IndexedDB resume, mic check, 180 s cap, text fallback; 1× stop confirmation; frame-accurate stop) · Review (said / seen / AI / unknown marks, evidence playback, friction, fix / don't keep) | Playwright flagship on mocks · fake-mic tests (mic denied → text fallback; tab killed at 25 s → ≥ 24 s recovered) · stop accuracy 20/20 · IME tests · axe 0 serious · JA/EN at 360 and 1280 px |
| **D1 Walking skeleton** | The whole storyboard on the **fake** provider | Network off, both laptops, `reset` + `preflight` working end to end |
| **P4b** (∥ P5) | Case · Living manual · Admin/Setup (retention blocks intake) · dictionary view · Edge on the Surface | Flagship Playwright on the real backend in Edge |
| **P5 Knowledge + search** | Edges · situation key (procedure + side + approach + step_term + anatomy / warning-sign overlap) · counts computed on read (distinct cases, current approved, non-withdrawn) · graph-first hybrid search (bigram channel in app code; governance filtered at query time; confusable **and** sounds-like never auto-expanded and labelled in every channel; three empty states) · degradation (dense off) | Recall@5 ≥ 0.9 + nDCG@10 on ≥ 60 test queries over ≥ 150 cards (≥ 30% EN / mixed) · 0 unlabelled confusables in the top 5 · withdrawn card never returned · 2 left + 1 right → no pattern · ASUS blocked → search < 2 s |
| **D2 Real-model demo** | Storyboard on `dev-cloud` | ≤ 6:30 on 3 runs · one run with the **internet cut** at minute 2 completes via the prepared lane, with honest banners |
| **P6 Evaluation** (P1–P5) | Scorer (aggregates only) · leak scanner · noise floor · provider-swap regression · ≤ 3 sealed runs before the L1 claim | L1 report on demand; activation records for every config in use |
| **P7 Demo hardening** (size M) | Runbook · rehearsals · cold-start tuning · honest-label taxonomy everywhere · tethering backup · the same install on the spare ASUS | Cold boot → `preflight` all green ≤ 10 min · 3 runs with ≤ 60 s `reset` (refuses non-synthetic data) · internet cut → completes via the prepared lane · the spare ASUS runs the demo too · **the CEO follows the runbook unaided** |

**Every gate also requires:** all edge cases tagged to that phase in `docs/edge-cases/` have a passing acceptance test or a written waiver.

---

## 6. Testing strategy

As v0 §6, plus:
- invariants and authz tested table-driven;
- **null-drafter** test;
- **leak scanner** across all storage surfaces;
- provider swaps judged against a measured **noise floor**;
- Playwright in **Edge on the Surface**;
- axe-core accessibility;
- IME composition;
- JA/EN layouts;
- offline zero-failed-requests;
- fake-mic capture tests;
- the frame-counter fixture for stop accuracy.

---

## 7. Synthetic data (fixtures)

- **Splits:**
  - dev (≥ 2 cases, visible);
  - tune (≥ 2);
  - **sealed** (≥ 4 cases + trap micro-scripts, ≥ 60 items per trap class).
- **Coverage:**
  - ≥ 2 synthetic surgeons;
  - ≥ 1 right-side case;
  - situations overlapping across ≥ 3 cases (so patterns can be tested).
- **Anti-circularity:**
  - the script generator family ≠ the drafter family ≠ the judge family;
  - a spoken-ness checklist: fillers, zero subjects, self-repair, ≥ 30% of answers with no reason given;
  - ≥ 50% of answers are improvised from bullet outlines, not read verbatim.
- **Audio:**
  - consented readers with written voice-use consent;
  - recorded through the real browser recorder on the Surface, plus a phone mic;
  - **CER reference = a human verbatim transcript, never the script**;
  - TTS is used only for regression and for ≥ 300 PII plants per class.
- **Traps:**
  - near-minimal pairs 副腎静脈/腎静脈, 動脈/静脈, 左/右;
  - negation after a pause; counterfactuals; self-repair;
  - 「に、さんミリ」;
  - IDs read digit by digit; dates in 8 formats;
  - question echo; cross-★ reference;
  - staff, other-patient and title-only names;
  - English device terms;
  - eponym and device negative controls (≥ 50);
  - overgeneralisation.
- **Media:**
  - a generated "torture set" (VFR, multi-track, rotation, overlays, OOB) + one licence-clean dry-lab video;
  - **archive OR audio is never transcribed**;
  - intake accepts only `media.lock` hashes.
- **Sealing:**
  - sealed scripts and keys are encrypted outside the repo;
  - the scorer outputs aggregates only;
  - the coding assistant is denied read access;
  - every sealed run is logged.

---

## 8. Engineering rules (v0 §8 plus these)

- ⏱ **Two time domains, never mixed:** case-video ms and answer-audio ms. Convert only through the playhead trace.
- 🔒 The **sealed raw zone** is the only place raw text lives. Job args carry IDs only.
- The **media privacy defaults** (B02):
  - proxies and clips with `-an -sn -dn -map_metadata -1`;
  - masks and OOB blanking burned in, never CSS;
  - content-addressed ASCII paths.
- **Clips carry no audio stream**; the surgeon's voice comes only from the answer audio.
- **`spoken_language` is independent of the UI locale.**
  - The original Japanese quote is always shown, and it is the evidence.
  - In English mode an English translation appears under it, marked 🤖 *machine translation*. It is never evidence.
- **Fail-closed config:**
  - no default `AI_PROFILE`;
  - cloud keys absent in `local`;
  - DEMO_MODE guard.
- **Honest-label taxonomy:**
  - 🧪 synthetic data;
  - 🧪 demo operator;
  - ⏱ prepared earlier;
  - 🤖 AI-structured;
  - 🖥/☁️ profile shown in the header.
- **Platform:**
  - `.gitattributes` LF;
  - `PYTHONUTF8=1`;
  - UTF-8 database;
  - one bitness per laptop;
  - DB `now()` for persisted timestamps;
  - LongPaths enabled.

---

## 9. Decisions

| # | Question | Decided (6 Oct) |
|---|---|---|
| Q1 | Who approves a surgeon's words? | **Hybrid:** ✅ the surgeon · 👁 a delegated owner (written, per case) · 👁 a reviewer for departed-surgeon archive cases · 🏥 department for procedure-level cards |
| Q2 | English mode and Japanese quotes | **Both:** original JP (the evidence) + 🤖 EN translation |
| Q3 | After review-due | Visible with ⚠; excluded from patterns and summaries |
| Q4 | LAN link | **Not needed:** the MVP runs on one laptop with AI through API keys |

**Still open before P0a (practical):**
- Approval to **install** dev tools on both laptops.
- The Smart App Control decision (it is in evaluation mode now). Decide after the P0a test.
- Repo location `C:\Users\karth\MyRobots\myrobots`.
- **API access** in `.env`: AWS Bedrock (Claude, `jp.` route, Tokyo) and OpenAI. Plus a third model family for script generation (a local open model on the ASUS, or another API).
- **Readers** for the practice audio: 2–4 people, with written consent.
- A licence-clean dry-lab video, if available.
