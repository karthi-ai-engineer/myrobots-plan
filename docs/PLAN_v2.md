# MY ROBOTS MVP — Consolidated Plan v2

> ⚠ **HISTORY ONLY.** Superseded on 24 Sep 2026 by `docs/MASTER_PLAN.md` (rethink from scratch). Where they differ, the master plan wins.
>
> v1.0 (PDF + Notion) + every decision made after it. When this file and the v1.0 PDF disagree, **this file wins**.
> Date: 21 Sep 2026.

---

## 0. Changes since v1.0 (read if you know the PDF)

| Area | v1.0 | v2 (now) |
|---|---|---|
| Working model | You + colleague | **Karthi + Claude only**; ready-made AI only, **no training** |
| Video AI (See) | Own trained models on GPU | **Eye v1** = cloud AI · **Eye v2** = ready-made research model (run-only, if licensed) · own trained eye after MVP |
| Pictures | 5 per second | **1 per second** |
| Audio | Scripted + synthetic for all 10 | **Track A**: 3 scripted test cases · **Track B**: surgeon think-aloud on the 10 cases |
| Languages | Each case in both | Both, **extracted separately then merged into one card**; English twin from the corrected Japanese transcript |
| Processing | After surgery (batch) | **Live mode** = batch in small slices; one code path |
| Blocks | 26 | **27** (+ F4 Live Streamer) + Backlog Planner |
| Pipeline | Forward only | **Two-way**: new surgeries + hospital archives (tiers) |
| AI provider | AWS Bedrock | **OpenAI for development**; two separate versions O (OpenAI) and B (Bedrock) |
| Gate 0 | One gate | **0a** dev start (now) · **0b** AWS setup (parallel) |
| Gate 2 | One gate | **2a** Track A · **2b** Track B |
| Human voice | Karthi + colleague read lines | Not needed |

---

## 1. Black box

**Inputs:** I1 surgery video · I2 OR voice (live, or surgeon narration for archives/Track B) · I3 case details (no patient info) · I4 surgeon review · I5 questions / expected obstacles · I6 dictionary (ontology) · I7 optional 30-s "why" note · (new) archive videos.
**Outputs:** O1 draft cards · O2 approved episodes · O3 procedure view · O4 answers with proof · O5 new-case prep brief ⭐ · O6 growth dashboard · (new) archive video index (Tier 1).
**We build and keep:** P1 web app · P2 OR audio capture (simulated) · P3 knowledge base · P4 ontology · P5 pipeline blocks · P6 test set.
**Roles:** surgeon (review, approve, why notes, weekly word approval) · resident (browse, ask, prep briefs; cannot approve) · admin/CEO (users, suggestion switch, audit, dashboard) · tester (factory, scores, answer keys).

---

## 2. Data plan

### 2.1 Videos
- Source: public datasets (Cholec80 ∩ CholecT50). Request access **in MY ROBOTS' name**, real purpose; license check for company use (CEO ask).
- 10 knowledge cases: **7 hard + 3 routine** (routine = trap for invented obstacles).
- Hardness signals: long Calot's triangle dissection · many irrigate/aspirate · long cleaning & coagulation · long total case → rank → watch top ~15 → pick 7 → surgeon confirms.
- 3 extra videos for Track A (not among the 10).
- **No model we run may have seen these videos** (ready-made research models trained on some public videos).

### 2.2 Obstacles
3–4 types at the general level (candidates: bleeding · dense adhesions · unclear anatomy at Calot's triangle · gallbladder tear / bile spill), each in 2–3 cases, ≥ 2 cases with a pair. Obstacles must be **visible** in the video. Marks: AI suggests (Amazon Nova, a different AI) → we confirm AND scan for misses → surgeon checks; every mark tagged AI-found / human-found.

### 2.3 Audio: two tracks
- **Track A (test data, now):** F1 AI-drafted scripts (Claude/OpenAI) → light edit → F2 synthetic voices (Polly has only 4 JP voices, 1 male → mix with open-source voices) → F3 mixer. Full team: surgeon, assistant, nurse, anesthetist.
- **Track B (knowledge data, needs the surgeon):** surgeon watches each video at 1× and explains aloud in Japanese (≈ 6–10 h total; can start with 3–4 cases). Plus a few synthetic nurse/anesthetist side lines (fake names for the privacy test). English twin: translation of the corrected transcript + synthetic voice.
- **Trap rules (Track A):** speak 1–5 s before/after actions · leave many actions unspoken · vague lines · off-topic chatter · a planted fake name · sometimes a word not in the dictionary.

### 2.4 Noise and versions
- F3 makes easy / medium / hard. **Primary pair** (🇯🇵 + 🇬🇧 at medium) → merged → knowledge base. **Stress test** (easy + hard) → speech-to-text scores only.

### 2.5 Answer keys (sealed, tester only)
Transcript · speaker tags (+ separate speaker files) · line times · fake names · planted obstacles. Track B: transcript = speech-to-text draft corrected carefully by hand; obstacles marked by the surgeon.

### 2.6 Two-way pipeline
- ➡️ Forward: new surgeries (live or after). ⬅️ Backward: hospital archive in batches. Same 27 blocks, one knowledge base per clinic.
- Archive tiers: **Tier 1** video index (automatic; labelled video-only; not knowledge; never in patterns) · **Tier 2** surgeon narration on selected moments → cards → review · **Tier 3** old recordings with voice → like new surgeries.
- Archive fixes: consent/ethics check · **Backlog Planner** (priorities + weekly review cap) · blur essential, edited videos flagged · evidence source on every card (🎙/🗣/👁) · case date on every card.

---

## 3. The dictionary (ontology)

- **Stack:** 🏠 our own layer (obstacle, warning sign, response, outcome, episode) · 🔬 OntoSPM main module only (CC-BY 4.0; skip non-commercial parts; keep our own copy) · 🏛 SNOMED CT slot empty until a license (Japan is not a SNOMED member → paid license).
- **Kinds:** step · anatomy · tool · action · obstacle · warning sign · response · outcome · safety check (CVS).
- **Wires:** step contains action · action uses tool · action acts on anatomy · warning sign comes before obstacle · obstacle happens in step · response answers obstacle · response leads to outcome.
- **Sources:** Cholec80 7 phases + Tokyo Guidelines 2018 safe steps · CholecT50 instruments/verbs/targets · CVS · guidelines + surgeon.
- **Growth: fast lane + gate:** new word → synonym check (attach if match) → pending zone (usable, unverified, not counted) → surgeon approves weekly.
- **Two levels** (general + specific); counts roll up to general; a card stores the most specific term the evidence supports. **Merge with alias** (old ID points to survivor; cards re-pointed; history kept). Versions, never overwrite.
- Drafted by AI → edited → surgeon checks. Severity scale + ontology v1 need the surgeon's check.

---

## 4. Blocks: key details

### Factory
- **F1** inputs: dataset labels, obstacle marks, dictionary, trap rules, roles/persona → script lines (time, speaker, 🇯🇵, 🇬🇧, tags). One master script per case.
- **F2** script → one clean audio file per speaker.
- **F3** speaker files + times + CC0 noise + silent video → video with OR audio (6 versions) + sealed answer keys.
- **F4** finished case → live stream at 1× (video + both audio tracks); 10× replay for tests. Later swapped for a real capture box (splitter + capture card; hospital engineer approval).

### Ingest
- **B01** ID · fingerprint (duplicates) · original to storage, never touched · status: received → processing → ready for review → approved · source live/archive · case date · short intake form for archives.
- **B02** 1 picture/sec · speech-ready audio · one clock · privacy blur slot (empty in MVP, required before real data).

### See
- **B03/B04** eye v1 (cloud AI, 10 pictures per request) or v2 (research model on GPU); better scorer per kind feeds B09; smoothing (min step length, join short tool gaps); allow "unknown step"; raw per-second kept.
- **B05** tool clues (irrigator, aspirate, coagulate) → cloud AI checks only the ~15-s clip → "bleeding yes/no + confidence"; everything marked HINT.

### Hear
- **B06** open-source engine on GPU (candidate: Whisper family), medical word list, word times; cloud benchmark (Transcribe / OpenAI) after the first end-to-end run, factory data only; also the "why" note.
- **B07** voice split + role from content; one line = one speaker turn (long turns split at pauses); knowledge only from surgeon + assistant; nurse/anesthetist = context; low role confidence → review.
- **B08** names/IDs/dates → [PATIENT]; eponym safe list; locked hidden list; text only in MVP (audio muting before real data); questions filtered too; raw transcript sealed.

### Understand
- **B09** one timeline; any clue opens a moment (voice keyword, tool clue, bleeding check, unusually long step); look-back window; join overlaps; strength 👁/👂/👁👂; only finds WHEN. Starting values tuned by tests.
- **B10** moment → draft card or "nothing here"; obstacle episodes only; evidence per field; unknown allowed; proposes severity; benchmark open LLM later.
- **B11** exact → synonym → meaning (embeddings) → AI judge → else pending; danger zone → review; **twin match** (agree/partial/disagree; disagreements shown); passes through with one language.

### Review
- **B12** card beside its ~20-s clip; confident fields folded; uncertain highlighted; edits from dictionary lists (+ suggest new term); one-tap reject reasons (wrong obstacle · wrong time · not an obstacle · duplicate · other); guard: danger-zone clip must be opened, too-fast approvals flagged; confirms severity + pairs; optional why note; approver = operating surgeon.
- **B13** best cards → examples; repeated fixes → rules (only after repeats); learn forward only.

### Know
- **B14** approved only; history of corrections; Postgres to start (graph tool chosen in Part 2).
- **B15** keyword + meaning + graph; IDs make 🇯🇵/🇬🇧 searchable together; separate index per AI version.
- **B16** pattern cards; instant updates; count + % + ⚠; responses grouped by resolving action (failed/unknown → "not resolved/unknown"); single-case labelled; disagreement side by side; unverified not counted.

### Ask
- **B17** privacy filter → IDs → evidence → write → checker removes uncited case claims → saved with snapshot → one-tap rating. **3 layers:** 📚 your cases (gaps first) · 🧠 general knowledge (labelled unverified; conflicts flagged) · 💡 suggestion (basis shown; admin switch). Japan's MHLW SaMD guideline weighs "presents recommended treatments" → suggestions need a regulatory check before hospital use.
- **B18** pick or describe expected obstacles → each pattern · seen together? · gaps. No prediction from patient data yet.
- **B19** cases · episodes · edit-rate trend · review time · coverage · pending words; show rejections too.

### Always On
- **B20** Google Workspace sign-in; roles; residents cannot approve; only tester sees answer keys.
- **B21** append-only; IDs not content; logs AI provider + model versions.
- **B22** pending zone, weekly approval, aliases, two levels, versions; every card remembers its dictionary version.
- **B23** scores vs answer keys; auto re-run after changes; 7 tuning + 3 locked; O vs B comparison; benchmarks.

---

## 5. Data units — full fields

**U1 Episode card:** episode_id · case_id · surgeon (persona/real) · languages · twin_agreement · start · end · step_id · obstacle_id (one) · anatomy_id + danger_flag · warning_sign_ids[] · response[] (ordered: action_id, tool_id, evidence) · outcome (worked/partial/failed/unknown) · severity (minor/moderate/major/unknown; proposed/confirmed) · why (text or unknown) · paired_with[] · evidence{field → line_ids, video_times, checks} · witness (video/voice/both) · confidence{field} · evidence_source (live/narrated/video-only) · case_date · status (draft/approved/rejected) · made_by (block, provider, model, dictionary version) · history (edits, reviewer, reason).
**U2 Case package:** file · fingerprint · case_id · procedure · surgeon · language · noise_level · source (factory/recorder; live/archive) · case_date · version_role (primary pair/stress test). Never: answer keys, patient info.
**U3 Video fact:** case_id · start · end · kind (step/tool/action_hint/event_check) · value (ontology ID) · confidence · source (eye v1/v2/tool rule) · made_by. Segments to B09; raw kept.
**U4 Transcript line:** line_id · case_id · language · version_role · start · end · voice · role · role_confidence · clean_text · words[] (text, time, confidence) · flags (context_only, low_conf_words, hidden_name) · made_by.
**U5 Moment:** moment_id · case_id · twin · start · end · video_fact_ids[] · line_ids[] (knowledge + context) · clues[] · strength · made_by.
**U6 Dictionary term:** id · kind · label_ja · label_en · synonyms[] · definition · ontospm_link · snomed_code (empty) · parent · children[] · danger_zone · safe_name · status (official/pending/retired) · source · version_added · history.
**U7 Pattern card:** obstacle_id (general) · procedure · dictionary_version · cases_seen / cases_total · small_sample · breakdown[] · steps{} · warning_signs{} · responses (by resolving action + sequences, outcomes, severity mix) · disagreements · pairs{} · gaps[] · proof (episode ids) · witness_mix.
**U8 Answer:** question · language · role · time · term_ids · layer_cases (statements + citations, or "no evidence") · layer_general (labelled) · layer_suggestion (basis; hidden if switch off) · uncited_removed_count · made_by · knowledge_snapshot · rating (+reason). Prep brief = special answer.

---

## 6. Quality

- 🔴 Hard rules (= 0): leaked names · low-confidence danger-zone auto-maps · uncited case claims · unapproved cards in the KB · wrongly hidden eponyms.
- 🟡 Pass marks: planted obstacles found ≥ 80% · invented in routine cases ≤ 1 total · review < 2 min/case · approved unedited ≥ 70% · same cards across languages (before merge) ≥ 90% · each pattern from 2+ cases 100% · edit rate falls · "seen together" 100%.
- 🔵 Measure first: speech errors (per language/noise) · roles · steps/tools accuracy (v1 vs v2 vs labels) · moments matched · "not enough evidence" correctness.
- Final exam: reported scores only from the 3 locked cases. Question test set: AI drafts from answer keys → edit → surgeon checks.

---

## 7. Costs (estimates)

| | 🟠 Version O · OpenAI (cash, dev) | 🟢 Version B · Bedrock (AWS credits) |
|---|---|---|
| Per full run (10 cases, live) | ≈ $27–62 | ≈ $42–73 |
| Base per month | ≈ $17 (laptop app, vast.ai GPU) | ≈ $111–141 (app server, storage, tests, benchmarks) |
| Typical month (2 runs) | ≈ $70–141 | ≈ $195–290 |
| Ceiling | ≈ $350/month spending limit | ≈ $560/month |

Assumptions: 10 cases × 60 min · 36,000 pictures/run · ≈ 250 tokens in / 30 out per picture · ≈ 300 card calls/run · OpenAI GPT-5.4 mini / GPT-5.4 / GPT-5.5 list prices · Bedrock Claude at Japan in-region rates (Sonnet 5 $3.30/$16.50, Haiku 4.5 $1.10/$5.50) · GPU g6.xlarge Tokyo $1.17/hr or vast.ai ≈ $0.40/hr · ¥160 = $1. Not included: Claude subscription, laptop, hospital box, legal/regulatory fees, surgeon pay.

---

## 8. Before ANY real data (checklist)
1. Video privacy blur built + tested · 2. Staff + patient consent · 3. Runs inside the hospital (or approved secure setup) · 4. Names muted in audio playback · 5. Cloud speech benchmark off · 6. Regulatory check on suggestions + general knowledge · 7. Raw transcripts deleted after scoring or kept in-hospital · 8. Consent/ethics check for archive videos.

## 9. CEO asks
1 GI surgeon (消化器外科) · 2 confirm live OR voice · 3 OK with public video + scripted/narrated demo · 4 dataset license · 5 dataset access in company name · 6 SNOMED license (later) · 7 regulatory specialist · 8 surgeon narration time (≈ 6–10 h) · 9 OpenAI company account + spending limit (≈ $350/month) · plus AWS account/credits (apply once via the bigger offer: Inception vs Deel)/GPU quota/Bedrock access.

## 10. Future plug-ins (not MVP)
Approval by other surgeons + flagging · teaching-tip cards · obstacle prediction from patient data · zoom-in frame sampling · own bleeding detector · own trained video eye · SNOMED codes · robotic procedure #2 · cross-clinic sharing (consent) · statistics rule for relaxing the small-sample tag.

## 11. How knowledge builds in a clinic
Videos are never merged; each case adds approved cards; patterns re-count on each approval; nothing combines until approved; surgeons shown side by side; corrections re-count patterns; each clinic's knowledge stays its own; empty start eased by the dictionary + labelled general knowledge + optional labelled reference library (never mixed).
