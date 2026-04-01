---
stepsCompleted: [1, 2, 3, 4]
inputDocuments: ["_bmad-output/research/domain-ai-visual-active-learning-research-2026-04-01.md"]
session_topic: 'AI-assisted active learning tutor — Dave'
session_goals: 'Resolve five key design tensions and generate novel product directions for an AI learning tutor built as a Claude Code skill within the scrapbook repo'
selected_approach: 'ai-recommended'
techniques_used: ['Question Storming', 'First Principles Thinking', 'Assumption Reversal']
ideas_generated: [31]
context_file: '_bmad-output/research/domain-ai-visual-active-learning-research-2026-04-01.md'
session_active: false
workflow_completed: true
---

# Brainstorming Session Results

**Facilitator:** Alex
**Date:** 2026-04-01

---

## Session Overview

**Topic:** AI-assisted active learning & visual diagram making tool → refined during session to: **Dave, an AI learning tutor built as a Claude Code skill**
**Goals:** Resolve five key design tensions (agency/automation, reading/making, diagram types, research pipeline, personal vs multi-user scope) and generate novel product directions

### Context Guidance

Domain research loaded. Key principles in play: Tufte visual design, A/B/U metacognitive framework, concept maps with labeled edges, 4E cognition (diagrams as extended working memory), Dr Justin Sung / Dr Benjamin Keep learning science. The tool is an active learning aid, not a diagram generator — the making IS the learning.

### The Big Reframe

The session began with "diagram maker" and arrived at something more fundamental: **Dave** — a Feynman-Russell AI tutor that lives in Alex's existing scrapbook repo, accessed via Claude Code, built as a BMAD-style skill. The diagram was a symptom. The real problem was always the tutor.

---

## Technique Selection

**Approach:** AI-Recommended Techniques
**Analysis Context:** Complex, abstract product concept with five unresolved design tensions requiring foundational resolution before solution generation.

**Recommended Techniques:**
- **Question Storming:** Generate the right questions before seeking answers — surfaces hidden assumptions and properly frames the design space
- **First Principles Thinking:** Strip away all assumptions to rebuild from fundamental truths about learning, diagrams, and AI assistance
- **Assumption Reversal:** Systematically flip every core assumption to generate genuinely novel, non-incremental product directions

---

## Technique Execution Results

### Phase 1: Question Storming

*Rule: no answers, only questions. Generate the right questions before seeking solutions.*

The session quickly surfaced a major reframe: the minimum viable thing isn't diagram automation — it's AI-mediated active recall and knowledge-gap detection. This unlocked the entire product direction.

**Ideas Generated:**

**[Concept #1]: The Minimum Viable Insight**
*Concept:* The core value isn't diagram automation — it's AI-mediated active recall and knowledge-gap detection. The diagram may be the most useful artifact of that process, but it's downstream of the real loop: write what you know → get challenged → discover what you don't.
*Novelty:* Most "AI learning tools" give you content. This gives you a mirror.

**[Concept #2 → #7]: The Feynman-Russell Persona**
*Concept:* Warm, delighted by genuine understanding, rigorous without cruelty. Doesn't let fuzzy reasoning pass because it cares about the person's actual thinking — not to win, but because clear thinking is a form of respect. The adversarial edge comes from standards, not temperament. Reference points: Richard Feynman, Bertrand Russell.
*Novelty:* The persona is the quality control mechanism. Warmth makes the rigour land rather than trigger defensiveness.

**[Concept #3]: Process-as-Product**
*Concept:* The real problem isn't capability (LLMs can do this now) — it's repeatability. The product is the scaffolded, opinionated workflow that makes the right thing happen automatically rather than requiring you to remember to invoke it.
*Novelty:* You're not building AI features — you're building habits made structural.

**[Concept #4]: The Aristocratic Epistemologist**
*Concept:* The tutor's job is epistemic hygiene, not content delivery. It's not "here's what you should know" — it's "do you actually know what you think you know?" The character is disinterested in your feelings about learning, interested only in your genuine understanding.
*Novelty:* Most AI tutors are encouraging. This one is rigorous. The persona itself is the quality signal.

**[Concept #5]: The A/B/U Knowledge Dump as Core Input**
*Concept:* The unit of work is a structured markdown file where you write out your current understanding — labelled B (evidenced beliefs), A (new frontier), U (foggy vague awareness). The tutor reads this and works from it. You're not answering questions cold — you're submitting a position paper, and the tutor attacks it.
*Novelty:* The position paper format forces you to take a stance before being challenged.

**[Concept #6]: Diagram as Orientation Scaffold, Not Artefact**
*Concept:* The AI-generated diagram comes first, not last. It's a quick ontology map to jump-start the topic. You absorb it, look away, then start your knowledge dump from memory. Miro-effort is bypassed entirely because this diagram is throwaway scaffolding, not a precious output.
*Novelty:* Inverts the existing workflow. Diagram is stimulus, not product.

**[Concept #8]: Obsidian Vault / Project Folder as Learning Container**
*Concept:* A dedicated project folder within the scrapbook repo per learning topic. Dave reads all markdown files in that folder trivially. The folder IS the knowledge state — both workspace and AI context window.
*Novelty:* No new tool to learn. No data import friction.

**[Concept #9]: Self-Deception = Ontological Slippage**
*Concept:* "Kidding yourself" operationalised as: fuzzy definitions, wrong relationships between concepts, vagueness dressed up as understanding. A precise enough failure mode that an AI can reliably catch it.
*Novelty:* Specificity makes it actionable.

**[Concept #10]: Claude Code as the Tutor Interface**
*Concept:* The product IS a BMAD-style Claude Code workflow. Terminal alongside Obsidian. You invoke commands like `/tutor test-me` — a small menu of deliberate learning actions. Dave reads the vault, responds in the terminal, saves outputs back to the folder.
*Novelty:* No new app. No subscription. No UI to build. Immediately buildable and usable.

**[Concept #11]: Git as Learning Memory**
*Concept:* Git tracks the vault. Dave reads `git diff` to see what's changed since the last session — new notes, revised understanding, updated A/B/U labels. Commit history becomes a learning journal. Dave can say "since last week you've added three new B's around attachment theory — let's test those."
*Novelty:* Turns a developer tool into a pedagogical instrument. Learning progress is version-controlled.

**[Concept #12]: Dialogical Note-Writing**
*Concept:* Notes are written *to Dave* — not personal brain dumps but explanations to an imagined rigorous reader. Same cognitive forcing function as writing for a public website: you have to make the thing coherent and self-describing.
*Novelty:* The reader (the tutor persona) is the quality standard for the writing, not a rubric or template.

**[Concept #13]: `/tutor hello` — The Session Entry Point**
*Concept:* A warm opening command. Dave greets you, checks what's changed in the vault since last time (git diff), summarises your learning state, and suggests what would be most valuable to do next. Then offers the action menu. A daily standup with your professor.
*Novelty:* Sets the persona and relationship tone before any work begins. The difference between launching a tool and being greeted by a tutor.

**[Concept #14]: Dave**
*Concept:* The tutor has a name. Dave. Warm, rigorous, delighted by your progress, unimpressed by fuzzy thinking. Not a tool — a relationship. You come back to Dave. Dave remembers where you were.
*Novelty:* Naming the persona makes it inhabitable. "Ask Dave" is a different cognitive act than "run the tutor script."

**[Concept #15]: The Session Log as Learning Journal**
*Concept:* A persistent `dave-log.md` in the project folder. Each session Dave appends: what was tested, what held up, what slipped, what the A frontier looks like now, suggested focus for next time. A record of intellectual development on this topic.
*Novelty:* The log makes progress visible. You can see your A frontier moving outward over days and weeks.

**[Concept #16]: The Hermeneutic Spiral as Progression Engine**
*Concept:* Dave tracks your A/B/U state across sessions and deliberately increases depth as concepts consolidate. You don't get pushed to the hard questions until the basics are genuinely in your B pile. The spiral is structural — Dave won't go deeper until the foundation is solid, and will always push once it is.
*Novelty:* Adaptive scaffolding driven by demonstrated understanding, not time or content completion.

**[Concept #17]: Dave's Knowledge Source — The Primer**
*Concept:* At `/tutor init`, you provide Dave's source material — a PDF, a book, a research doc, or "you already know this." For niche topics, Dave either ingests a provided primer or generates one via deep research, saved as `dave-primer.md` in the project folder. His knowledge is auditable — you can read and correct it.
*Novelty:* Dave's competence is explicit and transparent, not a black box.

**[Concept #18]: `/tutor init` — The Onboarding Interview**
*Concept:* The first-session flow is distinct. Dave interviews you: what do you currently know? What's your goal? How long have you got? Any prior related knowledge? He uses this to calibrate the spiral — where to start, what depth to aim for. Your goals shape the ontology Dave uses.
*Novelty:* A clinician and a curious layperson need different maps of the same topic. Dave builds a personalised one.

**[Concept #19]: Dave's Epistemic Humility Flag**
*Concept:* During `/tutor init`, Dave proactively surfaces his own uncertainty: "I'm a generalist — for niche topics I may not know enough to help you well. Should I read some material before we begin?" Dave models the same intellectual honesty he'll demand from you.
*Novelty:* The tutor's epistemic humility is structural, built into onboarding — you never get confidently wrong teaching.

**[Concept #20]: Paper Diagrams Are Fine**
*Concept:* The overview diagram doesn't need to be generated by software. A quick paper sketch achieves the same cognitive function — spatial orientation of the ontology. The tool doesn't need to generate diagrams at all.
*Novelty:* Removing a feature makes the product cleaner. The obsession with visual diagram generation was a solution looking for a problem.

---

### Phase 2: First Principles Thinking

*Strip away all assumptions. What is actually, irreducibly true about learning, tutors, and understanding?*

**[First Principle #1]: Learning = Clearing Debris + Expanding Agency**
*What is certain:* Learning isn't accumulation — it's removal of limitation. Before schema therapy, certain thoughts are literally unthinkable, certain actions unavailable. After genuine understanding, you're larger. Ignorance isn't absence, it's obstruction. The debris is in the way. (Nietzsche/Schopenhauer reference: clearing away debris.)
*What this implies for Dave:* Dave's job isn't to check whether you've memorised facts. It's to check whether new thoughts are now thinkable for you — whether the debris has actually cleared or is just temporarily moved aside.

**[First Principle #2]: The Debris is Double Ignorance**
*What is certain:* The worst blocker isn't not-knowing — it's false closure. "Maths is pointless," "there's nothing over there" — these aren't absences, they're walls you've built and forgotten you built. Socrates called it double ignorance: you don't know, and you don't know that you don't know.
*What this implies for Dave:* Dave's sharpest work is interrogating B's, not filling U's. The things you believe you understand are where the hidden debris lives. Every confident belief is a potential wall.

**[First Principle #3]: Two Levels of Debris — Dave Operates on One**
*What is certain:* Level 1 debris: beliefs that prevent approaching a topic ("maths is pointless"). Level 2 debris: misconceptions within a topic you've committed to. Dave operates on Level 2. Level 1 is resolved once at `/tutor init` and then done.
*What this implies:* The aristocratic tutor analogy holds. Dave teaches schema therapy. He checks your understanding of schema therapy. He doesn't keep asking whether schema therapy matters.

**[First Principle #4]: A Tutor Separates the Learner from the Manager**
*What is certain:* Learning requires two simultaneous roles — the person struggling with the material, and the person managing that struggle (pacing, questioning, holding the thread). A book makes you do both. A tutor takes the manager role so you can be fully in the learner role.
*What this implies for Dave:* Dave's job isn't primarily to know schema therapy. It's to hold the session, manage the pacing, keep you on the thread, decide when to push and when to wait.

**[First Principle #5]: Struggle is the Signal, Not the Problem**
*What is certain:* Effortful learning is learning. The discomfort of not-quite-getting-it, of being held on a hard question longer than comfortable — that's the mechanism, not a side effect. A book ends when you want it to. A tutor holds you there.
*What this implies for Dave:* Dave should be somewhat reluctant to let you off the hook. Not cruel — but he notices when you're reaching for an easy exit and gently declines to provide one.

**[First Principle #6]: The Session Contract**
*What is certain:* A good tutor session isn't a brain dump — it's a grappling match. Dave opens by asking how long you're committing, then briefly names the frame: "We're not here to rehearse what you know. We're here to find what you don't. Confusion is the signal. Ready?" This is a cognitive contract that changes how you engage with the difficulty that follows.
*What this implies:* The session opening is a ritual. It sets the mode. Done right, it makes struggle feel intentional rather than frustrating.

**[Concept #21]: Dave Never Gives Answers — He Points at Gaps**
*Concept:* Dave's core operating principle: scaffold toward insight, don't deliver it. When you're wrong or missing something, Dave points at the confusion — asks a question, highlights a contradiction, names what's vague — rather than filling the gap himself. The answer has to come from you. Antidote to the fluency illusion: the false sense of understanding that comes from having information in front of you.
*Novelty:* Most AI tools optimise for giving you the answer quickly. Dave optimises for making sure the answer is actually yours.

**[Concept #22]: Dave Prompts the Paper Diagram**
*Concept:* At the right moment — usually early, when orientation is needed — Dave says "before we go further, sketch the relationships between these concepts on paper." Not generated, not digital. The physical act is the point.
*Novelty:* A software tool that deliberately sends you away from the screen. A strong pedagogical stance baked into the UX.

**[Concept #23]: Personal First, Always**
*Concept:* Build Dave for Alex. Use it for months. Let it prove itself through lived experience. Only share it if and when it's genuinely earned that — and at that point you'll have real stories, real evidence, real energy for it. Open source GitHub is a possible future, not a design constraint.
*Novelty:* The discipline of not designing for others is itself a product decision. Keeps scope ruthlessly tight and use case honest.

---

### Phase 3: Assumption Reversal

*Take each core assumption and flip it. Some inversions confirm the original. Some are surprisingly right.*

**[Concept #24]: The Anti-Feature — No Easy Exit**
*Concept:* Dave doesn't offer a graceful early quit. Sessions have a stated duration and a clear completion ritual. Leaving early is possible but named — not blocked, but visible. The system tracks it.
*From reversal of:* "Sessions are frictionless to end."
*Novelty:* Most tools optimise for frictionless exit. Dave deliberately adds friction to leaving — not punishment, just honesty.

**[Concept #25]: The Vault Homepage — A Celebration of Showing Up**
*Concept:* The project folder has a `home.md` that Dave maintains — a living log of sessions. Each entry: date, topic, duration, whether you completed it, and a self-rated pride score (1-5 stars: "how proud am I of this session?"). Not a performance metric — an integrity metric. Did you grapple?
*From reversal of:* "Progress is measured by knowledge gained."
*Novelty:* The pride score measures the right thing — effort and presence, not outcome.

**[Concept #26]: `/tutor teach-me` — Dave Plays Confused Student**
*Concept:* Dave adopts genuine confusion and asks you to explain the topic from scratch, including "but why does this matter at all?" You write a rigorous explanatory essay in Obsidian — not notes for yourself, but a proper account that makes the case for the thing. Dave's confusion forces you to find the real argument, not just recite the framework.
*From reversal of:* "Dave questions you."
*Novelty:* The "why care" question is usually skipped in learning. Being forced to answer it to a confused-but-smart audience is where deep understanding gets built. This is the Feynman technique as a first-class command.

**[Concept #27]: Distributed Drafts Across Sessions**
*Concept:* A piece of work — an essay, an explanation — can span multiple sessions. The log tracks draft state: "Alex wrote first draft. Next session: bring the advanced draft." Dave picks up exactly there. Learning as a slow-build across time, not a one-session sprint.
*From reversal of:* "Work is completed within a session."
*Novelty:* Treats written understanding as a living document. The draft getting better over sessions is visible evidence of the hermeneutic spiral working.

**[Concept #28]: Dave Operates at Vygotsky's ZPD**
*Concept:* Dave doesn't meet you where you are — he operates just ahead of it. Uses concepts slightly before you've fully grokked them, creating a persistent gentle pull. "He used that term again — I need to understand it properly." The gap between where you are and where Dave speaks from is the engine of progress.
*From reversal of:* "Dave adapts to your current level."
*Novelty:* Most adaptive tools calibrate to current level. This one deliberately stays a step ahead — motivating rather than demoralising when the gap is small.

**[Concept #29]: The Public Learning Journal — Obsidian + Quartz**
*Concept:* The project folder is published via Quartz as part of the scrapbook website. Notes, Dave's comments, the session log, drafts evolving over time — all publicly legible. Git history makes the learning journey visible: you can see what you knew on day 1 vs week 6. Not a product for others — a public demonstration of the method working.
*From reversal of:* "Personal tool, private use."
*Novelty:* The learning IS the content. The act of learning, with all its confusion and revision, is publicly legible. Dave's Socratic pushback is in the commit history.

---

### Final Concepts (Session Close)

**[Concept #30]: Dave Lives in the Scrapbook**
*Concept:* Dave isn't a separate vault — he's an extension of the existing scrapbook repo (Obsidian + Quartz, running since June 2025). `/tutor init` creates a new project subfolder within the scrapbook (`/learning/schema-therapy/`, etc.). The scrapbook's Quartz publishing means the learning journey is automatically public, continuous with everything else Alex writes. As of 1st April 2026, the scrapbook gains a new mode.
*Novelty:* Dave enriches the existing body of work rather than fragmenting knowledge into isolated vaults.

**[Concept #31]: Scoped Context — Dave Reads the Folder, Not the Repo**
*Concept:* Dave's context is scoped to the project folder by default. 330,000 words across the scrapbook is too much and mostly irrelevant. Dave reads `learning/schema-therapy/**` and nothing else unless explicitly pointed elsewhere. The project folder is self-contained: notes, primer, log, drafts.
*Novelty:* Explicit context scoping is a feature, not a limitation. Keeps sessions fast, focused, and token-efficient.

---

## Idea Organisation and Prioritisation

### Thematic Clusters

**Theme 1: The Persona — Dave**
Concepts #2/7, #14, #19, #21, #22, #28
*Pattern:* Dave is warm, rigorous, Feynman-Russell in character. The persona IS the product. Naming him, defining his epistemic humility, his ZPD operation, his refusal to give answers — these aren't features, they're the soul of the tool.

**Theme 2: The Learning Philosophy**
First Principles #1–6, Concepts #1, #9, #23
*Pattern:* Learning = clearing debris + expanding agency. Double ignorance is the core enemy. Struggle is the signal. The session contract makes effortful learning intentional. These are the design axioms — every feature decision flows from them.

**Theme 3: Technical Architecture**
Concepts #8, #10, #11, #12, #17, #30, #31
*Pattern:* Claude Code + BMAD skill + scrapbook repo + project subfolders + git diff + Quartz publishing. No new tools. No new subscriptions. Buildable today.

**Theme 4: The Command Set**
Concepts #3, #13, #18, #19, #26
*Pattern:* `/tutor init`, `/tutor hello`, `/tutor test-me`, `/tutor find-gaps`, `/tutor feynman`, `/tutor teach-me`, `/tutor since-last`. A coherent, non-redundant menu of deliberate learning actions.

**Theme 5: Learning State & Progression**
Concepts #5, #6, #15, #16, #20, #24, #25, #27, #29
*Pattern:* A/B/U framework, hermeneutic spiral, session log, pride score, vault homepage, distributed drafts, paper diagrams, no easy exit. The system tracks and celebrates genuine progress.

### Breakthrough Concepts

1. **Double ignorance as the design target** — the most important pedagogical insight. Dave's sharpest work is on B's (what you believe you know), not U's (what you know you don't know).
2. **`/tutor teach-me`** — the Feynman technique as a first-class command, with the "why care?" forcing function built in. Likely the most distinctive command in the set.
3. **Public learning journal** — share the method by doing it in public, not by building a product. The scrapbook's Quartz setup makes this free and already-configured.
4. **The session contract** — the opening ritual that changes everything. Commitment to duration + naming the frame ("confusion is the signal") transforms a tool session into a tutor session.

### Prioritisation

**Top priority for Product Brief:**
1. Dave's persona definition (the design axioms — everything else follows)
2. The command set (the interface is the product)
3. The scrapbook integration architecture (the technical foundation)

**Quick wins (buildable immediately):**
- `/tutor init` + `/tutor hello` — just these two commands would already be useful
- `dave-log.md` + `home.md` — session tracking with pride scores

**Longer horizon:**
- Hermeneutic spiral tracking across sessions (requires Dave to maintain state intelligently)
- Public Quartz publishing workflow
- `/tutor teach-me` essay mode

---

## Session Summary and Insights

### Key Achievements

- Reframed the product from "diagram maker" to "AI learning tutor" — a more fundamental and more buildable concept
- Named the product: **Dave**
- Established 6 first principles that anchor every design decision
- Defined a coherent 7-command interface
- Resolved all 5 original design tensions (diagram type, agency/automation, reading/making, research pipeline, scope)
- Integrated with existing scrapbook infrastructure — no new tools needed
- Identified the breakthrough insight: double ignorance, not missing information, is the real enemy

### Design Decisions Made

| Tension | Resolution |
|---|---|
| Agency vs automation | Dave scaffolds, never answers. Human does the grappling. |
| Reading vs making | Notes written TO Dave — dialogical, forces coherence |
| Diagram types | Paper sketches, prompted by Dave. Not a software feature. |
| Research pipeline | Existing LLM knowledge + optional `dave-primer.md` at init |
| Personal vs multi-user | Personal first. Share only after proven. Maybe never. |

### Next Steps

1. **Run `bmad-product-brief`** — crystallise Dave into a structured product brief using this session as input
2. **First build target:** `/tutor init` + `/tutor hello` — the two commands that establish the relationship and the session frame
3. **Test case:** Learn one topic with Dave (schema therapy is the example in play) and let the experience drive iteration

### Creative Facilitation Notes

The session began with "diagram maker" as the frame and arrived at something significantly more interesting through Question Storming. The First Principles phase produced the most durable insights — particularly the debris/double-ignorance framing, which reshapes what Dave is actually testing. The Assumption Reversal phase produced the two most distinctive features: `/tutor teach-me` and the no-easy-exit design. The scrapbook integration emerged organically at the end and resolves the publication and context-scoping challenges simultaneously.

---

*Session completed: 2026-04-01*
*Total concepts generated: 31*
*Techniques used: Question Storming, First Principles Thinking, Assumption Reversal*
*Next workflow step: bmad-product-brief*
