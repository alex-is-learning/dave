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

## 2. Load `Dave Log - <topic>.md`

```bash
[ -f "Dave Log - ${TOPIC}.md" ] && echo "EXISTS" || echo "MISSING"
```

**If the log exists:** read it in full. You will use it in sections 3 and 5.

**If the log is missing:** this is a first session — the log was never created (or was deleted). Create it now using the same command as in `step-init.md` section 4:

```bash
TODAY=$(date '+%Y-%m-%dT%H:%M')
cat > "Dave Log - ${TOPIC}.md" << LOGEOF
---
date: ${TODAY}
tags: [Dave]
---

[[Alex and Dave]]

## Current A/B/U State

_Updated each session. Dave adds and promotes items here._

| Type | Content |
|------|---------|

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

From the `Dave Log - <topic>.md` you just read, locate the **"## Current A/B/U State"** section. Read the table there. Extract rows grouped by the `Type` column:

- **A** — new learnings that seem true and useful, but not yet empirically backed; prime targets for Socratic interrogation
- **B** — A items that have since received empirical backing; more settled
- **U** — things encountered that didn't land or resonate

Hold these three lists in your working context. They drive the action menu in section 6 and the entire session strategy.

If the table is empty (first session or no entries yet): note that the A/B/U state is blank — the session will start without pre-existing items.

---

## 4. Load `Dave Primer - <topic>.md`

```bash
[ -f "Dave Primer - ${TOPIC}.md" ] && echo "EXISTS" || echo "MISSING"
```

**If the primer exists:** read it in full. This is your primary knowledge source for the session. Do not surface its contents to Alex — use it to inform your questions, calibrate depth, and assess Alex's claims.

**If the primer is missing:** warn Alex and stop:

> `Dave Primer - ${TOPIC}.md` is missing. This usually means `/dave init` hasn't been run for this topic yet, or the primer was moved or deleted.
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

### 6a. Count session files vs log entries

```bash
FILE_COUNT=$(ls sessions/ 2>/dev/null | grep -c "^${TOPIC} Dave session " || echo 0)
LOG_COUNT=$(grep -c "^\*\*Session:\*\*" "Dave Log - ${TOPIC}.md" 2>/dev/null || echo 0)
```

If `FILE_COUNT` is 0: no previous sessions → skip to section 7.

### 6b. Compare counts and check last log block

**Case A — `FILE_COUNT == LOG_COUNT` and last log block has `Status: complete`:**
All good. Continue to section 7 without comment.

**Case B — `FILE_COUNT == LOG_COUNT` and last log block has `Status: incomplete`:**
The early exit was already recorded. Tell Alex:

> Your last session on **${TOPIC}** ended without a close ritual — no reflection was captured. Just so you know.

Then continue to section 7.

**Case C — `FILE_COUNT > LOG_COUNT`:**
A session was never closed. Find the unclosed transcript and extract its date from frontmatter:

```bash
LAST_FILE=$(ls sessions/ 2>/dev/null | grep "^${TOPIC} Dave session " | sort | tail -1)
MISSED_DATE=$(head -5 "sessions/${LAST_FILE}" | grep "^date:" | sed 's/date: //' | cut -c1-10)
MISSED_SESSION_N=$(echo "${LAST_FILE}" | sed 's/.*Dave session //' | sed 's/\.md//')
```

Append early exit block to `Dave Log - ${TOPIC}.md`:
```bash
cat >> "Dave Log - ${TOPIC}.md" << "EXITEOF"

**Session:** MISSED_DATE_PLACEHOLDER
**Status:** incomplete

---
EXITEOF
```

Insert incomplete row at top of `../Alex and Dave.md`:
```bash
NEW_ROW="| MISSED_DATE_PLACEHOLDER | [[TOPIC_PLACEHOLDER Dave session MISSED_SESSION_N_PLACEHOLDER]] | — | incomplete | — | — | — | — |"
awk -v row="$NEW_ROW" '/^\|---/{print; print row; next} {print}' "../Alex and Dave.md" > /tmp/dave_home.tmp && mv /tmp/dave_home.tmp "../Alex and Dave.md"
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
TODAY_FULL=$(date '+%Y-%m-%dT%H:%M')
TODAY_DATE=$(date '+%Y-%m-%d')
SESSION_N=$(ls sessions/ 2>/dev/null | grep -c "^${TOPIC} Dave session " || echo 0)
TRANSCRIPT="sessions/${TOPIC} Dave session ${SESSION_N}.md"

cat >> "${TRANSCRIPT}" << 'SESSEOF'
---
date: TODAY_FULL_PLACEHOLDER
tags: [Dave]
---

[[Alex and Dave]]

# TODAY_DATE_PLACEHOLDER

**Topic:** TOPIC_PLACEHOLDER

---

SESSEOF
```

Replace `TODAY_FULL_PLACEHOLDER`, `TODAY_DATE_PLACEHOLDER`, and `TOPIC_PLACEHOLDER` with the actual values before executing.

Store `TRANSCRIPT` and `SESSION_N` — every turn append in this session targets this file.

If the write fails, surface the error immediately and stop:
> Failed to create session transcript at ${TRANSCRIPT}: [error]. Stopping.

---

## 9. Action Menu

Present options derived from the A/B/U state you parsed in section 3.

**Build the menu dynamically:**

- For each A item in the current state → add: "Probe: [item]"
- For each U item in the current state → add: "Revisit: [item]"
- Always add: "Open new territory"

**If the A/B/U state is empty** (no items in any category): offer only "Open new territory" and note that this is a first session with a blank state.

**Example output with a populated state:**

> What would you like to work on today?
>
> 1. Probe: [A item 1]
> 2. Probe: [A item 2]
> 3. Revisit: [U item 1]
> 4. Open new territory
>
> Choose a number, or tell me what's on your mind.

Wait for Alex's choice. Once he chooses, the session is open. Load `./steps/step-session.md` for in-session behaviour from here forward.
