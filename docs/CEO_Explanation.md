# MY ROBOTS · Surgical Knowledge MVP
## The plan, explained for the CEO

> **Version 0.9 · 24 September 2026 · Prepared by Karthi (Engineering)**
> Status: the plan is **almost final**. A few points are still open and are marked **🟡 open**.
> ⏱ **Part A takes 5 minutes.** Parts B–D go deeper. Lines starting with **▶** can be clicked open for more detail. You can skip them.

---

### Contents

| Part | Sections | Read time |
|---|---|---|
| **A · The 5-minute version** | [A1 In one sentence](#a1-in-one-sentence) · [A2 The idea in one picture](#a2-the-idea-in-one-picture) · [A3 What we produce: the episode card](#a3-what-we-produce-the-episode-card) · [A4 What the MVP will prove](#a4-what-the-mvp-will-prove) · [A5 What we need from you](#a5-what-we-need-from-you) | 5 min |
| **B · How it works** | [B1 A day with the system](#b1-a-day-with-the-system) · [B2 How the data flows](#b2-how-the-data-flows) · [B3 How often and how fast](#b3-how-often-and-how-fast) · [B4 Features we will build](#b4-features-we-will-build) · [B5 What the screens look like](#b5-what-the-screens-look-like) | 8 min |
| **C · Technology** | [C1 Environments in the MVP](#c1-environments-in-the-mvp) · [C2 AI models](#c2-ai-models) · [C3 Accounts and API keys](#c3-accounts-and-api-keys) · [C4 Later: real hospital use](#c4-later-real-hospital-use) | 6 min |
| **D · Trust, testing, next steps** | [D1 Rules we never break](#d1-rules-we-never-break) · [D2 How we test](#d2-how-we-test) · [D3 Build stages](#d3-build-stages) · [D4 What changed since the earlier plan](#d4-what-changed-since-the-earlier-plan) · [D5 Main risks](#d5-main-risks) · [D6 Full list of asks](#d6-full-list-of-asks) | 5 min |
| **Glossary** | [Simple meanings of the terms used](#glossary) | as needed |

---

# Part A · The 5-minute version

## A1. In one sentence

> Operating-room video shows **what** the surgeon did. We also capture **why**, from what the team says during surgery and from a **2-minute check-in with the surgeon right after**. Important moments become **surgeon-approved knowledge** that the next surgeon can search and use to prepare.

## A2. The idea in one picture

```
      TODAY                                   WITH MY ROBOTS
      ─────                                   ──────────────
   🎥 surgery is recorded                  🎥 video + 🎙 OR voice, on one timeline
          │                                        │
          ▼                                        ▼
   📁 video is stored,                     🤖 AI finds the difficult moments and
      almost never watched again               drafts short "episode cards"
          │                                        │
          ▼                                        ▼
   🧠 the know-how stays in                👨‍⚕️ the surgeon approves them in ~2 minutes
      the surgeon's head                       right after the operation
                                                   │
                                                   ▼
                                           📚 the hospital's knowledge grows
                                              with every case
                                                   │
                                                   ▼
                                           📋 the next surgeon gets answers and a
                                              pre-case brief, with video proof
```

**The CEO's 4 requirements, and how the plan meets them:**

| Your requirement | How the plan meets it |
|---|---|
| 1. Pull surgical knowledge out **automatically** | AI watches, listens and drafts cards by itself |
| 2. The surgeon has to **fix very little** | A 2–3 minute check-in, one tap per card, at most 3 short questions |
| 3. Knowledge **piles up per procedure** | Every approved card joins the knowledge base for that procedure |
| 4. It gets **more valuable with every case** | Patterns, answers and briefs improve as cases are added, and we show this live ("growth replay") |

## A3. What we produce: the episode card

The basic unit of knowledge is **not a video**. It's **one difficult moment and how it was handled**.

```
 ⚡ EPISODE CARD · Case 07 · 23:38 → 24:50 · ✅ approved by the operating surgeon
 ─────────────────────────────────────────────────────────────────────────────────
 Obstacle       Bleeding from a small artery branch
 Step           Dissection of Calot's triangle (a key step of gallbladder removal)
 Warning sign   Fat was hiding the structures ................. 📎 voice, 23:31
 Response       ① gauze pressure → ② suction → ③ clip ......... 📎 video, 23:40–24:45
 Outcome        Worked
 Why            "Clip close to the gallbladder, away from the bile duct"
                ............................................... 📎 surgeon's debrief answer
 Evidence       👁 video + 👂 voice agree → strong
 Severity       Moderate (AI proposed → surgeon confirmed)
```

- **📎 Every line has proof.** One click jumps to the exact second of video or the exact sentence.
- **No proof, no line.** If nothing was said or seen, the card says "unknown". The AI is not allowed to guess.
- Cards from many cases combine into **pattern cards**, for example: *"Bleeding in Calot's triangle: 3 of 7 cases (43%) ⚠ small sample. Pressure then clip worked in 2 of 3."*

## A4. What the MVP will prove

**The claim:** *While a surgery runs, the system turns video + OR speech into evidence-backed cards. They're ready within minutes of the end, need few corrections, and add up into knowledge that grows with every case. The same system also handles old archive videos.*

| # | Promise | In plain words | MVP target |
|---|---|---|---|
| ① | **Finds** | Finds the real difficult moments and invents none | ≥ 80% found · ≤ 1 invented across all routine cases |
| ② | **Proves** | Every line on a card links to its proof | 100% |
| ③ | **Fast** | Cards ready soon after the surgery ends | ≤ 5 minutes |
| ④ | **Little work** | Most cards approved as they are; the check-in is short | ≥ 70% unchanged · ≤ 3 min · ≤ 3 questions |
| ⑤ | **Grows** | Every new case makes answers and briefs better | Pattern numbers 100% correct · coverage rises case by case |
| 🔴 | **Zero tolerance** | Leaked names, unproven claims, unapproved knowledge, risky anatomy mapped without review | **0**, always |

**We are honest about what an MVP can prove:**

| ✅ Proven in the MVP | 🎭 Shown in the MVP | 🔜 Proven later, in a hospital pilot |
|---|---|---|
| The 5 promises, measured automatically against exact answer keys | The surgeon's experience (debrief, answers, briefs), played by Karthi as a **"stand-in surgeon"** using published surgical guidelines | That real OR teams say the "why" out loud · real surgeons' time · medical correctness checked by clinicians |

> **Why a stand-in?** No surgeon time is available now, so the MVP uses **simulated surgeries**: real surgery videos + realistic OR conversations written by AI and spoken by synthetic voices. Every demo says this openly, and every such card carries a 🧪 / 🎭 label.

## A5. What we need from you

| # | Ask | Needed |
|---|---|---|
| 1 | **OpenAI API key** on the company account, with a monthly spending limit | To start building |
| 2 | Apply to the **OpenAI credits programme** | Before heavy testing |
| 3 | **Surgery videos** (any source; requirements in [D6](#d6-full-list-of-asks)) + the right **license / permission** | Before the test-case stage |
| 4 | **AWS:** credits + Bedrock access + **GPU quota** (the GPU quota is slow to approve, so ask early) | Later stages |
| 5 | A quick **legal check** on using licensed data inside the company | Before the test-case stage |

➡ Full list with details: [D6](#d6-full-list-of-asks).

---

# Part B · How it works

## B1. A day with the system

```
 🕗 08:00  Surgery starts. Camera + microphone feed the system.
           It records, watches and listens.  🚫 NOTHING is shown in the operating room.
 🕗 08:40  Bleeding at Calot's triangle. About 2–3 minutes later, a draft card exists (hidden).
 🕘 09:10  🏁 Surgery ends.
 🕘 09:14  📱 "Case 07: 3 cards ready, about 2 minutes."
 🕘 09:15  The surgeon opens the phone between cases:
             ✅ approves 2 cards · ✏️ fixes one field on the 3rd (picked from a list)
             🎙 answers one question by voice: "Why did you clip there?" (15 seconds)
 🕘 09:17  Done. Cards become knowledge; patterns update within seconds.
 📅 Next week, a younger surgeon has a difficult case tomorrow:
             💬 asks "What do we do when Calot's triangle bleeds?"
             📋 prints a one-page pre-case brief with video clips of our own cases.
```

**The other use case, equally important:**

```
 🗄 ARCHIVE   The hospital's old videos (usually no audio) → uploaded in batches
              → the system drafts "video-only" cards → a reviewer assigned by the hospital
              checks them at any time → labelled 👁 reviewer-checked
```

**Why the check-in happens right after surgery:** surgeons go straight to the next case, and their memory of *why* fades quickly. So the system must have everything ready **within minutes** of the end.

## B2. How the data flows

```
 ① CAPTURE      🎥 video + 🎙 OR audio → saved first; the original is never changed
        ▼
 ② PREPARE      1 picture per second · audio cut into 30-second pieces · one shared clock
        ▼
 ③ SEE          👁 AI looks at the pictures: which step? which tools? is there bleeding?
    HEAR        👂 speech → text → who spoke → 🔒 PRIVACY FILTER (names removed)
                                                  ▲ nothing reaches any cloud AI before this
        ▼
 ④ UNDERSTAND   ⏱ find "moments" (fixed rules, no AI)
                ✍️ AI drafts an episode card, with a proof link for every line
                📖 words matched to our medical dictionary (Japanese + English)
        ▼   🚫 kept hidden until the surgery ends
 ⑤ APPROVE      📱 surgeon's 2–3 minute debrief: ✅ approve · ✏️ fix · ❌ reject · 🎙 "why?"
        ▼
 ⑥ KNOWLEDGE    📚 approved cards only → 📊 patterns · 🔍 search · 💬 answers · 📋 pre-case briefs
```

| Station | What happens | Result |
|---|---|---|
| ① Capture | Video + audio arrive in small pieces and are saved immediately | A safe, untouched original |
| ② Prepare | Pictures and audio pieces are cut on one shared clock | Everything lines up to the second |
| ③ See + Hear | AI recognises steps, tools and bleeding; speech becomes text; names are removed | "Video facts" + clean transcript lines |
| ④ Understand | Rules spot moments; AI drafts cards with proof; words mapped to medical terms | Draft cards |
| ⑤ Approve | The operating surgeon (or the archive reviewer) confirms | Approved cards |
| ⑥ Knowledge | Approved cards feed patterns, search, answers and briefs | Knowledge that grows |

**Two safety locks built into the software itself (not just rules on paper):**
- 🔒 **Privacy lock:** the software **refuses** to send any text to a cloud AI unless the privacy filter has cleared it.
- 🚫 **OR lock:** drafts are **invisible** to all clinical users until the end-of-surgery signal. This keeps us clearly outside "medical device during surgery" territory.

<details>
<summary>▶ For the curious: the 28 building blocks, grouped into 9 lanes</summary>

Each block does one job with a fixed input and output, so any block can be improved or replaced without touching the others.

| Lane | Blocks |
|---|---|
| 🏭 Test factory | Script writer · Voice maker · OR sound mixer · Live simulator |
| 📥 Intake | Case intake (incl. end-of-surgery signal) · Media preparation |
| 👁 See | Eye (steps + tools + actions) · Bleeding check |
| 👂 Hear | Speech-to-text · Who spoke · Privacy filter |
| 🧠 Understand | Moment finder · Card drafter · Dictionary matcher |
| ✅ Review | Debrief builder · Review desk · Learning from corrections |
| 📚 Know | Knowledge graph · Search index · Pattern builder · Guideline library |
| 💬 Ask | Answer engine · Pre-case brief · Growth dashboard |
| 🛡 Always on | Login & roles · Audit log · Dictionary manager · Test harness |

</details>

## B3. How often and how fast

**For a 60-minute surgery in live mode:**

```
 0 min ─────────────────────────────── 60 min 🏁 ──────── +5 min ─────── +8 min
 │ every 10 s:  10 pictures → AI                │ last pieces │ 📱 debrief   │ next case
 │ every ~25 s: 30-s audio piece → text         │ + light     │   2–3 min    │
 │ ~2–3 min after each event: draft card        │   final     │              │
 │ (all hidden from clinical users)             │   check     │              │
```

| What | How often / how fast |
|---|---|
| Pictures taken from the video | **1 per second** |
| Pictures sent to the AI | in groups of **10, every 10 seconds** |
| Audio processed | in **30-second pieces** with 5 seconds of overlap, so no word is cut in half |
| A "moment" is closed | after **60 seconds of quiet** (at most 5 minutes) |
| Draft card ready | about **2–3 minutes** after the event (hidden until the end) |
| Surgery ends → debrief ready | **≤ 5 minutes** (target) |
| Surgeon's debrief | **≤ 3 minutes**, **≤ 3 questions** |
| Approval → knowledge updated | **seconds** |
| Test replays | at **10× speed**: a 60-minute case replays in about 6 minutes |

**Workload per 60-minute case (approximate counts):**

| Item | Count |
|---|---|
| Pictures | 3,600 |
| Picture requests to the AI (10 pictures each) | ~360 |
| Audio pieces | ~140 |
| Bleeding checks (only when there's a clue) | ~10–40 |
| Draft cards attempted | ~20–40 moments (most become "nothing here"; a few become real cards) |

> **Why "live" matters:** if we waited until the end, copying the video and processing it would take roughly **15–45 minutes** (estimate). By then the surgeon is already in the next case.

## B4. Features we will build

| For whom | Feature | What it does | MVP |
|---|---|---|---|
| 👨‍⚕️ **Operating surgeon** | Post-op debrief | Phone-friendly page; 2–3 minutes; one tap per card | ✅ |
| | Voice answers | Answers "why?" by speaking for ~15–30 seconds | ✅ |
| | Approve / fix / reject | Fixes are picked from lists; one-tap reject reasons | ✅ |
| 🧑‍⚕️ **Residents & colleagues** | Ask a question | Answers built only from approved cards; every sentence linked to proof; 3 clearly labelled layers | ✅ |
| | Pre-case brief | Pick the expected problems → what happened before, what worked, what failed, gaps, guideline reminders, video clips | ✅ |
| | Browse episodes | Cards with short video clips | ✅ |
| 🗄 **Hospital archive** | Archive upload + review | Old videos → video-only drafts → a reviewer checks them | ✅ |
| 🏥 **Management / admin** | Growth dashboard | Cases, approved cards, coverage, how much surgeons had to fix (trend), review time | ✅ |
| | Growth replay | Shows knowledge growing case by case (1 → 10) | ✅ |
| | Users & roles | Surgeon · resident · reviewer · admin · tester | ✅ |
| | Audit log | Who did what, and when | ✅ |
| | Suggestion switch | Turns AI "suggestions" on or off (off for clinical use until a regulatory check) | ✅ |
| ⚙️ **Behind the scenes** | Medical dictionary | ~150 terms in Japanese + English, with danger-zone anatomy flagged | ✅ |
| | Privacy filter | Removes names before any cloud AI | ✅ |
| | Live simulator | Plays a recorded video as if it were a live surgery | ✅ |
| | Test-case factory | Makes realistic OR conversations with synthetic voices, plus sealed answer keys | ✅ |
| | Test harness | Scores every run automatically | ✅ |
| | Two AI versions | OpenAI now, AWS Bedrock later; switched with one setting | ✅ (Bedrock later) |

**Not in the MVP (planned for later):** phone app · real OR capture box · hospital sign-in · more procedures (e.g. robotic surgery) · sharing across hospitals (with consent) · our own trained AI models · official medical codes (SNOMED) · predictions from patient data · teaching-tip cards.

## B5. What the screens look like

*Sketches only; the real design comes later.*

**📱 Post-op debrief (phone):**

```
 ┌────────────────────────────────┐
 │ Debrief · Case 07 · 3 cards    │
 │ ⏱ about 2 minutes              │
 ├────────────────────────────────┤
 │ 1 / 3   ⚡ Bleeding             │
 │ Calot's triangle · 23:38       │
 │ ▶ [ 20-second video clip ]     │
 │ Response: pressure → clip      │
 │ Outcome:  worked               │
 │                                │
 │ ❓ Why did you clip there?     │
 │    [ 🎙 hold to answer ]        │
 │                                │
 │ [✅ Approve] [✏️ Fix] [❌ Reject] │
 └────────────────────────────────┘
```

**💬 Answer to a question:**

```
 Q: "What do we do when Calot's triangle bleeds?"

 ⚠ Gaps first: none of our cases shows bleeding after clipping.

 📚 OUR CASES (approved cards only)
   • Bleeding during Calot's triangle dissection: 3 of 7 cases (43%) ⚠ small sample  [E07-1][E02-3][E09-1]
   • Surgeon A: pressure → clip, worked 2 of 2.  Surgeon B: bipolar, worked 1 of 1.
     (shown side by side, never averaged)                                            [E07-1][E09-1][E02-3]

 🧠 GENERAL KNOWLEDGE (from published guidelines, labelled)
   • Reach the "critical view of safety" before clipping.                            [Tokyo Guidelines 2018]

 💡 SUGGESTION: switched off
 ⭐ Rate this answer
```

**📊 Growth replay (sketch of the idea, not a result):**

```
 Cases in the knowledge base →   1    2    3    4    5    6    7    8    9   10
 Questions answered with our     ▁    ▂    ▂    ▃    ▄    ▅    ▅    ▆    ▇    ▇
 own cases (coverage)
```

---

# Part C · Technology

## C1. Environments in the MVP

```
 ┌─────────────────── 💻 Development laptop (Windows) ────────────────────┐
 │  MY ROBOTS app (Python)          PostgreSQL database (in Docker)        │
 │  file storage (local disk)       live simulator · test-case factory     │
 └───────────┬──────────────────────────────────┬──────────────────────────┘
             │ privacy-filtered text + pictures  │ code + plan documents
             ▼                                   ▼
     ☁️ OpenAI API (Version O, now)       🐙 GitHub (engineering account)
     ☁️ AWS Bedrock, Tokyo (Version B, later)
     🖥 AWS GPU server, Tokyo (later, only when needed)
     📦 AWS S3 storage, Tokyo (later)
```

| Environment | Used for | When |
|---|---|---|
| 💻 Development laptop | The whole app, database, internal demos | Now |
| ☁️ OpenAI API | AI for Version O | Now |
| 🐙 GitHub | Code and plan documents | Now (set up) |
| ☁️ AWS Bedrock (Tokyo) | AI for Version B | When AWS is ready |
| 🖥 AWS GPU (Tokyo) | In-house speech-to-text and "who spoke" | When needed (required before real data) |
| 📦 AWS S3 (Tokyo) | File storage | When AWS is ready |
| 🌐 Small cloud server | External demos (investors, hospitals) | Only after the data license is cleared |
| 🏥 Hospital box | Running inside a hospital | Later (pilot) |

**How "live" is shown in the MVP:** a **live simulator** plays a recorded surgery at normal speed and sends it to the system in 2–10-second pieces, exactly like a real OR camera would. For the hospital, we replace the simulator with a capture box, and nothing else changes.

## C2. AI models

**Two complete versions, one switch:**

```
 🟠 VERSION O  →  every cloud-AI job on OpenAI          (development now)
 🟢 VERSION B  →  every cloud-AI job on AWS Bedrock     (AWS credits; AI stays in Tokyo)
       Same app, same results format, one setting to switch. We compare them on the same cases.
```

| Job | 🟠 Version O (now) | 🟢 Version B (later) |
|---|---|---|
| Looking at pictures (step, tools, actions) | GPT-5.4 mini | Claude Haiku 4.5 |
| Bleeding check on short clips | GPT-5.4 mini | Claude Haiku 4.5 |
| Drafting episode cards | GPT-5.4 (GPT-5.5 for the final check) | Claude Sonnet 5 |
| Matching words to the medical dictionary | GPT-5.4 mini (as a judge, after rules) | Claude Haiku 4.5 |
| Search by meaning ("embeddings") | OpenAI text-embedding-3 | Cohere Embed Multilingual or Amazon Titan (chosen by a Japanese test) |
| Writing answers and briefs + a separate checker | GPT-5.4 + GPT-5.4 mini | Claude Sonnet 5 + Claude Haiku 4.5 |
| Speech → text (Japanese) | MVP: OpenAI transcription service (test data only) → then **Whisper large-v3** (open model) on our own GPU | same (shared) |
| Who spoke | **pyannote** (open model) on our own GPU | same (shared) |

**What uses NO AI, on purpose** (fixed rules are safer and can be tested exactly): 🔒 the privacy filter (local rules + an open-source Japanese language tool, GiNZA) · ⏱ finding moments · 📊 counting patterns · 📱 choosing debrief questions · every number shown to users.

**Test-case factory:** the OR conversations are written with **Claude** (our subscription) and spoken by **VOICEVOX** (free Japanese voice software). The factory's AI is a different family from the Version O AI, so the tests stay fair.

- 🧪 **No model training.** We only use ready-made models.
- Model names are confirmed at setup, because providers update their line-ups often.

## C3. Accounts and API keys

*An API key is a password that lets our software use a paid service.*

| Service | What it's for | Key / account | Owner | Needed |
|---|---|---|---|---|
| **OpenAI API** | Version O AI | API key on the **company** account, with a monthly spending limit | Company | To start |
| **OpenAI credits programme** | Pays for Version O use | Credits on the same account | Company | Before heavy testing |
| **AWS** | Bedrock (Version B), GPU, S3 storage, later servers | Company AWS account + credits; GPU quota request | Company | Later stages |
| **GitHub** | Code + documents | Engineering account (karthi-ai-engineer) | Karthi | ✅ Set up |
| **Claude subscription** | Development assistant; writes the test conversations | Subscription login (**no API key**) | Karthi | Now |
| **VOICEVOX** | Japanese synthetic voices | **None** (free software; a credit line is shown) | — | Test-case stage |
| Hospital sign-in | User login | Set up per hospital | Hospital | Pilot |

**How keys are protected:**
- 🔑 Keys live **only** in a private settings file on the machine (never in code, chat or e-mail), and later in AWS Secrets Manager
- 🛡 An automatic check blocks any key from being uploaded to GitHub
- 💳 Spending limits are set in each provider's dashboard
- 🔄 If a key ever leaks, it's replaced immediately
- 👥 Separate keys for development and for the hospital (later)

## C4. Later: real hospital use

**What changes when we move from the MVP to a real hospital:**

| | MVP (now) | Real hospital (later) |
|---|---|---|
| **Input** | Recorded video + synthetic voices, played "live" by the simulator | OR camera + microphone → **capture box** (video splitter + capture card, approved by hospital engineers) |
| **Privacy** | Names removed from text | + **video blur** (on-screen text) + names muted in audio playback |
| **Speech → text** | OpenAI service (test data only) | **Our own GPU** (AWS Tokyo or a hospital box). No cloud speech service |
| **AI** | OpenAI (Version O) | **Bedrock in Tokyo (Version B)**, or whatever the hospital approves |
| **App + database** | Laptop | AWS Tokyo servers, or a hospital box |
| **Storage** | Laptop disk | S3 Tokyo (encrypted), or hospital storage |
| **Users** | Stand-in surgeon (Karthi) | Real surgeons, residents, archive reviewers |
| **Login** | Simple accounts | Hospital or company sign-in |
| **Screens** | Phone-friendly web pages | + phone app |

**Three possible set-ups, to decide together with the hospital:**

```
 A ☁️ CLOUD (AWS Tokyo)       B 🏥 HOSPITAL BOX             C 🔀 HYBRID
 ┌──────────┐                 ┌──────────────────────┐      ┌──────────────────┐
 │ Hospital │── encrypted ──▶ │ Hospital             │      │ Hospital          │
 │ capture  │   video         │ capture + ALL        │      │ capture + privacy │
 └──────────┘      │          │ processing + storage │      │ + speech-to-text  │
                   ▼          └──────────────────────┘      └────────┬─────────┘
           ┌────────────────┐                          filtered text │ + pictures
           │ AWS Tokyo:     │                                        ▼
           │ everything     │                              ┌──────────────────┐
           └────────────────┘                              │ AWS Tokyo: AI +  │
                                                           │ knowledge + app  │
                                                           └──────────────────┘
 ✅ fastest, easy updates      ✅ strictest privacy          ✅ balance of both
 ⚠ data leaves the hospital   ⚠ hardware on site            ⚠ two places to run
```

**Our checklist before ANY real patient data:**

- [ ] Video blur built and tested
- [ ] Consent from staff and patients
- [ ] Runs inside the hospital, or in an approved secure set-up
- [ ] Names muted in audio playback
- [ ] No cloud speech service (speech-to-text on our own GPU)
- [ ] Regulatory check on AI suggestions and general knowledge (Japan's medical-software rules, MHLW SaMD)
- [ ] Raw transcripts deleted after scoring, or kept inside the hospital
- [ ] Ethics / consent check for archive videos

---

# Part D · Trust, testing and next steps

## D1. Rules we never break

| Rule | Meaning |
|---|---|
| ✅ **A human approves** | AI only proposes. Nothing becomes knowledge without a person's approval, and the label shows who: ✅ surgeon · 👁 archive reviewer · 🎭 stand-in (MVP) |
| 📎 **No proof, no line** | Every line of a card points to a video second, a sentence, a debrief answer or a cited guideline |
| ❓ **Unknown is allowed, guessing is not** | If nothing was said or seen, the card says "unknown" |
| 🔒 **Privacy before cloud** | Names are removed before anything goes to any cloud AI (the software enforces it) |
| 🚫 **Nothing during surgery** | Drafts stay hidden until the surgery ends |
| 🫀 **Danger zones** | Easily confused anatomy (e.g. cystic duct vs common bile duct) always goes to a human |
| 🔢 **Honest numbers** | "3 of 7 cases (43%) ⚠ small sample": counts first, always |
| ⚖️ **Surgeons are never averaged** | Different approaches are shown side by side |
| 🔏 **Approved means frozen** | Nothing silently changes an approved card; changes create a new version |
| 🏷 **Honest labels** | 🧪 synthetic · 🎭 stand-in · 👁 reviewer-checked · 🧠 general knowledge, on every screen and demo |
| 📜 **License before use** | Data is used only within its license |
| 🧪 **No model training** | Only ready-made AI models |

## D2. How we test

```
 13 CASES
 ├── 🔧 3 development cases   → for building; everyone may look
 ├── 🎯 7 practice cases      → for tuning; scored automatically
 └── 🔒 3 final-exam cases    → nobody sees the answers until the final exam; run once
```

- **Answer keys:** because we build the test cases, we know exactly what happened in each one: every difficult moment, every line spoken, every planted fake name. The answer keys are **sealed**, so only the test harness can read them.
- **Automatic scoring:** after every change, the test harness re-scores the cases and reports counts first ("4 of 5 (80%) ⚠ small sample").
- **Simulated reviewer:** the answer key plays a "perfect surgeon", so we can measure how much a surgeon would have to fix.
- **Blind stand-in:** Karthi does the debriefs on the final-exam cases **without** ever seeing their answer keys.
- **Fair tests:** the AI that writes the test conversations is from a different AI family than the AI that reads them, and the conversations include traps (vague lines, off-topic chatter, words spoken too early or late, a fake patient name).

## D3. Build stages

> 🟡 **Open, for discussion.** The order and timing below are a draft. We measure progress by **checklists, not dates**.

```
 0a  Dev start          laptop set-up · repository · OpenAI key · first safe AI call
 1   Skeleton           all 28 blocks exist with pretend data · one pretend case flows end to end
 2   Knowledge side     dictionary · guideline library · patterns · search · answers · briefs · screens
                        (needs no video, so we're never idle while the data question is settled)
     ── videos arrive ──▶ first real case end to end, rough
 3   Test factory       13 cases with conversations, voices, noise and sealed answer keys
 4   Senses             real picture AI, speech-to-text, who-spoke, privacy · works live
 5   Brain              real moments, cards, debrief builder, learning from corrections
 6   Final exam + demo  3 locked cases · all rules and targets hold · CEO demo
 In parallel:           AWS set-up → Version B
```

Each finished stage = a short update to you.

## D4. What changed since the earlier plan

| Topic | Earlier plan | Now | Why |
|---|---|---|---|
| Real surgeon narration | A surgeon explains 10 videos aloud | ❌ Dropped | No surgeon time available |
| Voice in the MVP | Mix of real + synthetic | **All synthetic** (unless real OR audio arrives with the data) | Follows from the above |
| Surgeon role in the MVP | Real surgeon | **Stand-in surgeon (Karthi)** using published guidelines, clearly labelled | No clinician available |
| After-surgery review | Review desk | **2–3 minute phone debrief** right after surgery, with voice answers | Surgeons have no time later; memory fades |
| Archive videos | Index only | **Equal priority**; reviewer-checked cards | Hospitals have large archives |
| Languages | Japanese and English extracted separately, then merged | **Japanese voice**; cards shown in Japanese + English | Simpler; target hospital is Japanese |
| Research video model | Planned | Parked | Trained on the same public videos + license limits |
| Test-case factory | Mixed tools | **Claude + VOICEVOX** | Fair tests without new accounts |
| Data source | Public datasets | **Open** (public, real clinical or other) | Being handled as a separate process |
| Building blocks | 27 | **28** | Added debrief builder + guideline library; merged two picture blocks |
| AWS Bedrock (Version B) | Parallel | Built when AWS is ready, **off the critical path** | Don't wait on AWS |

## D5. Main risks

| Risk | What we do about it |
|---|---|
| 🔴 **Data source unknown or late** | The plan works with any data; we build the parts that need no video first |
| 🔴 **Public datasets are "non-commercial"** (Cholec80 / CholecT50 use CC BY-NC-SA 4.0) | Internal use only until cleared; external demos wait for permission or a legal opinion |
| 🔴 **All MVP voice is synthetic** | We say it openly; real speech is proven in the hospital pilot |
| 🔴 **No clinician** | Every medical statement cites a published guideline; everything is labelled |
| 🟡 **Real data brings stricter rules** | The before-real-data checklist; AI in Tokyo (Bedrock) or a hospital box |
| 🟡 **Live timing** (≤ 5 min after surgery) | Parallel processing, small fast models; tested at real speed |
| 🟡 **Only two people** | Pretend-data skeleton first, clear list of things parked, scope cut in writing if needed |
| 🟡 **AWS set-up is slow** | Version B isn't on the critical path; ask for the GPU quota early |

## D6. Full list of asks

| # | Ask | Details | Needed |
|---|---|---|---|
| 1 | **OpenAI API key** | Company account · monthly spending limit set in the dashboard | To start |
| 2 | **OpenAI credits programme** | Apply; check the terms for commercial use and data use | Before heavy testing |
| 3 | **Surgery videos** | Laparoscopic gallbladder removal, full cases, **≥ 13 cases**, common video format, no patient identifiers visible (or blurred first); labels and case dates are a bonus | Before the test-case stage |
| 4 | **License / permission** | If public datasets: the owners' (CAMMA, Strasbourg) permission for company use, or a legal opinion. If hospital videos: a data agreement + the before-real-data checklist | Before external demos |
| 5 | **Legal check** | Using licensed data inside the company for R&D | Before the test-case stage |
| 6 | **AWS** | Credits · Bedrock access · **GPU quota** (slow to approve) · S3 | Later stages |
| 7 | **Regulatory specialist** | Japan's medical-software rules (MHLW SaMD) for AI suggestions | Before any hospital use |
| 8 | **Hospital partner** | For the pilot: real surgeons, real OR audio, real data under agreement | After the MVP |

**Decisions we ask you to confirm:**
- ✅ The plan's direction (Parts A–D)
- ✅ External demos wait until the data license is cleared
- ✅ The MVP is honest about being simulated (✅ proven · 🎭 shown · 🔜 pilot)

---

# Glossary

| Term | Simple meaning |
|---|---|
| **Episode card** | One difficult moment in a surgery, with what was done, why, and proof |
| **Obstacle** | The difficulty itself, e.g. bleeding, dense adhesions, unclear anatomy |
| **Moment** | A stretch of time that *might* contain an obstacle; the AI may decide "nothing here" |
| **Pattern card** | Many episode cards combined: how often, what worked, who did what |
| **Pre-case brief** | One page to prepare for a surgery: what to watch for, what worked before, gaps |
| **Debrief** | The surgeon's 2–3 minute check-in right after surgery |
| **Stand-in surgeon** | Karthi playing the surgeon's role in the MVP, using published guidelines |
| **Answer key** | The exact truth for a test case, sealed so the system can't "peek" |
| **Final-exam cases** | 3 test cases kept locked until the very end, so the score is honest |
| **Version O / Version B** | The same app running on OpenAI (O) or on AWS Bedrock (B) |
| **Live mode** | Processing while the surgery is still running, in small pieces |
| **Archive mode** | Processing old recorded videos in batches |
| **Medical dictionary (ontology)** | Our list of medical terms (Japanese + English) and how they relate |
| **Privacy filter** | Removes names and IDs before any cloud AI sees the text |
| **Danger zone** | Anatomy that is easily confused and dangerous to mix up; always checked by a human |
| **Critical view of safety (CVS)** | A standard safety check in gallbladder surgery before cutting anything |
| **Two witnesses** | Video and voice agree → strong; only one → flagged |
| **Growth replay** | Adding cases one by one to show knowledge improving |
| **API key** | A password that lets our software use a paid online service |
| **GPU** | A powerful computer chip used to run AI models like speech-to-text |
| **Capture box** | A small device that copies the OR camera and microphone signal to our system |
| **MHLW SaMD** | Japan's rules for software used as a medical device |

---

*Questions or corrections → Karthi. This document summarises the internal master plan (`docs/MASTER_PLAN.md`) and will be updated as open points are decided.*
