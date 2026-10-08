# MY ROBOTS · Master Plan v3 (merged product): DRAFT

> **Started:** 4 Oct 2026 · Karthi + Claude · **Status:** drafting, chapter by chapter. On approval this file replaces `MASTER_PLAN.md`, and v2 moves to `docs/history/`.
> **Why v3:** v2 was designed without the CEO's app in view. The comparison showed one product in two halves: the CEO's app = governance shell + UX, v2 = engine + knowledge core. v3 merges them (`decisions/2026-10-04-v3-approach.md`).
> **Evidence base (outside this repo while it is public):** `C:\Users\karth\MyRobots\analysis\00-SYNTHESIS.md` and `analysis\research\RS1–RS7` (≈ 550 dated sources).
> **How we work:** one topic at a time → options + pick → Karthi decides → decision file → chapter ✅. A red-team pass runs before approval.

---

## One-page summary (fills in as chapters are agreed)

**Product:** surgeons' judgment (the "why") captured in their own words, anchored to their video, drafted by AI as evidence-linked cards and approved by the surgeon. Knowledge accumulates per procedure → variant → surgeon into a department **living manual** that juniors use to prepare for cases.

**Wedge ✅:** **archive narration first.**
- A senior surgeon narrates over their *own* archive videos, building the department manual; juniors use it for pre-case study.
- The buyer is the department head.
- Prospective OR capture is phase 2, on the same engine.

**First procedure pack ✅:** adrenalectomy (pilot), then RARP (business). Side, approach, platform and indication are variants. The procedure follows the clinical partner.

**Claim ✅:** from a surgeon's own words anchored to their video, AI drafts evidence-linked decision cards that the surgeon approves in minutes; approved knowledge accumulates into a department manual that juniors actually use.

**Proof levels ✅:**
- 🧪 L1 engineering (synthetic);
- 🩺 L2 archive pilot (1 surgeon, 3–5 cases);
- 🏥 L3 department pilot (the CEO's targets as rates).

**Guardrails ✅:** 32 rules in 5 families (truth · human control · privacy and law · safety and regulation · honesty).

**Stages ✅:**
- S0 Build (Oct–Dec 2026);
- S1 Archive pilot (≈ Jan–Mar 2027);
- S2 Department pilot (≈ mid–late 2027);
- S3 Paid V1 (adds ASK and prospective OR capture);
- S4 Scale.

**Journeys ✅:** Narrate · Read · Govern · (V1) same-day debrief-in-review, on 6 screens.

**Knowledge ✅:**
- 4-type cards with per-field evidence;
- the AI fills structure only;
- a clinician-owned dictionary graph in Postgres;
- patterns need ≥ 3 cases & ≥ 2 surgeons.

**Retrieval ✅:**
- graph-first hybrid (graph + keyword + embeddings + reranker) from S0;
- Prepare = rule-selected cards plus an approved 🤖 summary.

**System ✅:**
- the narration pipeline with mandatory verification;
- Japan-only AI for real data (Opus 5.5 / Sonnet 4.6 `jp.`, cross-family verifier);
- AWS Tokyo in a separate real-data account;
- native Windows-ARM64 dev with cross-platform libraries.

**MVP setup ✅ (6 Oct):**
- the MVP runs **only on the two laptops**: the Surface runs the app and database, the ASUS Zenbook 48 GB runs the AI;
- every AI call goes through the **AI gateway**, which switches profile with one setting:
  - `dev-cloud`: APIs, for fast development on synthetic data;
  - `local`: open models on the ASUS;
  - `pilot-japan`: later, for real data;
- the cloud (AWS Tokyo) comes only after the MVP is approved.

**Not yet decided:** compliance · data · evaluation · roadmap · risks · ways of working.

---

## Chapters

| Part | # | Chapter | Main inputs | Status |
|---|---|---|---|---|
| **A Product** | 1 | North star: users, wedge, claim, metrics | RS7, RS2, RS6, synthesis §0–4 | ✅ 4 Oct |
| | 2 | Guardrails (v2's 22 rules + the CEO's non-negotiables + new) | v2 ch. 2, synthesis §3/§10, RS6 §4b, RS7 §6 | ✅ 4 Oct |
| | 3 | Scope and stages (MVP → pilot → V1 → scale; in / parked) | RS7 §5, synthesis §12, RS1 | ✅ 4 Oct |
| | 4 | User journeys and screens (contribute · review · consume · govern) | RS2 §6–8, the CEO's UX, RS7 §6 | ✅ 4 Oct |
| **B Knowledge** ⭐ | 5 | Knowledge model (card, ontology, governance states, cross-case patterns) | RS4 §3/§4b, synthesis §9, RS1 §3/§5 | ✅ 4 Oct |
| | 6 | Retrieval and answering (manual, briefs, ASK, citations, abstention) | RS4 §4, RS6 §4b (non-SaMD limits) | ✅ 4 Oct |
| **C System** | 7 | Pipeline lanes (Ingest → Hear → Privacy → Draft → Verify → Review → Know → Serve) | v2 blocks, RS3, RS4 §1 | ✅ 4 Oct |
| | 8 | AI per block and gateway (Japan routing, data classes, model swaps) | RS4 §2, RS3 §1, verified Bedrock `jp.` list | ✅ 4 Oct |
| | 9 | Architecture and deployment profiles (dev · in-Japan cloud · hospital) | synthesis §7–8, RS5, RS3 §7 | ✅ 4 Oct |
| | 10 | Tech stack and dev setup (ARM64 laptop + 48 GB laptop) | RS5 | ✅ 4 Oct |
| | 11 | Security, privacy and compliance (APPI, 3省2, MHLW v7.0, ethics, PMDA) | RS6 §4b, RS3 §5, RS5 §5 | ⬜ |
| **D Proof** | 12 | Data plan (synthetic fixtures, dry-lab, public data, clinical route) | RS6 §3–4, RS3 §6 | ⬜ |
| | 13 | Evaluation and quality (answer keys, metrics, evidence ladder) | RS6 §1–2, RS4 benchmark, RS3 benchmark | ⬜ |
| **E Execution** | 14 | Roadmap and work packages (split-ready for 2 engineers) | RS6 timeline, RS7 §5, RS5 §8 | ⬜ |
| | 15 | Risks and dependencies | all | ⬜ |
| | 16 | How we work | v2 ch. 13, synthesis §14 | ⬜ |

## Decisions so far

| Date | Decision | File |
|---|---|---|
| 4 Oct | v3 approach: one merged product · re-baseline v2 · full horizon, MVP detailed · this repo · 16 chapters | `decisions/2026-10-04-v3-approach.md` |
| 4 Oct | Wedge = archive narration first · first pack = adrenalectomy → RARP | `decisions/2026-10-04-v3-wedge-and-procedure.md` |
| 4 Oct | Claim · 5 parts · proof levels L1/L2/L3 + pass marks · one 45-min deep-reference interview in L2 | `decisions/2026-10-04-v3-claim-and-metrics.md` |
| 4 Oct | Guardrails: 32 rules; raw-audio retention = hospital decides, explicit at setup | `decisions/2026-10-04-v3-guardrails.md` |
| 4 Oct | Stages S0–S4 · "never" list · ASK in V1 after the PMDA consultation | `decisions/2026-10-04-v3-scope-and-stages.md` |
| 4 Oct | Journeys J1–J4 · 6 screens · facilitated-then-self-serve pilot · template follow-ups | `decisions/2026-10-04-v3-journeys-and-screens.md` |
| 4 Oct | Knowledge model: 4-type card · AI fills structure only · computed confidence · pattern ≥ 3 cases & ≥ 2 surgeons · Postgres only | `decisions/2026-10-04-v3-knowledge-model.md` |
| 4 Oct | Graph-first hybrid retrieval from S0 · Prepare = cards + 🤖 summary approved per version · ASK in V1 | `decisions/2026-10-04-v3-retrieval.md` |
| 4 Oct | Pipeline: new N1–N3 · B26 · B27; parked See lane etc.; drafting as-you-go during narration | `decisions/2026-10-04-v3-pipeline.md` |
| 4 Oct | AI: Japan-only routing for real data · Opus 5.5 vs Sonnet 4.6 `jp.` · verifier = best performer from another family · dev uses Japan models | `decisions/2026-10-04-v3-ai-per-block.md` |
| 4 Oct | Architecture: separate real-data AWS account · full video to S3 Tokyo (hospital approval; browser-clips fallback) · real traffic direct to Tokyo | `decisions/2026-10-04-v3-architecture.md` |
| 4 Oct | Stack approved · native Windows ARM64 dev, no WSL, cross-platform library rule · new private code monorepo | `decisions/2026-10-04-v3-tech-stack.md` |
| 6 Oct | **MVP runs on the two laptops only (no cloud hosting)** · all AI goes through the gateway with switchable profiles `dev-cloud` / `local` / `pilot-japan` · dev uses `dev-cloud` on synthetic data · demo target `local`, decided after the G1 benchmark | `decisions/2026-10-06-v3-mvp-local-and-gateway.md` |

---

## 1. North star

### 1.1 Who it's for, and the wedge ✅

```
 Senior surgeon / proctor           Department "living manual"           Juniors / fellows
 narrates over OWN archive video ─▶ approved "why" cards per step  ─▶   weekly pre-case study,
 (★ moments → ≤ 3 questions each)   evidence-linked · versioned          indexes the full video
          ▲                                                                       │
          └────────── Department head buys (≈ ¥1.5–2M/yr, under tender limit) ◀───┘
 Phase 2 (same engine): prospective OR capture + same-day debrief-in-review
```

| Role | Who | Job to be done | Evidence |
|---|---|---|---|
| Contributor | Senior surgeon / proctor | Pass on judgment without writing; "my words, my approval, no evaluation" | RS2 §5, RS7 §1, §6 |
| Weekly user | Juniors / fellows / 専攻医 | Prepare for cases outside work hours; the robotic guideline requires video study | RS7 §1 (89% study after hours) |
| Buyer | Department head (教授 / 診療科長) | Train more people with less expert time; regional dispatch and certification pressure | RS7 §1, §3 |
| Later | Nurses, CEs, hospital management, device makers, societies | V1 or later | RS7 §1 |

**First procedure pack ✅:** adrenalectomy → RARP (see `decisions/2026-10-04-v3-wedge-and-procedure.md`).

### 1.2 The claim ✅

> From a surgeon's own words anchored to their video, the system drafts **evidence-linked decision cards** that the surgeon approves in minutes. Approved knowledge accumulates into a department manual that juniors **actually use** to prepare for cases.

Five provable parts:
- ① **Captures**: finds the decisions that matter.
- ② **Proves**: every field is linked; nothing is invented.
- ③ **Light**: little expert time.
- ④ **Trusted**: few edits, and planted errors are caught.
- ⑤ **Used and grows**: juniors use it, and fewer questions are needed per case over time.

### 1.3 Proof levels and success metrics ✅

| Level | Data | Proves | Pass marks |
|---|---|---|---|
| 🧪 **L1 Engineering** | Synthetic + human-read scripts | Pipeline correct and safe | evidence IDs valid 100% · quotes match 100% · numbers not in source 0 · planted-PII leaks 0 · negation errors after verify 0 · debrief CER ≤ 10% |
| 🩺 **L2 Archive pilot** | 1 surgeon, 3–5 own cases | Feasible and light on real cases | expert time ≤ 30 min/case (ceiling 40) · ≥ 3 approved cards/case with the "why" filled · ≥ 59 links checked, 0 wrong · PII leaks 0 · capture rate (vs one 45-min deep-reference interview) and blinded peer usefulness reported · 1 LOI |
| 🏥 **L3 Department pilot** | ≥ 2–3 surgeons, ~20 cases, juniors | The CEO's targets as rates | capture ≥ 80% · source links ≥ 95% · review median ≤ 10 min · "why" filled ≥ 80% · juniors weekly ≥ 60% · ≥ 2 LOIs at a stated price |

Measurement rules:
- Two blind answer keys: source-grounded and surgeon-intent.
- "0 attribution errors" is a process guarantee: the surgeon confirms each one, and it is audited.
- Planted-error catch rate is measured.
- An LLM is never the only judge.
- Results are reported per surgeon, never averaged.
- Details: `decisions/2026-10-04-v3-claim-and-metrics.md`.

---

## 2. Guardrails ✅

32 rules in 5 families. Full text: `decisions/2026-10-04-v3-guardrails.md`.

| Family | Rules |
|---|---|
| **A Truth and evidence** | 📎 no evidence → no field · 🗣 the "why" = the surgeon's own words · ❓ unknown allowed · 🎯 moment = candidate · 🗣🎥 said + seen · ⏱ one clock |
| **B Human control** | ✅ surgeon approves · 📚 approved only · 🔏 approved = frozen · 👀 elicit before show · ↩️ withdraw any time · 🔐 separate gates (privacy ≠ approval ≠ release) · ⚖️ never averaged |
| **C Privacy and law** | 🇯🇵 Japan boundary (enforced) · 🏥 hospital boundary per pilot · 🔒 privacy filter before AI · 🎙 only consenting voices · 🗑 minimise (raw-audio retention: hospital decides, explicit at setup) · 📄 not a medical record · 🏥 no real data before the checklist · 📜 licence before use (data + models) |
| **D Safety and regulation** | ⚕️ education, not a device · 🚫 nothing before 🏁 · 🫀 danger zones + laterality rules · ⚠ serious events excluded by default · 🙅 no evaluation, ever |
| **E Honesty and engineering** | 🏷 honest labels (never fictional provenance) · 🔢 small numbers · 🔒 sealed envelope · 🔒 original untouched · 🔑 secrets · 🧪 no model training |

---

## 3. Scope and stages ✅

Full text: `decisions/2026-10-04-v3-scope-and-stages.md`.

| Stage | Adds | Exit |
|---|---|---|
| **S0 Build** (≈ Oct–Dec 2026) | Narration Studio: intake · video + step template · ★ mark · clip-anchored voice Q&A · ASR · privacy filter · drafting + verify · review queue · living manual · case detail · governance · audit · JA + EN · 🧪 demo | L1 |
| **S1 Archive pilot** (≈ Jan–Mar 2027) | Real-data security · consent + retention · AI-governance stamps · phone/iPad · research export | L2 + 1 LOI |
| **S2 Department pilot** (≈ mid–late 2027) | Multi-surgeon · Prepare · Learn · RARP pack · disagreement view | L3 + ≥ 2 LOIs |
| **S3 Paid V1** | Admin · notifications · TODAY · similar · **ASK** (after the PMDA consultation) · **prospective OR capture** (partner recorders) + same-day debrief-in-review · staff consent · accessibility · search | First paying department |
| **S4 Scale** | More departments and hospitals · RAPN · robotic GI · experience graph · educator · nurse/CE | — |

❌ **Never:**
- a "parity" frame;
- quality dashboards / surgeon scoring;
- 7 storage modes;
- HL7/FHIR;
- our own capture hardware;
- in-app billing;
- non-JP coding;
- operative notes;
- QI analytics.

KO/ZH/HI are frozen.

---

## 4. User journeys and screens ✅

Full text: `decisions/2026-10-04-v3-journeys-and-screens.md`.

```
 J1 NARRATE (surgeon · desktop/iPad · ≈ 20–30 min/case)
   once per procedure: task diagram (3–6 steps, priority steps)
   per case: skim 4–8× → ★ marks → per ★ (≤ 5): clip stops before the decision → 🎙 open question
             → type tap → 1–2 template probes → optional closer → review drafted cards
 J2 READ (juniors · phone/iPad · weekly): procedure → variant → step → cards (surgeons side by side) → ▶ clip
   S2 Prepare: night before (predict-then-reveal) · day of (top pitfalls) · just before (≤ 5 items)
 J3 GOVERN (clinical owner): setup (pack · roles · explicit retention · consent) → gates → withdraw → audit
 J4 PHASE 2 (V1): OR → optional 60-s flag → 1–2 h processing → same-day debrief-in-review (≤ 3 Qs, ≤ 5 min)
```

| Screen | Stage | | Screen | Stage |
|---|---|---|---|---|
| ① 語る Narrate | S0 | | ④ 症例 Case | S0 |
| ② 承認 Review (phone-first) | S0 | | ⑤ 管理 Admin | S0 |
| ③ 手術ノート Living manual | S0 | | ⑥ 術前ブリーフ Prepare | S2 |

Pilot sessions:
- facilitated for the first 1–2 cases, then self-serve;
- follow-ups use clinician-ratified templates; the LLM only classifies and fills slots;
- free LLM follow-ups run only as an observed A/B experiment.

---

## 5. Knowledge model ⭐ ✅

Full text: `decisions/2026-10-04-v3-knowledge-model.md`.

```
 CASE (procedure · side · approach · robot · technique ver. · surgeon pseudo-ID · data class)
   └─ UTTERANCES (append-only, redacted, timestamped; narration | debrief | OR)
        ▲ evidence anchors (W3C-annotation style: utterance range + exact quote + media t=start,end)
   CARD → CARD_VERSION (append-only; approved = immutable) + CEO governance columns
        └─ CARD_FIELD rows: status · evidence_kind · verifier verdict · computed confidence band
   DICTIONARY (clinician content, Karthi mechanism): kinds · JA/EN/slang/ASR labels · is_a (ltree)
        part_of · confusable_with · sounds_like · danger_zone · laterality rules · replaced_by · semver
   EDGES (derived from approved versions, rebuildable): card→term · card→card
```

- **4 card types:** decision point · pitfall · recovery · technique.
- **AI fills structure only.** Clinical-claim fields are surgeon-stated or *unknown*.
- Confidence is computed, never LLM-stated.
- Approved cards are never re-pointed on a dictionary merge.
- The DB is the dictionary's source, with a git export.
- Severity scales: ClassIntra + Clavien-Dindo (EAUiaiC mapped).
- **Patterns:**
  - a pattern needs ≥ 3 cases & ≥ 2 surgeons (configurable);
  - surgeons appear side by side; disagreement is proposed by rules and confirmed by a clinician;
  - promotion to procedure or institution level = a new clinician-approved card.
- Storage: **Postgres only**, no graph DB.

---

## 6. Retrieval and answering ✅

Full text: `decisions/2026-10-04-v3-retrieval.md`.

```
 query → ① entity linking → term IDs → ② graph expansion (confusables never auto-expanded: "did you mean?")
       → ③ 🕸 graph + 🔤 lexical + 🧠 dense channels (approved cards only) → ④ RRF fusion + filters
       → ⑤ Japanese reranker → ⑥ grouped by step → type → surgeon, with clips + explicit GAPS
```

- Built from S0. Each channel is ablation-measured.
- **Prepare (S2):**
  - cards are selected by rules;
  - a 🤖 summary is generated only from those cards, with a citation per sentence, a checker and a verifier;
  - a **clinical owner approves each summary version**.
- **ASK** comes in V1 on the same pipeline, with abstention and gaps.

---

## 7. Pipeline lanes ✅

Full text: `decisions/2026-10-04-v3-pipeline.md`.

```
 archive video → B01 → B02 (proxy · audio · clips · out-of-body · overlay masks)
 surgeon → 🎙 N1 step template → N2 ★ + clips → N3 Question Engine → voice answers
 → 👂 B06 ASR (2 engines) → B07 (1 speaker) → B08 privacy
 → 🧠 B09 moments → B10 drafter (structure-only AI) → B26 validators + verifier → B11 terms → bands/dedupe
 → ✅ B12 one queue → approved 🔏 → 📚 B14 graph · B15 index · B16 patterns → manual · search · B18 Prepare
 V1 prospective: recorder → … → N3 ≤ 3 Qs → same-day debrief-in-review
```

- **New blocks:** N1 · N2 · N3 · B26 · B27.
- **Parked:** B03 · B05 · B13 · B17 (V1) · B19 · B25 · F4.
- **Drafting runs as you go** during narration, and the surgeon reviews at the end of the session.

---

## 8. AI per block and gateway ✅

Full text: `decisions/2026-10-04-v3-ai-per-block.md`.

**Gateway:**
- every call carries a data class;
- **real data only on Japan routes**, enforced in code and by IAM/SCP;
- a privacy stamp;
- pinned model IDs plus provenance;
- golden-set re-run on any model change;
- EOL watch.

**Models (G1 picks the winners):**

| Stage | Choice |
|---|---|
| ASR | ReazonSpeech-k2 + kotoba-whisper (two engines) |
| Drafting | A/B: **Opus 5.5 `jp.`** vs **Sonnet 4.6 `jp.`** |
| Verifier | Best performer from a different family (gpt-oss-120b / Qwen3 / DeepSeek, In-Region Tokyo; disclosed to the hospital if Chinese-origin) |
| Embeddings | ruri-v3 vs Cohere v4 |

**Dev:**
- dev uses the same Japan models;
- OpenAI is used only for benchmarks;
- the local-LLM hospital box is plan only.

**Cost and flags:**
- ≈ $1.3–4.6 of LLM cost per case;
- ⚠ a legal opinion is needed on ReazonSpeech-corpus models.

---

## 9. Architecture and deployment profiles ✅

Full text: `decisions/2026-10-04-v3-architecture.md`.

**Profiles:**

| Profile | Where it runs |
|---|---|
| P-dev | Laptops, 🧪 only |
| P-cloud-JP synthetic | Cloudflare Access OK |
| **P-cloud-JP real** | Separate AWS account, region-locked to Tokyo/Osaka |
| P-browser-edge | Fallback |
| P-hospital-box | Plan only |

**Choices:**
- **Archive pilot: the full video is uploaded to S3 Tokyo.** This needs hospital approval; if refused, the pilot falls back to browser-cut masked clips.
- **Real-data traffic goes direct to AWS Tokyo** (hospital OIDC + Tokyo load balancer; no CDN for the API or media). Cloudflare Access is for dev and demos only.

---

## 10. Tech stack and dev setup ✅

Full text: `decisions/2026-10-04-v3-tech-stack.md`.

- **Native Windows ARM64 on both laptops (no WSL, no Docker)**, with a **cross-platform rule**: every library must run on Win-ARM64 *and* Linux-ARM64.
- **Library choices:**
  - sherpa-onnx + whisper.cpp for ASR;
  - ONNX Runtime for embeddings;
  - Janome or Lindera as the tokeniser;
  - Postgres 18 + pgvector native, with a small RDS dev DB as fallback.
- The 48 GB ASUS Zenbook runs the heavy jobs.

**Stack:**

| Layer | Choice |
|---|---|
| Backend | FastAPI · SQLAlchemy + Alembic · Procrastinate |
| Data | RDS PG 18 · S3 Tokyo |
| Frontend | Vite SPA + the CEO's design · Orval contract mocks |
| Infra | ECS Fargate ARM64 · Terraform · GitHub Actions on ARM |

**Code:** a **new private monorepo**; this repo stays docs-only.

---

## 11–16

⬜ Not started. Inputs are listed in the chapter table above.
