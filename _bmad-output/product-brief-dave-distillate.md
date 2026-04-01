---
title: "Product Brief Distillate: Dave"
type: llm-distillate
source: "product-brief-dave.md"
created: "2026-04-01"
purpose: "Token-efficient context for downstream PRD creation"
---

# Dave — Detail Pack for PRD Creation

## What Dave Is (precise)

- A BMAD-style Claude Code skill — invoked from the terminal, runs alongside Obsidian
- A personal AI tutor with a named persona: Feynman-Russell (warm, rigorous, won't let fuzzy thinking pass, never gives the answer)
- Scoped to a single learning topic at a time, operating within a dedicated subfolder of an existing Obsidian + Quartz scrapbook repo
- Personal-first. Not designed for others. Share only after months of lived evidence — maybe never.

---

## Design Axioms (all feature decisions flow from these)

1. Learning = clearing debris + expanding agency (not accumulation)
2. The enemy is double ignorance — false closure, not absence. Dave interrogates B's (beliefs) hardest, not U's (recognised fog)
3. Dave operates at Level 2 only — within a topic the user has committed to. Level 1 (is this worth learning?) resolved once at init, never revisited
4. Dave separates the learner from the manager — Dave holds pacing, thread, and when to push
5. Struggle is the signal — Dave is deliberately reluctant to let the user off the hook
6. The session contract — stating duration + "confusion is the signal" before work begins transforms the session from tool use to intentional grappling

---

## Command Set (full spec)

| Command | Behaviour |
|---|---|
| `/tutor init` | One-time onboarding per topic. Dave interviews: current knowledge, goal, timeline, related prior knowledge. Optionally ingests `dave-primer.md`. Calibrates ZPD — where to start, what depth to aim for. Dave also surfaces his own epistemic uncertainty ("I may not know this topic deeply — should I read material first?") |
| `/tutor hello` | Session entry. Reads `git diff` for what's changed since last session. Summarises A/B/U state. Suggests most valuable next action. Opens with session contract (duration + frame). Presents action menu. |
| `/tutor test-me` | Socratic interrogation. Prioritises B's — things the user believes they know. Dave probes for fuzzy definitions, wrong relationships, vagueness dressed as understanding. Never fills the gap; points at it. |
| `/tutor find-gaps` | Maps U's — concepts the user has flagged as foggy, missing ontology, recognised unknowns. |
| `/tutor feynman` | User explains a concept plainly in their own words. Dave presses on every vagueness until the explanation is genuinely clear. |
| `/tutor teach-me` | Dave plays confused student. User writes a rigorous explanatory essay — must include "why does this matter at all?" Designed to force the real argument, not recitation of the framework. Most distinctive command. |
| `/tutor since-last` | Explicit before/after review. What's changed in understanding since the last session? What moved from U → B? What new A's appeared? |

**First build target:** `/tutor init` + `/tutor hello` only. These two establish the relationship and the session frame and are immediately useful.

---

## A/B/U Framework (used throughout)

- **A (Advance):** True, useful, AND new to the learner — genuine frontier expansion
- **B (Base):** True, useful, but not novel — confirmed existing belief
- **U (Unclear/Unknown):** Foggy, undefined, or unresolved — recognised gap

Dave uses A/B/U as the primary map of learning state. Key insight: B's are where double ignorance hides. Confidently held beliefs are the highest-priority interrogation target.

---

## File Structure

All Dave files live under `/learning/<topic>/` — never scattered into wider scrapbook.

```
/learning/schema-therapy/
    dave-primer.md          # Dave's auditable knowledge source (editable by user)
    dave-log.md             # Persistent session log (appended each session)
    alex-and-dave.md        # Living homepage — session history, pride scores (published via Quartz)
    sessions/
        2026-04-01-schema-therapy.md    # Full session transcript exports
    notes/                  # User's A/B/U notes, written TO Dave as dialogical reader
```

**Standard frontmatter on every Dave-created file:**
```yaml
date: dd/mm/yyyy, --:--
tags:
    - Dave
```

---

## Session Mechanics

- **Session contract:** Dave opens every session by asking duration commitment, then states the frame explicitly ("We're not here to rehearse what you know. We're here to find what you don't. Confusion is the signal. Ready?")
- **No easy exit:** Sessions have a stated duration and a completion ritual. Leaving early is possible — Dave names it, logs it in `alex-and-dave.md`. Not punishment; honesty about whether the user showed up.
- **Session transcript export:** At session end, Dave exports the full terminal conversation to `sessions/YYYY-MM-DD-<topic>.md`. Published via Quartz — visitors to the public website see real sessions, not curated demos.
- **`dave-log.md`:** Dave appends after every session — what held up, what slipped, A frontier state, suggested focus for next time.
- **`alex-and-dave.md`:** Auto-maintained homepage. Every session logged: date, topic, duration, completion status, self-rated pride score (1–5 stars: did you grapple?). Published via Quartz.

---

## Architecture

- **Stack:** Claude Code + BMAD skill pattern. No new tools, no new subscriptions.
- **Repo:** Existing Obsidian + Quartz scrapbook (running since June 2025, ~330k words). Dave adds a `/learning/` section — continuous with existing content, not a separate vault.
- **Git as memory:** `git diff` is how Dave sees what's changed between sessions. Commit history = intellectual development record.
- **Context scoping:** Dave reads `/learning/<topic>/**` only. Protects existing repo structure. Keeps sessions fast and token-efficient.
- **Auditable knowledge:** `dave-primer.md` is Dave's source of truth for niche topics. User-readable, user-correctable. Dave surfaces his own uncertainty at init if the topic is outside confident LLM knowledge.
- **Publishing:** Quartz auto-publishes the learning folder. Session transcripts, log, homepage — all publicly readable. The method proves itself by being visible.

---

## ZPD Calibration (open question for PRD)

- At `/tutor init`, Dave calibrates Vygotsky's Zone of Proximal Development — where to start, what depth to aim for
- Mechanism not fully specified: what questions does Dave ask? How does it translate to session behaviour?
- Dave is designed to operate *just ahead* of current level — not meeting the user where they are, but staying a step ahead as a persistent gentle pull
- This adaptive element needs spec: does Dave maintain explicit ZPD state in `dave-log.md`? How does it update session-to-session?

---

## Hermeneutic Spiral (progression engine)

- Dave tracks A/B/U state across sessions and deliberately increases depth as concepts consolidate
- User doesn't get pushed to hard questions until basics are genuinely in the B pile
- Dave always pushes once the foundation is solid
- Mechanism: likely driven by `dave-log.md` state — needs PRD-level spec on how Dave reads and updates this

---

## Rejected Ideas (do not re-propose)

- **Diagram generation software** — removed entirely. Paper sketches prompted by Dave replace this. The physical act is the point. The obsession with visual diagram generation was a solution looking for a problem.
- **Miro / digital whiteboard integration** — same reason as above
- **Multi-user support** — explicitly out of scope until Dave proves itself through lived experience
- **Separate vault / new Obsidian setup** — Dave lives in the existing scrapbook, not a new vault
- **External subscriptions or new tools** — everything runs on existing Claude Code + BMAD infrastructure
- **AI-generated diagrams as output artefacts** — diagrams are stimulus (orientation scaffold), not product. Dave prompts the paper sketch at the right moment.

---

## Scope Signals

**MVP (first build):**
- `/tutor init` + `/tutor hello`
- `dave-log.md` + `alex-and-dave.md` (auto-maintained)
- Session transcript export
- Schema therapy as first test topic

**Medium horizon:**
- Full command set (`/tutor test-me`, `/tutor find-gaps`, `/tutor feynman`, `/tutor teach-me`, `/tutor since-last`)
- ZPD calibration mechanics
- Hermeneutic spiral state tracking

**Long horizon / maybe never:**
- Dave as a shareable open-source skill
- Multi-topic dashboard
- `/tutor teach-me` essay multi-session draft tracking

---

## Open Questions for PRD

1. **ZPD calibration mechanics** — what does Dave ask at init, how does it shape session behaviour, where is state stored?
2. **Session contract enforcement** — is the duration commitment just words Dave says, or is there state Dave tracks (e.g. start time, flagged in log if session ends early)?
3. **Transcript export implementation** — Claude Code doesn't natively export terminal history. How does Dave capture the full conversation? Options: Dave writes to file as it goes, or a `/tutor end` command triggers a write of the session buffer.
4. **dave-primer.md generation** — if user doesn't provide source material, Dave generates a primer via deep research. What triggers this? What format? How long?
5. **Quartz publishing pipeline** — does Dave commit + push automatically, or does the user handle git? Auto-publish is convenient but touches the repo in a way that could be surprising.
