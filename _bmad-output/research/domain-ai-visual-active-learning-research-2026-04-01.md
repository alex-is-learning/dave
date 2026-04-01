---
stepsCompleted: [1, 2, 6]
inputDocuments: ["pdfs/Edward Tufte - The Visual Display of Quantitative Information (2001) copy.pdf"]
workflowType: 'research'
lastStep: 6
research_type: 'domain'
research_topic: 'AI-powered active learning and visual diagram maker'
research_goals: 'Understand domain landscape for visual communication, learning science, ontology/hierarchy visualisation; inform product brief and brainstorming'
user_name: 'Alex'
date: '2026-04-01'
web_research_enabled: true
source_verification: true
---

# Domain Research: AI-Powered Active Learning & Visual Diagram Maker

**Date:** 2026-04-01
**Author:** Alex
**Scope:** Visual communication principles, learning science, hierarchy/ontology visualisation. Competitive landscape and technology trends excluded per user request.

---

## Domain Research Scope Confirmation

**Research Topic:** AI-powered active learning and visual diagram maker
**Research Goals:** Understand the landscape for visual communication, learning science, and knowledge representation to inform a product brief and brainstorming session.

**Research Confirmed:** 2026-04-01

---

## 1. Visual Communication & Information Design

### 1.1 Tufte's Core Principles (from VDQI, 2001)

Tufte's five principles (p.105) form the bedrock of rigorous visual communication:

1. **Above all else, show the data**
2. **Maximize the data-ink ratio** — "The larger the share of a graphic's ink devoted to data, the better. Every bit of ink on a graphic requires a reason. And nearly always that reason should be that the ink presents new information."
3. **Erase non-data-ink** — Ink that fails to depict information doesn't just waste space; it actively clutters the data
4. **Erase redundant data-ink** — A labeled bar chart encodes its value 6 ways simultaneously; 5 can be erased
5. **Revise and edit** — "Just as a good editor of prose ruthlessly prunes out unnecessary words, so a designer of statistical graphics should prune out ink that fails to present fresh data-information."

**Key corollary for learning diagrams:** Tufte explicitly defends high information density on a single page: *"We look at one page at a time and the more on the page, the more effective and comparative our eye can be."* (p.168) — a direct vindication of Alex's one-pager instinct.

**The elegance principle** (p.177): *"Graphical elegance is often found in simplicity of design and complexity of data."* This is the core tension to resolve: visually simple, informationally rich.

### 1.2 Chartjunk — What to Avoid

Three categories of chartjunk (p.107–115):

1. **Unintentional optical art (moiré vibration)** — Cross-hatching, dense patterns, and fill textures that produce visual shimmer. "Inevitably bad art and bad data graphics." Use tint screens (shades of gray) instead.
2. **The dreaded grid** — "Grids should usually be muted or completely suppressed so that their presence is only implicit — lest it compete with the data." Dark grid lines carry no information, clutter the graphic, and generate visual activity unrelated to content.
3. **The self-promoting graphical duck** — Decoration that exists to showcase the designer rather than reveal data. Interior decoration of graphics "comes cheaper than the hard work required to produce intriguing numbers and secure evidence."

### 1.3 The Friendly Data Graphic (p.183)

Tufte's contrast between friendly and unfriendly graphics is directly applicable to learning diagrams:

| Friendly | Unfriendly |
|----------|------------|
| Words spelled out, no mysterious encoding | Abbreviations abound, requiring decoding |
| Words run left to right | Words run vertically or in multiple directions |
| Little messages help explain data | Graphic is cryptic, requires repeated reference to scattered text |
| Labels placed directly on the graphic (no legend required) | Obscure codings require going back and forth to legend |
| Graphic attracts viewer, provokes curiosity | Graphic is repellent, filled with chartjunk |
| Colors chosen to be accessible (color-deficient viewers) | Red and green used for essential contrasts |
| Type is clear, modest; upper-and-lower case with serifs | Type is clotted, overbearing, all-caps sans serif |

### 1.4 Data/Text Integration (p.180–182)

*"Data graphics are paragraphs about data and should be treated as such."*

- Words and pictures belong together — words on graphics are data-ink
- Labels placed in the plotting field (not in a separate legend) use the space freed by erasing redundant ink
- "The eye is not required to dart back and forth between textual material and the graphic"
- The same typeface should be used for text and graphic

**For learning diagrams:** This argues strongly against floating labels, external legends, and diagrams separated from their explanatory text.

### 1.5 Small Multiples (p.170–175)

A small multiple is a series of graphics sharing the same design, indexed by a changing variable. Well-designed small multiples are:
- Inevitably comparative
- Deftly multivariate
- Shrunken, high-density
- Usually based on a large data matrix
- Drawn almost entirely with data-ink
- Efficient in interpretation
- Often narrative — showing shifts as an index variable changes

**The Shrink Principle:** *"Graphics can be shrunk way down."* Many graphics can be reduced to half their published size with virtually no loss of legibility. This enables multiple small views on a single page.

**The aphorism:** *"For non-data-ink, less is more. For data-ink, less is a bore."*

### 1.6 Shape and Proportion (p.186–187)

- Graphics should tend toward **horizontal** (wider than tall) — eyes are better at detecting deviations from the horizontal
- Easier labelling: words read left to right in a wider field
- Horizontal emphasises causal flow: cause on x-axis, effect on y-axis

### 1.7 Gestalt Principles of Visual Perception

Developed by Wertheimer, Koffka, and Köhler in the 1920s — the laws of how humans automatically group and perceive visual elements:

- **Proximity** — Elements close together are perceived as related. Use spatial clustering to encode conceptual grouping.
- **Similarity** — Elements sharing shape, colour, or size are grouped. Use visual consistency to signal membership of a category.
- **Continuity** — Elements arranged along a line or curve are perceived as related. Use alignment to guide the eye through a hierarchy.
- **Figure/Ground** — The brain distinguishes focal elements (figure) from background. Use whitespace to let content emerge clearly.
- **Closure** — The brain completes incomplete shapes. Implies that suggestive structures can be lighter than their informational weight.
- **Prägnanz (Good Form)** — The eye prefers the simplest, most regular interpretation. Complexity should be informational, not structural.

**Application to learning diagrams:** Gestalt principles explain *why* Tufte's data-ink ratio works — the brain is doing active visual work to group and relate elements. Every non-data mark disrupts that work.

*Sources: [IxDF Gestalt Principles](https://ixdf.org/literature/topics/gestalt-principles), [Smashing Magazine](https://www.smashingmagazine.com/2014/03/design-principles-visual-perception-and-the-principles-of-gestalt/), [PMC: A Century of Gestalt Psychology](https://pmc.ncbi.nlm.nih.gov/articles/PMC3482144/)*

---

## 2. Learning Science & Active Sense-Making

### 2.1 The Problem with Rote Learning and Anki

**Dr Justin Sung** (iCanStudy, former medical doctor, learning coach) makes a key distinction:

> *"People don't understand the difference between free recall versus cued recall. They'll build pattern recognition for a very specific cue without having to activate the retrieval process that would happen in any other context. So flashcards can create an illusion of competence."*

His critique: active recall and spaced repetition are both *lower-order* learning. They work for fact memorisation but don't build understanding. Higher-order learning — the ability to apply, analyse, synthesise, and evaluate — requires something different: **sense-making**.

*"Humans are both curious and sense-making creatures: only mental representations that follow a certain logic and are deemed interesting can be successfully stored into long-term memory."*

**Dr Benjamin Keep** (Stanford PhD in Learning Sciences, Cornell JD) shares this skepticism. He bridges cognitive science research and practice, focusing on:
- Myths about learning (including overclaiming for spaced repetition)
- How to apply cognitive science to education
- Research literacy — how to critically evaluate learning claims
- His community is *"dedicated to evidence and self-skepticism, but also hope and positivity"*

*Sources: [benjaminkeep.com](https://www.benjaminkeep.com/about-me/), [iCanStudy](https://www.icanstudy.com/), [Justin Sung LinkedIn](https://nz.linkedin.com/in/justin-sung)*

### 2.2 Higher-Order Learning

Dr Sung's framework distinguishes:
- **Lower-order:** memorisation, recall, recognition (what Anki optimises for)
- **Higher-order:** understanding, application, analysis, synthesis — "the last thing, if ever, that technology can fully replace"

His learning sequence: priming → reference → retrieval/interleaving → overlearning → encoding. The key move is **pre-processing** material to build a cognitive scaffold before encoding details.

**Implication for diagrams:** Creating a visual diagram is itself a higher-order act — it forces you to make sense of relationships, not just retain facts. The *process* of constructing the diagram *is* the learning.

### 2.3 Visual Diagram-Making as Active Learning

Research evidence from cognitive science (via PMC studies):

- *"Creating visual explanations improves learning"* — asking participants to self-explain with a diagram resulted in greater learning than self-explaining from text ([Cognitive Research, 2016](https://cognitiveresearchjournal.springeropen.com/articles/10.1186/s41235-016-0031-6))
- Active concept-map creation improves comprehension and retention by forcing the learner to draw connections themselves
- *"The process helps embed representations, using graphical and textual semiotic conventions of their creator's understanding of a given issue"*
- Visual representations provide opportunities for sense-making that text alone does not

**The key tension for the tool:** If the AI *makes* the diagram, the user loses the learning benefit of *making* it. The tool needs to preserve active engagement while reducing the mechanical burden. Possible resolution: the AI generates a draft, the user annotates, restructures, and challenges it — using the diagram as a thinking partner.

*Sources: [Visual Note-taking and Active Learning, ResearchGate](https://www.researchgate.net/publication/344697184_Visual_Note-taking_Opportunities_to_Support_Student_Agency_in_Active_Learning), [Creating visual explanations improves learning, PMC](https://pmc.ncbi.nlm.nih.gov/articles/PMC5256450/)*

### 2.4 Concept Maps vs Mind Maps

Important distinction for diagram type selection:

| | **Mind Map** | **Concept Map** |
|---|---|---|
| Structure | Tree, single central topic | Network/DAG, multiple parents allowed |
| Relationships | Implicit (just hierarchy) | Explicit labeled relationships ("causes", "requires", "is a type of") |
| Best for | Brainstorming, initial exploration | Understanding complex systems, showing causal/semantic relationships |
| Cognitive load | Lower | Higher (but more epistemically honest) |

For learning, **concept maps** are more powerful because they force you to articulate *how* things relate, not just *that* they're related. Mind maps can create an illusion of understanding through apparent structure.

*Sources: [NN/g Cognitive Mind Concept](https://www.nngroup.com/articles/cognitive-mind-concept/), [Concept Map vs Mind Map, Mapify](https://mapify.so/blog/concept-map-vs-mind-map), [PMC: Complexity of Concept Maps](https://pmc.ncbi.nlm.nih.gov/articles/PMC8788906/)*

### 2.5 4E Cognitive Science and Learning

4E cognition describes cognitive processes as **Embodied, Embedded, Enactive, and Extended**:

- **Embodied** — Cognition is shaped by the whole body, not just the brain
- **Embedded** — Cognition is co-determined by the physical, social, and cultural environment
- **Extended** — Cognitive processes can be offloaded onto external tools and devices
- **Enactive** — Cognition arises from a two-way exchange between organism and environment

**Most relevant for diagram tools:** The "extended" dimension. Diagrams on a whiteboard or in Miro function as **extended cognitive infrastructure** — they literally extend the mind's working memory and reasoning capacity onto a stable external surface. Good diagrams don't just record thought; they *enable* thought that couldn't otherwise occur.

The "embedded" dimension also matters: learning in context, with the content grounded in real problems and purposes, produces more durable understanding than decontextualised study.

*Sources: [4E Cognition and Education, Kinems](https://blog.kinems.com/4e-cognition-embodied-embedded-enactive-and-extended-new-inspirations-in-education/), [PMC: 4E Cognition Review](https://pmc.ncbi.nlm.nih.gov/articles/PMC7250653/), [PMC: Experiential Learning and 4E](https://pmc.ncbi.nlm.nih.gov/articles/PMC11580775/)*

### 2.6 The A/B/U Framework (Open Research Institute)

A powerful metacognitive tool for active engagement with new material:

- **A (Advance):** "true & useful, AND new to me" — a genuine learning event, expanding your frontier
- **B (Base):** "true & useful, but NOT novel" — confirms existing knowledge
- **U (Unknown/Unclear):** "unclear, undefined, unknowable, or untrue" — irrelevant regardless of truth status

**Why it matters:**
- Requires honest self-assessment — cannot be gamed
- Finds are rated relative to *your* knowledge, not absolute truth
- A ratings represent new mental models being integrated; B ratings reflect established understanding
- Finding someone who gives you A insights demonstrates they've surpassed your knowledge frontier

**The B+A+[U]* publication structure:** ORI advocates establishing shared foundations (Bs) before advancing to novel insights (As), with uncertainties (Us) appropriately flagged. This "ladder structure" measures learning difficulty through "Inferential Distance."

**Maladaptive Bs** (from Alex's second link): You can hold well-evidenced beliefs (Bs) that still cause harm. When corrective feedback arrives, it often lands as a U because existing Bs create cognitive friction — the person rejects corrective input rather than integrating it. Kegan's Immunity to Change explains this: "Big Assumptions" generate fears that make maladaptive Bs feel "perfectly sensible" from a defensive perspective.

**Application to the tool:** The A/B/U framework could be built into the active learning loop — as the user engages with a diagram or report, they explicitly tag each insight as A, B, or U. This turns passive reading into metacognitive engagement and builds an honest map of the learning frontier.

*Sources: [ORI A/B/U Framework](https://openresearchinstitute.org/onboarding/A_B_U.html), [Alex's Scrapbook: Maladaptive Bs](https://alexislearning.me/scrapbook/2026/A-B-U-gifs,-maladaptive-Bs,-and-Kegan's-%22Immunity-to-Change%22)*

---

## 3. Hierarchy, Ontology & Knowledge Representation

### 3.1 Types of Knowledge Structure

Different knowledge structures serve different purposes:

- **Taxonomy/Tree hierarchy** — A single-parent "is-a" relationship (Kingdom > Phylum > Class > Order...). Clean, simple, but imposes an artificial linear structure on multi-dimensional knowledge.
- **Ontology** — A formal, explicit specification of a conceptual domain. Includes classes, properties, and named relationships. Can model complex many-to-many relationships.
- **Knowledge graph** — Nodes (concepts) connected by labeled edges (relationships). More flexible than taxonomy, less formally rigorous than OWL ontology.
- **Concept map** — An informal, human-readable knowledge graph. Nodes connected by labeled propositions. Designed for human comprehension over machine readability.

### 3.2 Visualisation Best Practices for Hierarchies

Key findings on ontology/hierarchy layout:

- **Hierarchical layout** (top-down or left-right) is recommended for `rdfs:subClassOf` / part-of relationships — maps to how humans naturally read hierarchies
- **Tree layout** is ideal for pure taxonomies; **radial layout** emphasises a central entity and its connections; **organic/force-directed** is good for exploring unknown structures
- **The 20-50 node rule:** Start with a focused view showing only 20-50 relevant nodes; use progressive disclosure by collapsing/expanding groups to prevent visual overload
- For a **single-page, grokable** diagram: hierarchical or radial layout is strongly preferred over force-directed (which sprawls unpredictably)

**Visual complexity management:**
- Filter by node type / relationship type to reduce noise
- Group by namespace or module
- Use visual weight (size, colour, stroke weight) to encode importance/centrality
- Progressive disclosure: collapse subtrees until needed

**The tension with Tufte:** Tufte's data-ink principles apply differently to relational diagrams than to statistical charts. In a concept map, *every edge is data* — the relationships are the content. But the same chartjunk dangers apply: gratuitous use of icons, 3D effects, decorative arrows, and gradient fills all reduce signal-to-noise.

*Sources: [yFiles Knowledge Graph Guide](https://www.yfiles.com/resources/how-to/guide-to-visualizing-knowledge-graphs), [Datavid Knowledge Graph Visualisation](https://datavid.com/blog/knowledge-graph-visualization), [OWLGrEd Online Visualiser](https://owlgred.lumii.lv/online_visualization)*

### 3.3 OWL/RDF Standards (for reference)

If the tool needs to export structured knowledge (not just visual):
- **OWL (Web Ontology Language)** — W3C standard for formal ontologies; supports rich inference
- **RDF (Resource Description Framework)** — Subject-predicate-object triples; foundation of the semantic web
- **SKOS (Simple Knowledge Organisation System)** — Simpler than OWL; good for thesauri and concept schemes

For a personal learning tool, these are likely overkill. A simpler graph model (nodes + typed edges) would suffice.

---

## 4. Research Synthesis

### 4.1 Key Tensions to Resolve in Product Design

1. **Agency vs automation:** Creating diagrams *is* the learning. If AI automates diagram creation entirely, it potentially removes the cognitive work that makes diagrams pedagogically valuable. The tool needs to preserve active engagement — perhaps as AI-assisted authoring rather than AI-generated output.

2. **One-pager constraint vs informational richness:** Tufte directly validates the single-page approach — "the more on the page, the more effective and comparative our eye can be." But this requires high data-ink density with low visual noise. This is hard to achieve automatically.

3. **Concept maps vs mind maps:** Mind maps are easier to generate automatically and easier to read, but concept maps are more epistemically honest and more valuable for learning. The tool should probably produce concept maps with labeled relationships, not just unlabeled hierarchies.

4. **Reading vs making:** Alex's current workflow (read report → make diagram) separates comprehension from sense-making. A tool that generates diagrams from reports removes the sense-making step. Preserving A/B/U engagement and some form of active annotation/restructuring may be essential.

### 4.2 Foundational Principles for the Tool's Visual Output

Drawing directly from Tufte and gestalt research, any AI-generated diagram should:

- **Maximise signal, minimise noise:** No decorative elements; every mark carries meaning
- **Label directly:** No legends; labels sit on or beside their referents
- **Use spatial proximity to encode conceptual proximity** (gestalt: proximity)
- **Use visual weight to encode importance** (thicker edges = stronger relationships; larger nodes = more central concepts)
- **Avoid moiré patterns, gradient fills, and icon-heavy nodes** (chartjunk in diagram form)
- **Tend horizontal:** Wider than tall where possible
- **Integrate words and structure:** Relationship labels on edges are data-ink; don't strip them for "cleanness"
- **Keep to a single page** with progressive disclosure for depth

### 4.3 Foundational Principles for the Tool's Learning Approach

Drawing from learning science:

- **The diagram is a thinking partner, not a product:** The value is in engaging with it, restructuring it, challenging it — not in receiving it
- **A/B/U tagging** as an engagement mechanism: every node or claim should be rateable as A (new), B (known), or U (unclear/uncertain)
- **Higher-order over lower-order:** The tool should push toward synthesis and relationship-identification, not fact recall
- **Extended cognition:** The diagram functions as externalised working memory — it should be designed for *working with*, not just reading
- **Inferential distance:** Content should be sequenced from established foundations (Bs) to new insights (As) — not dumped as a flat list

### 4.4 Diagram Types Most Relevant to Alex's Use Case

| Type | Best for | Notes |
|------|----------|-------|
| **Concept map** | Complex domains with non-tree relationships | Most epistemically honest; labeled edges are key |
| **Hierarchy/taxonomy** | Well-structured domains (biology, law, software) | Clean and readable; loses cross-relationships |
| **Process/flow diagram** | Causal chains, procedures, mechanisms | Tufte's *Visual Explanations* covers this specifically |
| **Comparison matrix** | Side-by-side concept comparison | Under-used in learning; Tufte's small multiples apply |
| **Timeline** | Historical/developmental narratives | Time-series principle applies |

---

## Sources

### Primary Sources
- Tufte, E.R. (2001). *The Visual Display of Quantitative Information* (2nd ed.). Graphics Press. [PDF read directly]
- [Open Research Institute: A/B/U Framework](https://openresearchinstitute.org/onboarding/A_B_U.html)
- [Alex's Scrapbook: Maladaptive Bs and Kegan's Immunity to Change](https://alexislearning.me/scrapbook/2026/A-B-U-gifs,-maladaptive-Bs,-and-Kegan's-%22Immunity-to-Change%22)

### Learning Science
- [iCanStudy / Dr Justin Sung](https://www.icanstudy.com/)
- [Benjamin Keep PhD](https://www.benjaminkeep.com/about-me/)
- [Creating Visual Explanations Improves Learning (PMC)](https://pmc.ncbi.nlm.nih.gov/articles/PMC5256450/)
- [Visual Note-taking and Active Learning (ResearchGate)](https://www.researchgate.net/publication/344697184_Visual_Note-taking_Opportunities_to_Support_Student_Agency_in_Active_Learning)
- [4E Cognition and Education](https://blog.kinems.com/4e-cognition-embodied-embedded-enactive-and-extended-new-inspirations-in-education/)
- [PMC: Experiential Learning and 4E Cognition](https://pmc.ncbi.nlm.nih.gov/articles/PMC11580775/)

### Visual Communication
- [Smashing Magazine: Gestalt Principles](https://www.smashingmagazine.com/2014/03/design-principles-visual-perception-and-the-principles-of-gestalt/)
- [IxDF: Gestalt Principles](https://ixdf.org/literature/topics/gestalt-principles)
- [Graficto: Tufte's Essential Principles](https://graficto.com/blog/discover-edward-tuftes-essential-principles-for-effective-data-visualization/)

### Knowledge Representation & Ontology
- [yFiles: Knowledge Graph Visualisation Guide](https://www.yfiles.com/resources/how-to/guide-to-visualizing-knowledge-graphs)
- [Datavid: Knowledge Graph Visualisation](https://datavid.com/blog/knowledge-graph-visualization)
- [NN/g: Cognitive, Mind, and Concept Maps](https://www.nngroup.com/articles/cognitive-mind-concept/)
- [PMC: Complexity of Concept Maps and Learning](https://pmc.ncbi.nlm.nih.gov/articles/PMC8788906/)
