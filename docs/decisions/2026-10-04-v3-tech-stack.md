# v3 ch. 10: Tech stack and dev setup

- **Topic:** Languages, frameworks, infrastructure, code repository and the developer environment on the two laptops.
- **Inputs:** RS5 §1–8 (platform research), ch. 8–9 decisions, and Karthi's hardware constraints.

## Hardware (fact, 4 Oct 2026)

| Laptop | Spec |
|---|---|
| **Main dev laptop** | Surface Laptop 7 · Snapdragon X Plus (ARM64) · 15.6 GB RAM · Windows 11 Home ARM64 · 375 GB free disk |
| **Heavy-jobs laptop** | ASUS Zenbook, Snapdragon (ARM64, per Karthi) · 48 GB RAM · Windows (to verify) |

The ASUS Zenbook is likely the same class of machine as the CEO's planned OR "ASUS PC", so it doubles as the test bench for the P-browser-edge and P-hospital-box fallbacks.

## Dev environment (Karthi's decision): native Windows on both laptops — no WSL, no Docker

- **Reason:** keep this 16 GB laptop light (RAM and disk headroom). Karthi declined WSL2.
- **Cross-platform rule:** every library must run on **both Windows-ARM64 and Linux-ARM64**, so the same code deploys unchanged to the AWS Tokyo Linux ARM64 servers at the pilot. CI runs on both `windows-11-arm` and `ubuntu-24.04-arm` runners to enforce this.
- **Library choices under the rule:**

| Need | Choice | Notes |
|---|---|---|
| ASR | sherpa-onnx (ReazonSpeech) + whisper.cpp (kotoba-whisper) | Native Win-ARM ✅ |
| Embeddings / reranker | ruri-v3 + reranker via ONNX Runtime | NPU via QNN as an option |
| Japanese tokeniser | Janome or Lindera, whichever passes the setup-day check | Replaces SudachiPy |
| Privacy NER | Rules + per-case name list (kana) + in-Japan LLM pass | Replaces GiNZA |
| Diarization | Not needed in the MVP (narration = 1 speaker) | pyannote deferred to V1 (decide its platform then) |
| Database | Postgres 18 + pgvector native on Windows ARM64 (verify on setup day) | **Fallback:** small RDS Postgres in the AWS *synthetic* account as the dev DB |
| Jobs | Procrastinate in dev on Windows (verify) | **Fallback:** a minimal runner using the same stage functions |

- Python 3.12 **native ARM64** builds (pinned; avoid uv's default x86-64 build on Windows). Node 24 ARM64; FFmpeg ARM64.
- Heavy jobs (large ASR models, local-LLM benchmarks, full-length audio) run directly on the ASUS Zenbook with the same native setup.
- **Not chosen:**
  - WSL2 on both laptops (Claude's first pick): RAM and disk concern.
  - Backend in AWS from day 1: needs internet and a monthly cost.

  Facts noted for the record: WSL2's 50% memory is a cap, not a reservation; its disk grows with use (≈ 10–15 GB for this stack).

## Stack (delegated picks, approved)

FastAPI · SQLAlchemy 2.1 + Alembic · psycopg 3 · Procrastinate + own run/stage tables · RDS PostgreSQL 18 (pgvector, ltree, pg_bigm, pgAudit) · S3 Tokyo (separate buckets per data class) · append-only audit + hash checkpoints to S3 Object Lock · Vite SPA + React 19 + the CEO's shadcn/Tailwind design · React Router · TanStack Query · i18next · Orval (client + mocks from OpenAPI) · oasdiff + Schemathesis contract gates · ECS Fargate ARM64 (ECS Express Mode for the API) · Terraform · GitHub Actions on ARM runners.

**Superseded RS5 rows:**
- Front door → ch. 9: direct to AWS Tokyo for real data.
- LLM → ch. 8: Opus 5.5 / Sonnet 4.6 `jp.`.

**Knock-on amendments:**
- ch. 6 "Sudachi-tokenised keywords" → the cross-platform tokeniser above.
- ch. 8 B08 "Japanese NER" → rules + name list + in-Japan LLM.

## Code repository (Karthi's decision)

- **A new private monorepo** for all product code: backend, pipeline, frontend, infra, fixtures. The same strict git rules apply (only karthi-ai-engineer, no assistant attribution), and the repo can later be transferred to a company org.
- `myrobots-plan` stays docs-only.
- *Option considered:* code inside `myrobots-plan`.

**Decided by:** Karthi (dev environment, code repo) + Claude (delegated stack, accepted) · 4 Oct 2026


> ⚠ **Amended 6 Oct 2026:** see `2026-10-06-v3-mvp-local-and-gateway.md`. The MVP runs on the two laptops only, and all AI goes through the gateway with switchable profiles (`dev-cloud` · `local` · `pilot-japan`).
