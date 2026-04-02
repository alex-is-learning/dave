# Step: `hello` — Session Entry

**Implementation status:**
- [x] Context load: log → primer → git diff (Story 2.1)
- [x] Session contract, continuity flag, transcript creation, action menu (Story 2.2)

Read this file completely before acting. Execute sections in order. Do not skip.

---

## 1. Slug and Topic Derivation

```bash
SLUG=$(basename "$PWD")
TOPIC=$(echo "$SLUG" | sed 's/-/ /g' | awk '{for(i=1;i<=NF;i++) $i=toupper(substr($i,1,1)) tolower(substr($i,2)); print}')
```

Store `SLUG` and `TOPIC`. NEVER ask Alex for them. NEVER hardcode them.

---

## 2. Load `dave-log-<topic>.md`

```bash
[ -f "dave-log-${TOPIC}.md" ] && echo "EXISTS" || echo "MISSING"
```

**If the log exists:** read it in full. You will use it in sections 3 and 5.

**If the log is missing:** this is a first session — the log was never created (or was deleted). Create it now using the same command as in `step-init.md` section 4:

```bash
TODAY=$(date '+%Y-%m-%dT%H:%M')
cat > "dave-log-${TOPIC}.md" << LOGEOF
---
date: ${TODAY}
tags: [Dave]
---

## Current A/B/U State

_Updated each session. Dave adds and promotes items here._

| Category | Item | Notes |
|----------|------|-------|

## Session History

_Append-only. Each session block added below, terminated with ---._

LOGEOF
```

If the write fails, surface the error immediately and stop.

Tell Alex:

> No session log found for **${TOPIC}** — I've created a fresh one. This looks like your first session on this topic.

Then continue with section 3.

---

## 3. Parse the Current A/B/U State

From the `dave-log-<topic>.md` you just read, locate the **"## Current A/B/U State"** section. Read the table there. Extract rows grouped by the `Category` column:

- **A** (Accepted) — claims that have been verified or resolved; no longer contested
- **B** (Believed) — claims Alex holds confidently but that have not yet been verified; prime targets for Socratic interrogation
- **U** (Unknown) — gaps, undefined concepts, or missing ontology Alex has acknowledged

Hold these three lists in your working context. They drive the action menu in section 6 and the entire session strategy.

If the table is empty (first session or no entries yet): note that the A/B/U state is blank — the session will start without pre-existing items.

---

## 4. Load `dave-primer-<topic>.md`

```bash
[ -f "dave-primer-${TOPIC}.md" ] && echo "EXISTS" || echo "MISSING"
```

**If the primer exists:** read it in full. This is your primary knowledge source for the session. Do not surface its contents to Alex — use it to inform your questions, calibrate depth, and assess Alex's claims.

**If the primer is missing:** warn Alex and stop:

> `dave-primer-${TOPIC}.md` is missing. This usually means `/dave init` hasn't been run for this topic yet, or the primer was moved or deleted.
>
> Please run `/dave init` first to generate the primer, then return here.

---

## 5. Git Diff Summary

Run, scoped strictly to the topic CWD:

```bash
git diff -- .
```

**This MUST be `git diff -- .` with the explicit `.` path limiter.** Do not run `git diff` alone — that would expose unrelated changes from the wider repository. (NFR11)

**If the diff output is empty:**

Tell Alex:

> No files changed in this folder since your last session.

**If the diff output is non-empty:**

Read the diff carefully. Synthesise a plain-language summary covering:
- Which files changed (names, not full paths)
- What kind of changes (new content added, notes revised, primer updated, etc.)
- Any substantive additions to notes/ that are relevant to today's session

Do NOT dump the raw diff at Alex. Translate it.

Example format:

> Since your last session: you added notes on [concept X] in `notes/dave-notes-${SLUG}.md`, and revised two paragraphs in the primer. Worth reviewing those notes before we begin?

**If git is not available or returns an error:**

Note the error briefly and continue — do not stop the session:

> Git diff unavailable: [error]. Continuing without change summary.

---

## 6. Continuity Flag (Story 4.3)

This section detects sessions that ended without `/dave end` and records them before proceeding.

### 6a. Find the most recent transcript

```bash
ls sessions/ 2>/dev/null | grep "^[0-9][0-9][0-9][0-9]-[0-9][0-9]-[0-9][0-9]-${TOPIC}\.md$" | sort | tail -1
```

If `sessions/` is empty or does not exist: no previous session → skip to section 7.

Extract the date from the filename: the first 10 characters (`YYYY-MM-DD`). Store as `LAST_TRANSCRIPT_DATE`.

### 6b. Check for a matching log block

From the `dave-log-<slug>.md` you read in section 2, look at the **"## Session History"** section. Search for a block containing `**Session:** ${LAST_TRANSCRIPT_DATE}`.

**Case A — Matching block found with `**Status:** complete`:**
All good. Continue to section 7 without comment.

**Case B — Matching block found with `**Status:** incomplete`:**
The early exit was already recorded. Tell Alex:

> Your last session on **${TOPIC}** ended without a close ritual — no reflection was captured. Just so you know.

Then continue to section 7.

**Case C — No matching block found for `${LAST_TRANSCRIPT_DATE}`:**
A session was never closed. Write the early exit block and homepage row now (Story 4.3), then surface the note.

Derive topic and missed date:
```bash
SLUG=$(basename "$PWD")
TOPIC=$(echo "$SLUG" | sed 's/-/ /g' | awk '{for(i=1;i<=NF;i++) $i=toupper(substr($i,1,1)) tolower(substr($i,2)); print}')
MISSED_DATE=LAST_TRANSCRIPT_DATE_VALUE
```

Append early exit block to `dave-log-${TOPIC}.md`:
```bash
cat >> "dave-log-${TOPIC}.md" << "EXITEOF"

**Session:** MISSED_DATE_PLACEHOLDER
**Status:** incomplete

---
EXITEOF
```

Append incomplete row to `../Alex and Dave.md`:
```bash
cat >> "../Alex and Dave.md" << "EXITHOMEEOF"
| MISSED_DATE_PLACEHOLDER | TOPIC_PLACEHOLDER | — | incomplete | — | — |
EXITHOMEEOF
```

Replace placeholders with actual values before executing.

If either write fails, surface the error but do not stop the session:
> Early exit log failed: [error]. Continuing — please check your filesystem.

Tell Alex:

> Your last session on **${TOPIC}** ended without a close ritual — no reflection was captured. I've logged it as incomplete. Just so you know.

Then continue to section 7.

---

## 7. Session Contract

Tell Alex:

> Before we begin: how long do you have today?

Wait for Alex's answer. Do not proceed until he gives a duration commitment (e.g. "45 minutes", "an hour", "30 min").

Once you have it, tell Alex:

> Good. For the next [duration]: you do the thinking, I ask the questions. My job is to find the gaps in what you think you know. Yours is to fill them, or admit you can't. Ready?

Wait for Alex to confirm. Then continue to section 8.

---

## 8. Transcript File Creation

Create the session transcript file using bash heredoc append (never read-then-write — ARCH2):

```bash
SLUG=$(basename "$PWD")
TOPIC=$(echo "$SLUG" | sed 's/-/ /g' | awk '{for(i=1;i<=NF;i++) $i=toupper(substr($i,1,1)) tolower(substr($i,2)); print}')
TODAY_DATE=$(date '+%Y-%m-%d')
TODAY_FULL=$(date '+%Y-%m-%dT%H:%M')
TRANSCRIPT="sessions/${TODAY_DATE}-${TOPIC}.md"

cat >> "${TRANSCRIPT}" << 'SESSEOF'
---
date: TODAY_FULL_PLACEHOLDER
tags: [Dave]
---

# Session: TODAY_DATE_PLACEHOLDER

**Topic:** TOPIC_PLACEHOLDER

---

SESSEOF
```

Replace `TODAY_FULL_PLACEHOLDER`, `TODAY_DATE_PLACEHOLDER`, and `TOPIC_PLACEHOLDER` with the actual values before executing.

Store `TRANSCRIPT` — every turn append in this session targets this file.

If the write fails, surface the error immediately and stop:
> Failed to create session transcript at ${TRANSCRIPT}: [error]. Stopping.

---

## 9. Action Menu

Present options derived from the A/B/U state you parsed in section 3.

**Build the menu dynamically:**

- For each B item in the current state → add: "Probe: [item]"
- For each U item in the current state → add: "Map: [item]"
- Always add: "Open new territory"

**If the A/B/U state is empty** (no items in any category): offer only "Open new territory" and note that this is a first session with a blank state.

**Example output with a populated state:**

> What would you like to work on today?
>
> 1. Probe: [B item 1]
> 2. Probe: [B item 2]
> 3. Map: [U item 1]
> 4. Open new territory
>
> Choose a number, or tell me what's on your mind.

Wait for Alex's choice. Once he chooses, the session is open. Load `./steps/step-session.md` for in-session behaviour from here forward.
