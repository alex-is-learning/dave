---
stepsCompleted:
  - step-01-validate-prerequisites
  - step-02-design-epics
inputDocuments:
  - _bmad-output/prd.md
  - _bmad-output/architecture.md
---

# Dave - Epic Breakdown

## Overview

This document provides the complete epic and story breakdown for Dave, decomposing the requirements from the PRD and Architecture into implementable stories.

## Requirements Inventory

### Functional Requirements

FR1: Dave can conduct a structured onboarding interview to assess the learner's current knowledge, goals, and timeline for a new topic.
FR2: Dave can calibrate ZPD based on demonstrated knowledge from the interview, defaulting to a lower calibration when uncertain.
FR3: Dave can generate a topic primer (dave-primer-<topic>.md) via deep research when no source material is provided.
FR4: Dave can ingest user-provided source material as the basis for a topic primer when supplied at init.
FR5: Dave can surface his own epistemic uncertainty about niche topics explicitly, rather than presenting potentially incorrect content with false confidence.
FR6: Dave can scaffold the full topic folder structure (sessions/, notes/, all required files) at init.
FR7: Dave can derive the topic slug from the CWD folder name without requiring manual input.
FR8: Dave can read git diff -- . scoped to the topic CWD and present a meaningful summary of what has changed since the last session.
FR9: Dave can read and summarise the current A/B/U state from dave-log-<topic>.md.
FR10: Dave can open every session with an explicit session contract: duration commitment from the learner, followed by the session frame statement.
FR11: Dave can present an action menu at session start, based on the current A/B/U state and session history.
FR12: Dave can surface the fact that the previous session ended early, as a continuity reference at the next session start.
FR13: Dave can probe confidently held beliefs (B's) with targeted Socratic questions, prioritising B's over recognised gaps (U's).
FR14: Dave can withhold direct answers when the learner asks for them, redirecting to the gap instead.
FR15: Dave can identify vagueness or hedging in learner explanations and name the specific imprecision.
FR16: Dave can map recognised gaps (U's) and missing ontology — concepts the learner has flagged as unclear or undefined.
FR17: Dave can conduct a Feynman-style session where the learner explains a concept in plain language and Dave presses on every vague point. [POST-MVP]
FR18: Dave can play a confused student, prompting the learner to write a rigorous explanatory essay including "why does this matter at all?" [POST-MVP]
FR19: Dave can conduct an explicit before/after review of what has changed in understanding since the last session. [POST-MVP]
FR20: Dave can write session content to a transcript file incrementally as the session progresses (append-only, never overwrites).
FR21: Dave can create a new transcript file per session with a dated filename (sessions/YYYY-MM-DD-<topic>.md).
FR22: Dave can produce transcript files in clean, legible markdown with session headers and clearly marked turns, readable without additional context.
FR23: Dave can leave a partial transcript intact and valid when a session ends early — no data loss on early exit.
FR24: Dave can prompt the learner through an end-of-session A/B/U reflection, asking about each category in turn with open text input.
FR25: Dave can capture a self-rated pride score (1–5) at session end.
FR26: Dave can append a structured session block to dave-log-<topic>.md after every session, including A/B/U reflection content, pride score, and session summary.
FR27: Dave can update alex-and-dave.md after every session with: date, topic, duration, completion status, pride score, and A/B/U reflection summary.
FR28: Dave can log early exits with incomplete status and omit pride score when session ends without reflection.
FR29: Dave can read dave-primer-<topic>.md as the primary knowledge source for session content.
FR30: Dave can read dave-log-<topic>.md to retrieve A/B/U history and prior session summaries.
FR31: Dave can read files in the notes/ subfolder and incorporate them into session context. Notes are dialogical — written to Dave as a rigorous reader.
FR32: Dave can create all files with standard frontmatter (date, tags: [Dave]).
FR33: Dave can resolve and write to alex-and-dave.md at the /learning/ level from within any topic CWD.
FR34: Dave can create sessions/ and notes/ subdirectories if they do not exist.
FR35: Dave can name all topic-scoped files using the topic slug to ensure Obsidian vault-wide filename uniqueness.
FR36: Dave can prompt the learner to sketch concept relationships on paper at appropriate moments during a session.
FR37: Dave can surface the dialogical note-writing standard when introducing the notes/ subfolder at init.

### NonFunctional Requirements

NFR1: Session startup (/tutor hello) completes git diff read and log parse without noticeable delay before the session contract is presented.
NFR2: Context loading is scoped to /learning/<topic>/ only. Dave must not load the full scrapbook vault (~330k words) at any point.
NFR3: Incremental transcript writes must not interrupt conversational flow — each write is a background file append, not a blocking operation.
NFR4: Session transcripts are append-only. No write operation may overwrite or truncate an existing transcript. A failed or interrupted write leaves previous content intact.
NFR5: dave-log-<topic>.md entries are discrete, self-contained blocks. A failed append must not corrupt previously written entries.
NFR6: alex-and-dave.md updates are structured so a failed write does not corrupt the existing session table.
NFR7: Dave must never silently fail on a file write. If a write fails, Dave surfaces the error immediately.
NFR8: All Dave-created markdown files must be valid Obsidian markdown — correct frontmatter format, no syntax that breaks Obsidian rendering.
NFR9: All Dave-created filenames must be unique across the Obsidian vault. Topic-scoped naming (dave-log-<topic>.md) is the enforced mechanism.
NFR10: Session transcripts and alex-and-dave.md must render correctly in Quartz without additional post-processing. Standard markdown only — no Obsidian-specific syntax in Quartz-published files.
NFR11: git diff -- . must reliably scope to the topic CWD. Dave must not rely on global git state that could expose unrelated changes.

### Additional Requirements

- ARCH1: Skill installed at ~/.claude/skills/dave/ using workflow.md + steps/ directory structure.
- ARCH2: Bash heredoc append used for all writes to transcript, log, and homepage — never read-then-write (eliminates overwrite risk).
- ARCH3: Topic slug derived from `basename "$PWD"` — never hardcoded or asked for manually.
- ARCH4: CWD depth contract — Claude Code always started one level below /learning/; ../alex-and-dave.md verified at /tutor init (created if absent).
- ARCH5: Context loading order at /tutor hello: dave-log → dave-primer → git diff.
- ARCH6: dave-log two-section structure: Current A/B/U State (updated each session) + Session History (append-only).
- ARCH7: /tutor end is an MVP command — triggers the full close ritual (reflection → pride score → log append → homepage update).
- ARCH8: workflow.md contains persona (Feynman-Russell), hard constraints (never give the answer), shared conventions, and step dispatch — loaded at every command invocation.

### UX Design Requirements

N/A — Dave is a CLI tool (Claude Code skill). No UI.

### FR Coverage Map

| FR | Epic | Description |
|---|---|---|
| FR1 | Epic 1 | Onboarding interview |
| FR2 | Epic 1 | ZPD calibration |
| FR3 | Epic 1 | Primer generation |
| FR4 | Epic 1 | Ingest user-provided material |
| FR5 | Epic 1 | Surface epistemic uncertainty |
| FR6 | Epic 1 | Scaffold folder structure |
| FR7 | Epic 1 | Slug from CWD |
| FR8 | Epic 2 | Git diff summary |
| FR9 | Epic 2 | A/B/U state summary |
| FR10 | Epic 2 | Session contract |
| FR11 | Epic 2 | Action menu |
| FR12 | Epic 2 | Surface early exit |
| FR13 | Epic 3 | Probe B's |
| FR14 | Epics 1+3 | Withhold answers (workflow.md + in-session) |
| FR15 | Epic 3 | Identify vagueness |
| FR16 | Epic 3 | Map U's |
| FR17 | POST-MVP | Feynman session |
| FR18 | POST-MVP | Teach-me session |
| FR19 | POST-MVP | Since-last review |
| FR20 | Epic 3 | Incremental transcript append |
| FR21 | Epic 2 | Create session file at start |
| FR22 | Epic 3 | Clean transcript format |
| FR23 | Epic 3 | Partial transcript valid on exit |
| FR24 | Epic 4 | A/B/U reflection prompts |
| FR25 | Epic 4 | Pride score capture |
| FR26 | Epic 4 | Log append |
| FR27 | Epic 4 | Homepage update |
| FR28 | Epic 4 | Early exit logging |
| FR29 | Epic 2 | Read primer at session start |
| FR30 | Epic 2 | Read log at session start |
| FR31 | Epic 3 | Read notes on demand |
| FR32 | Epic 1 | Standard frontmatter |
| FR33 | Epic 1 | Path resolution for alex-and-dave.md |
| FR34 | Epic 1 | Create sessions/ and notes/ |
| FR35 | Epic 1 | Topic-scoped filenames |
| FR36 | Epic 3 | Paper sketch prompt |
| FR37 | Epic 1 | Dialogical note-writing standard |

## Epic List

### Epic 1: `/tutor init` — Topic Initialisation
Alex can start learning any new topic with Dave. Dave conducts the onboarding interview, calibrates ZPD, generates a primer, scaffolds the folder structure, and sets up `alex-and-dave.md`. Also contains cross-cutting foundations: `workflow.md` (persona, hard constraints, shared conventions), bash heredoc proof-of-concept, slug derivation, and path resolution.
**FRs covered:** FR1, FR2, FR3, FR4, FR5, FR6, FR7, FR14 (workflow.md), FR32, FR33, FR34, FR35, FR37
**ARCHs:** ARCH1, ARCH2, ARCH3, ARCH4, ARCH8
**NFRs:** NFR7, NFR8, NFR9

### Epic 2: `/tutor hello` — Session Entry
Alex can start a session on an existing topic. Dave shows git diff, summarises A/B/U state, flags early exit from last time, presents session contract, gets duration commitment, creates transcript file, and offers action menu.
**FRs covered:** FR8, FR9, FR10, FR11, FR12, FR21, FR29, FR30
**ARCHs:** ARCH5, ARCH6
**NFRs:** NFR1, NFR2, NFR11

### Epic 3: Active Session — Socratic Interaction & Transcript
Alex can grapple with the topic. Dave interrogates B's, calls out vagueness, maps U's, prompts paper sketches, reads notes on demand, and every exchange is captured incrementally to the transcript as it happens.
**FRs covered:** FR13, FR14 (in-session), FR15, FR16, FR20, FR22, FR23, FR31, FR36
**ARCHs:** ARCH2 (per-turn heredoc)
**NFRs:** NFR3, NFR4

### Epic 4: `/tutor end` — Session Close & Logging
Alex has a permanent, public record of every session. Dave prompts A/B/U reflection, captures pride score, appends structured block to log, updates homepage table, handles early exits with incomplete status.
**FRs covered:** FR24, FR25, FR26, FR27, FR28
**ARCHs:** ARCH7
**NFRs:** NFR5, NFR6, NFR8, NFR9, NFR10
