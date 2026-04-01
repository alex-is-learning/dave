---
title: "Product Brief: Dave"
status: "complete"
created: "2026-04-01"
updated: "2026-04-01"
inputs:
  - "_bmad-output/brainstorming/brainstorming-session-2026-04-01-1200.md"
  - "_bmad-output/research/domain-ai-visual-active-learning-research-2026-04-01.md"
---

# Dave — An AI Tutor That Refuses to Let You Off the Hook

## The Problem

You've been reading about schema therapy for three weeks. You feel like you're getting it. But when someone asks you to explain it, the words come out soft and imprecise. You thought you understood it. You didn't — you understood the feeling of reading about it.

This is the fluency illusion: the false sense of understanding that comes from having information in front of you. Most AI tools make this worse. Ask an AI about schema therapy and it gives you a clear, confident answer — and you feel like you now know it, without doing any of the grappling.

The real obstacle to learning isn't missing information. It's **double ignorance**: believing you understand something you don't, building a mental model that feels complete but has critical gaps you can't see because you don't know they're there. Double ignorance is a wall, not an absence.

A good tutor's job is to find those walls and make them visible. No existing tool does this reliably.

---

## The Solution: Dave

Dave is a BMAD-style Claude Code skill — a personal AI tutor that lives inside your existing Obsidian scrapbook repo and is invoked from the terminal alongside it.

**Persona:** Feynman-Russell. Warm, delighted by genuine understanding, rigorous without cruelty. Dave doesn't let fuzzy reasoning pass — not to win, but because clear thinking is a form of respect. He never gives you the answer. He points at the gap and waits.

**Core design principle:** Dave's job is to clear debris, not deliver content. Learning = removing obstruction + expanding agency. When you genuinely understand schema therapy, thoughts that were previously unavailable become thinkable. Dave measures that, not whether you can recite the framework.

---

## How It Works

### The Session

Dave operates at the level of a learning topic. You invoke `/tutor hello` — Dave greets you, checks what you've written since the last session via `git diff`, summarises your current A/B/U state (A: new frontier, B: confirmed beliefs, U: recognised fog — a metacognitive framework for honest knowledge-mapping), and asks what you're committing to today.

The session opens with a contract: duration + the frame. *"We're not here to rehearse what you know. We're here to find what you don't. Confusion is the signal. Ready?"*

### The Commands

| Command | What Happens |
|---|---|
| `/tutor init` | Dave interviews you: current knowledge, goals, timeline. Optionally ingests a primer (`dave-primer.md`). Calibrates ZPD. |
| `/tutor hello` | Session entry. Git diff review, learning state summary, action menu. |
| `/tutor test-me` | Socratic interrogation — Dave probes your B's (what you believe you know) first. |
| `/tutor find-gaps` | Maps your U's — concepts you've recognised as foggy, missing ontology. |
| `/tutor feynman` | You explain a concept plainly. Dave presses on every vagueness. |
| `/tutor teach-me` | Dave plays confused student. You write a rigorous essay explaining the topic, including "why does this matter at all?" |
| `/tutor since-last` | Explicit review of what's changed in understanding since the last session. |

### The Architecture

- **No new tools.** Claude Code + BMAD skill pattern. Terminal alongside Obsidian.
- **Lives in the scrapbook.** `/learning/<topic>/` subfolder within the existing repo (330k words, Obsidian + Quartz, running since June 2025). Quartz publishing means the learning journal is automatically public.
- **Git as learning memory.** `git diff` gives Dave a precise view of what's changed between sessions. Commit history is the intellectual development record.
- **Dedicated subfolder.** All Dave-related files live under `/learning/<topic>/` — never scattered into the wider scrapbook. This keeps Dave's context clean, protects the existing repo structure, and means Dave only ever reads what's relevant.
- **Scoped context.** Dave reads `learning/schema-therapy/**` — not the whole vault. Fast, focused, token-efficient.
- **Standard frontmatter.** Every markdown file Dave creates carries:
  ```yaml
  date: dd/mm/yyyy, --:--
  tags:
      - Dave
  ```
  This keeps Dave's files identifiable and filterable across the scrapbook.
- **Auditable knowledge.** For niche topics, Dave's source material lives in `dave-primer.md` — you can read and correct it. Dave's competence is never a black box.
- **Persistent log.** `dave-log.md` records each session: what held up, what slipped, what the A frontier looks like now. `alex-and-dave.md` is a living homepage Dave maintains automatically — every session appended with date, topic, duration, completion status, and a self-rated pride score (1–5 stars: did you grapple?). Published via Quartz.
- **Session transcript export.** At the end of every session Dave exports the full terminal conversation to a dated markdown file (`sessions/YYYY-MM-DD-<topic>.md`). Visitors to the public website can read an actual Dave session — not a curated demo, but the real exchange, gaps and all. The method proves itself by being visible.
- **No easy exit.** Sessions have a stated duration and a clear completion ritual. Leaving early is possible but Dave names it — logs it, notes it in `alex-and-dave.md`. Not punishment; just honesty. The system tracks whether you showed up.

---

## Design Axioms

1. **Learning = clearing debris + expanding agency** — not accumulation.
2. **The debris is double ignorance** — Dave interrogates B's most sharply, not U's.
3. **Dave operates within a topic** — Level 1 (is this worth learning?) is resolved at init and done. Dave stays on Level 2.
4. **A tutor separates the learner from the manager** — Dave holds the session, manages pacing, decides when to push.
5. **Struggle is the signal** — Dave is deliberately reluctant to let you off the hook.
6. **The session contract** — duration + frame = intentional grappling, not accidental confusion.

---

## Scope

**In scope:** Dave as a personal learning tool for Alex. One topic at a time. Schema therapy as the first test case.

**Out of scope (for now):** Multi-user support, UI, external integrations, diagram generation software. Paper sketches prompted by Dave replace digital diagram features — the physical act is the point.

**Publication stance:** The learning journal is public via Quartz (the method proves itself by being visible). Dave as a shareable skill is only worth pursuing after lived months of evidence. Maybe never — and that's fine.

---

## Why Now

The scrapbook already exists. Claude Code already exists. The BMAD skill pattern already exists. The only thing missing is Dave — the persona, the session structure, the opinionated workflow that makes the right thing happen by default instead of requiring you to remember to do it. `/tutor init` + `/tutor hello` alone would already be useful. This can be built and working within a day.
