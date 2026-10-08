# v3 ch. 5: Knowledge model

- **Topic:** The unit of knowledge, evidence, ontology, governance states, cross-case aggregation and storage.
- **Inputs:**
  - RS4 §4, §6, §7.8–7.9;
  - synthesis §9 (merged card);
  - v2 ch. 4 (knowledge core, ≈ 75% confirmed by RS4);
  - the CEO's `knowledge_versions` / governance model;
  - RS1 §3 (domain structure).
- **Delegated to Claude as in v2, with Karthi's veto.** Karthi decided two product questions.

## Model

- **Unit:** a knowledge card of 4 types: decision point · pitfall · recovery · technique.
  - "Obstacle" (v2) = a pitfall/recovery subtype. Response steps are kept as ordered `actions`.
  - The type list is to be ratified by a clinician; the CEO's 22 types survive only as optional tags.
- **Minimal required fields:** `knowledge_type` · procedure context · pseudonymous surgeon · `decision` · `why` (or *unknown*) · per-field evidence for every filled field · governance `state` · provenance stamp.
- **Per-field rows (`card_field`):**
  - `status`: filled | unknown | n/a;
  - `evidence_kind`: stated_narration | stated_debrief | stated_OR | inferred;
  - verifier verdict;
  - **computed confidence band**: high | check | low, derived from verifier + self-consistency + ASR + speaker-role signals, never an LLM-stated number.
- **Evidence anchors:** W3C Web-Annotation-style selectors (utterance range + TextQuote exact/prefix/suffix + char offsets + media fragment t=start,end). Provenance is PROV-O-style (run, model, prompt, dictionary version, input hashes).
- **Versions:** cards are append-only versions. Approved versions are immutable (DB trigger); an edit creates vN+1 `in_review`.
- **Governance (CEO, as columns on `card_version`):**
  - state (draft · in_review · approved · rejected · superseded · withdrawn);
  - scope (case · surgeon · procedure_common · institutional) and authority;
  - privacy state · release purposes · review-due · serious-event signal.
- **Dictionary:**
  - Clinician-owned content, Karthi-owned mechanism, ≈ 150–300 terms per procedure.
  - Kinds; SKOS-style pref/alt/hidden labels in JA/EN (slang, ASR variants); `is_a` as an `ltree` path; `part_of` · `confusable_with` · **`sounds_like`**; `danger_zone`; `safe_name`; **laterality validation rules**.
  - `replaced_by`: never delete. **Approved cards are never re-pointed on a merge**; chains resolve at read time.
  - Semver releases; every card version stores its `dictionary_version`.
  - **The DB is the source**, with a git export for diffs (reverses v2).
- **External ontologies:** map, don't adopt (`skos:exactMatch/closeMatch` rows: OntoSPM, FMA/Uberon, K-code, STEM). The SNOMED slot stays empty because Japan is not a member.
- **Severity scales:** ClassIntra (intra-op, cross-specialty) + Clavien-Dindo (post-op), with EAUiaiC mapped for urology display.
- **Cross-case aggregation:**
  - Situation key = procedure (+ variant) + step + overlap on warning-sign/anatomy/finding IDs (rolled up via `is_a`), recomputed on read and never itself an approval.
  - Counts carry provenance (every number links to its cards). Surgeons appear in side-by-side columns, never as pooled rates.
  - Rules propose `contrasts_with` and a clinician confirms it. "Consensus" is a computed view, never a stored field.
  - Promotion to procedure-common/institutional = a **new clinician-approved card** citing its member cards, with counts inserted by code. The LLM may only write words around inserted numbers, with a citation per sentence.
- **Storage:** Postgres only (relational + JSONB + typed edge table + `ltree` + pgvector + pg_bigm).
  - Edges are derived from approved versions and can be rebuilt.
  - No graph DB: revisit at ≥ 10⁷ edges or interactive paths deeper than 4 hops.
  - Apache AGE is not on RDS, so it is skipped.

## Karthi's decisions

- **AI-filled fields: structure only (chosen).**
  - Clinical-claim fields (decision · why · alternatives · applicability · limitations · lesson) are **surgeon-stated or *unknown***.
  - The AI fills only structure (type, step, term IDs, times, outcome category) and paraphrases what was said.
  - Option considered: the AI also suggests clinical fields marked 🤖.
  - *Consequence:* the RS2 review-card sketch's "他の選択肢 🤖" (other options, AI-suggested) example is replaced. The low-confidence 🤖 marks now apply only to structural fields (e.g. step or term mapping).
- **Pattern label: ≥ 3 cases & ≥ 2 surgeons, configurable per department by the clinical owner (chosen).**
  - Below the bar: "single-case observation" (1 case), "this surgeon's practice" (≥ 2 cases, 1 surgeon).
  - Percentages are never shown below n = 20.
  - Option considered: ≥ 5 cases & ≥ 3 surgeons.

**Decided by:** Karthi (2 product choices) + Claude (delegated design, accepted) · 4 Oct 2026
