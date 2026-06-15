# TopoGlyph 9.0: Application Design Comprehension

A TopoGlyph extension for the cognitive task of reverse-building a mental model of an existing software system from artifacts of unequal authority and unequal staleness.

Versions 1.0–8.0 model cognition itself (insight, breakthrough, knowledge ecosystems). 9.0 targets a specific cognitive task that prior modules don't capture cleanly: **reading a built codebase to understand what it is, what it isn't, what's intentional, and what's silt.**

## Why a new module is warranted

Application-design comprehension has properties no prior TopoGlyph module captures:

1. **Substrate heterogeneity** — Markdown design notes, schemas, route handlers, commit messages, and Docker files all encode the system but ask different questions of the reader.
2. **Authority asymmetry** — sources contradict; one wins. Comprehension means knowing the precedence rule, not just reading.
3. **Stratigraphy** — code accumulates in layers; surface reading often gives the wrong layer.
4. **Intent vs. emergence** — some structure is designed, some is silt; treating silt as design (or vice versa) is the most common comprehension error.
5. **Half-wired features** — legacy code can have dead plumbing that looks live.
6. **Boundaries are typed** — function call, HTTP request, MCP invocation, tenancy filter, and decryption all *look* like "→" in a diagram but behave differently under failure.

## Modules

### 1. Artifact / Substrate

Every glyph in a comprehension walk should be substrate-tagged, because authority and staleness rules differ per substrate.

- `▦` Code artifact (file, module, function)
- `▤` Documentation artifact (design note, README, comment)
- `▥` Schema artifact (Mongo collection, TS type, migration)
- `▧` Runtime artifact (container, port, process, queue)
- `▩` Protocol artifact (REST endpoint, WS event, MCP tool, function signature)
- `▪` Decision artifact (commit msg, issue, ADR, PR description)

Convention: substrate prefix binds tighter than content. `▤ARCHITECTURE.md` reads as "the documentation artifact named ARCHITECTURE.md" — distinct from `▦architecture.ts`.

### 2. Authority / Veracity

- `⫶` Authoritative source (declared source of truth in this domain)
- `⫵` Derivative / index (legitimate, but defers to ⫶)
- `⫷` Known stale (was true, no longer)
- `⫸` Suspected stale (needs verification)
- `⊢` "Source X *claims* Y"
- `⊨` "Y *verified* against current state"
- `⩙` Authority precedence operator: `A ⩙ B` reads "A wins over B in conflict"

### 3. Typed Boundaries

- `║` Module / import boundary (in-process, cheap)
- `▒▒▒` Process boundary (separate deployable, IPC required)
- `≣` Network boundary (REST/WS, can fail/timeout)
- `≢` Encryption boundary (plaintext one side, ciphertext other)
- `≟` Tenancy boundary (workspaceId / orgId scoping)
- `≠` Trust boundary (user input → validated server input)
- `⊟` Protocol boundary (e.g., MCP — typed tool calls across process)

Reading rule: a comprehension walk that crosses a `≣` without naming the timeout, retry, and failure mode is **incomplete**.

### 4. Intent / Emergence

- `◈` Designed (intentional, has rationale in `▤` or `▪`)
- `◇` Emergent (arose by accretion, no `▤` justifies it)
- `◆` Accidental coupling (not intended, but load-bearing)
- `⟦◈⟧` Declared invariant
- `⟦◇⟧` De facto invariant (relied on but never declared — fragile)
- `⟦◆⟧` Hidden coupling (will surprise the next refactor)

### 5. Stratigraphy

- `▔` Current layer (recently touched, actively maintained)
- `▁` Legacy stable (works, untouched, expected to keep working)
- `▂` Deprecated (works, slated for removal)
- `▃` Dead (no live callers — verify before declaring)
- `⩥ⁿ` Generation tag (n=1 original, n=2 first refactor, …)
- `⥯ᴹ` Migration arrow, labelled M (transforms generation n → n+1)
- `⥍` Naming-skin lag (concept renamed; code still uses old name)

### 6. Comprehension Operators

The verbs of building a mental model:

- `?` Open question
- `?!` Hypothesis (suspected, not yet verified)
- `!` Confirmed (resolved against an authoritative source)
- `⤳` Trace (follow a call, request, or data path)
- `⤴` Lift (zoom out to higher abstraction)
- `⤵` Drill (zoom into a specific implementation)
- `⊜` Reconcile (compare two sources, resolve discrepancy)
- `◬` Triangulate (≥3 independent sources agree)
- `⊘` Discard (this artifact is misleading, ignore)

## The Process: Comprehension Walk

A *process* in 9.0 is an ordered sequence of these operators on substrate-tagged nodes. Eight steps; not every walk visits all of them in order, but each step is a primitive move.

### Step 1 — Locate authority

```
?⦗■System⦘ ⤳ ⫵▤README ⤳ ⫵▤ARCHITECTURE ⤳ ⫶▤DESIGN-*
        precedence: ⫶ ⩙ ⫵
```

Output: a precedence rule for which sources win on conflict.

### Step 2 — Build the intent map

```
⫵▤ARCHITECTURE ⤴ ◈⦗Subsystems⦘
```

Output: the **designed** decomposition. Anything in code that doesn't fit a slot is `◇` / `◆` / `?`.

### Step 3 — Type every boundary

```
▒▒▒(▦A) ≣ ▒▒▒(▦B) ⊟ ▒▒▒(▧C)
       ≟ tenancyId    ≢ at <single-decryption-fn>
```

Output: every arrow in your future diagram has a known failure mode.

### Step 4 — Trace the spine

Pick one end-to-end happy path and `⤳` it through every layer. Every other flow is later describable as a *delta* from the spine.

### Step 5 — Stratigraphy pass

Walk the repo with the temporal-layer lens (`▔ ▁ ▂ ▃ ⥍ ⩥ⁿ ⥯ᴹ`). Identify renames-not-fully-propagated, abandoned scaffolding, and migration history.

### Step 6 — Reconcile claims with source

For every load-bearing claim in `▤`, run `⊢ → ⊜ → {⊨ | ⥍ | ⊘}`. Each doc statement ends in one of three states: verified, skin-only, or discard.

### Step 7 — Surface invariants

Promote things to `⟦◈⟧` (declared) or `⟦◇⟧` (de facto). The `⟦◈⟧` set is what a refactor must preserve. The `⟦◇⟧` set is the fragile-trust list — load-bearing facts not codified anywhere.

### Step 8 — Park unresolved questions

`?`-items at end-of-walk are not failures; they are the productive entry points for the next session.

## Walk shape, condensed

```
?⦗System⦘
   ⤳ ⫵▤index ⤳ ⫶▤authoritative_set      [1]
   ⤴ ◈⦗Subsystems⦘                         [2]
   ⤵ typed boundaries (║ ▒▒▒ ≣ ⊟ ≟ ≢ ≠)   [3]
   ⤳ spine (one end-to-end happy path)    [4]
   ⊞ stratigraphy (▔ ▁ ▂ ▃ ⥍ ⩥ⁿ ⥯ᴹ)        [5]
   ⊜ reconcile (⊢ → ⊨ | ⥍ | ⊘)             [6]
   ⟦◈⟧ ⟦◇⟧ invariant set                   [7]
   ?-list = comprehension debt              [8]
```

## Output contract

A single comprehension walk yields a small, structured artifact:

1. **Authority precedence rule** — `⫶ ⩙ ⫵ ⩙ ⫷` for this codebase.
2. **Subsystem map** (`◈⦗…⦘`).
3. **Typed boundary set** (every cross-component edge labelled).
4. **Spine trace** (one concrete happy path, end-to-end).
5. **Stratigraphy notes** (`⥍` skin-lag list, `⩥ⁿ ⥯ᴹ` migration generations).
6. **Reconciled claim ledger** (`⊨` / `⥍` / `⊘`).
7. **Invariant set** (`⟦◈⟧` declared, `⟦◇⟧` de facto).
8. **Question list** (`?` comprehension debt).

This artifact is small enough to hand to the next person (or your future self) without re-reading the repo.

## When this module earns its keep

Modules in 9.0 earn their keep most clearly in codebases with:

1. **Declared authority precedence** in writing (e.g., a README that says "DESIGN-* wins over ARCHITECTURE.md on conflict") — so `⫶ ⩙ ⫵` has real teeth.
2. **Recorded schema history** (boot migrations, ADRs) — so stratigraphy reads cleanly off the repo rather than having to be reconstructed from `git log`.
3. **Recent renames not fully propagated** — so `⥍` skin-lag is needed on day one.

A different codebase would stress different modules; the point isn't that every walk visits all eight steps — it's that the substrate-tagging, boundary-typing, authority-precedence, and stratigraphy primitives are now *available* as a vocabulary, and a process can be assembled from them per codebase.
