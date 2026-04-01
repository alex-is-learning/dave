---
stepsCompleted:
  - step-01-validate-prerequisites
  - step-02-design-epics
  - step-03-create-stories (epic-1)
  - step-03-create-stories (epic-2)
  - step-03-create-stories (epic-3)
  - step-03-create-stories (epic-4)
  - step-04-final-validation
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

---

## Epic 1: `/tutor init` — Topic Initialisation

Alex can start learning any new topic with Dave. Dave conducts the onboarding interview, calibrates ZPD, generates a primer, scaffolds the folder structure, and sets up `alex-and-dave.md`. Also contains cross-cutting foundations: `workflow.md` (persona, hard constraints, shared conventions), bash heredoc proof-of-concept, slug derivation, and path resolution.

### Story 1.1: Skill Skeleton and Append Proof-of-Concept

As Alex,
I want `workflow.md` installed at `~/.claude/skills/dave/` with the Feynman-Russell persona, hard constraints, shared conventions, and step dispatch — plus stub step files and a verified bash heredoc append,
So that the skill foundation is in place and the highest-risk write mechanism is proven before any feature is built on top of it.

**Acceptance Criteria:**

**Given** the skills directory exists
**When** the skill files are created
**Then** `~/.claude/skills/dave/workflow.md` exists containing: Feynman-Russell persona, the "never give the answer" hard constraint, shared conventions (slug from `basename "$PWD"`, frontmatter format `date: DD/MM/YYYY, HH:MM` + `tags: [Dave]`, turn format `**Alex:**` / `**Dave:**`), and step dispatch for all four MVP commands (`/tutor init`, `/tutor hello`, `/tutor end`, in-session)

**Given** `workflow.md` is created
**When** step stubs are written
**Then** `steps/step-init.md`, `steps/step-hello.md`, `steps/step-session.md`, `steps/step-close.md` all exist with placeholder content

**Given** the stubs exist
**When** a bash heredoc append is executed to a test file
**Then** the content is appended and the previous content is unaffected

**Given** a second append runs on the same test file
**When** complete
**Then** both appends are present — the file is not truncated or overwritten

### Story 1.2: Topic Folder Scaffolding

As Alex,
I want `/tutor init` to derive the topic slug from the CWD, scaffold `sessions/` and `notes/`, create `dave-log-<slug>.md` with the correct two-section schema, and verify or create `../alex-and-dave.md`,
So that the topic's file structure is ready before the interview begins.

**Acceptance Criteria:**

**Given** I run `/tutor init` from within `/learning/<topic>/`
**When** slug derivation runs
**Then** the slug equals the CWD folder name via `basename "$PWD"` — never hardcoded or asked for manually

**Given** the slug is derived
**When** folder scaffolding runs
**Then** `sessions/` and `notes/` directories exist in the topic CWD

**Given** scaffolding runs
**When** `dave-log-<slug>.md` is created
**Then** it has correct frontmatter and two sections: "Current A/B/U State" (updated each session) and "Session History" (append-only blocks)

**Given** scaffolding runs
**When** `../alex-and-dave.md` is checked
**Then** if it exists Dave proceeds; if absent Dave creates it with a header and empty session table

**Given** `../../alex-and-dave.md` exists instead of `../alex-and-dave.md`
**When** Dave checks the path
**Then** Dave warns and stops — does not proceed

**And** any file write failure is surfaced immediately; Dave does not continue silently (NFR7)
**And** all created filenames include the topic slug for vault-wide uniqueness (NFR9)
**And** all files have valid Obsidian markdown frontmatter (NFR8)

### Story 1.3: Onboarding Interview and ZPD Calibration

As Alex,
I want `/tutor init` to conduct a structured interview that probes my actual knowledge rather than accepting self-report, calibrate my ZPD conservatively, and introduce the dialogical note-writing standard,
So that Dave has an accurate baseline before any Socratic work begins.

**Acceptance Criteria:**

**Given** folder scaffolding is complete (Story 1.2)
**When** the interview begins
**Then** Dave asks about current knowledge, learning goal, and timeline in structured sequence

**Given** I state a knowledge level
**When** Dave probes
**Then** Dave asks at least one follow-up that tests what I actually know, rather than accepting self-report at face value

**Given** uncertainty about my level from interview responses
**When** ZPD is calibrated
**Then** Dave defaults to a lower calibration (assumes less, not more)

**Given** the interview is complete
**When** Dave introduces `notes/`
**Then** Dave presents the dialogical note-writing standard: notes are written to Dave as a rigorous reader, not as personal reminders

**And** Dave never gives a subject-matter answer during the interview, regardless of how any question is phrased (hard constraint from workflow.md)

### Story 1.4: Primer Generation

As Alex,
I want `/tutor init` to generate `dave-primer-<slug>.md` via research (or ingest user-supplied material if I provide it), with explicit uncertainty surfaced for niche topics and an invitation to correct errors,
So that Dave has a reliable knowledge source for subsequent sessions.

**Acceptance Criteria:**

**Given** no source material is provided at init
**When** primer generation runs
**Then** Dave conducts research and writes `dave-primer-<slug>.md` to the topic CWD

**Given** source material is provided at init
**When** primer generation runs
**Then** Dave ingests the supplied material as the primer basis instead of researching independently

**Given** the primer is generated on a niche topic or Dave is uncertain about any section
**When** the primer is written
**Then** Dave explicitly flags that uncertainty in the relevant section — no false confidence

**Given** the primer is complete
**Then** Dave invites Alex to review and correct errors before proceeding

**And** `dave-primer-<slug>.md` has correct frontmatter and a topic-scoped filename (NFR8, NFR9)
**And** any file write failure is surfaced immediately (NFR7)

---

## Epic 2: `/tutor hello` — Session Entry

Alex can start a session on an existing topic. Dave shows git diff, summarises A/B/U state, flags early exit from last time, presents session contract, gets duration commitment, creates transcript file, and offers action menu.

### Story 2.1: Context Load — Git Diff, Primer, and Log

As Alex,
I want `/tutor hello` to load dave-log, dave-primer, and git diff in order, parse the current A/B/U state, and present a meaningful summary of what has changed since the last session,
So that Dave is fully oriented before the session contract is presented.

**Acceptance Criteria:**

**Given** I run `/tutor hello` from within `/learning/<topic>/`
**When** context loading begins
**Then** Dave loads in order: `dave-log-<slug>.md` → `dave-primer-<slug>.md` → `git diff -- .` (ARCH5)

**Given** context is loaded
**When** A/B/U state is parsed from `dave-log-<slug>.md`
**Then** Dave correctly identifies the current items in each category (A = accepted, B = believed, U = unknown) from the "Current A/B/U State" section

**Given** git diff runs
**When** complete
**Then** the diff is scoped to the topic CWD only (`git diff -- .`) — no unrelated changes from the wider repo are surfaced (NFR11)

**Given** there are changes in the git diff
**When** Dave presents the summary
**Then** Dave gives a meaningful description of what changed — not a raw patch dump

**Given** there are no changes in the git diff
**When** Dave presents the summary
**Then** Dave notes that no files changed since the last session

**And** context loading completes without noticeable delay before the session contract is presented (NFR1)
**And** context is scoped to `/learning/<topic>/` only — the full vault is never loaded (NFR2)

### Story 2.2: Session Contract, Continuity Flag, and Action Menu

As Alex,
I want `/tutor hello` to flag if the previous session ended early, get my duration commitment, create the session transcript file, and present the action menu,
So that the session is formally opened with a clear frame and I can immediately direct where to focus.

**Acceptance Criteria:**

**Given** context is loaded (Story 2.1)
**When** Dave checks the previous session block in `dave-log-<slug>.md`
**Then** if the previous session has incomplete status, Dave surfaces this as a continuity note before the contract (FR12)

**Given** the continuity check is done
**When** the session contract is presented
**Then** Dave states the session frame and asks for a duration commitment before proceeding (FR10)

**Given** a duration commitment is received
**When** the transcript file is created
**Then** a new file `sessions/YYYY-MM-DD-<slug>.md` is created with correct frontmatter and a session header (FR21)

**Given** the transcript file exists
**When** the action menu is displayed
**Then** Dave presents options derived from the current A/B/U state — e.g. probe a B, map a U, open new territory (FR11)

**And** the transcript file creation uses bash heredoc append — no read-then-write (ARCH2)
**And** the transcript filename is unique and topic-scoped (NFR9)
**And** any file write failure is surfaced immediately (NFR7)

---

## Epic 3: Active Session — Socratic Interaction & Transcript

Alex can grapple with the topic. Dave interrogates B's, calls out vagueness, maps U's, prompts paper sketches, reads notes on demand, and every exchange is captured incrementally to the transcript as it happens.

### Story 3.1: Incremental Transcript Append

As Alex,
I want every Dave turn appended to the session transcript immediately via bash heredoc, with the transcript remaining valid if the session ends early,
So that a complete, append-only record of the session exists even if I stop mid-conversation.

**Acceptance Criteria:**

**Given** a session transcript file exists (created in Story 2.2)
**When** Dave generates a response
**Then** that response is immediately appended to the transcript as `**Dave:** [response]` with one blank line between turns

**Given** Alex sends a message
**When** it is appended
**Then** it is written as `**Alex:** [message]` — same format, same blank line convention

**Given** a session ends abruptly without `/tutor end`
**When** the transcript is read afterwards
**Then** all turns up to the interruption are intact and the file is valid markdown

**And** each append uses bash heredoc — no read-then-write at any point (ARCH2)
**And** a failed append is surfaced immediately — previous content is never corrupted (NFR4)
**And** transcript writes do not interrupt conversational flow (NFR3)

### Story 3.2: Socratic Interrogation — Probing B's and Naming Vagueness

As Alex,
I want Dave to prioritise probing my believed-but-unverified claims (B's) with targeted Socratic questions, and to call out specific vagueness or hedging in my explanations rather than letting it pass,
So that my false closures are surfaced and I am forced to confront gaps I didn't know I had.

**Acceptance Criteria:**

**Given** the current A/B/U state contains B items
**When** Dave chooses what to engage with
**Then** Dave prioritises probing B's before addressing U's

**Given** Dave is probing a B
**When** a question is posed
**Then** the question is targeted and Socratic — it points at a specific gap without supplying the answer (FR13)

**Given** Alex's explanation contains vagueness or hedging
**When** Dave responds
**Then** Dave names the specific imprecision — not generic encouragement, not the answer (FR15)

**Given** Alex directly asks Dave for the answer to a subject-matter question
**When** during any point in the session
**Then** Dave withholds the answer and redirects to the gap instead (FR14)

**And** all exchanges following these rules are appended to the transcript (Story 3.1)

### Story 3.3: Mapping U's, Notes on Demand, and Paper Sketch Prompts

As Alex,
I want Dave to track recognised gaps and missing concepts (U's), read my notes on demand and incorporate them into the session, and prompt me to sketch concept relationships on paper at the right moments,
So that my unknown unknowns are made visible and embodied learning is woven into the session.

**Acceptance Criteria:**

**Given** a gap or undefined concept is identified during the session
**When** Dave responds
**Then** Dave names it as a U and notes it for the A/B/U state — missing ontology is mapped, not glossed over (FR16)

**Given** Alex references or requests a note from `notes/`
**When** Dave reads the file
**Then** Dave incorporates the note content into the session context and treats it as a rigorous argument addressed to Dave (FR31)

**Given** a conceptual relationship would benefit from visual representation
**When** Dave judges the moment is right
**Then** Dave prompts Alex to sketch the relationship on paper before continuing (FR36)

**And** all exchanges are appended to the transcript (Story 3.1)

---

## Epic 4: `/tutor end` — Session Close & Logging

Alex has a permanent, public record of every session. Dave prompts A/B/U reflection, captures pride score, appends structured block to log, updates homepage table, handles early exits with incomplete status.

### Story 4.1: A/B/U Reflection and Pride Score

As Alex,
I want `/tutor end` to guide me through a structured A/B/U reflection and capture a self-rated pride score (1–5),
So that each session ends with explicit consolidation of what moved and how I felt about the work.

**Acceptance Criteria:**

**Given** I type `/tutor end`
**When** the close ritual begins
**Then** Dave asks about each A/B/U category in turn — what moved, what is newly believed, what gaps were named — with open text input for each

**Given** the reflection is complete
**When** Dave asks for the pride score
**Then** Dave accepts an integer 1–5 and does not proceed until a valid score is given (FR25)

**Given** a valid pride score is received
**Then** Dave holds the reflection content and score ready for the log append (Story 4.2)

### Story 4.2: Session Log Append and Homepage Update

As Alex,
I want `/tutor end` to append a structured session block to `dave-log-<slug>.md` and update the session table in `alex-and-dave.md`,
So that every session is permanently recorded and the homepage always reflects current progress.

**Acceptance Criteria:**

**Given** reflection and pride score are captured (Story 4.1)
**When** the log append runs
**Then** a structured block is appended to the "Session History" section of `dave-log-<slug>.md` containing: date, duration, A/B/U reflection content, pride score, and session summary — terminated with `---` (FR26, ARCH6)

**Given** the log block is appended
**When** the homepage update runs
**Then** `../alex-and-dave.md` is updated with a new row: date, topic, duration, completion status (complete), pride score, and A/B/U reflection summary (FR27)

**And** both writes use bash heredoc append — no read-then-write (ARCH2)
**And** a failed log append does not corrupt previously written entries (NFR5)
**And** a failed homepage write does not corrupt the existing session table (NFR6)
**And** all output files are valid Obsidian markdown and render correctly in Quartz (NFR8, NFR10)
**And** any write failure is surfaced immediately (NFR7)

### Story 4.3: Early Exit Logging

As Alex,
I want Dave to log sessions that end without `/tutor end` with incomplete status and no pride score,
So that abandoned sessions are recorded honestly and continuity is preserved for the next session.

**Acceptance Criteria:**

**Given** a session ends without `/tutor end` being typed
**When** `/tutor hello` is next run
**Then** Dave surfaces the fact that the previous session has incomplete status as a continuity note (FR12, FR28)

**Given** Dave appends an early exit block to `dave-log-<slug>.md`
**When** the block is written
**Then** it contains: date, incomplete status, no pride score, and ends with `---`

**Given** `alex-and-dave.md` is updated for an early exit
**When** the row is written
**Then** completion status is "incomplete" and pride score field is blank

**And** the partial transcript from the abandoned session remains intact (NFR4)
**And** any write failure is surfaced immediately (NFR7)
