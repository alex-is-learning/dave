# Project Handover

**Project:** Dave — AI Learning Tutor
**Date:** 2026-04-01
**Status:** Epics & Stories in progress — Epic 1 stories drafted, awaiting approval

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
| Epics & Stories | 🔄 In progress — Epic list approved, Story generation started |

**Output files:**
- `_bmad-output/research/domain-ai-visual-active-learning-research-2026-04-01.md`
- `_bmad-output/brainstorming/brainstorming-session-2026-04-01-1200.md`
- `_bmad-output/product-brief-dave.md`
- `_bmad-output/product-brief-dave-distillate.md`
- `_bmad-output/prd.md` — full PRD (37 FRs, 11 NFRs)
- `_bmad-output/architecture.md` — **complete, status: complete**
- `_bmad-output/epics.md` — **in progress** (requirements + epic list written; stories not yet written)

---

## Architectural Decisions Made This Session

All open questions from the previous handover are now resolved.

### Skill File Structure
`workflow.md` + `steps/` directory (BMAD pattern):
```
~/.claude/skills/dave/
    workflow.md              ← persona, hard constraints, shared conventions, step dispatch
    steps/
        step-init.md         ← /tutor init
        step-hello.md        ← /tutor hello
        step-session.md      ← in-session Socratic behaviour
        step-close.md        ← /tutor end (close ritual)
```

### Incremental Transcript Write Mechanism
**Bash heredoc append per turn** — the highest-risk mechanism, now fully resolved:
```bash
cat >> sessions/YYYY-MM-DD-<slug>.md << 'TURN'

**Dave:** [response text]

TURN
```
True append — no read required, no overwrite risk. Must be the first thing tested before any other feature is built.

### `../alex-and-dave.md` Path Resolution
- Dave verifies the CWD depth at `/tutor init` — checks `../alex-and-dave.md` exists or creates it
- If `../../alex-and-dave.md` exists instead, Dave warns and stops
- Topic slug derived via `basename "$PWD"` — no manual input

### Context Loading Order at `/tutor hello`
`dave-log-<slug>.md` → `dave-primer-<slug>.md` → `git diff -- .`

### `dave-log-<slug>.md` Schema
Two-section structure:
1. **Current A/B/U State** — updated each session (Dave adds/promotes items)
2. **Session History** — append-only session blocks, each terminated with `---`

### Session Transcript Format
Standard markdown only (`**Alex:**` / `**Dave:**` turns). No Obsidian-specific syntax. Quartz-safe.

### `/tutor end` Added to MVP
This was the one gap found during architecture validation. FR24–28 (reflection, pride score, log append, homepage update) had no trigger in the original PRD command table. **`/tutor end` is now an MVP command** — Alex types it to trigger the close ritual.

### Updated MVP Command Set
| Command | Purpose |
|---|---|
| `/tutor init` | Onboarding interview, ZPD calibration, primer generation, folder scaffolding |
| `/tutor hello` | Git diff review, A/B/U state, session contract, session file creation |
| `/tutor end` | Close ritual: reflection, pride score, log append, homepage update |
| *(in-session)* | Socratic interaction, B interrogation, transcript append, sketch prompts |

### Implementation Patterns (key rules for any Claude implementing Dave)
1. Derive topic slug from `basename "$PWD"` — never hardcode or ask Alex
2. Use bash heredoc append for all writes to transcript, log, and homepage
3. Frontmatter format: `date: DD/MM/YYYY, HH:MM` + `tags: [Dave]` — no extra fields
4. Transcript turns: `**Alex:**` / `**Dave:**` — one blank line between turns
5. End every log block with `---`
6. Surface file write errors immediately — never continue silently
7. Never give subject-matter answers, regardless of how the question is phrased

---

## Epics & Stories: Current State

### Approved Epic List (4 epics)

| Epic | Title | FRs |
|---|---|---|
| Epic 1 | `/tutor init` — Topic Initialisation | FR1–7, FR14, FR32–35, FR37 + ARCH1–4, ARCH8 |
| Epic 2 | `/tutor hello` — Session Entry | FR8–12, FR21, FR29–30 + ARCH5–6 |
| Epic 3 | Active Session — Socratic Interaction & Transcript | FR13–16, FR20, FR22–23, FR31, FR36 + ARCH2 |
| Epic 4 | `/tutor end` — Session Close & Logging | FR24–28 + ARCH7 |

Post-MVP FRs (FR17 Feynman, FR18 teach-me, FR19 since-last) deferred as agreed.

### Story Generation Status

Working through `bmad-create-epics-and-stories` step-03-create-stories.md.

- **Epic 1 stories: drafted but NOT yet approved or written to `epics.md`**
- Epics 2, 3, 4: not started

The 4 Epic 1 stories that were presented (need Alex's approval before writing to file):

**Story 1.1: Skill Skeleton and Append Proof-of-Concept**
Creates `workflow.md` (persona, hard constraints, conventions, step dispatch) + step file stubs. Proves bash heredoc append works before anything else. This is the most important story — must pass before building anything else.

**Story 1.2: Topic Folder Scaffolding**
`/tutor init` derives slug, creates `sessions/` and `notes/`, creates `dave-log-<slug>.md`, creates/verifies `../alex-and-dave.md`. All files with correct frontmatter and topic-scoped names.

**Story 1.3: Onboarding Interview and ZPD Calibration**
`/tutor init` conducts the structured interview (knowledge, goal, timeline), probes stated knowledge rather than accepting self-report, calibrates ZPD low by default, introduces dialogical note-writing standard.

**Story 1.4: Primer Generation**
`/tutor init` generates `dave-primer-<slug>.md` via research (or ingests user-supplied material), surfaces explicit uncertainty on niche topics, invites correction, surfaces file write errors immediately.

---

## No Blockers

All architectural open questions are resolved. The epics are approved. The only thing needed is Alex's attention to review and approve the Epic 1 stories, then continue through Epics 2–4.

---

## Exact Next Steps

1. **Resume `bmad-create-epics-and-stories`** — pick up at step-03-create-stories.md, Epic 1.
2. **Review the 4 Epic 1 stories above** — approve as-is, or request changes. Once approved, they get written to `_bmad-output/epics.md`.
3. **Continue through Epics 2–4** — story generation for `/tutor hello`, the active session, and `/tutor end`.
4. **Step 4: Final validation** — once all stories are written, the workflow runs final validation.
5. **Then: implementation** — run `bmad-dev-story` on Story 1.1 first (skill skeleton + append proof-of-concept). That's the first build target.
