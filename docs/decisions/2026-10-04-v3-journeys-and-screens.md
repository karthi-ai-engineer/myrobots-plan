# v3 ch. 4: User journeys and screens

- **Topic:** How each user moves through the product, and which screens exist at each stage.
- **Inputs:**
  - RS2 §3.5 (question selection), §6.3–6.4 (review card), §7.3 (reading modes), §8.2–8.4 (debrief design);
  - the CEO's UX and design system;
  - RS7 §6 (positioning and naming).

## Journeys

- **J1 Narrate** (senior surgeon · desktop/iPad · ≈ 20–30 min per case):
  1. Task diagram, once per procedure (10–15 min). The surgeon ratifies the step template and priority steps, which seeds the dictionary.
  2. Skim the video at 4–8× and tap ★ on 4–8 moments.
  3. For each ★ (up to 5): the clip stops before the decision → open voice question → type tap → 1–2 template probes.
  4. Optional closer.
  5. Review the drafted cards (same session or later).
- **J2 Read** (juniors · phone/iPad · weekly): procedure → variant (side · approach · robot) → step → cards, with surgeons side by side → clip. S2 adds **Prepare** modes:
  - night before: predict-then-reveal clips;
  - day of: top pitfalls and recoveries for the chosen variant;
  - just before: ≤ 5 items, one screen.

  Modes change priority, never eligibility.
- **J3 Govern** (department clinical owner): setup (procedure pack · roles · explicit retention choice · consent records) → gates (privacy · release per purpose · review-due) → withdraw · audit.
- **J4 Phase 2** (V1): OR → optional 60-s closure flag → 1–2 h processing → same-day debrief-in-review. At most 3 voice questions and 5 minutes; this is "A′" from RS2 §8.4.

## Screens

| Screen | Stage |
|---|---|
| ① 語る Narrate | S0 |
| ② 承認 Review (phone-first review card) | S0 |
| ③ 手術ノート Living manual | S0 |
| ④ 症例 Case | S0 |
| ⑤ 管理 Admin | S0 |
| ⑥ 術前ブリーフ Prepare | S2 |

- **Review card:**
  - one card per screen;
  - 🗣 said / 🎥 seen / 🤖 structured marks, never mixed;
  - evidence one tap away;
  - uncertainty highlighted;
  - buttons: 確認して残す (confirm and keep) · 直す 🎙 (fix) · 残さない (don't keep).
- **Defaults:**
  - the CEO's design system;
  - Japanese by default, English toggle;
  - product name 「判断の記憶 / Surgical Memory」;
  - avoid 「レガシー」 in Japanese copy.

## Choices

- **Narration sessions:** facilitated first, then self-serve (chosen). A MY ROBOTS person sits in for the first 1–2 pilot cases (observe, fix UX, run the deep-reference interview). The product is built to be self-serve.

  Options considered: self-serve from case 1 · always facilitated.
- **Follow-up questions:** templates, with free LLM follow-ups only in a tested A/B (chosen).
  - A clinician-ratified question bank plus rules choose the questions.
  - The LLM only classifies answers and fills slots.
  - Free-form LLM follow-ups run only as an observed A/B experiment in the archive pilot. The reason is false-memory and leading-question risk (RS2 §4.4).

  Option considered: free LLM follow-ups in production.

**Decided by:** Karthi (accepted Claude's picks) · 4 Oct 2026
