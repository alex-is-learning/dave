# Dave — AI Learning Tutor

A Claude Code skill that acts as a personal Socratic tutor, living inside an Obsidian + Quartz vault. Dave helps you learn topics deeply through active recall, A/B/U structured writing, and deliberate grappling. It never gives you the answer — it points at the gap and waits.

**[See Dave in action →](https://alexislearning.me/scrapbook/2026/learning/Alex-and-Dave)**

---

## Philosophy

**Learning = clearing debris + expanding agency.** The enemy is double ignorance (false closure, not absence of knowledge). Struggle is the signal. Dave uses a Feynman-Russell persona: warm, rigorous, unwilling to let fuzzy thinking pass.

**A/B/U framework:**
- **A:** New learnings that seem true and useful, but not yet empirically backed — Dave's primary interrogation target
- **B:** A items that have since received empirical backing; more settled
- **U:** Things encountered that didn't land or resonate — the honest frontier

Dave probes A's first (false closure hides here), revisits U's second, opens new territory last.

---

## Commands

| Command | Purpose |
|---|---|
| `/dave init` | Onboarding interview, ZPD calibration, primer generation, folder scaffolding |
| `/dave hello` | Context load (log → primer → git diff), session contract, action menu |
| `/dave end` | Close ritual: A/B/U reflection, pride score, log append, homepage update |
| *(in-session)* | Socratic interaction, A interrogation, vagueness naming, U mapping, transcript append |

---

## How It Works

Dave is installed as a Claude Code skill at `~/.claude/skills/dave/`. Each topic lives in its own folder inside your learning directory.

### Session flow

1. **Create a topic folder** in your learning directory and `cd` into it
2. Run `/dave init` — Dave interviews you, calibrates to your level (ZPD), and generates a primer
3. Run `/dave hello` to start a session — Dave loads context, surfaces a continuity note if you bailed last time, and presents a session contract
4. **Grapple.** Dave interrogates your beliefs, names your vagueness, maps your gaps. You can ask Dave to read your notes. It will never give you the subject-matter answer.
5. Run `/dave end` — structured A/B/U reflection, pride score (1–5), log and homepage updated

### File structure

For each topic, Dave creates:

```
<topic-folder>/
├── Dave Primer - <Topic>.md   # Dave's knowledge source (generated or user-provided)
├── Dave Log - <Topic>.md      # Persistent A/B/U state + session history
├── sessions/
│   └── <Topic> Dave session N.md  # Session transcripts (0-indexed, incremental append)
└── notes/                     # Your dialogical notes (read by Dave on demand)
```

Plus a cross-topic homepage at `../Alex and Dave.md` (one level up from the topic folder), published via Quartz.

### Technical design

- **Incremental append writes** — every Dave turn is appended to the transcript immediately via bash heredoc. No read-then-write. A crash mid-session leaves all prior content intact.
- **Context scoped per topic** — Dave never loads your full vault; only the log, primer, and `git diff` for the current folder.
- **Git as epistemological memory** — `git diff` at session start shows what changed since your last commit. Your commit history is your intellectual development record.
- **Hard behavioural constraints** — enforced at the skill level, not subject to LLM drift. Dave cannot be argued into giving answers.
- **Early exit detection** — if you close without running `/dave end`, the next `/dave hello` detects it, logs the incomplete session, and surfaces a continuity note.

---

## Installation

The skill files live at `~/.claude/skills/dave/`:

```
~/.claude/skills/dave/
├── SKILL.md
├── workflow.md          # Persona, hard constraints, conventions, dispatch
└── steps/
    ├── step-init.md     # /dave init
    ├── step-hello.md    # /dave hello
    ├── step-session.md  # In-session behaviour
    └── step-close.md    # /dave end
```

---

## Project Artefacts

Design and planning documents are in `_bmad-output/`:

| File | Contents |
|---|---|
| `prd.md` | Full PRD — 37 functional requirements, 11 non-functional requirements |
| `architecture.md` | Architecture decisions — file schemas, write mechanisms, path resolution |
| `epics.md` | 12 stories across 4 epics |
| `product-brief-dave.md` | Original product brief |
| `product-brief-dave-distillate.md` | LLM-distilled brief (token-optimised) |
| `research/` | Domain research: active learning, visual design |
| `brainstorming/` | Brainstorming session notes |

---

## Status

MVP complete (all 12 stories). Currently in active testing inside the scrapbook Obsidian vault.

Post-MVP features planned (from PRD):
- FR17: Feynman-style session (learner explains, Dave presses)
- FR18: Confused-student essay mode
- FR19: Before/after review since last session
