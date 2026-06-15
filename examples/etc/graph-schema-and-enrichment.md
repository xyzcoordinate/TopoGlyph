# TopoGlyph 11.0: Declarative Graph Schemas and Progressive Enrichment

A TopoGlyph extension for designing, reading, and reasoning about **stored knowledge graphs** — schemas where every connector carries metadata the store enforces (type, cardinality, uniqueness, containment), where entities carry an epistemic layer (draft/inferred/asserted), and where the graph is built up by a serial pipeline of staged transformations against a single shared store.

Targeted at datascript / Datomic / EDN-style schemas, but the constructs generalize to any typed property graph (Neo4j, RDF/SHACL, Postgres + FK constraints) and any pipeline that enriches a single store across stages.

## Why a new module is warranted

Prior TopoGlyph treats nodes and connectors as *cognitive concepts* — `─` is "a connection," `⦗ ⦘` is "a domain." For schema design that's not enough. A stored knowledge graph has structural commitments the store enforces:

1. **Typed connectors** — a reference to another entity is fundamentally different from a scalar value, and the store knows the difference.
2. **Cardinality** — single-valued vs many-valued is a hard constraint, not a stylistic choice.
3. **Identity vs uniqueness** — identity attributes enable upsert lattices that make pipelines re-runnable without integer-id threading.
4. **Containment vs reference** — most stores cannot distinguish "parent owns child" from "parent points at child," but the *schema designer* must, because cascade-on-delete depends on it. This gap is itself a topological feature worth surfacing.
5. **Provenance as a perpendicular axis** — entities in a work-in-progress graph carry an epistemic layer (draft, inferred, asserted) that floats over the structural graph and changes independently from it.
6. **Progressive enrichment** — the same store is mutated by a serial pipeline of stages; the structural shape is stable but the populated-attribute set evolves stage by stage.

None of these have notation in TopoGlyph 1.0–10.0. This module supplies it.

## Modules

### Module A — Schema Connector Decoration

**Connector types** (kind of value the attribute holds):
- `⊸` reference attribute (typed pointer; replaces a manual edge table)
- `⊷` scalar attribute (primitive value: string, int, etc.)
- `⊶` compound attribute (struct/nested-shape value)

**Cardinality markers** (suffix on the connector):
- `∙¹` single-valued (cardinality one)
- `∙ⁿ` many-valued (cardinality many)
- `⁰¹` optional single-valued

**Constraint markers** (prefix on the attribute):
- `⊪` identity attribute (`:db.unique/identity` — addressable, upsertable by value)
- `⊫` unique-but-not-identity (rejects duplicates, no upsert semantics)
- `⊩` required attribute (must be present on every entity of this kind)

**Containment vs reference** (semantic overlay on `⊸` — not enforced by the store, but load-bearing for the design):
- `⊐` strong containment (parent owns child; lifetime coupled; cascade on delete)
- `⊳` weak reference (independent lifetime; child outlives parent; no cascade)
- `⊵` back-reference (inverse pointer, often derivable from forward refs)

The store usually distinguishes scalar vs ref but **does not** distinguish `⊐` from `⊳`. That gap is a *topological feature*, not an oversight: it tells you exactly where the codec or transaction layer must take over. Make it visible in the diagram so the gap doesn't get forgotten.

### Module B — Provenance Layers

Every entity in a work-in-progress graph carries an epistemic status. The same structural slot can host different layers, and the layer changes over time:

- `▦` draft layer (asserted by an LLM or upstream stage; not yet author-blessed)
- `▥` inferred layer (derived deterministically from other entities)
- `■` asserted layer (canonical truth, author- or stage-blessed)
- `↥` promotion operator (`▦` → `■` or `▥` → `■`)
- `↧` demotion operator (`■` → `▦` on retraction)

Provenance is **orthogonal** to structure. Render it as a perpendicular axis floating over the entity graph, not as part of the entity glyph itself. Mixing them collapses two independent dimensions into one and obscures which one is changing at any given stage.

### Module C — Progressive Enrichment

For graphs whose final shape emerges through staged writes against a single shared store:

- `▷ₙ` stage n transformation (writes a defined slice of the schema)
- `⥲ₛ` stage s reads from the graph
- `⥱ₛ` stage s writes-and-merges into the graph
- `⊞↥` upsert by identity attribute (idempotent, re-run-safe)
- `⌗` stage barrier (preserves ordering; can be a no-op)

Stages compose strictly: `▷₁ → ⌗ → ▷₂ → ⌗ → ▷₃ …`. Hold the barriers even when a stage is a no-op so the structural invariant "stages run strictly in order" is preserved for any future idempotent-re-run feature.

The relationship between Module A's `⊪` (identity) and Module C's `⊞↥` (upsert by identity) is the load-bearing one: identity attributes are what make `⊞↥` possible, and `⊞↥` is what makes a pipeline re-runnable without threading integer ids through stage code. If you have one without the other you've left value on the table.

## How to read a schema with these constructs

Three passes, in order:

**Pass 1 — decomposition skeleton.** Find the `⊐∙ⁿ` (strong-containment, many) chains. These form the tree backbone of the graph. Common shapes: `Project ⊐ Module ⊐ File`, `Document ⊐ Section ⊐ Paragraph`, `Arc ⊐ Beat ⊐ Scene ⊐ Panel`. Cascade-on-delete must follow these edges.

**Pass 2 — ontology fill.** Find the `⊳∙ⁿ` and `⊳∙¹` (weak references). These are the cross-cutting entities that get referenced from multiple points in the tree: people, locations, tags, types, events. Cascade-on-delete must **not** follow these edges.

**Pass 3 — identity lattice and provenance overlay.** Find the `⊪` attributes — these are the upsert addresses. Then look for the provenance axis: are entities marked `▦`/`▥`/`■`? Is there a stage that does `↥` promotion? If yes, the schema is hosting a work-in-progress, and the structural commitments belong to the *finished* shape while the populated content reflects the in-progress state.

If a schema has no `⊪` attributes and no provenance axis, it's a static schema and Modules B and C don't apply.

## The deepest signature: tree-plus-lattice with provenance overlay

Schemas of this kind exhibit a recurring meta-pattern:

```
⟦Stable Structural Schema⟧  ⊕  ⟦Mutable Provenance Layer⟧  →  ⟦Pipeline Graph⟧
                                       ↥/↧
                              (only this changes per-stage)
```

The structural skeleton is the schema of a *finished* artifact. Provenance is the mechanism that lets the finished-artifact shape host an in-progress artifact's content. The same schema can serve stage 1 (where almost everything is empty) and stage N (where everything is filled) because the structural commitments are stable; only the provenance markers and the populated-attribute set change.

This is isomorphic (`⥇`) to:
- **Filesystems with symlinks** — directory tree (`⊐`) plus cross-cutting symlinks (`⊳`).
- **ASTs with symbol tables** — syntactic tree (`⊐`) plus semantic name resolution (`⊳`).
- **Databases with soft-deletes** — row stays put; a flag floats over it.
- **Git's working tree vs index vs HEAD** — one tree shape, multiple layered states.
- **Type systems with refinement types** — type stays; predicates layer in.

Recognizing the isomorphism predicts which UX patterns the system will eventually want — diff views, promotion approval UIs, layer-isolation queries, find-references, cascading-delete confirmations — and which integrity-bug shapes will eventually bite (orphaned refs, dangling pointers, stale back-references, premature cascade).

## Worked example: the narrative-graph dsSchema

A datascript schema that turns comic outline beats into structured story-world entities and panel-level prose, built up across ten pipeline stages.

### Two-axis topology

```
                     Decomposition axis (containment ⊐)
                     ─────────────────────────────────►

        Arc  ⊐∙ⁿ  Beat  ⊐∙ⁿ  Scene  ⊐∙ⁿ  Panel  ⊐∙ⁿ  Dialogue
                    │          │            │
                    ⊳∙ⁿ        ⊳∙ⁿ          ⊳∙¹
                    ▼          ▼            ▼
                  ┌────────────────────────────────┐
                  │      Ontology axis (refs ⊳)    │
                  │                                │
                  │  Character   Location          │
                  │  Event       LoreElement       │
                  └────────────────────────────────┘
```

Vertical edges are `⊐∙ⁿ` (strong containment, many): deleting a beat *should* cascade to its scenes; deleting a scene *should* cascade to its panels. Datascript does not enforce this — the codec layer must.

Diagonal edges are `⊳∙ⁿ` (weak reference, many): Avis appears in five beats; deleting one beat must not delete Avis. Datascript does this correctly automatically because refs are entity ids, not contained values — but a careless cascade rule would break it.

### Identity lattice

```
⊪ :entity/name      every world-entity is name-addressable
⊪ :beat/position    every beat is position-addressable ("Prologue", "1", …)
```

Two upsert lattices. Stage code never threads integer ids:

```
⊞↥ ["entity/name" "Avis"]    addresses Character "Avis" across all stages
⊞↥ ["beat/position" "Prologue"]    addresses the Prologue beat across all stages
```

This is the structural commitment that makes per-stage re-running tractable as a v2 extension, even though v1 doesn't implement it. The capability exists today; a workflow that exploits it can be added later without schema migration.

### Provenance overlay

```
Structural layer:    Arc ⊐ Beat ⊐ Scene ⊐ Panel
                      │     │       │       │
Provenance layer:    [■]   [■]     [■]     [▦]      ← varies per-entity, per-stage
```

The same `:character/Avis` entity can be `▦draft` after stage 2, then `■asserted` after stage 4 promotes it via `↥`. Provenance is per-entity, not per-stage and not per-attribute.

### Enrichment stack

```
▷₁  parse outline          ⥱₁ {Arc, Beat}                          no LLM
⌗
▷₂  extract entities       ⥱₂ {▦Character, ▦Lore, ▦Loc, ▦Event}    Claude
⌗
▷₃  reconcile              ⥱₃ {alias consolidation}                deterministic
⌗
▷₄  enrich beat            ⥲all  ⥱₄ {Beat enrichment, ↥Event}     Claude
⌗
▷₅  consistency            ⌗ (no-op for single beat)
⌗
▷₆  decompose scenes       ⥱₆ {Scene} ⊐ Beat                      Claude
⌗
▷₇  draft scenes           ⥱₇ {:scene/draftProse, ▦Event}         GPT-5
⌗
▷₈  decompose panels       ⥱₈ {Panel skeleton} ⊐ Scene            Claude
⌗
▷₉  panel prose            ⥱₉ {:panel/visual/*, :panel/dialogue}  Claude
⌗
▷₁₀ writeback              ⥲all  → graph.edn.json + panels.md
```

The graph is monotonically enriched in v1 — no stage deletes, no stage replaces. The barrier `⌗` is held even where a stage is a no-op (`▷₅`) so the ordering invariant survives intact for v2's idempotent re-runs.

### Schema integrity gaps surfaced by topology

Three things the dsSchema cannot declare and must therefore enforce elsewhere:

| Gap | Where it lives instead |
|---|---|
| Containment vs reference (`⊐` vs `⊳`) | Codec conventions (`src/schema/codec.ts`) and writeback |
| Per-stage write authority | Stage modules (`src/stages/`) — convention only |
| Legal provenance transitions (`↥`, `↧`) | Implicit in stage logic (only `▷₄` does `↥` on Events) |

These gaps are *intentional* in v1: the cost of a custom datascript wrapper exceeded the benefit. They're forward-compatible because the codec is a chokepoint for every transaction. Topology makes the gaps visible; deferring them is a deliberate trade, not an oversight.

### Why the same schema serves stages 1 and 10

The structural commitments — Arc contains Beats, Scenes have a Location, Panels have Dialogue with Speakers — are the commitments of a *complete* story. Provenance is the mechanism that lets that finished-story shape host in-progress content:

```
▦draft entities + ■asserted skeleton  =  legal store at every stage
```

This is the meta-pattern of the whole pipeline. It's why v1 (a single-beat slice) and v2 (multi-beat with consistency passes and re-runs) can share a schema: the structural topology is stable across the entire pipeline, and what evolves is the layer of provenance draped over it.

## When to use this module

- Designing a datascript / Datomic / EDN schema for any domain where a single store will be progressively enriched.
- Reviewing a schema design doc for missing identity attributes, ambiguous containment semantics, or absent provenance dimensions.
- Auditing a pipeline that mutates a shared store across stages, looking for stage-ordering hazards or missing barriers.
- Recognizing when a draft/asserted distinction in the data model is doing structural work and deserves first-class treatment.
- Predicting which integrity-bug shapes and UX features a system will need by mapping it onto the tree-plus-lattice-with-provenance signature.

## When *not* to use this module

- Pure cognitive-process modeling — TopoGlyph 1.0–8.0 already does that well.
- Static schemas with no enrichment pipeline and no provenance axis — Module A is useful but B and C are dead weight.
- Document-tree systems without typed cross-references — TopoGlyph 10.0 (Documentation Topology) is the better fit.
- Schemas where everything is already enforced by the type system (e.g. SQL with full FK constraints + check constraints + RLS) — the gap-surfacing value of Module A's containment-vs-reference distinction is reduced.
