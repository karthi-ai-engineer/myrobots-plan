# Paper note: Procedural Graphs (arXiv 2609.09153, 8 Sep 2026)

**Title:** Procedural Graphs: Self-Evolving Execution Structures for LLM Agents
**Authors:** Lu, Chen, Wu, Arık (Google, Georgia Tech, Peking U.)
**Read by:** Claude, 24 Sep 2026 · **Status:** 🅿️ **Parked for later versions** (Karthi, 24 Sep). Not part of the current MVP plan. The original PDF was removed; re-download from arXiv if needed.

## What it says (short)

- A **knowledge graph** stores facts as (entity, relation, entity) → answers *what is*.
- A **procedural graph (PG)** stores know-how as (procedure, relation, procedure) → answers *what to do next*.
- Edges carry 3 text fields: **condition** (when) · **guidance** (how) · **pitfalls** (what to avoid).
- Relations used: LEADS_TO · TRIGGERS · PROVIDES_INPUT_FOR · CONVERGES_TO. Graphs are small (7–17 nodes).
- **Online:** find the agent's current node → take its 2-hop neighbourhood → an LLM turns it into step advice. Local neighbourhood beats the full graph and beats plain top-k retrieval (keeps prerequisites connected).
- **Offline self-evolution:** an LLM compares failed vs successful runs → proposes graph edits → **commit only if a held-out validation score does not drop** → rejected edits kept as "don't try again" memory. **No model training.**
- Evidence: agent benchmarks (HotpotQA, ALFWorld, τ-bench, BFCL, GDPval, EnterpriseArena). Not medical. Gains vary (≈ 0 on HotpotQA, up to +9 points elsewhere).

## Fit for MY ROBOTS

| Where | Use | Fit |
|---|---|---|
| Knowledge core (B14/B16) | A **procedure graph for lap chole**: steps, states, obstacles, responses; edges with condition / guidance / pitfalls, **built only from approved cards** | ⭐ strong |
| Search + answers + briefs (B15/B17/B18) | "Localise + 2-hop neighbourhood" retrieval: the brief pulls the connected steps (e.g. CVS before clipping), not just similar snippets | ⭐ strong |
| Feedback Learner (B13) | Self-evolution discipline for **pipeline rules** (extraction prompts/rules): propose → score on tuning cases → commit or reject → rejection memory | ✅ good |
| Live guidance in the OR | ❌ not allowed (nothing shown to the operating surgeon; medical-device line) | ❌ |

## Adaptations required by our rules

| Paper does | Our rule | Adaptation |
|---|---|---|
| LLM auto-edits the graph when a score improves | ✅ AI proposes, surgeon approves | Self-evolution only for **pipeline rules**, never for medical knowledge. Medical edges come only from approved cards |
| One LLM-written "guidance" text per edge | ⚖️ Disagreement side by side · 📎 evidence | Edge attributes = **lists of cited items per surgeon** (episode IDs), not one merged text |
| No provenance on edges | 📎 No evidence → no field | Every edge links to episode IDs (or is labelled 🧠 general / guideline) |
| No counts | 🔢 Small numbers | Edges show "N of M cases ⚠ small sample" |
| Validates on 20–100 tasks | We have ~7 tuning cases | Gate on field-level scores (many fields), not case-level; expect noise |
| Steers the agent's next action | 🚫 Nothing in the OR · MHLW SaMD | Use only for pre-case briefs and answers; suggestions stay behind the admin switch |

## Is it new?

Partly. Surgical process models (SPM) are an established idea, and **OntoSPM** (already in our plan) is the *Ontology for Surgical Process Models*. What's new here: edges with condition/guidance/pitfalls, local-subgraph guidance for LLMs, and the validation-gated self-evolution loop.
