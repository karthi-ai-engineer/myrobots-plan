# v3 ch. 6: Retrieval and answering

- **Topic:** How approved knowledge is found and served (manual, search, Prepare, and later ASK).
- **Inputs:** RS4 §5 (retrieval, embeddings, rerankers, GraphRAG evidence), §6 (aggregation); ch. 5 knowledge model.

## Clarification recorded

The knowledge graph **is** the core:
- the dictionary/ontology is the nodes;
- typed edges link cards to terms and to other cards.

What is excluded:
- **a separate graph database**, because the graph lives in Postgres;
- **"GraphRAG" frameworks** (LLM-written community summaries), because our graph is already curated and benchmarks show GraphRAG often underperforms plain RAG (RS4 §5.4).

## Retrieval pipeline: graph-first hybrid, built from S0

One pipeline serves search (S0), Prepare (S2) and ASK (V1).

1. **Entity linking:** the query text is matched against dictionary labels (pref/alt/slang/ASR variants), plus screen context (procedure, variant), giving term IDs.
2. **Graph expansion:** `is_a` / `part_of` roll-ups and same-situation neighbours. **`confusable_with` is never auto-expanded**; the user is asked "did you mean…?" instead (danger-zone safety).
3. **Three channels over approved cards only:**
   - 🕸 graph: cards linked to the term IDs, plus `recovers_from` / `follows` neighbours;
   - 🔤 lexical: Sudachi-tokenised keywords + pg_bigm partial match;
   - 🧠 dense: Japanese embeddings, either ruri-v3-310m self-hosted in Japan or Cohere Embed v4 (Bedrock Tokyo). Benchmark, then pick; one embedding provider per index.
4. **Fusion:** reciprocal rank fusion, graph-weighted, with filters: approved · released for the purpose · variant · not withdrawn · review-due ⚠.
5. **Rerank:** a Japanese cross-encoder (ruri-v3-reranker-310m candidate).
6. **Show:** results grouped by graph structure (step → type → surgeon columns) with clips. **Gaps** (no approved card for a step or term) are shown explicitly and feed the next narration's question selection.

**Measurement:**
- per-channel ablation;
- Recall@5 ≥ 0.9 and nDCG;
- V1 ASK: abstention accuracy ≥ 0.9 on unanswerable questions, and a citation for every sentence.

## Prepare brief (S2): two layers

- **Layer 1, cards (authoritative):** rules select and order approved cards and clips per mode (night before / day of / just before) and variant. Procedure-level only.
- **Layer 2, 🤖 summary (reading aid), generated only from the selected approved cards:**
  - one citation per sentence;
  - numbers inserted by code;
  - a citation checker drops unsupported sentences;
  - a cross-family verifier;
  - wording rules: procedure-level, attributed ("術者Aは…", "Surgeon A …"), no patient-specific advice.

  If any check fails, the cards are shown alone.
- **Summary approval:** summaries are cached per procedure × variant × mode × card-set version. A **clinical owner approves each version once** before juniors see it, and summaries are regenerated only when the card set changes. This keeps "approved only" true for everything juniors read.

## ASK (V1, after the PMDA consultation)

The same pipeline, then an answer generated from a snapshot of approved cards, with a citation per sentence, a checker, and abstention plus shown gaps when evidence is thin.

## Options considered

- Prepare: rules only · LLM summary only → **both layers (Karthi)**.
- Search: keyword + dictionary in S0, vectors at V1 (Claude's first pick) → **full graph-first hybrid from S0 (Karthi)**. This costs ≈ 1–2 extra weeks in S0 and gives per-channel evidence and a tuned stack by V1.
- Summary approval: checker only → **clinical owner approval per version**.

**Decided by:** Karthi · 4 Oct 2026
