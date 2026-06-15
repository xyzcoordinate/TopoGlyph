---
muid: topoglyph-13-llm-facing-ontology-design-space
refs:
  - ~/.topoglyph/modules/topoglyph.md
  - ~/.topoglyph/modules/application-design-comprehension.md
  - ~/.topoglyph/modules/topoglyph-9-1-onto-operational-form.md
  - ~/.topoglyph/modules/graph-schema-and-enrichment.md
  - ~/.topoglyph/modules/topoglyph-12-ontology-comparison.md
created_at: 2026-05-23
---

# TopoGlyph 13 — LLM-Facing Ontology Design Space

A vocabulary for navigating the design choices when building an ontology whose **primary consumer is an LLM** (not a human, not a database query planner). Names the dimensions of variation, the cognitive functions the ontology plays, the cost/benefit operators specific to context-bound consumers, and the archetypal strategies that occupy distinct regions of the space.

Where 9.0 specifies *how to read* an existing codebase, 9.1 specifies the *operational form* of a single ontology, 11.0 specifies the *structural primitives* of a stored schema, and 12.0 specifies *how to relate two ontologies*, module 13 specifies how to **choose what kind of ontology to build in the first place** when the reader is an LLM.

## Why a new module is warranted

LLM-facing ontologies have properties that don't apply to ontologies built for humans or for database query planners:

1. **Context cost is the binding constraint.** Every node loaded costs tokens; an exhaustive ontology that won't fit in context is worse than a sparse one that does.
2. **The reader can reconstruct.** An LLM given a pointer can re-derive a fact from source. The ontology need not store what the LLM can rebuild cheaply — it should store what the LLM *cannot* rebuild, or what the LLM rebuilds *wrong*.
3. **The reader is stateless across sessions.** No tacit accumulation. The ontology must contain its own bootstrapping primer or every session starts cold.
4. **Multiple readers must agree.** When agents A and B both say "the Composer," the ontology earns its keep by making them mean the same thing. Without canonical handles, agents drift in private.
5. **Reader confusions are predictable.** Type vs instance, name vs concept, runtime vs model — the same misreadings recur. The ontology can pin them.
6. **The reader is fallible at structure but strong at semantics.** LLMs can interpret prose well but lose track of large graphs. The ontology's *shape* matters more than its *vocabulary*.

None of 9.0–12.0 helps a designer decide *which* shape to build. This module supplies the design-space vocabulary.

## Module A — Cognitive functions an ontology plays for an LLM

What is the ontology *for*, from the reader's perspective? Most working ontologies play several of these at once; weighting them deliberately is the first design move.

| Symbol | Function | What the LLM gains |
|---|---|---|
| `⟁ᵍ` | **Grounding** | Shared vocabulary — two agents saying "X" mean the same X |
| `⟁ˢ` | **Scaffolding** | Skeleton to hang specific knowledge on without re-deriving structure |
| `⟁ᵈ` | **Disambiguation** | Pins for predictable confusions (type vs instance, layer vs implementation) |
| `⟁ᵃ` | **Affordance map** | Catalog of moves the system supports, with preconditions |
| `⟁ʳ` | **Reading order** | What to load first for a given task; budget allocation |
| `⟁ᶜ` | **Coordination handle** | Stable place where multiple agents register decisions, plans, claims |
| `⟁ᵛ` | **Verifier** | Something to check generated claims against, so hallucinations have a target |

A *grounding-heavy* ontology looks like a glossary of canonical names. A *verifier-heavy* ontology looks like a typed graph the LLM can be tested against. A *scaffolding-heavy* ontology looks like an architecture map with hooks for further specification. These are not interchangeable.

## Module B — Axes of design variation

Each axis is a continuum. A candidate ontology sits at one point on each. The set of choices *is* the design.

| Axis | Symbol | Endpoints |
|---|---|---|
| Articulation depth | `⫾ ⫿` | shallow (names only) ↔ deep (full semantics) |
| Anchoring | `⫹ ⫺` | floating (LLM-asserted) ↔ anchored (every node binds to source) |
| Authority shape | `⩙` | flat (all sources equal) ↔ tiered (precedence rule declared) |
| Persistence | `⊞· ⌭` | session-scoped (rebuild each time) ↔ accumulated (long-running artifact) |
| Drift posture | `⌬ ⌯` | eager-verify (check on read) ↔ lazy (trust until contradicted) |
| Storage strategy | `⨎ ⫙` | exhaustive (every fact stored) ↔ seed-and-rebuild (store enough to re-derive) |
| Multi-agent shape | `⨊ ⨉⨉` | private (one agent's notes) ↔ shared (consensus enforced) |
| Grain | `▤ ▦` | coarse (concepts/modules) ↔ fine (lines of code) |
| View count | `⟁ ⟂` | mono (one canonical structure) ↔ poly (multiple projections of same entities) |
| Self-description | `⫼` | implicit (humans transmit conventions) ↔ self-bootstrapping (ontology contains its own schema) |

A point in this 10-dimensional space is a *candidate design*. Most design failures correspond to inconsistent points — e.g. *exhaustive + session-scoped* (rebuilds an exhaustive ontology every session, burning tokens), or *floating + shared* (agents agree on names that aren't tethered to anything, so they silently drift).

## Module C — Cost & benefit operators specific to LLM consumers

The currency in which design moves are evaluated:

| Operator | Meaning |
|---|---|
| `¢ᶜ` | **Context cost** — tokens consumed when loaded |
| `¢ᵐ` | **Maintenance cost** — operator effort to keep current |
| `¢ᵛ` | **Verification cost** — work per `⩕` (per 9.1's freshness check) |
| `$ᵍ` | **Grounding gain** — confusion-reduction value across agents |
| `$ʳ` | **Reasoning gain** — inferences enabled that would otherwise require re-walking source |
| `$ᵃ` | **Action gain** — moves the LLM can make safely that it couldn't before |

`Δ = ($ᵍ + $ʳ + $ᵃ) − (¢ᶜ + ¢ᵐ + ¢ᵛ)` is the **net value** of a design move.

Most ontology failures are unmeasured `¢ᵐ` (operator burnout — hand-curated ontology decays into stale liability) or unmeasured `¢ᶜ` (token budget devoured by ontology weight that the LLM could have rebuilt from source for the same cost). A design move with `Δ ≤ 0` is a tax; one with `Δ ≥ 0` only if you actually measured.

## Module D — Archetypal strategies

Each archetype is a coherent point in the Module B space, weighting Module A's functions differently. They are not exhaustive — the value of the vocabulary is in *naming the regions* so candidates can be compared.

### 1. Anchored Skeleton + Code-as-Ground `(⫹ ⫾ ⊞· ⨊)`
Shallow articulation; every ontology node carries an anchor to source file/line. The LLM reconstructs semantics by reading code through the anchor. The ontology stores *pointers*, not *facts*.
- **Weights:** `⟁ᵍ` high, `⟁ʳ` low, `⟁ᵛ` strong via re-derivation
- **Costs:** low `¢ᶜ`, low `¢ᵐ`, moderate `¢ᵛ`
- **Failure mode:** useless when source is opaque; LLM keeps re-deriving the same thing

### 2. Schema-First Typed Graph `(⫷O⫸ ⊪ ⊞↥)`
Per 11.0 — schema declares entity kinds, typed relationships, identity attributes. Stored in a graph DB; agents query rather than read prose. The ontology is *queryable structure*.
- **Weights:** `⟁ˢ` high, `⟁ʳ` high, `⟁ᵛ` strong (claims testable against the store)
- **Costs:** high `¢ᵐ` (schema design effort), low `¢ᶜ` per query, drift-prone
- **Failure mode:** schema drifts ahead of populated content; LLM queries return empty for entities the schema implies should exist

### 3. Stratified Comprehension-Walk Artifact `(▦▤▥ ⫶ ⩙)`
The ontology *is* the output of 9.0's eight-step walk: substrate-tagged, authority-resolved, stratigraphy-aware, invariants surfaced. Built once per major change; consumed by new agents joining.
- **Weights:** `⟁ᵍ` strong, `⟁ʳ` strong for onboarding, `⟁ʳ` decays over time
- **Costs:** high `¢ᵐ` per walk; very high `$ᵍ` for new agents
- **Failure mode:** ages out fast; no `⩕` oracle; staleness invisible until contradicted

### 4. Affordance Catalog `(⊕ᵃ registry)`
The ontology is a *list of moves* — operations with preconditions, effects, costs. Per-task scope. The LLM uses it to *plan*, not to *comprehend*. The MCP tool surface is a working example.
- **Weights:** `⟁ᵃ` dominant, `⟁ʳ` low, `⟁ˢ` low
- **Costs:** low `¢ᶜ` (load per task), moderate `¢ᵐ` (every new affordance must register)
- **Failure mode:** silent when the LLM needs to understand *why* a move exists; can't reason about novel compositions

### 5. Disambiguation Pin Set `(⫳ registry)`
The ontology is *only* the things LLMs are observed to get wrong. Tiny; often lives inline in CLAUDE.md or system prompt. Grows as confusions are observed.
- **Weights:** `⟁ᵈ` dominant; `$ᵍ` per token is very high
- **Costs:** very low `¢ᶜ`; `¢ᵐ` requires observing failures (slow accumulation)
- **Failure mode:** covers only *known* confusions; new ones slip through silently

### 6. Reconstruction Seed `(⫙ + ⨎)`
The ontology is a *minimal canonical handle set* + a *reading order*. The LLM rebuilds the rest from source on demand using the handles as pivots.
- **Weights:** `⟁ᵍ` high, `⟁ʳ` deferred (paid at query time), `⟁ᶜ` strong
- **Costs:** lowest `¢ᶜ`; highest `¢ᵛ` per query (re-derivation each time)
- **Failure mode:** re-derivation cost compounds across many small queries; doesn't amortize

### 7. Polyontology `(⟂ᵢ projection set)`
Same entities, multiple views — structural (types/files), behavioral (call graph/runtime), intentional (features/journeys), historical (decisions/migrations). Each view is itself an archetype 1–6; projections between them are first-class per 12.0.
- **Weights:** highest `⟁ʳ` for cross-cutting tasks; `⟁ᵛ` strong (views cross-check)
- **Costs:** highest `¢ᵐ` (N ontologies to maintain); risk of inter-view drift
- **Failure mode:** views drift apart silently — structural says X; behavioral says Y; nobody owns the reconciliation

## Module E — Mode selectors (task → strategy)

No single archetype serves all LLM tasks. The meta-ontology often needs to be a **router** — a small index that picks an archetype per task — rather than a single artifact.

```
?⦗LLM task⦘
   ⤳ debugging       → 1 + 7-behavioral  (anchored skeleton + runtime view)
   ⤳ planning        → 4 + 5             (affordances + disambiguations)
   ⤳ onboarding      → 3                 (comprehension walk artifact)
   ⤳ schema design   → 2                 (typed graph)
   ⤳ refactoring     → 1 + 7-structural  (anchored + structural view + ⟦◈⟧ invariants)
   ⤳ feature work    → 4 + 6             (affordances + reconstruction seed)
   ⤳ cross-agent coord → 5 + canonical handles
```

The router itself is `⟁ʳ` (reading order) for the *ontology* — a meta-`⫼ʳ`. It tells a cold agent which slice of the ontology to load for the task it's about to do.

## Module F — The self-description imperative

Unique to LLM-facing ontologies: the reader is stateless across conversations. The ontology must contain enough to bootstrap a cold LLM into using it.

| Symbol | Required contents |
|---|---|
| `⫼ʳ` | **Reading-order primer** — "for task T, load files X, Y, Z in this order" |
| `⫼ˢ` | **Schema description** — the ontology's own shape, stated in natural language |
| `⫼ᵛ` | **Vocabulary index** — canonical handles with disambiguation pins |
| `⫼ᵒ` | **Operations manual** — how to query, update, verify, extend |

9.1's `README.md` (the `⊞ʳ` rules file) is the operational instance of `⫼` for the flat-file form. Generalize: **every LLM-facing ontology needs a constitution file** the LLM can read first, or it's not actually deployable across sessions.

Without `⫼`, the ontology depends on the operator re-transmitting it through conversation each time — which means the operator is the ontology and the on-disk artifact is decoration.

## Reading the design space

Given a candidate ontology, the diagnostic walk:

1. **Function mix.** Which `⟁` functions does it weight? Is the weighting deliberate, or a side effect of how it was built?
2. **Axis point.** Where does it sit on each axis in Module B? Are any pairs internally inconsistent (e.g. *exhaustive + session-scoped*)?
3. **Net value per move.** For the last three ontology changes: estimate `Δ`. Were they `Δ > 0` or accidental tax?
4. **Archetype mapping.** Which archetype(s) does it resemble? Does it occupy a coherent region or an unintentional hybrid?
5. **Task router presence.** Is there a Module E selector, even informally? Or do agents discover the right slice by trial?
6. **`⫼` completeness.** Can a cold LLM bootstrap from on-disk artifacts alone, or does the operator have to brief it?

The diagnostic output is a small structured artifact: function-mix vector, axis-point profile, archetype label(s), `⫼` completeness rating, and a `?`-list of design moves with unmeasured `Δ`.

## Applied to System 3

System 3's *de facto* ontology surface, characterized through the vocabulary:

| Artifact | Substrate | Archetype | Function weight |
|---|---|---|---|
| `▤CLAUDE.md` files | operator-curated prose | 5 (Disambiguation Pin Set) | `⟁ᵈ`, `⟁ᵍ` |
| `▥` Neo4j schema (`packages/graph-schema/`) | typed graph | 2 (Schema-First Typed Graph) | `⟁ˢ`, `⟁ᵛ` |
| `▩` MCP tool surface (`apps/mcp/`) | TS handlers + Zod | 4 (Affordance Catalog) | `⟁ᵃ` |
| `▤` TopoGlyph modules (`.topoglyph/modules/`) | prose with formal vocabulary | 3 (Stratified Comprehension Artifact) | `⟁ˢ`, `⟁ᵍ` |
| `▤` `.meta/onto/` if present | flat-file per 9.1 | 1 (Anchored Skeleton) | `⟁ᵍ`, `⟁ᵛ` |
| `▪` git log + commit messages | history | 3-fragments | `⟁ʳ`, provenance |
| `▦` codebase itself | source | substrate of 1 | grounding substrate |

This is already a **polyontology (archetype 7)** — multiple projections coexist. The vocabulary surfaces the research questions:

1. **Cold-start primer is distributed.** No single `⫼ʳ` for the whole System 3 ontology; instead it's smeared across multiple CLAUDE.md files plus the TopoGlyph modules. A consolidated constitution file would let a cold agent enter through one door.

2. **Canonical-handle registry is implicit.** When two agents say "the Composer," "schema-add," "kernel-react" — are they grounded in the same definition? Currently the operator vouches. A `⫳` registry mapping common LLM confusions to canonical handles would convert this to a `⫼ᵛ` artifact.

3. **Behavioral and intentional views are missing.** The structural view (Neo4j schema, file tree, type graph) is strong. The behavioral view (call graph, runtime topology, dispatch flow) and intentional view (features → user journeys → which code realizes them) get re-derived per session. Either is an archetype-7 projection waiting to be materialized.

4. **Drift detection has no oracle.** 9.1's `⩕` is deterministic; most current "is this still true?" questions require operator judgment. The CLAUDE.md ontologies in particular have no `hash:`/`auth:` — they decay without signal.

5. **The affordance catalog is fragmented.** MCP tools are explicit. But agent-level affordances (which subagent for which task, which slash command, which skill) are scattered across `.claude/agents/`, `.claude/skills/`, and operator memory. A consolidated `⊕ᵃ` registry would reduce wasted exploration.

6. **The router is implicit and operator-shaped.** Currently the operator chooses which ontology slice an agent loads. A Module E router that maps task → slice would let agents self-select.

## Research directions surfaced by the vocabulary

These are open territories the design-space topology makes visible:

1. **Token-cost-aware ontology authoring** — instrument every ontology change with `¢ᶜ` measurement; reject changes where measured `Δ ≤ 0`. (Most working ontologies have never measured this.)

2. **Lazy materialization protocol** — store reconstruction recipes (`⫙`), not facts. The LLM evaluates the recipe when needed. Pairs naturally with 9.1's `auth:`/`hash:` design.

3. **Multi-agent canonical handle protocol** — `⨎` registry with drift detection when two agents diverge on a handle's meaning. The drift signal becomes a *first-class event*, not a silent corruption.

4. **Self-describing schema as constitutional prompt** — `⫼ˢ` becomes part of every agent's system prompt in the project. Cold-start cost drops to zero.

5. **Projections as a query language** — `⟂ᵢ` operators per 12.0 that derive one view from another on demand. The behavioral view becomes a *query over the structural view*, not a separately maintained artifact.

6. **Drift-as-debt instrumentation** — `⌭` count exposed as a metric in the same place build/test failures are visible. Currently invisible debt becomes visible.

7. **Negative-space ontology** — what the system *isn't*, codified. Most LLM errors are claims about things that don't exist; pinning the boundary as explicit `⊘` markers may have higher `$ᵍ` per token than positive claims.

8. **Function-mix tuning** — given a workflow, derive the optimal `⟁` weighting and refactor the on-disk ontology toward it. Currently the mix is a side effect of how artifacts accumulated.

## When to use this module

- Designing or refactoring the on-disk knowledge artifacts that multiple LLM agents read across sessions
- Auditing why an existing ontology isn't earning its keep (most often: unmeasured `¢ᵐ` or missing `⫼`)
- Choosing between candidate ontology designs — gives a shared vocabulary for comparing them by function-mix, axis-point, and archetype
- Bootstrapping a new project's LLM-facing knowledge layer and wanting to avoid the default "accumulate CLAUDE.md until it sprawls" trajectory
- Designing the protocols by which multiple agents share a canonical handle space

## When *not* to use this module

- Single-agent, single-session work — no persistence requirement, no `⫼` needed
- Ontologies whose primary consumer is a human or a query planner — token-cost operators don't apply; standard ontology-design literature serves
- Implementation-level schema design — use 11.0 (graph schema) for the typed-graph layer once archetype 2 has been chosen
- Comparing two finished ontologies — use 12.0; this module is for choosing *which kind* to build, not relating two builds

## Meta-insight

The design space of LLM-facing ontologies has been entered mostly by accident — projects accumulate `▤CLAUDE.md`, `▥schema`, `▩MCP`, `▤design-notes` separately and discover after the fact that they have a polyontology. The deliberate move is to **pick the function-mix first**, then choose archetypes that realize it, then place the archetype on the axes consciously, then size the `⫼` to make the result deployable across sessions.

The most under-weighted function in observed practice is `⟁ᶜ` — coordination. Multi-agent projects need *canonical handles* the way single-author projects need *names*. Without a `⨎` registry, every multi-agent collaboration silently re-invents grounding from scratch.

The most under-measured cost in observed practice is `¢ᵐ`. Ontologies that nobody updates become liabilities faster than they accrue value. An ontology whose `¢ᵐ` is unowned will end up stale, contradictory, and trusted — the worst combination.
