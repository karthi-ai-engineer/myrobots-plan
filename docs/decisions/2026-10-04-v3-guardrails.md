# v3 ch. 2: Guardrails (32 rules)

- **Topic:** The "never broken" rules for the merged product. They replace v2's 22 rules (`MASTER_PLAN.md` §2) and `CLAUDE.md` §3 once v3 is approved.
- **Sources:**
  - v2's 22 rules;
  - the CEO's non-negotiables in `myrobots-production`;
  - the medico-legal design rules (RS7 §6.3);
  - the review-UX principles (RS2 §5.6, §6.3);
  - evaluation and ethics (RS6);
  - the knowledge-core research (RS4).
- **Legend:** `=` kept from v2 · ✏️ changed · 🏢 from the CEO's app · 🆕 from research.

## The set

**A. Truth and evidence**
- 📎 No evidence → no field (=).
- 🗣 The "why" is the surgeon's own words: a quote plus light paraphrase, or *unknown*. The AI never infers a reason (🆕).
- ❓ Unknown allowed, guessing not (=).
- 🎯 A moment is a candidate (=).
- 🗣🎥 **Said + seen** replaces "two witnesses": a card is strong when the surgeon's words and a surgeon-confirmed video anchor agree. AI outputs never count as a witness (✏️).
- ⏱ One clock (=).

**B. Human control**
- ✅ The surgeon approves; the label shows who (✏️).
- 📚 Approved only (=).
- 🔏 Approved = frozen. Dictionary changes never re-point approved cards (✏️).
- 👀 Elicit before show. Pitfall, recovery and danger-zone cards need their evidence opened before approval, and cannot be batch-approved (🆕).
- ↩️ Withdraw any time: 「残さない」 (don't keep) on every answer; approved cards can be withdrawn, with history kept (🆕).
- 🔐 Separate gates: privacy clearance ≠ clinical approval ≠ purpose release. Release is per purpose and carries a review-due date (🏢).
- ⚖️ Never averaged. A pattern needs ≥ 3 cases and ≥ 2 surgeons and is a clinician-written card (✏️).

**C. Privacy and law**
- 🇯🇵 **Japan boundary:** real data never leaves Japan. The AI gateway allows only `jp.` / in-Region routes for real data, enforced by IAM/SCP. US-processed APIs are for synthetic data only (✏️).
- 🏥 Hospital boundary set per pilot, in writing (🆕).
- 🔒 Privacy filter before any AI call (=).
- 🎙 Only consenting voices. Archive narration records only the narrating surgeon. No voiceprints. OR-staff notice and consent come before any prospective capture (🆕).
- 🗑 Minimise. Unapproved drafts expire; cards contain no patient identifiers. **Raw narration/debrief audio retention: the hospital decides, with no product default.** Setup cannot be completed without an explicit retention choice (🆕).
- 📄 Not a medical record: never written to the EHR, no operative notes (🆕).
- 🏥 No real data before the checklist: ethics approval · consent · contract (joint research if we learn from the data) · security posture (✏️).
- 📜 Licence before use, for datasets **and** models (✏️).

**D. Safety and regulation**
- ⚕️ Education, not a device. No intra-op guidance, no patient-specific recommendations, no risk scores. Briefs stay at procedure level. PMDA 該当性相談 (device-classification consultation) before anything grey (✏️).
- 🚫 Nothing before 🏁 in prospective capture (=).
- 🫀 Danger zones are never auto-mapped at low confidence. Left/right anatomy is validated by ontology rules (✏️).
- ⚠ Serious-event cases are excluded by default and need medical-safety approval. The product never decides reportability (🏢).
- 🙅 No evaluation, ever. No ranking, scores or per-surgeon analytics for management. A non-evaluation clause goes in every contract (🏢).

**E. Honesty and engineering**
- 🏷 Honest labels: 🧪 synthetic · ✅ surgeon-approved · 🤖 AI-structured. Never fictional provenance or hard-coded results (✏️).
- 🔢 Small numbers, counts first (=).
- 🔒 Sealed envelope (=).
- 🔒 Original never touched (=).
- 🔑 Secrets only in secret stores (=).
- 🧪 No model training. Pilot data improves the product only under a contract that allows it (✏️).

## Dropped from v2

- "⚡ Unit = one obstacle episode": it is a design choice, not a rule, so it moves to ch. 5.
- The 🎭 stand-in label: it is no longer used as evidence.

## Options considered for raw audio

- Delete after 30 days (configurable).
- Keep.
- **Hospital decides, no default (chosen).**

**Decided by:** Karthi · 4 Oct 2026
