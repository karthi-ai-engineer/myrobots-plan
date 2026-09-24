# Knowledge core (ch. 4)

- **Topic:** How knowledge is modelled, stored, searched, counted and grown
- **Decided by:** Claude, delegated by Karthi ("for this rapid MVP, you decide"), 24 Sep 2026. Karthi may veto any point.
- **Decisions (full detail in `MASTER_PLAN.md` §4):**

| # | Decision | Alternatives considered | Why |
|---|---|---|---|
| 1 | **Approved card versions are the source of truth**; graph, patterns, indexes are derived and rebuildable | Graph as source of truth | Fits "approved = frozen", "approved only", per-AI-version indexes; corrections re-count automatically; safe rebuilds |
| 2 | **Four stores kept apart**: dictionary (rule book) · cases (diary) · general library (textbook) · derived graph | One mixed store | Stops single observations becoming "facts"; keeps literature cited but never counted |
| 3 | Dictionary: 9 kinds, 2 levels, 🇯🇵/🇬🇧 + slang synonyms, danger-zone + safe-name flags, relations is_a / part_of / confusable_with, pending zone approved weekly by the stand-in, ≈ 150 terms, seed + versions as git files | Full OntoSPM import; free vocabulary | Small, reviewable, enough for lap chole; confusable_with powers the danger-zone rule |
| 4 | **Graph inside Postgres** (no graph database in the MVP) | Neo4j or another graph DB | ~10 cases, simple walks (roll-up, neighbours); one less service |
| 5 | **IDs-first hybrid retrieval** (IDs + graph walk, meaning, Japanese-capable keyword, rank fusion) | Plain vector RAG | Language-independent, exact counts, fewer misses |
| 6 | **The LLM writes words, the database writes numbers** | LLM summarises counts | No invented statistics |
| 7 | Citation checker removes unsupported 📚 sentences; answers stored with a knowledge snapshot | Trust the writer | Hard rule "0 uncited"; reproducible answers |
| 8 | **Patterns computed by rules only**, recomputed on every approval | LLM-written patterns | Deterministic, testable against answer keys |
| 9 | **Growth replay** (cases added 1 → 10, question set re-run) as proof of claim ⑤ and investor demo | Only a final snapshot | Shows "grows with every case" directly |
| 10 | Feedback learner: examples + rules from repeated fixes (≥ 3), adopted only if tuning scores don't drop; learn forward only | Fine-tuning (not allowed) | No training; safe |

- **To verify in the research pack:** the severity scale basis (e.g. ClassIntra) · eponym safe list completeness.
