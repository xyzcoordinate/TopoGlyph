---
muid: topoglyph-12-ontology-comparison
refs:
  - ~/.topoglyph/modules/topoglyph.md
  - ~/.topoglyph/modules/topoglyph-9-1-onto-operational-form.md
  - ~/.topoglyph/modules/graph-schema-and-enrichment.md
created_at: 2026-05-17
---

# TopoGlyph 12 — Ontology Comparison and Projection

Treats a planning ontology as a closed graph of node kinds + edge kinds, with each concept articulated at one of three levels: `implicit`, `property`, or `node`. Lets agents compare two ontologies structurally and project from one to the other by mapping articulation levels and recording lossy projections.

Where 9.1 specifies the *operational form* of a single ontology (how it's stored, how drift is detected), 12 specifies how to *relate two ontologies* — what's preserved, what's lost, what's introduced when you project from one to another.

## When to reach for this module

- Comparing two planning ontologies structurally (not just feature-list)
- Deriving ontology B's spec from ontology A's spec
- Building agents that seed one `⌗ᵒⁿᵗᵒ` folder from another
- Auditing a multi-ontology system for round-trippability
- Justifying why a primitive was promoted to node status (or demoted)

Do not use it for:
- Comparing two *instances* under the same ontology (use 9.1 drift detection)
- Designing a new ontology from scratch (use the planning-primitive design modules)
- Generic graph isomorphism (this is about *meaning-preserving* projection, not structural identity)

## Core elements

### Ontology-level

| Symbol | Meaning |
|---|---|
| `⫷O⫸` | A planning ontology — closed set of node kinds + edge kinds + invariants |
| `◈ᴿ` | A node kind in an ontology (rank-promoted concept) |
| `Δᴬ` | **Articulation level** of a concept: one of `implicit \| property \| node` |
| `⊙ᵒ` | **Implicit operation** the ontology is shaped to support |
| `⊕ᵃ` | Affordance: a traversal that's a single query in this ontology |
| `⊖ᵃ` | Anti-affordance: a traversal that requires leaving the ontology |

### Cross-ontology operators

| Symbol | Meaning |
|---|---|
| `⫶≡⫶` | Isomorphism (same shape, different names) |
| `⫶⊆⫶` | Containment (A's primitives are a subset of B's) |
| `⫶⨯⫶` | Orthogonal (different axes emphasized; not comparable axis-wise) |
| `⫶⊕⫶` | Composition (one ontology layered on another) |

### The five projection operators

When projecting ontology A → B, every concept in A maps to B via exactly one of:

| Operator | Symbol | Definition |
|---|---|---|
| **Conservation** | `≡` | A's primitive maps 1:1 to a B primitive (possibly renamed) |
| **Promotion** | `⨇` | Concept that was implicit/property in A becomes a node in B |
| **Demotion** | `⨆` | Concept that was a node in A becomes property/implicit/absent in B |
| **Bifurcation** | `⨄` | One A primitive splits into multiple B primitives |
| **Conflation** | `⨃` | Multiple A primitives merge into one B primitive |

Every demotion to *absence* is a recorded `⊖ᵃ` — a query that A could answer and B cannot. Every conflation is a recorded loss of distinction. Every primitive in B with no source in A is a `⊕ᵃ` — an affordance B brings that A lacks.

## The projection protocol

To project `⫷A⫸ → ⫷B⫸`:

```
①  Enumerate A's primitives {a_i}
②  For each a_i, classify Δᴬ(a_i, B):
       node       → ≡ conservation (with possible rename)
       property   → ⨆ demotion (record where the property attaches)
       implicit   → ⨆ demotion to absence (record ⊖ᵃ)
       split      → ⨄ bifurcation (record the multiple targets)
       merged     → ⨃ conflation (record what was lost in the merge)
③  Enumerate B's primitives {b_j} with no source in A
       these are ⊕ᵃ — B-native affordances
④  Emit:
       mapping_table:  {a_i ↔ b_j ∪ losses ∪ B-native}
       loss_inventory: {⊖ᵃ queries A could answer, B cannot}
       gain_inventory: {⊕ᵃ queries B answers natively}
⑤  Verify round-trip:
       can B → A reconstruct A's content? if not, B is a strict projection,
       not an equivalent ontology
```

## Dimensions of variation

A standard matrix for comparing two ontologies:

| Axis | Question |
|---|---|
| Outcome / Approach separation | Are "desired state" and "chosen approach" distinct node kinds? |
| Question as node | Is `known-unknown` a first-class primitive or a comment? |
| Constraint as node | Is `bounded-by` first-class or a document? |
| Bet revisability | Can a chosen approach be abandoned without losing its parent outcome? |
| Rationale capture | Does the schema record *why* this approach was chosen? |
| Execution layer separate | Is the do-this-now layer structurally distinct from the planning layer? |
| Cadence built in | Is fixed-period container (Sprint, Cycle) a primitive? |
| Team / role primitives | Are roles, assignees, ownership first-class? |
| Aggregation up org | Does the ontology support cascade from individual → team → org? |
| Multi-approach per outcome | Can multiple competing bets coexist for one outcome? |

## Worked example: default → Agile

```
⫷default⫸ = {Project, Outcome, Constraint, Move, Question, Task, Learning}
⫷agile⫸   = {Project, Epic, Story, Task, Subtask, Spike, Sprint, DoD-doc}

Projection (default → agile):
  Project    ≡ Project
  Outcome   ⨃ Story         (merged with Move into Story body)
  Move      ⨃ Story         (merged with Outcome into Story body)
  Constraint ⨆ DoD-doc      (demoted to checklist; loses queryability)
  Question  ⨆ Spike         (demoted to task-shaped research; loses "blocks Move" edge)
  Task       ≡ Task
  Learning  ⨆ Retro-notes   (demoted to per-sprint ritual artifact)

  agile-native (⊕ᵃ):
  Epic       — intermediate layer between Project and Story
  Subtask    — finer decomposition under Task
  Sprint     — fixed-cadence container
  Story Pts  — estimation property on Story
```

Loss inventory: an Agile user cannot ask "which Outcomes have no active Move?" because Outcome and Move are conflated into Story. They cannot ask "which Moves are blocked by which Questions?" because Question is reduced to a task-shaped Spike with no `blocks_move` edge.

## Worked example: default → Lean

```
⫷lean⫸ = {Hypothesis, Experiment, Result, Assumption, ValidatedLearning, Pivot/Persevere}

Projection (default → lean):
  Project    ⨆ initiative-scope (implicit at MVP level)
  Outcome   ⨆ implicit (assumed one level above the loop)
  Constraint ⨆ absent
  Move       ≡ Hypothesis        (strongest isomorphism — both bets, both revisable)
  Question   ≡ Assumption         (both known-unknowns that change the bet)
  Task       ≡ Experiment         (renamed to emphasize test-not-execute)
  Learning   ≡ ValidatedLearning  (Lean-named)

  lean-native (⊕ᵃ):
  Result     — outcome of running Experiment against Hypothesis
  Pivot/Persevere — decision derived from Result
```

The Move ≡ Hypothesis correspondence is the cleanest cross-ontology match in this group: both are revisable bets with rationale, both can be abandoned without losing the parent. Lean and default share a *bet-shaped* topology; Agile does not.

## Meta-insight

Promotion of a concept from `implicit` or `property` to `node` is the highest-leverage design move in any planning ontology. It changes what is countable, queryable, ageable, linkable, surfaceable. Most planning ontologies are distinguishable from each other *entirely* by which concepts they promoted to node status.

When projecting A → B, the asymmetric losses (a concept demoted, a query no longer answerable) are usually more revealing than the symmetric mappings. The loss inventory is the honest summary of what the projection costs.
