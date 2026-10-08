# v3 ch. 1.2–1.3: claim, proof levels, success metrics

- **Topic:** What the MVP must prove, at what level of evidence, and with which pass marks.

## Claim

> From a surgeon's own words anchored to their video, the system drafts evidence-linked decision cards that the surgeon approves in minutes. Approved knowledge accumulates into a department manual that juniors actually use to prepare for cases.

## Five provable parts

① **Captures**: finds the decisions that matter.
② **Proves**: every field linked to its source, nothing invented.
③ **Light**: little expert time per case.
④ **Trusted**: few edits, but not rubber-stamped (planted errors get caught).
⑤ **Used and grows**: juniors use it, peers rate it useful, and fewer questions are needed per case as the library grows.

## Proof levels

These replace v2's "✅ proven by tests" and "🎭 stand-in surgeon".

| Level | Data | What it proves | Pass marks |
|---|---|---|---|
| **L1 Engineering** 🧪 | Synthetic + human-read scripts | Pipeline correct and safe | evidence IDs valid 100% · quotes match 100% · numbers not in the source 0 · planted-PII leaks 0 · negation/counterfactual errors after verification 0 · speech-recognition error rate (CER) on debrief audio ≤ 10% (detail in ch. 13) |
| **L2 Archive pilot** 🩺 | 1 surgeon, 3–5 own archive cases | Feasible and light on real cases | expert time ≤ 30 min per case (hard ceiling 40) · ≥ 3 approved cards per case with the "why" filled · ≥ 59 source links checked with 0 wrong (shows ≥ 95% at 95% confidence) · PII leaks 0 · capture rate and blinded peer usefulness reported · 1 letter of intent |
| **L3 Department pilot** 🏥 | ≥ 2–3 surgeons, ~20 cases, juniors | The CEO's targets as rates | capture ≥ 80% · source links ≥ 95% · review median ≤ 10 min · "why" filled ≥ 80% · juniors' weekly use ≥ 60% · ≥ 2 letters of intent at a stated price |

## Measurement rules

- **Two answer keys, both built without seeing AI output:**
  - a source-grounded key (what was actually said), which judges the extractor;
  - a surgeon-intent key (the surgeon's priority decisions), which measures capture.

  Correct cards missing from a single surgeon's list are not counted as inventions; experts omit 34–77% of their decisions (RS2, RS6).
- **"0 attribution errors" is a process guarantee:** the surgeon confirms every attribution, and the audit records it. It cannot be shown by sampling (RS6 §2).
- **Capture rate (L2):** one 45-min deep reference interview (Critical Decision Method) on one archive case. Its decision points are the denominator. The literature expects ≈ 65% (RS2 §8.2).
- **Planted-error catch rate:** known errors are seeded into drafts to test whether review really checks (RS6 §1, RS2 §6.2).
- **Never an LLM as the only quality judge;** per-surgeon reporting; surgeons never averaged.
- **Options considered:**
  - a tighter L2 (≥ 5 cards per case, ≤ 40 min, juniors' use measured in L2), rejected because 3–5 cases can't support it and juniors' use needs a department;
  - no deep reference interview, rejected because there would be no defensible capture denominator.
- **Decided by:** Karthi (accepted Claude's picks) · 4 Oct 2026
