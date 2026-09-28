# NEXT STEPS

> Updated 24 Sep 2026. The plan is in `docs/MASTER_PLAN.md`; the current state is in `HANDOVER.md`.
> The earlier "Blueprint Part 2" tech-pick list is done: see `MASTER_PLAN.md` §5–8.

---

## A. Now: finish the plan (no code yet)

- [ ] **Block-by-block design** (`docs/blocks/README.md`): ✅ Ingest · ✅ AI gateway · ✅ See · 👂 Hear (B06 ✅ B07 ✅ → **B08 next**) · then Understand · Review · Know · Ask · Always on · Factory
- [ ] Karthi reviews `docs/CEO_Explanation.md` and shares it with the CEO
- [ ] Discuss **ch. 11 Roadmap & budget** (draft in `MASTER_PLAN.md` §11): build order, monthly OpenAI limit
- [ ] Apply the "few more changes" Karthi expects to the master plan
- [ ] Karthi's **final approval** → mark the plan ✅ and log it in `docs/decisions/`
- [ ] Answer open question: are the local company GPU machines available for this project?

## B. Asks running in parallel (CEO)

- [ ] OpenAI API key (company) + monthly spending limit
- [ ] OpenAI credits programme (check commercial-use and data terms)
- [ ] Surgery videos meeting the intake requirements (`MASTER_PLAN.md` §9.2) + license / permission
- [ ] Legal check: internal R&D use of licensed data
- [ ] AWS: credits · Bedrock access · GPU quota (ask early)

## C. After the final go: Gate 0a (dev start)

- [ ] Python 3.12 environment via uv
- [ ] PostgreSQL + pgvector in Docker Compose
- [ ] `.env` from `.env.example` with the OpenAI key (never committed)
- [ ] Pre-commit checks: ruff + gitleaks
- [ ] AI gateway stub + one safe test call (no patient data, no real names)
- [ ] Confirm current model names and prices at the providers

**Gate 0a is done when:** the laptop environment runs, the key works through the gateway, and nothing secret is in git.

## D. Then: Gate 1 (walking skeleton)

All 28 blocks exist with their plugs and **fake** outputs; one fake case flows end to end in live (slices) and archive mode; every output saved + stamped; Test Harness + Audit Log work; minimal screens (case list · debrief · archive review · ask). Order of the later gates depends on the ch. 11 discussion.
