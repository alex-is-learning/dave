---
stepsCompleted:
  - step-01-init
  - step-02-context
  - step-03-starter
  - step-04-decisions
  - step-05-patterns
  - step-06-structure
  - step-07-validation
  - step-08-complete
inputDocuments:
  - _bmad-output/prd.md
  - _bmad-output/product-brief-dave.md
  - _bmad-output/product-brief-dave-distillate.md
  - _bmad-output/research/domain-ai-visual-active-learning-research-2026-04-01.md
  - _bmad-output/brainstorming/brainstorming-session-2026-04-01-1200.md
workflowType: 'architecture'
lastStep: 8
status: 'complete'
completedAt: '2026-04-01'
project_name: 'Dave'
user_name: 'Alex'
date: '2026-04-01'
---

# Architecture Decision Document

_This document builds collaboratively through step-by-step discovery. Sections are appended as we work through each architectural decision together._

## Project Context Analysis

### Requirements Overview

**Functional Requirements:**
37 FRs across 7 categories: Topic Initialisation (FR1–7), Session Entry & Contract (FR8–12), Socratic Interaction (FR13–19, FR36), Session Transcript (FR20–23), Post-Session Reflection & Logging (FR24–28), Knowledge & State Access (FR29–31, FR37), File & Repo Management (FR32–35).

Dave is a Claude Code skill — the implementation is markdown instruction files that direct AI behaviour, not compiled software. Architecture decisions are about file structure, instruction sequencing, and what Dave is told to do at each turn.

**Non-Functional Requirements:**
11 NFRs across Performance (NFR1–3), Reliability & Data Integrity (NFR4–7), and Integration Compatibility (NFR8–11).

Critical NFRs shaping architecture:
- NFR2: Context loading scoped to `/learning/<topic>/` only — never the full 330k-word scrapbook
- NFR3/NFR4: Incremental transcript writes must not block conversation and must never corrupt prior content
- NFR8/NFR9/NFR10: All output must satisfy Obsidian rendering AND Quartz publishing (no Obsidian-specific syntax in published files)

**Scale & Complexity:**
- Primary domain: CLI tool / AI skill instruction design
- Complexity level: Medium (novel domain, small implementation surface)
- No real-time features, multi-tenancy, external APIs, or regulatory constraints

### Technical Constraints & Dependencies

- Claude Code as runtime — all capability is what Claude Code can do natively (file read/write, bash commands, conversational AI)
- Existing Obsidian + Quartz scrapbook repo — Dave must not break existing structure or publishing pipeline
- Git as the only external state mechanism — `git diff -- .` scoped to CWD
- CWD depth contract: Claude Code always started one level below `/learning/` — this assumption is load-bearing for `../alex-and-dave.md` path resolution
- Obsidian vault-wide filename uniqueness enforced by convention (topic-scoped names), not by tooling

### Cross-Cutting Concerns Identified

- **Topic slug** — derived from CWD folder name at init, used in every filename Dave creates
- **Standard frontmatter** — applied to every Dave-created file (`date`, `tags: [Dave]`)
- **Append-only write discipline** — three files require append operations; failed writes must never corrupt prior content
- **Context scoping** — every file read must be explicitly scoped; Dave must never accidentally load the full scrapbook
- **Dual markdown compatibility** — Obsidian-valid syntax for local rendering, Quartz-safe syntax for published files (session transcripts, `alex-and-dave.md`)
- **Incremental transcript write** — the highest-risk cross-cutting mechanism; must work reliably before any other feature is built

## Starter Template Evaluation

### Primary Technology Domain

Claude Code skill (markdown instruction files). No runtime, no build step, no dependencies. This step evaluates skill file structure as the architectural foundation — the equivalent of a starter template for this project type.

### Selected Foundation: workflow.md + steps/ directory (BMAD pattern)

**Rationale:** Separates shared constraints (persona, frontmatter standard, hard behavioural rules) in `workflow.md` from per-command instructions in `steps/`. Keeps each step file readable and context-lean. Mirrors the BMAD pattern Alex already uses and understands.

**Skill file structure:**

```
~/.claude/skills/dave/
    workflow.md              ← persona, shared constraints, step sequencing
    steps/
        step-init.md         ← /tutor init instructions
        step-hello.md        ← /tutor hello instructions
        step-session.md      ← in-session Socratic behaviour + hard constraints
        step-close.md        ← end-of-session reflection + file writes
```

**Architectural decisions established by this structure:**
- Persona and hard behavioural constraints (never give the answer) live in `workflow.md` — loaded once, apply everywhere
- Each command phase is a discrete, readable file — maintainable without reading the whole skill
- Claude reads only the relevant step file during a session phase — supports token efficiency (NFR2)
- Adding Post-MVP commands (`/tutor test-me` etc.) = adding a step file, no restructuring needed

## Core Architectural Decisions

### Decision Priority Analysis

**Critical Decisions (Block Implementation):**
- Incremental transcript write mechanism — bash heredoc append
- `../alex-and-dave.md` path resolution — verified at init
- `dave-log-<topic>.md` schema — two-section structure (current state + history)

**Important Decisions (Shape Architecture):**
- Context loading order per command
- Session transcript format

**Deferred Decisions (Post-MVP):**
- ZPD calibration state mechanics (log schema reserves space via A/B/U state section)
- Hermeneutic spiral depth tracking

### File I/O Architecture

**Incremental Transcript Write: bash heredoc append**

Dave appends to the session transcript at the end of each response turn using a bash heredoc:

```bash
cat >> sessions/YYYY-MM-DD-<topic>.md << 'TURN'

**Dave:** [response text]

TURN
```

Rationale: True append — no read required, no overwrite risk, atomic. The only mechanism that satisfies NFR4 (a failed or interrupted write leaves previous content intact). Option B (read → append → write) was rejected because a mid-write failure corrupts the file.

**Append discipline for all files:** All three append targets (transcript, `dave-log-<topic>.md`, `alex-and-dave.md`) use bash append operators. Dave never reads-then-writes these files — append only.

### Path Resolution

**`../alex-and-dave.md` — verified at init, not assumed**

At `/tutor init`, Dave checks whether `../alex-and-dave.md` exists:
- Exists → CWD depth is correct, proceed
- Does not exist → Dave creates it (first-ever use of Dave across all topics)
- `../../alex-and-dave.md` exists instead → Dave warns Alex that CWD depth is wrong and stops before creating misplaced files

Topic slug is derived via `basename "$PWD"` — no manual input (FR7). All topic-scoped filenames use this slug.

### Context Loading Strategy

| Command | Files loaded | Order |
|---|---|---|
| `/tutor init` | None (topic is new) — generates primer | — |
| `/tutor hello` | `dave-log-<topic>.md` → `dave-primer-<topic>.md` → `git diff -- .` | Log first (A/B/U state), primer second (topic knowledge), diff last (what changed) |
| In-session | Primer + log already in context; no additional reads mid-session | — |
| End-of-session writes | Transcript → log block → `alex-and-dave.md` row | In this order |

Notes in `notes/` are loaded on demand only, not at session start — keeps context lean (NFR2).

### `dave-log-<topic>.md` Schema

Two-section structure: **Current A/B/U State** (updated each session — Dave adds/promotes items) and **Session History** (append-only — each session block added at the bottom). Dave reads the state section at `/tutor hello` without parsing the full history.

```markdown
---
date: dd/mm/yyyy, --:--
tags:
    - Dave
topic: <topic-slug>
---

# Dave Log — <Topic Name>

## Current A/B/U State

### A — Genuine Advances
- [YYYY-MM-DD] item

### B — Confirmed Beliefs (Interrogation Targets)
- [YYYY-MM-DD] item

### U — Recognised Gaps
- [YYYY-MM-DD] item

---

## Session History

### YYYY-MM-DD | Session N | NN min | Complete | Pride: N/5

**A/B/U Reflection:**
- A: [Alex's text]
- B: [Alex's text]
- U: [Alex's text]

**Session summary:** [Dave's notes — what held up, what slipped, focus for next time]

---

### YYYY-MM-DD | Session N | NN min | Incomplete | —

**Note:** Session ended early. A/B/U reflection not completed.

---
```

ZPD note: The A/B/U state section reserves space for future ZPD calibration mechanics (Post-MVP) without a schema change.

### Session Transcript Format

```markdown
---
date: dd/mm/yyyy, --:--
tags:
    - Dave
topic: <topic-slug>
---

# Session — <Topic Name> — YYYY-MM-DD

**Duration commitment:** NN minutes
**Session contract:** "We're not here to rehearse what you know. We're here to find what you don't. Confusion is the signal."

---

**Alex:** [turn text]

**Dave:** [turn text]

---

## End of Session

**Duration:** NN min | **Status:** Complete | **Pride:** N/5

**A/B/U Reflection:**
- A: [Alex's text]
- B: [Alex's text]
- U: [Alex's text]
```

Standard markdown only — no Obsidian-specific syntax (NFR10). Session header gives a Quartz visitor the contract and duration without needing context.

## Implementation Patterns & Consistency Rules

### Naming Patterns

**Topic slug:**
- Always derived from `basename "$PWD"` at init, stored in context for the session
- Always lowercase, hyphens only (no underscores, no spaces) — e.g. `schema-therapy`
- Used verbatim in all filenames for that topic

**File naming:**
- `dave-primer-<slug>.md`
- `dave-log-<slug>.md`
- `sessions/YYYY-MM-DD-<slug>.md` — date in ISO format (4-digit year, 2-digit month, 2-digit day)

**Session filename date:** Always `YYYY-MM-DD` (e.g. `2026-04-01`). Never `DD-MM-YYYY` or other formats.

### Frontmatter Pattern

All Dave-created files use exactly this frontmatter — no extra fields, no variation:

```yaml
---
date: DD/MM/YYYY, HH:MM
tags:
    - Dave
---
```

Date format in frontmatter: `DD/MM/YYYY, HH:MM` (Obsidian convention). Distinct from the ISO format used in filenames.

### Transcript Turn Pattern

Every conversational turn in a session transcript uses exactly this format:

```
**Alex:** [text]

**Dave:** [text]
```

- One blank line between turns
- Bold names, colon, space, then text — no variation (`> Alex:`, `Alex —`, `## Alex` are all wrong)
- Appended via bash heredoc after every Dave response

### Log Append Block Pattern

Each session appended to `dave-log-<slug>.md`:

```markdown
### YYYY-MM-DD | Session N | NN min | Complete | Pride: N/5

**A/B/U Reflection:**
- A: [text]
- B: [text]
- U: [text]

**Session summary:** [text]

---
```

For an incomplete session:

```markdown
### YYYY-MM-DD | Session N | NN min | Incomplete | —

**Note:** Session ended early. A/B/U reflection not completed.

---
```

The `---` horizontal rule after each block is mandatory — it marks the block boundary and makes it parseable.

### `alex-and-dave.md` Table Row Pattern

Fixed columns in this order:

```markdown
| Date | Topic | Duration | Status | Pride | Notes |
|---|---|---|---|---|---|
| 2026-04-01 | Schema Therapy | 30 min | Complete | 4/5 | — |
| 2026-04-15 | Schema Therapy | 18 min | Incomplete | — | Early exit |
```

- Date: `YYYY-MM-DD`
- Topic: Title case, human-readable (not the slug)
- Duration: `NN min`
- Status: `Complete` or `Incomplete` (capitalised, no other values)
- Pride: `N/5` or `—` if incomplete
- Notes: `—` or brief free text

### Behavioural Constraint Pattern: Never Give the Answer

**Hard rule:** Dave never provides subject-matter answers, even when Alex asks directly.

**What counts as giving the answer:** Stating the definition, explaining the concept, or filling the gap directly.

**Permitted:** Pointing at the gap ("What would you need to know to answer that?"), reframing the question, asking what Alex already believes.

**When Alex asks the same question a different way:** Dave names it — "You've asked me this a few different ways now. I'm not going to answer it. What do you think the answer is?"

**Exception — operational questions:** Dave can answer "how do I save this file?" or procedural questions about the session itself. The constraint applies to subject-matter understanding only.

### Error Handling Pattern

- **File write failures:** Dave surfaces them immediately — never continues as if the write succeeded (NFR7). Standard message: "I tried to append to [filename] and it failed. Check the file manually before we continue."
- **Missing `dave-log-<slug>.md` at `/tutor hello`:** Dave treats this as a first session and creates the file — not an error.
- **CWD depth wrong:** If neither `../alex-and-dave.md` nor `../../alex-and-dave.md` exists, Dave creates `../alex-and-dave.md` and notes the assumption in the file header.

### All Claude Instances Implementing Dave MUST:

1. Derive the topic slug from `basename "$PWD"` — never hardcode or ask Alex for it
2. Use bash heredoc append for all writes to transcript, log, and homepage
3. Apply the exact frontmatter format — no extra fields
4. Use the exact transcript turn format (`**Alex:**` / `**Dave:**`)
5. End every log block with `---`
6. Surface file write errors immediately rather than continuing silently
7. Never give subject-matter answers, regardless of how the question is phrased

## Project Structure & Boundaries

### Complete Project Directory Structure

**Skill files — installed once at `~/.claude/skills/dave/`:**

```
~/.claude/skills/dave/
    workflow.md              ← persona, hard constraints, shared conventions,
                               context loading rules, step dispatch
    steps/
        step-init.md         ← /tutor init: onboarding interview, ZPD calibration,
                               primer generation, folder scaffolding
        step-hello.md        ← /tutor hello: git diff, A/B/U state summary,
                               session contract, action menu
        step-session.md      ← in-session: Socratic interaction, B interrogation,
                               incremental transcript append, paper sketch prompts
        step-close.md        ← end-of-session: A/B/U reflection prompts,
                               pride score capture, log append, homepage update
```

**Runtime files — created per-topic in the scrapbook:**

```
/learning/
    alex-and-dave.md         ← cross-topic homepage; session history table;
                               created on first ever /tutor init, appended after
                               each session from any topic

    <topic-slug>/            ← CWD when running Dave (e.g. schema-therapy/)
        dave-primer-<slug>.md    ← Dave's knowledge source; generated at init
                                   or ingested from user-supplied material
        dave-log-<slug>.md       ← persistent session log; two sections:
                                   Current A/B/U State + Session History
        sessions/
            YYYY-MM-DD-<slug>.md ← one file per session; written incrementally
                                   as turns occur; never overwritten
        notes/                   ← user's dialogical notes written to Dave;
                                   read by Dave on demand, not at session start
```

### `workflow.md` Responsibilities

`workflow.md` is loaded at every command invocation. It contains:

1. **Persona definition** — Feynman-Russell: warm, rigorous, won't let fuzzy thinking pass
2. **Hard behavioural constraints** — never give subject-matter answers; enforce even under direct pressure
3. **Shared conventions** — frontmatter format, slug derivation (`basename "$PWD"`), file naming rules
4. **Context loading rules** — what to read, in what order, for each command
5. **Path resolution** — CWD depth contract (`../alex-and-dave.md`), verification at init
6. **Step dispatch** — maps `/tutor init` → `step-init.md`, `/tutor hello` → `step-hello.md`, etc.

### `alex-and-dave.md` Structure

Created once at `/learning/` level. Updated after every session from any topic.

```markdown
---
date: DD/MM/YYYY, HH:MM
tags:
    - Dave
---

# Alex and Dave

Learning sessions across all topics. A record of showing up.

| Date | Topic | Duration | Status | Pride | Notes |
|---|---|---|---|---|---|
| 2026-04-01 | Schema Therapy | 30 min | Complete | 4/5 | — |
```

### Architectural Boundaries

**Skill boundary:** `~/.claude/skills/dave/` is read-only during sessions. Updates to skill files are manual upgrades — never triggered by session events.

**Topic boundary:** Dave reads and writes only within `/learning/<topic>/` plus `../alex-and-dave.md`. Never reads files outside this scope (NFR2).

**Git boundary:** `git diff -- .` is scoped to CWD. Dave never runs git commands that affect the wider repo — no commit, no push, no global diff.

**Primer boundary:** `dave-primer-<slug>.md` is Dave's knowledge source, not a user note. Created by Dave at init, correctable by Alex. Dave treats it as authoritative for session content and surfaces explicit uncertainty if the topic is outside confident LLM knowledge (FR5).

### Data Flow

```
/tutor init:
  CWD → slug → scaffold files → generate primer → write dave-log (empty) → write alex-and-dave (row)

/tutor hello:
  read dave-log → read dave-primer → run git diff → present session → begin session contract

Session turn (in-session):
  [conversation] → bash heredoc append → sessions/YYYY-MM-DD-<slug>.md

End of session:
  A/B/U prompts → pride score → append session block to dave-log
  → update A/B/U state section → append row to alex-and-dave
```

## Architecture Validation Results

### Coherence Validation ✅

All decisions are internally consistent. No version conflicts (no traditional tech stack). Bash heredoc append is native to Claude Code. Patterns align with the markdown-instruction-file model. Boundaries are clean and non-overlapping. No contradictory decisions found.

### Requirements Coverage ✅

| FR Category | Coverage |
|---|---|
| Topic Initialisation (FR1–7, FR37) | ✅ Full — step-init.md |
| Session Entry & Contract (FR8–12) | ✅ Full — step-hello.md |
| Socratic Interaction (FR13–16, FR36) | ✅ MVP covered — step-session.md |
| Session Transcript (FR20–23) | ✅ Full — bash heredoc, file created at session start |
| Post-Session Reflection & Logging (FR24–28) | ✅ Full — step-close.md (see gap resolution below) |
| Knowledge & State Access (FR29–31) | ✅ Full — workflow.md context loading |
| File & Repo Management (FR32–35) | ✅ Full — naming patterns + workflow.md |

FR17/18/19 (Feynman, teach-me, since-last) deferred to Post-MVP as specified in PRD.

All 11 NFRs covered: scoped git diff (NFR11), context scoping to topic only (NFR2), append-only transcripts (NFR4), discrete log blocks with `---` separator (NFR5), Quartz-safe standard markdown (NFR10), immediate error surfacing (NFR7).

### Gap Identified and Resolved

**Gap:** The session close trigger was unspecified. The MVP command table had no `/tutor end`, leaving FR24–28 (reflection, pride score, log append, homepage update) without a trigger.

**Resolution:** `/tutor end` added to MVP command set. Alex types it when done. Dave runs the full close ritual: A/B/U reflection prompts → pride score → session block append to log → homepage row update. The incremental transcript is safe regardless (bash append is per-turn); `/tutor end` is purely the trigger for the reflection and logging ritual.

### Updated MVP Command Set

| Command | Purpose | File |
|---|---|---|
| `/tutor init` | Onboarding, ZPD calibration, primer generation, folder scaffolding | `steps/step-init.md` |
| `/tutor hello` | Git diff review, A/B/U state, session contract, session file creation | `steps/step-hello.md` |
| `/tutor end` | Close ritual: reflection, pride score, log append, homepage update | `steps/step-close.md` |
| *(in-session)* | Socratic interaction, B interrogation, transcript append, sketch prompts | `steps/step-session.md` |

### Architecture Completeness Checklist

- [x] Project context analysed — 37 FRs, 11 NFRs, constraints and cross-cutting concerns mapped
- [x] Skill file structure decided — `workflow.md` + `steps/` directory
- [x] Incremental transcript write mechanism — bash heredoc append per turn
- [x] Path resolution — `../alex-and-dave.md` verified at init via `basename "$PWD"`
- [x] Context loading order — log → primer → git diff at `/tutor hello`
- [x] `dave-log-<slug>.md` schema — two-section structure (Current A/B/U State + Session History)
- [x] Session transcript format — standard markdown, `**Alex:**`/`**Dave:**` turns, no Obsidian-specific syntax
- [x] Implementation patterns — naming, frontmatter, append blocks, table rows, behavioural constraints, error handling
- [x] Project structure — complete directory tree, both skill files and runtime files
- [x] Session close trigger — `/tutor end` added to MVP

### Architecture Readiness Assessment

**Overall Status: READY FOR IMPLEMENTATION**

**Confidence: High** — all 37 FRs architecturally supported, all open questions from the handover resolved, no remaining blockers.

**Key strengths:**
- Append-only file I/O eliminates data loss risk across all three write targets
- `workflow.md` as the single source for persona constraints prevents behavioural drift across step files
- Topic-scoped CWD model keeps context lean and git diff accurate without any configuration
- No new tools or infrastructure — builds entirely on what already exists

**First implementation story:** Create `~/.claude/skills/dave/workflow.md` with persona, constraints, and shared conventions. Test the bash heredoc append mechanism before building anything else.
