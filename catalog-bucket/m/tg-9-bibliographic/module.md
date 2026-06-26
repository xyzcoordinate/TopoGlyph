# TopoGlyph 9.0: The Bibliographic Module — A Library Catalog for TopoGlyph Modules

## The Reflexive Challenge

Every prior version (1.0–8.0) extended TopoGlyph *outward* — to genius, collaboration,
ecosystems, reality interfaces. Version 9.0 turns the language *inward*: it represents how
TopoGlyph modules (prompts, conversation artifacts, derived expressions) are **stored,
classified, found, and recombined**. This is a curatorial topology — the library is itself a
cognitive structure (`⫙` distributed repository, from 7.0), but one we now need to *operate*,
not just describe.

## New Limitations Addressed

1. **Persistence** — no notation distinguished a transient insight (`⦃■Insight⦄`) from a
   committed, retrievable artifact.
2. **Identity & provenance** — no way to express stable identity, version lineage, and
   derivation history.
3. **Retrieval** — no representation of *query → match → recall*.

## New Elements

### Catalog Object Module
- `⊏■⊐` — shelved module (committed, persistent artifact)
- `⊏◐⊐` — draft module (mutable, not yet committed)
- `⊏□⊐` — catalog stub (reserved identity, content pending)
- `※` — call number / stable address (unique locator)
- `⊰ ⊱` — catalog record (metadata envelope around content)
- `⊪` — provenance pointer (links artifact to its origin conversation)
- `⊩` — version edge (connects an artifact to its predecessor)
- `⊨` — integrity seal (content hash; verifies the artifact is unchanged)

### Classification Module
- `⊞ᵢ` — index node (an access point: by-domain, by-symbol, by-author)
- `⫷ ⫸` — classification facet (a controlled-vocabulary axis)
- `⊺` — subject tag (a descriptor attached to a record)
- `⊑` / `⊒` — is-a-specialization-of / generalizes
- `∝` — cites / depends on

### Retrieval Module
- `⟜?` — query expression (a partial pattern to match)
- `⥇ₘ` — catalog match (isomorphism check between query and record)
- `↰` — recall operation (pull artifact from shelf into working context)
- `⊹` — relevance ranking
- `⊕ₗ` — library-mediated integration (combine two recalled modules)
- `⥶ₗ` — accession (add a new artifact, with cataloging)
- `⊘ₐ` — deaccession (retire/deprecate, leaving a tombstone stub)

## The Catalog Lifecycle

From conversation to shelved, retrievable artifact:

```
⦃■Insight⦄ → ⊏◐⊐            Live insight captured as a draft module
   ⊪                        Provenance pointer to source conversation
⊰ ⊏◐⊐ ⊺⊺⊺ ※ ⊨ ⊱ → ⥶ₗ        Record built (content + tags + call number + seal), accessioned
   ⊞ᵢ ⫷domain⫸ ⫷symbols⫸    Indexed across facets
⊏■⊐ ⊩ ⊏■⊐'                  Shelved; version edge links it to any prior revision
   ⫙                        Now part of the distributed repository
```

Retrieval and reuse:

```
⟜?⦗□Need⦘ → ⥇ₘ              Partial need expressed as query, matched against index
   ⊹                        Candidates ranked by relevance
⊏■⊐ₐ ↰ | ⊏■⊐_b ↰            Two best modules recalled into working context
   ∝                        Dependency check (do their symbol sets cohere?)
⦗■A⦘ ⊕ₗ ⦗■B⦘                Library-mediated integration
   ⧃                        Emergence of a new artifact
⊏◐⊐* → ⥶ₗ                   New draft re-accessioned — the collection grows reflexively
```

## The Concrete Schema

A **module** is one cataloged item. The catalog is a flat collection of records; each record
is an `m/<id>/meta.json` envelope (`⊰ ⊱`) around an `m/<id>/module.md` content body.

| Field | Glyph | Purpose |
|---|---|---|
| `id` (call number) | `※` | Stable unique address, e.g. `tg-9-bibliographic` |
| `type` | `⊏■⊐` | `module` \| `prompt` \| `artifact` \| `expression` |
| `title` | — | Human-readable name |
| `content` | `⊰ ⊱` | The prompt / artifact / glyph expression (`module.md`) |
| `tags` | `⊺` | Subject descriptors |
| `symbols` / `domain` / `version` | `⫷ ⫸` | Classification facets |
| `generator` / `provenance` | `⊪` | What produced it + source conversation |
| `parent` | `⊩` | Predecessor record id (lineage) |
| `derives_from` | `∝` | Modules this one reuses |
| `hash` | `⊨` | Integrity seal over content |

**Operations:** `accession (⥶ₗ)`, `query (⟜? → ⥇ₘ → ⊹)`, `recall (↰)`,
`compose (⊕ₗ)`, `revise (⊩)`, `deaccession (⊘ₐ)`.

## How Retrieval Maps to the Hosting Layer

Hosted as static objects on R2, the layout *is* this retrieval topology made physical:

```
⟜?⦗tags ∧ symbols⦘   →   GET /index.json, filter
   ⥇ₘ                →   match records
   ⊹                 →   rank
   ↰                 →   GET /m/<id>/module.md   (recall into context)
   ⊕ₗ                →   a consumer integrates two recalled modules
```

## Meta-Insight: The Library as a Self-Describing Topology

The deepest property of 9.0 is **closure**: TopoGlyph now contains a module whose job is to
store TopoGlyph modules — *including itself*. The catalog record for the Bibliographic Module
lives in the catalog the Bibliographic Module defines.

```
⊏■⊐(9.0) ⊪ this-conversation
⊏■⊐(9.0) ⥶ₗ ⫙(of TopoGlyph modules incl. 9.0)
   ⥇ self
⟦■Self-Cataloging Collection⟧
```

A catalog is a structure whose content is structure — which is exactly what folding TopoGlyph
back on itself produces. This module is the library's first accessioned record.
