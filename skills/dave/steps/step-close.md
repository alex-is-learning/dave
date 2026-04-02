# Step: `end` — Session Close & Logging

**Implementation status:**
- [x] A/B/U reflection + pride score (Story 4.1)
- [x] Log append + homepage update (Story 4.2)
- [x] Early exit block format defined; detection trigger in step-hello.md section 6 (Story 4.3)

Read this file completely before acting. Execute sections in order. Do not skip.

---

## 1. Setup

```bash
SLUG=$(basename "$PWD")
TOPIC=$(echo "$SLUG" | sed 's/-/ /g' | awk '{for(i=1;i<=NF;i++) $i=toupper(substr($i,1,1)) tolower(substr($i,2)); print}')
SESSION_DATE=$(date '+%Y-%m-%d')
SESSION_N=$(($(ls sessions/ 2>/dev/null | grep -c "^${TOPIC} Dave session ") - 1))
TRANSCRIPT="sessions/${TOPIC} Dave session ${SESSION_N}.md"
SESSION_START_STR=$(grep "^date:" "${TRANSCRIPT}" | sed 's/date: //')
SESSION_START=$(date -j -f '%Y-%m-%dT%H:%M' "${SESSION_START_STR}" '+%s' 2>/dev/null)
SESSION_END=$(date '+%s')
ELAPSED_MINS=$(( (SESSION_END - SESSION_START) / 60 ))
if [ -z "$SESSION_START" ] || [ "$ELAPSED_MINS" -le 0 ]; then
  ACTUAL_DURATION="unknown"
elif [ "$ELAPSED_MINS" -lt 60 ]; then
  ACTUAL_DURATION="${ELAPSED_MINS} min"
else
  ACTUAL_DURATION="$((ELAPSED_MINS / 60))h $((ELAPSED_MINS % 60))min"
fi
```

Store all variables. You'll need them throughout this step.

---

## 1a. Transcript Integrity Check

Before beginning the close ritual, verify the transcript has session content:

```bash
TURN_COUNT=$(grep -c "^\*\*Alex:\*\*\|^\*\*Dave:\*\*" "${TRANSCRIPT}" 2>/dev/null || echo 0)
```

**If `TURN_COUNT` is 0:** The per-turn append was not executed during this session. The transcript exists but contains only the frontmatter header — no actual turns were written. Reconstruct the full transcript now from your conversation context.

Write every **Alex:** and **Dave:** turn from this session (from after the action menu in `/dave hello`, up to but not including this close ritual) to the transcript using heredoc append:

```bash
cat >> "${TRANSCRIPT}" << 'RECONEOF'

**Alex:** [first message]

**Dave:** [first response]

[all subsequent turns in order]
RECONEOF
```

One heredoc is fine — write all turns in a single block. Use the exact messages from the conversation. Do not include the close ritual itself.

Tell Alex:
> Transcript was empty — I've reconstructed it from context. Continuing with close ritual.

If the write fails:
> Transcript reconstruction failed: [error]. Please check your filesystem.

Do not proceed after a write failure.

**If `TURN_COUNT` > 0:** Proceed normally — turns were already written during the session.

---

## 2. A/B/U Reflection (FR24)

Open with:

> Good. Let's close this out. I'll ask about each category in turn — be honest about what actually moved, not what you wish had moved.

Ask each question in sequence. Wait for a full response before asking the next.

### 2a. A items — new learnings

> What landed today — new things that seem true and useful that you'd put on the A list? Not things you've confirmed yet, just things that arrived and feel worth tracking.

Wait for Alex's response. "Nothing" is a valid answer.

### 2b. B items — A items that got empirical backing

> Did any existing A items receive empirical backing today — things you'd now move to B? What actually tested out?

Wait for Alex's response.

### 2c. U items — things that didn't land

> What came up that didn't land or resonate? Things you encountered but couldn't integrate yet?

Wait for Alex's response.

Hold all three responses in context. They go into the log in section 4.

---

## 3. Pride Score (FR25)

Ask:

> On a scale of 1 to 5 — how do you rate the quality of your grappling this session? Not whether you made progress. Whether you went in.
>
> 1 = going through the motions  
> 3 = engaged honestly  
> 5 = all the way in

Wait for Alex's answer.

Accept only an integer from 1 to 5. If Alex gives anything else:

> That's not a number between 1 and 5. Try again.

Do not proceed to section 4 until a valid integer is received.

---

## 4. Session Log Append (FR26, ARCH2, NFR5)

Compose a session summary: 1–2 sentences describing what was covered and what most changed. Factual, no editorializing.

`ACTUAL_DURATION` was computed in section 1 from the transcript's start timestamp vs. now.

Append the session block to `Dave Log - <topic>.md` using bash heredoc append — never read-then-write:

```bash
cat >> "Dave Log - ${TOPIC}.md" << "CLOSEEOF"

**Session:** SESSION_DATE_PLACEHOLDER
**Duration:** ACTUAL_DURATION_PLACEHOLDER
**Status:** complete
**Pride:** SCORE_PLACEHOLDER/5

**A:** A_REFLECTION_PLACEHOLDER
**B:** B_REFLECTION_PLACEHOLDER
**U:** U_REFLECTION_PLACEHOLDER

**Summary:** SUMMARY_PLACEHOLDER

---
CLOSEEOF
```

Replace every placeholder with the actual values before executing. Do not leave placeholders in the file.

If the write fails:
> Log append failed: [error]. Session not recorded. Please check your filesystem before continuing.

Do not proceed to section 5 after a write failure. (NFR7)

---

## 5. A/B/U State Update (ARCH2)

If the reflection in section 2 surfaced any new items — new B's, newly confirmed A's, or new U's — append them to the Current A/B/U State table in `Dave Log - <topic>.md`:

```bash
cat >> "Dave Log - ${TOPIC}.md" << "STATEEOF"
| A | ITEM_PLACEHOLDER |
STATEEOF
```

Run one heredoc per new item. Use `| A |`, `| B |`, or `| U |` as appropriate. Repeat for each item.

**Note on promotions (B→A):** Append-only means you cannot remove a B row when it becomes A. Add the new A row. Alex can manually remove the old B row between sessions if he wants a clean table. The log block in section 4 contains the full reflection — the table is a working reference, not the canonical record.

If there are no new items to add, skip this section.

---

## 6. Homepage Update (FR27, ARCH2, NFR6)

Insert a new row at the top of the session table in `../Alex and Dave.md` (newest sessions first):

```bash
NEW_ROW="| SESSION_DATE_PLACEHOLDER | [[TOPIC_PLACEHOLDER Dave session SESSION_N_PLACEHOLDER]] | DURATION_PLACEHOLDER | complete | SCORE_PLACEHOLDER/5 | A_SUMMARY_PLACEHOLDER | B_SUMMARY_PLACEHOLDER | U_SUMMARY_PLACEHOLDER |"
awk -v row="$NEW_ROW" '/^\|---/{print; print row; next} {print}' "../Alex and Dave.md" > /tmp/dave_home.tmp && mv /tmp/dave_home.tmp "../Alex and Dave.md"
```

Keep each A/B/U summary to one short phrase. The full reflection is in the log; this is the index row.

Replace every placeholder with actual values before executing. `SESSION_N` and `TRANSCRIPT` were derived in section 1.

If the write fails:
> Homepage update failed: [error]. The log block was written successfully — only the homepage row is missing. Please check your filesystem.

(NFR7)

---

## 7. Close


Tell Alex:

> Done. Session logged.

---

## Early Exit Block Format (Story 4.3)

When a session ends without `/dave end` being called, step-hello.md section 6 detects this on the next `/dave hello` and writes the following early exit block.

**Log block (append to `Dave Log - <topic>.md`):**

```bash
cat >> "Dave Log - ${TOPIC}.md" << "EXITEOF"

**Session:** MISSED_DATE_PLACEHOLDER
**Status:** incomplete

---
EXITEOF
```

**Homepage row (insert at top of `../Alex and Dave.md`):**

```bash
NEW_ROW="| MISSED_DATE_PLACEHOLDER | [[TOPIC_PLACEHOLDER Dave session SESSION_N_PLACEHOLDER]] | — | incomplete | — | — | — | — |"
awk -v row="$NEW_ROW" '/^\|---/{print; print row; next} {print}' "../Alex and Dave.md" > /tmp/dave_home.tmp && mv /tmp/dave_home.tmp "../Alex and Dave.md"
```

The partial transcript from the missed session remains intact. No reflection or pride score is captured for an early exit. (NFR4, FR28)
