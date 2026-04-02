# Project Handover

**Project:** Dave — AI Learning Tutor
**Date:** 2026-04-02
**Status:** All 12 MVP stories complete — active testing in scrapbook Obsidian vault

---

## What This Project Is

Alex wants to build **Dave**: a Claude Code skill (BMAD-pattern inspired, no BMAD infrastructure required) that acts as a personal AI tutor (Feynman-Russell persona) living inside his existing Obsidian + Quartz scrapbook repo. Dave helps Alex learn topics deeply through active recall, A/B/U structured writing, Socratic scaffolding, and session-based grappling. Personal-first — built for Alex, shared only after months of lived evidence.

**Core philosophy:** Learning = clearing debris + expanding agency. The enemy is double ignorance (false closure, not absence). Struggle is the signal. Dave never gives the answer — he points at the gap and waits.

---

## BMad Workflow Progress

| Step | Status |
|------|--------|
| BMad core config (`_bmad/core/config.yaml`) | ✅ Complete |
| Domain Research | ✅ Complete |
| Brainstorming | ✅ Complete |
| Product Brief | ✅ Complete |
| PRD | ✅ Complete |
| Architecture | ✅ Complete — all open questions resolved |
| UX Design | N/A — CLI tool |
| Epics & Stories | ✅ Complete — 12 stories across 4 epics, all FRs covered |

**Output files:**
- `_bmad-output/research/domain-ai-visual-active-learning-research-2026-04-01.md`
- `_bmad-output/brainstorming/brainstorming-session-2026-04-01-1200.md`
- `_bmad-output/product-brief-dave.md`
- `_bmad-output/product-brief-dave-distillate.md`
- `_bmad-output/prd.md` — full PRD (37 FRs, 11 NFRs)
- `_bmad-output/architecture.md` — complete
- `_bmad-output/epics.md` — complete — 12 stories, all 37 MVP FRs covered

---

## Story Implementation Progress

| Story | Title | Status |
|-------|-------|--------|
| 1.1 | Skill Skeleton and Append Proof-of-Concept | ✅ Complete |
| 1.2 | Topic Folder Scaffolding | ✅ Complete |
| 1.3 | Onboarding Interview and ZPD Calibration | ✅ Complete |
| 1.4 | Primer Generation | ✅ Complete |
| 2.1 | Context Load — Git Diff, Primer, and Log | ✅ Complete |
| 2.2 | Session Contract, Continuity Flag, and Action Menu | ✅ Complete |
| 3.1 | Incremental Transcript Append | ✅ Complete |
| 3.2 | Socratic Interrogation — Probing B's and Naming Vagueness | ✅ Complete |
| 3.3 | Mapping U's, Notes on Demand, and Paper Sketch Prompts | ✅ Complete |
| 4.1 | A/B/U Reflection and Pride Score | ✅ Complete |
| 4.2 | Session Log Append and Homepage Update | ✅ Complete |
| 4.3 | Early Exit Logging | ✅ Complete |

---

## Installed Skill Files

All files at `~/.claude/skills/dave/`:

```
~/.claude/skills/dave/
    workflow.md              ✅ Complete — persona, constraints, conventions, dispatch
    steps/
        step-init.md         ✅ Complete — all sections (Stories 1.2–1.4)
        step-hello.md        ✅ Complete — all sections (Stories 2.1–2.2, 4.3)
        step-session.md      ✅ Complete — all sections (Stories 3.1–3.3)
        step-close.md        ✅ Complete — all sections (Stories 4.1–4.3)
```

---

## Architectural Decisions

### Skill File Structure
`workflow.md` + `steps/` directory (BMAD pattern):
```
~/.claude/skills/dave/
    workflow.md              ← persona, hard constraints, shared conventions, step dispatch
    steps/
        step-init.md         ← /dave init
        step-hello.md        ← /dave hello
        step-session.md      ← in-session Socratic behaviour
        step-close.md        ← /dave end (close ritual)
```

### Command Name Note
The skill directory is `dave/` so the commands are `/dave init`, `/dave hello`, `/dave end`. The architecture docs use `/tutor` terminology but the path was always `~/.claude/skills/dave/`. To use `/tutor` commands, rename the directory to `tutor/`.

### Incremental Transcript Write Mechanism
**Bash heredoc append per turn:**
```bash
cat >> sessions/YYYY-MM-DD-<slug>.md << 'TURN'

**Dave:** [response text]

TURN
```
True append — no read required, no overwrite risk. Proven working in Story 1.1 and retested in Story 3.1.

### `../alex-and-dave.md` Path Resolution
- Dave verifies the CWD depth at `/dave init` — checks `../alex-and-dave.md` exists or creates it
- If `../../alex-and-dave.md` exists instead, Dave warns and stops
- Topic slug derived via `basename "$PWD"` — no manual input

### Context Loading Order at `/dave hello`
`dave-log-<slug>.md` → `dave-primer-<slug>.md` → `git diff -- .`

### `dave-log-<slug>.md` Schema
Two-section structure:
1. **Current A/B/U State** — updated each session (Dave appends new items; B→A promotions noted but old row left in place — Alex can manually clean between sessions)
2. **Session History** — append-only session blocks, each terminated with `---`

### Session History Block Format (`/dave end`)
```
**Session:** YYYY-MM-DD
**Duration:** [duration]
**Status:** complete
**Pride:** [score]/5

**A:** [reflection]
**B:** [reflection]
**U:** [reflection]

**Summary:** [1–2 sentences]

---
```

### Early Exit Block Format (written by `/dave hello` on next session)
```
**Session:** YYYY-MM-DD
**Status:** incomplete

---
```

### Early Exit Detection Mechanism (Story 4.3)
There is no automatic trigger when a session simply stops. Detection happens at the next `/dave hello`:
1. List transcript files in `sessions/` matching `YYYY-MM-DD-<slug>.md`; take the most recent
2. Extract the date (first 10 chars of filename)
3. Check `dave-log-<slug>.md` Session History for a block with that date
4. **Case A** — block found with `Status: complete` → proceed normally
5. **Case B** — block found with `Status: incomplete` → surface continuity note, proceed
6. **Case C** — no block found → write early exit block, update homepage with incomplete row, surface note, proceed

### Session Transcript Format
Standard markdown only (`**Alex:**` / `**Dave:**` turns). One blank line between turns. No Obsidian-specific syntax. Quartz-safe.

### `/dave end` is an MVP Command
Triggers the full close ritual: A/B/U reflection (sequential, open text) → pride score (integer 1–5, validated) → log append → A/B/U state update → homepage update.

### A/B/U State Update — Append-Only Constraint
ARCH2 forbids read-then-write. The Current A/B/U State table is updated by appending new rows at session close. B→A promotions add a new A row; the old B row remains until Alex manually cleans it. The Session History block is the canonical record; the table is a working reference.

### MVP Command Set
| Command | Purpose |
|---|---|
| `/dave init` | Onboarding interview, ZPD calibration, primer generation, folder scaffolding |
| `/dave hello` | Git diff review, A/B/U state, session contract, transcript file creation, early exit detection |
| `/dave end` | Close ritual: A/B/U reflection, pride score, log append, homepage update |
| *(in-session)* | Socratic interaction, B interrogation, vagueness naming, U mapping, transcript append, notes, sketch prompts |

### Implementation Patterns (key rules for any Claude implementing Dave)
1. Derive topic slug from `basename "$PWD"` — never hardcode or ask Alex
2. Use bash heredoc append for all writes to transcript, log, and homepage
3. Frontmatter format: `date: YYYY-MM-DDTHH:MM` + `tags: [Dave]` — no extra fields
4. Transcript turns: `**Alex:**` / `**Dave:**` — one blank line between turns
5. End every log block with `---`
6. Surface file write errors immediately — never continue silently
7. Never give subject-matter answers, regardless of how the question is phrased
8. In-session: B items before U items; probe load-bearing assumptions, not topics in general
9. Vagueness: name the specific word or phrase, one at a time, then wait without softening
10. U items tracked in context mid-session; written to log at `/dave end`

---

## Repo Structure

```
~/claude/dave/
├── README.md
├── HANDOVER.md
├── skills/
│   └── dave/                    ← skill implementation (source of truth)
│       ├── SKILL.md
│       ├── workflow.md
│       └── steps/
│           ├── step-init.md
│           ├── step-hello.md
│           ├── step-session.md
│           └── step-close.md
└── _bmad-output/                ← planning artefacts
    ├── prd.md
    ├── architecture.md
    ├── epics.md
    └── ...
```

`~/.claude/skills/dave` is a symlink to `~/claude/dave/skills/dave`. Editing files in the repo is immediately live — no deployment step.

---

## Bugs Fixed (2026-04-02)

### Obsidian YAML date parsing
- **Bug:** Date frontmatter written as `DD/MM/YYYY, HH:MM` (e.g. `01/04/2026, 18:33`) was being parsed by Obsidian as a garbled date (`20/04/2001`)
- **Fix:** Changed all date format strings to ISO 8601: `date '+%Y-%m-%dT%H:%M'`
- **Files changed:** `workflow.md`, `step-init.md` (3 occurrences), `step-hello.md` (2 occurrences)
- **Note:** Any files Dave already created have the old format — fix manually if needed

---

## Current Status

All 12 stories implemented. Active testing in scrapbook Obsidian vault. One bug fixed (date format). No known blockers.

---

## Next Steps

1. **Continue smoke testing** `/dave init` → `/dave hello` → in-session → `/dave end` on a real topic in the scrapbook vault
2. **Verify date fix** — confirm new sessions show the correct date in Obsidian properties
3. **Test early exit detection** — start a session, don't run `/dave end`, verify Case C triggers correctly on next `/dave hello`
4. **If testing surfaces more bugs** — fix in `skills/dave/` (changes are immediately live via symlink), then commit and push

---

## Post-MVP FRs to consider later (from PRD)

- FR17: Feynman-style session (learner explains, Dave presses)
- FR18: Confused-student essay mode
- FR19: Before/after review since last session
