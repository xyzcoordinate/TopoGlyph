---
muid: topoglyph-9-asymmetric-human-ai-collaboration
refs:
  - ~/.topoglyph/modules/topoglyph.md
created_in_session: 0a3d9800-51da-4f4f-a677-f854f9df353c
created_at: 2026-05-17
---

# TopoGlyph 9 — Asymmetric Human–AI Cognitive Partnership

A module for the cognitive topology of *one human and one AI agent collaboratively producing software artifacts*, where the agent runs inside a context-bounded harness (Claude Code or similar) and the human carries durable tacit context that never gets fully serialized. Distinct from TopoGlyph 7.0's collective-intelligence module (which models symmetric peer networks) and from base TopoGlyph's collaboration glyphs `⦿₁⨸⦿₂` / `⨹` (which assume overlapping competence between agents). This module assumes overlapping *ignorance* in different regions of the knowledge space, with crisp context boundaries and asymmetric intent.

## Provenance

This module was developed in a single Claude Code session on 2026-05-17 (session `0a3d9800-51da-4f4f-a677-f854f9df353c`) while the operator (Zach Wild) was scoping a feature for System 3 — a Claude-Code-session viewer — and recognized a deeper structural problem underneath: he had no stable way to provide an agent with the project context needed to plan competently. The verbatim prompts that motivated the module are preserved below; their voice is part of the context.

### Motivating prompt #1 (verbatim)

> **User:** Extend TopoGlyph to model the following problem: I am building this project (System 3) with Claude whom is using the Claude Code harness. I would like to make Claude more effective as a building partner, for example I would like to work with them to extend System 3 to support the following (these are my rough notes describing the feature I have in mind, and my meta commentary that I need an agent to help me create this [which prompted this conversation]):
>
> ```
> i want a way to view all of my claude code sessions, there should be an interface that
> lets me do this better than the conversation hsitory feature which sucks.
> -- need an agent to help me create this feature,
> 	it should know what questions it needs to ask
> 	me in order to extend the system to support this
> ```
>
> There are a few problems and I do not claim to know what each of them are, there are unknown unknowns for certain. One of them is that I do not have a solid system for providing Claude with the appropriate background they need to understand anything about the project - they are forced to read files and just find out on their own. This is messy and leads to mixed results which prevent me from improving these resources for them. There should be a strategic approach for this (I call this the context compilation problem).
>
> Another issue is more of a capability that I would like to provide Claude. Once we provide them with sufficient information about the application, etc. <don't have a stable ontology for all the information we'd compile for them; would like some guarantees here - e.g. we can guarantee that Claude knows about the Neo4j schema, where it is located, and how it relates to the application's services/subsystems> then I would like them to strategically ask me questions (we should work together cooperatively) which refine my rough notes into a more well-engineered proposal. So going from rough working notes -> q/a loop -> simple plan; <IMPORTANT ASPECT THAT IS DIFFICULT FOR ME TO COMMUNICATE WELL: THE PLAN SHOULD BE COMPOSABLE AND PERSISTABLE, SO THAT A PLAN CAN BE ITERATED ON MULTIPLE TIMES IN A SINGLE CONVERSATION OR ACROSS CONVERSATIONS :: for example, the initial simple plan may be do perform another q/a loop, which extends the plan, and so on.>.
>
> Again, extend TopoGlyph to model this problem.

The framing carries three distinctive topological features that base TopoGlyph through 8.0 does not capture cleanly:

1. **Sharp asymmetric knowledge boundary** — the AI lives inside a context window with a crisp edge; at the start of each conversation it knows almost nothing beyond what is read in-session. The human carries durable tacit knowledge (intent, history, judgment) that never gets fully serialized.
2. **Discovery-vs-compilation tension** — context arrives at the AI either opportunistically (read whatever the current task requires) or strategically (a pre-compiled bundle guaranteed to contain specific knowledge). The operator explicitly identifies the absence of the latter as a core problem.
3. **Plan as recursive shared artifact** — the desired output is not a static plan but a *persistent, composable, self-extending* structure where a plan-step can itself be "run another Q/A loop and extend this plan."

## Why base TopoGlyph through 8.0 falls short here

| Existing element | Why it falls short |
|---|---|
| Collaborative module (`⦿₁⨸⦿₂`, `⨹`) | Models symmetric peer scientists, not deeply asymmetric agents with crisp context boundaries |
| External memory (`⬒`) | Captures "stuff exists outside the mind" but not *strategic compilation with guarantees* — bundling context for a specific agent's specific upcoming task |
| Tacit knowledge (`⋄`, `⋇`) | Captures unspoken expertise but not *intent asymmetry* — where one party knows the goal and the other must elicit it |
| Recursive self-improvement (`⨳`, `⟜⥯⟝`) | Models a system improving itself, not two agents iteratively refining a shared external artifact across discontinuous sessions |
| Incubation (`⦅ ⦆`, `⦃ ⦄`) | Captures unconscious cooking-time but not *persistent plan state surviving a hard conversation boundary* |

## The module

### 9.A — Asymmetric Agent Topology

Agents are distinguished by knowledge profile, not by index:

| Symbol | Meaning |
|---|---|
| `⊙H` | Human collaborator: durable tacit context, intent-bearer, judgment locus |
| `⊙A` | AI collaborator: context-bounded, file-omniscient *within* session, pattern-rich, intent-blind |
| `⌗ᶜ` | **Context boundary** of `⊙A`: sharp, conversation-scoped edge separating what is loaded from what is not |
| `▣ᴴ` | Knowledge held by H, absent from A |
| `▣ᴬ` | Knowledge derived by A within session, absent from H's working memory |
| `▣ᴴᴬ` | Shared knowledge (currently inside `⌗ᶜ` and confirmed with H) |
| `▤` | **Latent artifact knowledge**: in the codebase, reachable by A, not yet loaded |
| `⊙H ⟷⌗ ⊙A` | Collaboration channel that crosses the context boundary |

The defining topological fact: `⊙H` and `⊙A` have **complementary deficits**, not similar ones. Standard collaboration glyphs assume overlapping competence; this module assumes overlapping *ignorance* in different regions of the knowledge space.

### 9.B — Strategic Context Compilation

Distinguishes opportunistic discovery from compiled provision:

| Symbol | Meaning |
|---|---|
| `⊞ᶜᵒᵐᵖⁱˡᵉʳ` | Context compiler (a meta-process that selects and packages background) |
| `⦷` | **Context bundle**: curated package destined to cross `⌗ᶜ` into `⊙A` |
| `⦷ ⤓ ⊙A` | Bundle loading operation |
| `⊠ᴳ{X}` | **Knowledge guarantee** that `⊙A` now provably has X (contrast with hopeful `▤ → ▣ᴬ` discovery) |
| `⊡ᴿ` | Resource pointer (bundle includes "where to find X" rather than X itself) |
| `⧖ᶜ` | Context staleness clock (compiled bundles decay as the project evolves) |
| `⌗ᵒⁿᵗᵒ` | **Ontology boundary**: the set of knowledge slot-types the compiler knows how to fill |
| `⊞ˢˡᵒᵗ` | A typed slot in the ontology (the compilable unit) |
| `⨉` | Cross-slot binding (e.g., `⊞ˢ ⨉ ⊞ˢᵉʳᵛ`: this schema entity is owned by this service) |

Canonical project-specific slot types (`⊞ˢ` schema, `⊞ˢᵉʳᵛ` service, `⊞ᴰ` design intent, `⊞ᶜ` conventions, `⊞ᴴⁱˢᵗ` history, `⊞ᵁ` user profile) are *examples* of what `⌗ᵒⁿᵗᵒ` may contain; the ontology is project-extensible.

### 9.C — Q/A Refinement Loop as Cognitive Operator

The question/answer loop is a first-class operator, not just an emergent pattern:

| Symbol | Meaning |
|---|---|
| `▥` | **Specification gap**: a region of ambiguity in the working artifact |
| `⌒ᴳ` | Gap detector (`⊙A`'s mechanism for noticing what is underspecified) |
| `?ᴬ→ᴴ` | A-initiated question (gap-driven query) |
| `!ᴴ→ᴬ` | H's answer (carries intent + judgment across `⌗ᶜ`) |
| `⟳ᵠ` | Q/A cycle operator (question → answer → artifact update) |
| `⫑` | **Specification ratchet** invariant: each `⟳ᵠ` strictly reduces ambiguity; never increases it |
| `⥎` | Provenance link (back-pointer from artifact element to the `!ᴴ→ᴬ` that produced it) |

The crucial constraint is `⫑`. Many real conversations *violate* this — the AI asks a question, the answer reveals a deeper gap, and the artifact ends up *less* coherent than before. A well-designed Q/A loop is one with a ratchet: if a `⟳ᵠ` would reduce coherence, it commits to a new sub-plan-node containing the deeper gap rather than rewriting the prior artifact.

### 9.D — Plan as Persistent Composable Artifact

The plan is not a deliverable; it is a *medium* shared between H and A across time:

| Symbol | Meaning |
|---|---|
| `⊟ᴾ` | Plan node (a step, decision, or subplan) |
| `⟦P⟧ₚ` | **Persistent plan** (survives `⌗ᶜ`; rehydrated next session) |
| `⟨P\|cₙ⟩` | Plan state at conversation n |
| `⟨P\|cₙ⟩ ⟼ ⟨P\|cₙ₊₁⟩` | Cross-session plan evolution |
| `⊕ᴾ` | Plan extension (monotonic addition; old elements stay valid) |
| `⊟ᴾ ⊃ ⟳ᵠ` | **Recursive plan**: a plan step *is* another Q/A loop |
| `⊟ᴾ ⊃ ⟦P'⟧ₚ` | Subplan containment |
| `⥎` | Provenance link from plan element back to originating Q/A (shared with 9.C) |

Composability operators on plans:

| Operator | Meaning |
|---|---|
| `⟦P₁⟧ ⊞ ⟦P₂⟧` | Sequential composition |
| `⟦P₁⟧ ⊗ ⟦P₂⟧` | Cross-product (two planning dimensions interleaved) |
| `⟦P⟧ ⥁ ⟦P⟧'` | Self-modification (plan rewrites itself in light of progress) |

## Topological diagnosis: the *current* state of unaided collaboration

```
⊙H{▣ᴴ Intent, ▣ᴴ Tacit(project architecture)}
   ⌗ᶜ                                          Sharp context boundary
⊙A{∅ → ad-hoc reads}                            A starts empty each session
   ⌒ᴳ unguided                                  No strategic gap detection
⦗▤▤▤▤▤⦘                                        Project knowledge scattered, latent
   ?ᴬ→ᴴ (random) → !ᴴ→ᴬ (variable)              Unstructured Q/A
⦗▣/▥/▥⦘                                        Mixed artifact: some signal, much gap
   no ⥎ provenance                              Can't trace decisions to source
   no ⟦P⟧ₚ persistence                          Plan dies at end of conversation
⟨P|c₁⟩ — ✗ — ⟨P|c₂⟩                            Next session starts from zero
```

Two coupled pathologies:

1. **Context compilation is opportunistic** → `⊙A`'s working knowledge varies wildly per session, so the *outputs* vary wildly, so there is no stable signal to improve *which context* should have been compiled.
2. **Q/A loops lack the ratchet** → without `⫑`, conversations drift; without `⥎`, you cannot tell which questions actually moved the spec forward.

## Topological model of the *desired* state

```
⌗ᵒⁿᵗᵒ {⊞ˢ, ⊞ˢᵉʳᵛ, ⊞ᴰ, ⊞ᶜ, ⊞ᴴⁱˢᵗ, ⊞ᵁ, …}     Stable ontology of slot types
   ⊞ᶜᵒᵐᵖⁱˡᵉʳ                                   Strategic compiler
⦷{filled slots, ⨉ cross-bindings, ⊡ᴿ pointers}  Bundle assembled
   ⦷ ⤓ ⊙A                                      Loaded across boundary
⊠ᴳ{schema, services, conventions, history}      Guarantees: A demonstrably knows X
   ⊙H ⟷⌗ ⊙A                                   Channel opens with informed parity
⊙A.⌒ᴳ informed                                  Gap detection grounded in guaranteed context
   ?ᴬ→ᴴ (strategic, scoped)
!ᴴ→ᴬ (intent + judgment crosses ⌗ᶜ)
   ⟳ᵠ ⫑                                        Ratcheted Q/A cycle
⊟ᴾ₀ ⊕ᴾ ⊟ᴾ₁ ⊕ᴾ ⊟ᴾ₂                              Plan accretes monotonically
   ⥎ each ⊟ᴾ back to !ᴴ→ᴬ                       Full provenance preserved
⟦P|c₁⟧ₚ ⟼ ⟦P|c₂⟧ₚ ⟼ ⟦P|c₃⟧ₚ                  Persistence across sessions
   ⊟ᴾᵢ ⊃ ⟳ᵠⱼ                                  Plan steps can BE Q/A loops
⟦Pfinal⟧ → ⧋ → ⦗■Feature⦘                      Translation to implementation
```

## The central novel pattern: recursive planning via embedded Q/A

The structure the operator explicitly flagged as "IMPORTANT ASPECT THAT IS DIFFICULT FOR ME TO COMMUNICATE WELL":

```
⟦P|c₀⟧ₚ:  ⊟ᴾ₀ "Build <feature>"
              ⊃ ⟳ᵠ₁ {scope, success criteria, data sources}
                ⊕ᴾ ↓
⟦P|c₁⟧ₚ:  ⊟ᴾ₀ ⊞ ⊟ᴾ₁ "N subsystems identified"
                       ⊃ {⟳ᵠ₂ₐ, ⟳ᵠ₂ᵦ, ⟳ᵠ₂ᵧ}
                ⊕ᴾ ↓
⟦P|c₂⟧ₚ:  ⊟ᴾ₀ ⊞ ⊟ᴾ₁ ⊞ ⊟ᴾ₂ "per-subsystem designs"
                              ⊃ ⟳ᵠ₃ (cross-cutting concerns)
                ⊕ᴾ ↓
⟦P|c₃⟧ₚ:  …  ⊞ ⊟ᴾ₃ "implementation-ready"
                     ⧋ → ⦗■Feature⦘
```

Each `⊟ᴾ` is annotated with `⥎` links back to the `!ᴴ→ᴬ` answers that produced it. Each `⟳ᵠ` is bounded — it terminates when its local `▥` set is empty, then commits `⊕ᴾ` to `⟦P⟧ₚ`. The plan is **fractal**: a step is either an executable instruction *or* a deferred Q/A loop that will further refine itself.

## Meta-insight: why the two stated problems are one problem

The motivating prompt described two problems — "context compilation" and "Q/A refinement loops" — as if separate. This module reveals they are coupled through `⌗ᵒⁿᵗᵒ`:

- Without a stable ontology, `⊞ᶜᵒᵐᵖⁱˡᵉʳ` has no schema to compile against → bundles are ad-hoc → `⊙A`'s knowledge state varies → `⌒ᴳ` cannot reliably detect gaps → `⟳ᵠ` loses its `⫑` ratchet → plans drift → no signal to improve the ontology. A vicious cycle.
- *With* a stable ontology, both problems become tractable simultaneously, and a virtuous cycle forms: each completed plan reveals which ontology slots were under-specified, refining `⌗ᵒⁿᵗᵒ` itself.

The leverage point is therefore `⌗ᵒⁿᵗᵒ`. The first concrete artifact a project should build is not the target feature, and not the agent — it is the *initial ontology specification*: an enumeration of `⊞` slots, the authoritative source for each, the cross-bindings (`⨉`) between them, and a minimum-viable compiler that produces a `⦷` bundle with explicit `⊠ᴳ` guarantees.

## What capabilities the agent the operator wants must have

The agent described in the motivating prompt — "an agent that knows what questions it needs to ask" — is, stated in this module, a **specification ratchet over a fractal persistent plan, grounded in a compiled context bundle drawn from a stable project ontology**. Five capabilities follow:

1. **Ontology curator** (`⌗ᵒⁿᵗᵒ`): defines which knowledge slots exist for the project.
2. **Context compiler** (`⊞ᶜᵒᵐᵖⁱˡᵉʳ → ⦷ → ⊠ᴳ`): fills the slots from authoritative sources and produces a verifiable bundle.
3. **Gap detector** (`⌒ᴳ`): given the current `⟦P|cₙ⟧ₚ` and the guaranteed context, identifies `▥` regions and selects which to surface to H.
4. **Ratcheted Q/A driver** (`⟳ᵠ` with `⫑` invariant): runs the loop with the constraint that each cycle commits to `⟦P⟧ₚ` with `⥎` provenance.
5. **Plan persistence layer** (`⟦P⟧ₚ`, `⟨P|cₙ⟩ ⟼ ⟨P|cₙ₊₁⟩`): serializes plan state across `⌗ᶜ`. Crucially, must support `⊟ᴾ ⊃ ⟳ᵠ` — plan nodes that *are* deferred Q/A loops.

### Motivating prompt #2 (verbatim)

After the module above was introduced, the operator asked:

> **User:** Sketch the initial ⌗ᵒⁿᵗᵒ

This prompted the first concrete instantiation of the module — an enumeration of `⊞` slots for System 3, mapped against authoritative sources in that codebase. The instantiation revealed that ~80% of the ontology was already present in the project as scattered artifacts (a schema-mirror directory, a vocabulary file, an architecture doc) but had never been *named as a slot set with guarantees*. Naming it as such surfaced one specific load-bearing gap (`⊞ᴱˣᵗ` — the external integration format) that was blocking the original feature work.

The general pattern of an `⌗ᵒⁿᵗᵒ` sketch:

```
For each candidate slot ⊞:
  • name and shape (what is in it)
  • auth source (canonical file path or query)
  • cross-bindings ⨉ (which other slots it co-references)
  • staleness vector ⧖ᶜ (what event invalidates it)
  • guaranteeability ⊠ᴳ (can the compiler currently promise content?)
```

Slots that *cannot* yet carry a `⊠ᴳ` are the most informative output of the sketch — they are the gaps the next Q/A loops should fill.

## Sibling module

A direct follow-up in the same session addressed *how* to actually build and maintain `⌗ᵒⁿᵗᵒ` operationally — file formats, drift detection, multi-session extensibility. See sibling module **[topoglyph-9-1-onto-operational-form](topoglyph-9-1-onto-operational-form.md)** which defines `⊞·` (slot file), `⩕` (freshness check), `⊞ʳ` (rules file), and the five invariants I₁–I₅ that make `⌗ᵒⁿᵗᵒ` maintainable as a flat directory of single-file slots.

## When to reach for this module

- You are designing a long-running human-AI collaboration where the AI runs inside a context-bounded harness (Claude Code, an IDE assistant, an agent with a session window).
- You notice that the AI's outputs vary wildly between sessions in ways that trace to *which files it happened to read first*, not to the task itself.
- You want to plan a feature whose scope is itself uncertain and will be refined through dialogue — and you want that dialogue's output to survive the conversation.
- You are designing an agent whose job is "ask the right questions" and need a structural specification of what "the right questions" actually means (the answer: questions targeting `▥` regions detectable by `⌒ᴳ` against a known `⌗ᵒⁿᵗᵒ`).

## When *not* to reach for this module

- Symmetric collaboration between peers (use base TopoGlyph 7.0's collective-intelligence module).
- Solo cognition (use TopoGlyph 4.0–6.0's individual-cognition modules).
- One-shot interactions where no plan persists between sessions (the persistence machinery is overhead without payoff).
