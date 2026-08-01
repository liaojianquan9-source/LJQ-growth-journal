# Growth Journal · A Conversational Journaling Skill

**English** | [中文](README.md)

> You talk, the AI listens. It organizes your words into blocks, you confirm, then it saves.
> Records for your future self — and for future AI.

## Why this Skill

After studying 20+ mainstream personal-record practices (Bullet Journal, Morning Pages, GTD, KISS/GRAI reviews, Decision Journals, Quantified Self, and GitHub projects like LifeOS and AI Journaling), one conclusion kept recurring:

> **Review compounds. Without review, everything is random.**

Recording alone does not compound — **the review loop does**. Most tools solve "write it down"; few solve the review loop. This Skill makes review a first-class citizen: daily journaling, weekly review, annual review, and decision revisits are all built in.

And in the AI era, your records carry a second layer of value: **they are the personal data source every future AI application will draw from** — writing, content creation, pattern-spotting ("why do I keep making this mistake?"). That only works if records are **dual-readable**: pleasant for humans, cheap for AI to retrieve. Every format decision in this Skill follows from that.

## Features

- **Conversational journaling**: talk freely (voice input works). The AI only listens — no judgment. Say "wrap it up", it organizes everything into four blocks, and saves only after your confirmation.
- **Two execution paths**: AI agents with file tools (Kimi / Claude Code, etc.) write directly into your Obsidian vault; any plain chat model (mobile apps included) outputs clean Markdown for you to copy — no feature reduction.
- **Your voice, un-AI-ified**: Events/Actions may be condensed into bullets; Thoughts/Lessons must keep your exact words — no polishing, no elevating, no AI-speak.
- **Voice-input friendly**: obvious transcription errors are silently corrected without interrupting you; uncertain words are marked 【?】and asked once at confirmation.
- **Dual-readable format**: plain Markdown + YAML frontmatter + inline tags — natively compatible with Obsidian (Daily Notes / Periodic Notes / Dataview).
- **Memory index**: in file-writing mode, an `index.md` directory of the whole vault is maintained; weekly/annual reviews read the index first, then full entries — fast at any scale.
- **Four-layer review loop**: Daily → Weekly (KISS + recurring-issue list) → Annual (Sahil Bloom's 7 questions) → Decision revisits (Decision Journal).

## Installation

### Option 1: As a Skill (AI environments that support Skills)

Unzip `ljq-growth-journal.skill` (it is a zip archive) and place the `ljq-growth-journal/` folder into your skills directory:

```
~/.config/agents/skills/     # recommended
~/.kimi/skills/
~/.claude/skills/
# or project-level: <your-project>/.agents/skills/
```

The AI loads it automatically when you say a trigger phrase.

### Option 2: Plain text (any LLM, including mobile)

Open `SKILL.md` and paste its full content as a system prompt (or simply send it to the AI saying "follow these rules from now on"). Zero dependencies.

## Usage

### Trigger phrases

| Phrase | Function |
|---|---|
| "Log 260801" / "journaling" / "record today" | Daily record |
| "weekly review" / "wrap up this week" | Weekly review |
| "annual review" / "yearly summary" | Annual review |
| "record a decision" / "I made a big decision today" | Decision journal |
| "observation mode on / off" | Toggle proactive observations (default: off) |

(Triggers are semantic, not exact-match — anything close in meaning works.)

### A complete daily session

```
You: Log 260801
AI: (first time: explains the rules + asks where to save; after that it only replies "Got it.")
You: ... (talk freely, voice input fine, as many turns as you like)
You: That's all, wrap it up.
AI: lays out your day in four blocks: Events / Thoughts / Lessons / Actions
You: Change XX / Looks good.
AI: Path A: writes journal/2026/08/成长记录-第1天-20260801.md and updates index.md
    Path B: outputs one filename line + one Markdown code block, nothing else
```

In-flow commands: "that's all / wrap it up" ends listening; "looks good / save it" confirms.

## Output format

### Daily file

```markdown
---
date: 2026-08-01        # required, ISO date (Obsidian-compatible)
tags: [work, health]    # dimension tags: unlimited, auto-extracted
mood:                   # optional
decision: false         # true when a major decision was made
revisit:                # decision revisit date
---

## Events     # what objectively happened (skeleton; may be condensed)
- 

## Thoughts   # feelings & reflections (flesh; your exact words)
- #good        what went well
- #difficult   what was hard
- #different   what to change

## Lessons    # insights worth reusing (material for future writing)
- #lesson

## Actions    # to-dos (the AI will ask about them next time)
- [ ] 
```

**Rules**: every block is optional — an empty block is a valid record; dimensions live in tags, never in fixed columns.

### Tag system

- **5 fixed functional tags**: `#good` `#difficult` `#different` (thoughts) + `#经验` `#教训` (lessons/decisions) — structural anchors for weekly review and AI retrieval.
- **Unlimited dimension tags**: `tags:` in frontmatter are extracted from content (work/health/finance/relationships/learning are just examples) — add or remove freely as life changes.

### File naming

| Type | Path | Example |
|---|---|---|
| Daily | `journal/YYYY/MM/成长记录-第N天-YYYYMMDD.md` | `journal/2026/08/成长记录-第1天-20260801.md` |
| Weekly | `weekly/成长周报-YYYY-Wxx.md` | `weekly/成长周报-2026-W31.md` |
| Annual | `yearly/年度总结-YYYY.md` | `yearly/年度总结-2026.md` |

Naming is a sensible default — state any preference and the AI will follow yours.

## Structure

### Skill package

```
ljq-growth-journal/
├── SKILL.md                  # main file: triggers / dual paths / flow / iron rules
├── 说明.md                   # this file (bilingual)
├── references/               # extended features (loaded on demand)
│   ├── weekly-review.md      # weekly: KISS + recurring issues
│   ├── annual-review.md      # annual: Bloom 7 questions + if-X-then-Y rules
│   └── decision-log.md       # decision journal + revisit
└── assets/
    └── templates/            # file skeletons
        ├── daily.md
        ├── weekly.md
        ├── annual.md
        └── decision.md
```

### Generated vault

```
your-vault/ (location of your choice; can sit inside an Obsidian vault)
├── index.md                            # memory index: one line per entry (file-writing mode only)
├── journal/YYYY/MM/成长记录-第N天-YYYYMMDD.md
├── weekly/成长周报-YYYY-Wxx.md
├── yearly/年度总结-YYYY.md
└── decisions/                          # decision journal (optional)
```

### Execution logic

```
ljq-growth-journal execution/
├── 1. Trigger/  daily · weekly · annual · decision
├── 2. Opening (first time only)/  explain rules + confirm save location
├── 3. Daily flow (core)/
│   ├── Step1 Listen♻️: no judgment, no interruption
│   ├── Step2 Organize & present: frontmatter + four blocks
│   │   ├── Iron rule 1 · No AI-ification: skeleton may condense, flesh keeps your words
│   │   └── Iron rule 2 · Silent correction: uncertain words marked 【?】, asked once
│   ├── Step3 Confirm♻️: changes → back to Step2; approved → Step4
│   └── Step4 Execute: Path A write file + update index / Path B clean output
├── 4. Weekly: read index.md → KISS draft + recurring issues → confirm → save
├── 5. Annual: read index.md → all weekly reviews → Bloom 7 questions → if-X-then-Y → 3+3
├── 6. Decision: reasons/confidence/expectation/revisit → proactive revisit when due
└── 7. History linkage (Path A only): action follow-ups · on-this-day · revisit scan
```

```mermaid
flowchart TD
    A[Trigger phrase] --> B{First time this session?}
    B -- Yes --> C[Opening: rules + confirm save location]
    B -- No --> D{Which function?}
    C --> D
    D -- Daily --> E[Step1 Listen ♻️]
    E --> F{User says "wrap it up"?}
    F -- Not yet --> E
    F -- Yes --> G[Step2 Organize & present<br>Iron rule 1: no AI-ification<br>Iron rule 2: silent correction]
    G --> H[Step3 Confirm ♻️]
    H -- Changes --> G
    H -- Approved --> I{Path?}
    I -- A: file tools --> J[Write file + update index.md]
    I -- B: plain text --> L[Filename + Markdown code block<br>nothing else]
    D -- Weekly --> M[Read index.md → KISS + recurring issues]
    D -- Annual --> N[Read index.md → Bloom 7 questions → if-X-then-Y]
    D -- Decision --> O[Decision template → revisit when due]
```

## Design philosophy

1. **Events / Thoughts / Lessons / Actions** — converged from three independent sources: Bullet Journal, GDD memos, and the jishi/suixiang template split.
2. **Dimensions live in tags, not fixed columns** — add or remove anytime, aggregate across days.
3. **frontmatter + Markdown + date naming** — the de facto standard for dual-readable records.
4. **The review loop is the only definition of compounding** — on-this-day, decision revisits, weekly and annual reviews.
5. **Restraint and whitespace** — every field optional; an empty block is a valid record.
6. **Specific over general, honest over presentable** — never beautify your past bias, or you're just rewriting memory with outcomes.
7. **Configurable AI persona** — listener or critical partner is the user's switch (default: listener; say "observation mode on" to switch).

## Words we believe

> Let everything happen; let it flow naturally.

> A journal only matters when it tells the truth.

> Record your emotions, information, and biases as they were — otherwise you're just rewriting memory with outcomes.

> Standardize the process; personalize the output.

> Don't record for recording's sake; record for a goal.

> Specific beats general.

> We suffer more in imagination than in reality. — Seneca

> Know thyself. — Inscription at the Temple of Delphi

## Acknowledgements

Design research drawn from: [lifeos-template](https://github.com/seandavi/lifeos-template), [ai-journaling-template](https://github.com/TimoBakx/ai-journaling-template), [myself_skeleton](https://github.com/cnxzcxy/myself_skeleton) (GitHub); *The Miracle of Morning Journal* (Sato Den); [Bullet Journal](https://bulletjournal.com); the GDD memo workflow; KISS / KPT / GRAI / PDCA / 4Fs review models; [Sahil Bloom's Personal Annual Review](https://www.sahilbloom.com/newsletter/the-personal-annual-review); Tim Ferriss's Past Year Review; Decision Journal practice; the Quantified Self community; Obsidian docs and community template ecosystem.

## License

[CC BY 4.0](https://creativecommons.org/licenses/by/4.0/) — free to use, modify, and distribute with attribution.
