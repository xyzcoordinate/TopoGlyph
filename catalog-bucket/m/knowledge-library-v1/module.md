# TopoGlyph 9.0: The Knowledge Library Problem

## Extending to Project Knowledge Architecture

Drawing from information science, library science, information architecture, and software engineering to model the problem of building a living knowledge library for a software project.

---

## The Problem Space

A software project generates knowledge constantly -- in code, commits, conversations, design docs, schema definitions, API contracts, sprint artifacts, and the tacit mental models of its contributors. Most of this knowledge is either ephemeral (lost when a conversation ends), implicit (trapped in one person's head), or scattered (spread across files, tools, and time). The challenge: how to capture, organize, surface, and evolve project knowledge so that it remains findable, trustworthy, and useful -- to both humans and LLMs -- as the project itself changes.

This is not a documentation problem. Documentation is an artifact. This is a **knowledge topology** problem: the shape of what is known, how parts relate, where gaps and decay exist, and how new knowledge integrates without breaking old structures.

---

## New Elements for Knowledge Library Topology

### 1. Knowledge Unit Types

Knowledge in a project is not homogeneous. Different kinds of knowledge have different properties: volatility, formality, derivability, audience.

- **Knowledge States**
  - `▪` -- Codified knowledge (written down, findable, current)
  - `▫` -- Latent knowledge (exists but not yet captured)
  - `▧` -- Decaying knowledge (was accurate, now drifting from reality)
  - `▩` -- Contested knowledge (multiple conflicting accounts exist)

- **Knowledge Sources**
  - `S` -- Source code (the ground truth for behavior)
  - `D` -- Design documents (intent, rationale, constraints)
  - `T` -- Transactional records (commits, PRs, issues, conversations)
  - `M` -- Mental models (held by individuals, not externalized)
  - `L` -- Learned patterns (conventions, heuristics, "how we do things here")

### 2. Library Operations

- **Cataloging Operations**
  - `[+k]` -- Classification (assigning a knowledge unit to a category)
  - `[-k]` -- Cross-referencing (linking related units by stable ID)
  - `[=k]` -- Authority control (resolving which source is canonical)
  - `[!k]` -- Provenance marking (recording where knowledge came from and when)

- **Collection Operations**
  - `>>` -- Accession (new knowledge enters the library)
  - `<<` -- Weeding (obsolete knowledge removed or archived)
  - `??` -- Gap analysis (identifying what the library lacks)
  - `<>` -- Reconciliation (resolving conflicts between sources)

### 3. Findability Structures

- **Navigation Elements**
  - `>|` -- Wayfinding path (a route through knowledge for a specific need)
  - `|<` -- Backlink (reverse reference -- "what points here?")
  - `[F]` -- Faceted entry point (access by domain, audience, recency, etc.)
  - `[S]` -- Contextual scope (what you need to know *before* reading this)

- **Retrieval Dynamics**
  - `<-` -- Precision filter (narrowing results to exactly what's needed)
  - `->` -- Recall expansion (broadening to include related knowledge)
  - `[R]` -- Relevance ranking (ordering by usefulness to the seeker)
  - `[~]` -- Serendipity channel (enabling discovery of unexpected connections)

### 4. Temporal Knowledge Dynamics

- **Freshness and Decay**
  - `@t` -- Timestamp anchor (when knowledge was last verified against reality)
  - `~t` -- Staleness gradient (how quickly this knowledge type decays)
  - `!t` -- Refresh trigger (event that should prompt re-verification)
  - `^t` -- Temporal layering (history of how this knowledge evolved)

---

## The Fundamental Tensions

Every project knowledge library exists in a field of tensions. These are not problems to solve but polarities to manage. TopoGlyph can make them visible:

### Tension 1: Completeness vs. Maintenance Cost

```
▪▪▪▪▪▪▪▪▪                    Comprehensive codification
   [loop]                     Each unit demands maintenance
~t~t~t                        Staleness accumulates with scale
   ▧▧▧▧▧                     Large portions decay
[▪▪▧▧▧] -> filter            Decayed library erodes trust in all entries
   [neg-fb]                   Negative feedback: contributors stop consulting or updating
[▫▫▫▫▫]                      Knowledge retreats to latent state (people's heads)
```

The library science insight: collection development policy matters more than collection size. A small, rigorously maintained collection outperforms an exhaustive but decaying one.

### Tension 2: Single Source of Truth vs. Knowledge Distribution

```
[=k][S Code]                  Code is canonical for behavior
   boundary                   But code cannot express intent, rationale, or future direction
D[# Design Docs]              Design docs capture "why"
   ~t                         But drift from code as implementation diverges
T[# Commit History]           Commits record the chronology of change
   <-                         But retrieving the right commit requires knowing what to search for
M[# Mental Models]            Contributors hold the richest context
   distortion                 But this knowledge is inaccessible and leaves when they do
```

The information architecture insight: no single source *can* be the truth for all knowledge types. The architecture must define which source is authoritative *per knowledge type* and maintain the mappings between them.

### Tension 3: Structure vs. Organic Growth

```
[+k][# Taxonomy]              Predefined classification system
   pressure                   Selection pressure from new concepts that don't fit
>>[▪ New Knowledge]           New knowledge that resists existing categories
   tension                    Tension: force it into existing structure vs. let structure evolve
expand -> adapt               Classification adapts
[# Evolved Taxonomy]          Living organizational scheme
```

The information science insight: Ranganathan's principle -- "every classification is a theory." The taxonomy embodies assumptions about the domain. When knowledge resists classification, the taxonomy may need to change, not the knowledge.

---

## The Knowledge Lifecycle in a Software Project

### Full Lifecycle Model

```
M[▫ Tacit Insight]             Someone learns something (debugging, conversation, reading code)
   >>                         Accession decision: is this worth capturing?
[!k][# Provenance]            Record source, date, confidence
   [+k]                       Classify: which domain? which audience? which volatility?
[-k][# Cross-references]       Link to related knowledge units by stable ID
   @t                         Timestamp: verified as of now
▪[# Codified Knowledge]       Knowledge enters the library

   ~t ~t ~t                   Time passes; code changes; context shifts
   !t                         Refresh trigger fires (related code changed, sprint boundary, etc.)

   <>                         Reconciliation: compare stored knowledge against current reality
▪[# Still Valid] | ▧[# Drifted]   Branch: still true, or decayed?
   << | update                 Weed if obsolete; update if correctable
[-k][# Updated Links]          Re-validate cross-references
   @t                         Re-timestamp
▪[# Refreshed Knowledge]      Knowledge re-enters the library with current confidence
```

### Knowledge Accession Heuristic

Not all knowledge should be codified. The accession decision is the most consequential operation in the library:

```
M[▫ Candidate Knowledge]
   evaluate                   Evaluate against criteria:

>| Derivable from code?         -> S (don't codify -- the code IS the knowledge)
>| Derivable from git history?  -> T (don't codify -- the history IS the knowledge)
>| Single-use context?          -> discard (ephemeral, not worth maintenance cost)
>| Cross-cutting rationale?     -> D (codify in design docs)
>| Convention or heuristic?     -> L (codify in CLAUDE.md / architecture docs)
>| Surprising or non-obvious?   -> ▪ (codify -- this is the highest-value knowledge)
```

This mirrors the principle already in asta's memory system: "save what is surprising or non-obvious." Derivable knowledge has an authoritative source already; ephemeral knowledge doesn't justify its maintenance cost.

---

## The Knowledge Graph Topology

### Asta's Existing Structure as a TopoGlyph Expression

```
[S][# CLAUDE.md]                           Always-loaded orientation layer
   >| >| >|                               Wayfinding paths to deeper knowledge
[F][# .system3/index.md]                   Faceted entry point (by domain, by ID prefix)
   [-k]                                    Cross-references via stable IDs
[# .system3/ontology.md]                   Controlled vocabulary for domains
   [+k]                                    Classification authority
[# pages/ ] [-k] [# api/ ] [-k] [# data-model/ ]   Three catalogs, cross-linked by ID
   [=k]                                    Authority control: schema.prisma is canonical for data
S[# schema.prisma]                         Ground truth for persisted shapes
   boundary                                Boundary: wire shapes live in api/ docs, not schema
S[# Code]                                  Ground truth for behavior
   |<                                      Backlinks from docs to code, code to docs
[# Layered Knowledge Architecture ]        The system as a whole
```

This is already a sophisticated knowledge architecture. It embodies several library science principles:
- **Authority control** (schema.prisma is canonical; the ontology governs domain terms)
- **Stable identifiers** (FR-*, PG-*, EP-*, M-*, ADR-*, Q-*)
- **Faceted classification** (pages, api, data-model as independent but cross-linked facets)
- **Layered access** (CLAUDE.md -> .system3 -> colocated READMEs -> inline comments)

### What's Missing: A TopoGlyph Analysis

```
??                                        Gap analysis reveals:

▫[ Temporal context ]                     Why was this decision made? What was true then?
   ^t missing                             No temporal layering on design decisions
   |< partial                             ADRs exist as concept but aren't populated

▫[ Audience routing ]                     Who needs what, when?
   >| implicit                            Wayfinding assumes the reader knows the structure
   [F] single-axis                        Entry is by catalog type, not by task or role

▫[ Decay detection ]                      What's gone stale?
   !t absent                              No mechanism to flag docs when related code changes
   ~t unknown                             No staleness signal on any knowledge unit

▫[ Tacit->Codified pipeline ]             How do insights from work sessions become durable knowledge?
   >> ad-hoc                              Memory system captures some; most is lost
   [!k] informal                          Provenance is conversation context, which compresses away

▩[ Cross-tool fragmentation ]             Knowledge lives in Linear, Slack, PRs, and docs
   <> manual                              Reconciliation happens in someone's head, if at all
```

---

## Architectural Patterns for Project Knowledge Libraries

### Pattern 1: The Inverted Pyramid

From journalism and information architecture -- structure knowledge so the most important, most frequently needed information is encountered first, with detail available on demand.

```
[S][▪ Orientation ]             What is this project? Where do I start?
   >|
[S][▪ Navigation ]              What are the major areas? How are they related?
   >|
[S][▪ Reference ]               Detailed specs, schemas, API contracts
   >|
[S][▪ Archaeology ]             Why was it built this way? What was tried and rejected?
```

Each layer answers a different question at a different depth. Readers (human or LLM) enter at the top and go deeper only as needed. Asta's CLAUDE.md-first approach already implements this -- the extension is making each layer's purpose and audience explicit.

### Pattern 2: The Controlled Vocabulary Spine

From library science -- the organizational backbone that makes everything findable.

```
[# Ontology ]                   Canonical domain terms
   [+k]                        Every knowledge unit tagged with domain(s)
[# ID Registry ]                Every documented thing has a stable, greppable ID
   [-k]                        Cross-references always use IDs, never prose
[# Authority Rules ]            Per knowledge type: which source is canonical
   [=k]                        Conflicts resolved by consulting the authority

   >> new term                 When new knowledge resists existing vocabulary:
[# Ontology' ]                  The ontology evolves (in the same commit as the knowledge)
```

The key insight from library science: the vocabulary *is* the architecture. Change the vocabulary, and you change what's thinkable within the system.

### Pattern 3: The Living Bibliography

From library science's collection development -- the library as an active process, not a static collection.

```
▪[# Knowledge Unit ]
   @t last_verified: 2026-06-15
   ~t volatility: high (touches actively developed code)
   !t refresh_when: [schema.prisma changes, related EP-* changes]
   [!k] source: PR #4155, conversation with Zach 2026-06-20

   !t fires!                   Refresh trigger: schema.prisma was modified
   <>                          Reconciliation: compare doc against new schema
   ▪ -> ▧                      Doc has drifted
   update                      Update doc
   @t last_verified: 2026-06-23
   ▪[# Refreshed ]             Knowledge is current again
```

This pattern addresses the most common failure mode of project documentation: silent decay.

### Pattern 4: The Dual-Audience Architecture

From information architecture -- the same knowledge base serves both human navigation and LLM consumption, but their access patterns differ fundamentally.

```
[# Human Reader ]               Navigates visually, skims, follows links
   >| browsing                  Needs wayfinding cues, visual hierarchy, progressive disclosure
   [F] by task                  Enters with a task: "how do I add a new API endpoint?"

[# LLM Reader ]                 Reads linearly, reasons over full text, follows instructions
   >| instruction               Needs explicit rules, greppable IDs, unambiguous authority
   [F] by context window        Enters via CLAUDE.md, then reads referenced files

   sync                        Synchronization: both audiences served by same artifacts
[▪ Shared Knowledge Unit ]      One document, structured to serve both:
   - Front-matter (machine-readable metadata, IDs, cross-refs)
   - Prose body (human-readable explanation)
   - Conventions section (LLM-actionable rules)
```

---

## Meta-Pattern: Knowledge as Topology

The deepest insight from applying TopoGlyph to this problem:

```
S[# Code ]       ============= Ground truth for WHAT
   ...
D[# Design ]     ============= Ground truth for WHY
   ...
T[# History ]    ============= Ground truth for WHEN and WHO
   ...
M[▫ Mental Models ] ========== Ground truth for HOW (to think about it)
   ...
L[▪ Conventions ]  =========== Ground truth for HOW (to work with it)

   [-k] [-k] [-k]             Cross-references form the topology
   [=k]                       Authority control prevents contradiction
   @t !t                      Temporal anchors prevent decay
   >| [F]                     Navigation makes the topology traversable
   >> <<                       Accession and weeding keep the topology alive

[# Knowledge Library ]         Not a collection of documents --
                               a maintained topology of relationships
                               between sources of truth
```

A project knowledge library is not "documentation." It is the **maintained topology of relationships between heterogeneous sources of truth**, each authoritative for a different facet of the project, linked by stable identifiers, navigable by multiple audiences, and actively defended against decay.

The library's value is not in any single node. It is in the edges -- the cross-references, the authority rules, the temporal anchors, the navigation paths. Lose the edges, and you have a pile of files. Maintain the edges, and you have a knowledge system.

---

## Prescriptive Implications

The TopoGlyph analysis suggests three high-leverage interventions, ordered by the tensions they address:

**1. Decay detection** (`!t` refresh triggers): When code that a document describes changes, the document should be flagged for review. This could be as simple as a comment in the doc listing the files it describes, checked by a CI step or git hook. Without this, the library silently rots.

**2. Accession discipline** (`>>` with criteria): The heuristic "save what is surprising or non-obvious" is exactly right. Extend it to all knowledge capture. Before writing a doc, ask: is this derivable from an existing authoritative source? If yes, link to that source instead.

**3. Temporal provenance** (`@t` + `[!k]`): Every knowledge unit benefits from knowing when it was last verified and what prompted its creation. Not as bureaucracy -- as a signal to future readers (human or LLM) about how much to trust what they're reading.

---

*TopoGlyph 9.0 extends the framework from modeling how cognition creates knowledge to modeling how knowledge is organized, maintained, and made findable -- the topology not of thought, but of what thought has produced and left behind for others.*
