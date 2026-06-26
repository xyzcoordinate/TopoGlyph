# TopoGlyph 10.0: Health Informatics Substrate Module

A domain extension for representing the *data and information topology* of
health informatics systems — the layered standards (FHIR, OMOP, openEHR,
Phenopackets), terminology graphs (SNOMED/RxNorm/LOINC/HPO via UMLS), evidence
representations (EBMonFHIR, GRADE, EvidenceMap), knowledge graphs (DrugBank,
PrimeKG, Hetionet), CDS plumbing (CDS Hooks, CQL), and the integration
patterns (ETL, federated query, agentic Graph RAG) that bind them.

This module is a *substrate*. TopoGlyph 9.0 (Personalized Supplement Decision)
sits *on top of it* — every `☥`, `⟦Supplement⟧`, `⟜─⟝`, and `⥯` from 9.0
ultimately resolves to typed entities and flows expressible in the notation
introduced here.

## Why This Module Is Needed

The 9-layer health informatics stack (sources → patient state → terminology →
evidence → CDSS → drug interactions → personalization → decision → AI
augmentation → specialty domains) has structural features that prior TopoGlyph
versions cannot cleanly express:

1. **Layered stratification with typed adapters.** Information ascends and
   descends through layers, transformed at each boundary. TopoGlyph's `⦚`
   (paradigm boundary) is too coarse — these boundaries are *typed
   conversions* (FHIR↔OMOP, openEHR→FHIR), some lossless, some lossy.
2. **Concept identity across coordinate systems.** A single clinical concept
   (e.g., "Type 2 Diabetes") exists in SNOMED, ICD-10, ICD-11, and is
   referenced from every other layer. This is a *manifold-in-charts* relation
   that needs explicit notation.
3. **Provenance is structural, not optional.** Evidence claims must carry
   citation chains. Without provenance, a recommendation is unauditable.
4. **Seams are the high-leverage points.** The survey identified that "the
   most leverage lies in the seams between layers." Seams need to be
   first-class objects with attributes (lossy / lossless / computed).
5. **Uncertainty must propagate or be flagged when it doesn't.** A transform
   that drops uncertainty is a structural defect — currently invisible.
6. **Output shape is constrained.** The trichotomy frame is a render-time
   invariant, not just a presentation choice.

## New Elements

### 1. Layer Stratification

Health informatics is fundamentally layered. The notation makes layers visible:

- `≣` — Layer / stratum boundary (horizontal divider)
- `↟` — Information ascent (data flowing up the stack toward decision)
- `↡` — Constraint descent (rules/policies flowing down toward sources)
- `⫪` — Layer adapter (a typed conversion at a layer boundary)

### 2. Representation & Format

Multiple syntactic representations of the same underlying data:

- `〈Format〉` — Representation namespace, e.g., `〈FHIR〉`, `〈OMOP〉`, `〈Phenopacket〉`, `〈openEHR〉`, `〈CQL〉`
- `⤲` — Bidirectional transform (round-trip-able)
- `⤳` — One-way transform (information cannot be reconstructed)
- `⥬` — Lossless transform (full information preserved)
- `⥭` — Lossy transform (information dropped or coerced)
- `⊜⤳` — Identity-preserving transform (same concept, different syntax)

### 3. Terminology Binding

Concepts live in vocabularies; vocabularies are aligned but not identical:

- `▤Vocab` — Vocabulary namespace, e.g., `▤SNOMED`, `▤RxNorm`, `▤LOINC`, `▤HPO`, `▤MONDO`, `▤ICD11`
- `⊪` — Terminology binding (a value carries its vocabulary anchor)
- `⊫` — Multi-vocabulary alignment edge (UMLS-style equivalence)
- `≜` — Concept identity across vocabularies (asserted same concept)
- `≇` — Near-but-not-equivalent across vocabularies (alignment caveat)

### 4. Provenance & Evidence Chain

Every claim points back to a source. Without this, the system is unauditable:

- `←❡` — Provenance edge (points to source artifact)
- `※[id]` — Citation anchor (PMID, DOI, guideline ID)
- `⟪claim⟫` — Computable claim (typed, structured)
- `⟪claim⟫ᴳ` — GRADE-graded claim; subscript indicates quality (e.g., `ᴳ:high`)
- `⟪claim⟫ᴾ` — Population-scoped claim; superscript indicates population
- `⟪claim⟫ᴳᴾ` — Combined: graded *and* population-scoped

### 5. Knowledge Structures

Knowledge graphs are increasingly the substrate for retrieval and reasoning:

- `⩘` — Knowledge graph node
- `⩙` — Typed knowledge graph edge
- `⩚` — Subgraph extraction (a RAG retrieval window)
- `⩘[KG-name]` — Named KG (e.g., `⩘[PrimeKG]`, `⩘[Hetionet]`, `⩘[DrugBank]`)

### 6. Temporal Substrate

Health data is intrinsically longitudinal:

- `⫞` — Versioned axis (the time dimension itself)
- `⌗ᵗ` — Snapshot of an entity at time t
- `⥻ᵗ` — Time-evolving entity (state changes along ⫞)
- `⌬⥯` — Re-evaluation trigger (event-driven, not periodic)
- `Σᵗ` — Cumulative/integrated quantity over time (relevant for exposure)

### 7. Integration Patterns

Recurring topologies of how layers compose:

- `⤇` — ETL pipeline (extract / transform / load)
- `⫼` — Federated query (single query, multiple stores)
- `⩘ ⊕ LLM` — KG-augmented LLM
- `⟲ᵃ` — Agentic loop (retrieve → evaluate → refine → emit)
- `⫦` — Stream / continuous ingestion (wearables, CGMs)

### 8. Seam Markers (the high-leverage points)

A seam is where two layers meet. Seams have *quality*:

- `⩏` — **Clean seam**: information flows with full fidelity
- `⩎` — **Lossy seam**: information bleeds; the gap is a known hazard
- `⩐` — **Computed seam**: a function bridges the two sides (the bridge itself is a place to inspect)
- `⩑` — **Unbuilt seam**: known integration that does not yet exist

The seam markers are essentially *risk markers* for the architect. Every `⩎`
or `⩑` in a topology is a place to either invest engineering effort or accept
explicit limitations.

### 9. Uncertainty Flow

Uncertainty is a value, and it propagates (or fails to):

- `δ` — Uncertainty quantum attached to a value (variance, posterior width, GRADE level)
- `⤳δ` — Uncertainty-preserving transform
- `⤳!δ` — **Uncertainty-discarding transform** (a structural defect — flag it)
- `⊕δ` — Uncertainty composition (combining two uncertain inputs)
- `δ→0` — Spuriously confident output (uncertainty was lost upstream)

### 10. Output Shape Constraints

The render boundary is where structured data becomes human-facing:

- `⊙` — Render boundary (computable model → user-facing artifact)
- `⫹⫺⫻` — Trichotomy frame (benefits | risks | unknowns rendered as three buckets)
- `⫶` — Comparison frame (multiple alternatives rendered for choice)
- `⊙[card]`, `⊙[diagram]`, `⊙[summary]` — Render targets

## The Integrated 9-Layer Topology

```
≣─ L9: Specialty Domains (Nutrition / Supplements) ────────────────≣
   ⩘[NatMed]   ⩘[DSLD]   ⩘[USDA-FDC]   ⩘[FoodOn]   ⩘[Examine]
       ⩎ (no clean ⊪RxNorm binding for botanicals)         ← LOSSY SEAM
       ⩑ (FoodOn ↔ FHIR Medication binding unbuilt)        ← UNBUILT SEAM

≣─ L5: Drug Interactions ──────────────────────────────────────────≣
   ⩘[DrugBank-6.0] ⊫ ⩘[DDInter] ⊫ ⩘[FAERS]+▤MedDRA
       ⊪▤RxNorm  ⊪▤UNII  ⊪▤ChEBI                         ← BINDING
       ⩐ Knowledge-graph DDI prediction (KnowDDI/PrimeKG)  ← COMPUTED SEAM
       ⩎ (severity grading noisy; supplements sparse)      ← LOSSY SEAM

≣─ L6: Personalization ────────────────────────────────────────────≣
   ⟪CPIC⟫ᴳ:high ←❡ ⩘[PharmGKB] ←❡ ⩘[ClinGen]
       ↡ produces CDS-ready content
   ⟪PRS⟫ᴳ:varies  — ◍ ancestry-fragile (91% European GWAS)  ← APPLICABILITY ⩎
   ⟪ITE⟫           — research-grade only                     ← UNBUILT ⩑

≣─ L4: CDSS Plumbing ──────────────────────────────────────────────≣
   〈CDS Hooks〉 ⊕ 〈SMART on FHIR〉 ⊕ 〈CQL v1.5.3〉
       ↡ consumes Layer 3 + Layer 2
       ⩎ no uncertainty field on cards                      ← LOSSY SEAM

≣─ L3: Evidence ───────────────────────────────────────────────────≣
   〈EBMonFHIR〉:
     ⟪Evidence⟫ᴳ  ⟪EvidenceVariable⟫  ⟪Citation⟫  ⟪ArtifactAssessment⟫ᴳ
   〈EvidenceMap〉:  entity / proposition / map (extends PICO)
   ⟪claim⟫ᴳᴾ ←❡ ※[Cochrane / PubMed / UpToDate]
       ⩎ PDFs → computable evidence (mostly aspirational)   ← LOSSY SEAM
       ⩑ population → individual-applicability function     ← UNBUILT SEAM ★

≣─ L2: Terminologies ──────────────────────────────────────────────≣
   ▤SNOMED ⊫ ▤ICD11 ⊫ ▤ICD10 ⊫ ▤RxNorm ⊫ ▤LOINC
   ▤HPO ⊫ ▤MONDO ⊫ ▤MedDRA ⊫ ▤ATC ⊫ ▤UNII ⊫ ▤ChEBI ⊫ ▤GO
                           ⊕
                       ▤UMLS  (the alignment graph)
       ⩎ ▤ICD11 ⊫ ▤SNOMED still incomplete                  ← LOSSY SEAM
       ↟ every higher layer binds via these                ← BINDING SUBSTRATE

≣─ L1: Patient State ──────────────────────────────────────────────≣
   〈FHIR〉 (R4/R5/R6) ⤲ 〈openEHR〉 ⤲ 〈Phenopacket-v2〉
       ⤇ ETL → 〈OMOP-CDM〉   (analytics back-plane)
       ⫞ longitudinal axis  ⌗ᵗ snapshots  ⥻ᵗ evolving state
       ⩏ FHIR↔OMOP via TermX                               ← CLEAN SEAM (recent)
       ⩎ openEHR → OMOP loses semantic depth               ← LOSSY SEAM

≣─ L0: Sources ────────────────────────────────────────────────────≣
   ⩘[EHR]   ⩘[Labs]   ⩘[Wearables]⫦   ⩘[CGM]⫦   ⩘[Literature]
       ⤇ ingestion  ⫦ continuous streams
```

The diagram is *both* a layer map and an annotated risk map: every `⩎` is a
known information loss, every `⩑` is a known missing integration, every `⩏`
is a recently completed clean integration. The single `★` marks the seam the
field considers most consequential.

## The Cross-Cutting Recommendation Flow

The path data takes from raw source to a trichotomy card on a user's screen:

```
⩘[L0 sources] ⤇ ⥻ᵗ⟨FHIR⟩ ↟
                        ⊪▤terminology ↟
                                     ⟪Evidence⟫ᴳᴾ + ⟪CPIC⟫ᴳ ↟
                                                          ⩐ Applicability(⟪claim⟫ᴾ, ☥) ↟
                                                                                     ⊕δ uncertainty composition ↟
                                                                                                                ⩐ MCDA scoring + ⏃ goals ↟
                                                                                                                                       ⫹⫺⫻ trichotomy frame
                                                                                                                                            ⊙[card] → user
                                                                                                                                                ⌬⥯ re-evaluation triggers
```

Two `⩐` (computed seams) carry almost all the conceptual difficulty:

1. **`Applicability(⟪claim⟫ᴾ, ☥)`** — turns a population-scoped, GRADE-graded
   evidence claim into a confidence that it applies to *this* `☥`. This is
   the population→individual bridge the field has not built.
2. **`MCDA scoring + ⏃ goals`** — combines benefit/risk/unknown across
   multiple competing interventions, weighted by the person's goals and
   costs. Standards exist (ISPOR MCDA), but uncertainty propagation through
   them is not solved.

Both must preserve `δ` end-to-end. A `⤳!δ` anywhere in this chain produces a
recommendation that is *spuriously confident* — `δ→0` at the render boundary.

## Sub-Topology: Agentic Graph RAG (the 2025 dominant AI pattern)

```
⟨user query⟩
    ↓
⟲ᵃ {
    query  ⤳  ⩚ subgraph extraction from ⩘[KG]   (PrimeKG / Hetionet / PubMed-MKG)
           ⤳  ⟪Evidence⟫ᴳ retrieval from 〈EBMonFHIR〉
           ⤳  evaluate(retrieved, query) → δ confidence
           ⤳  if δ > threshold:  refine subgraph, recurse
           ⤳  else:               compose answer
}
    ↓
⊕δ uncertainty composition
    ↓
⊙[answer + provenance ←❡ ※[sources]]
```

The agentic loop is structurally an `⟲ᵃ` over a `⩚`–`⟪⟫`–`δ` triple. Two
failure modes are visible in the topology:

- The KG `⩘` itself can be stale or biased — RAG cannot rescue a flawed
  substrate.
- The `evaluate` step is itself a model; adversarial prompting can produce
  `δ→0` outputs even when retrieval is sparse (the 2025 finding).

## The Seam Catalog (where to invest)

Every `⩎` and `⩑` in the 9-layer topology is a defined engineering target.
Listed in priority order from the survey's gap analysis:

| Seam | Layers | Type | Status |
|------|--------|------|--------|
| Population → individual applicability | L3 → L6 | `⩑` | **Unbuilt — top priority** |
| Uncertainty propagation across layers | All | `⩎` everywhere | No standard supports |
| Studied-null vs genuine-unknown distinction | L3 → L7 | `⩑` | Needs `⊜` vs `⊟` first-class |
| Supplement → RxNorm/DrugBank binding | L9 → L5 | `⩎` | Botanicals especially |
| FoodOn ↔ FHIR Medication | L9 → L1 | `⩑` | Unbuilt |
| EHR PDF → computable evidence | L0 → L3 | `⩎` | LLM-driven, immature |
| ICD-11 ↔ SNOMED full mapping | L2 internal | `⩎` | WHO–SNOMED ongoing |
| openEHR → OMOP semantic preservation | L1 internal | `⩎` | Lossy by design |
| PRS ancestry portability | L6 internal | `⩎` | Methodological |
| N-of-1 / ITE data model | L1 + L6 | `⩑` | Phenopacket+OMOP+posterior unstandardized |

A framework's value is largely measured by *which seams it closes* and
*which it honestly flags as open*.

## Composition with TopoGlyph 9.0 (Supplements)

The supplement decision module's primitives all resolve to substrate
constructs:

```
TopoGlyph 9.0 element        →  TopoGlyph 10.0 substrate

☥{profile}                   →  ⥻ᵗ〈Phenopacket〉 over ⫞ + ⊪▤SNOMED + ⊪▤RxNorm
                                 + ⊵ values from ⩘[L0-labs] / ⩘[L0-wearables]⫦

⟦Supplement⟧                 →  ⩘[DrugBank] ⊕ ⩘[NatMed] ⊕ ⩘[USDA-FDC]
                                 ⊪▤RxNorm/▤UNII  (where binding exists; ⩎ for botanicals)

●●● / ●●○ / ●○○ / ◐ / ◍      →  ⟪claim⟫ᴳ:high / ᴳ:mod / ᴳ:low / ᴳ:conflicting / ◍-applicability
                                 ←❡ ※[PMID...]  via 〈EBMonFHIR〉

⇑ / ⇡ / ⊜ / ⇣ / ⇓ / ⊠ / ⊟    →  effect-direction field on ⟪claim⟫
                                 ⊜ vs ⊟ enforced as distinct types

⨯ / ⊣ / ⊢ / ↯ / ⇋ / ⊜ᵢ       →  ⩙ edges in ⩘[DrugBank] / ⩘[PrimeKG]
                                 (⩐ computed seam: GNN-based prediction for novel pairs)

⌊min⌋ ⟜─⟝ ⌈max⌉              →  dose-response curve attached to ⟪claim⟫
                                 (no current standard — novel field)

⥯  re-measure                →  ⌬⥯ event-driven trigger over ⫞
                                 (consumes new ⌗ᵗ from ⩘[L0-labs])

⏵⏵⏵ + ⌚                      →  〈FHIR〉 MedicationRequest + Timing

✓ / ⊘ / ⚠ / ❓ / ⏸           →  〈FHIR〉 PlanDefinition / RequestGroup output
                                 ⊙[card] via 〈CDS Hooks〉

Trichotomy card              →  ⫹⫺⫻ ⊙[card]  (the render-time invariant)
```

This composition makes the supplement module *implementable* against real
standards rather than purely notational. Every TopoGlyph 9.0 symbol now
points to a production dependency or a flagged gap.

## A Worked Substrate Example

Vitamin D₃ recommendation for the same person from TopoGlyph 9.0, traced
through the substrate:

```
☥ ≡ ⥻ᵗ〈Phenopacket〉{
       ⊪▤SNOMED:73211009 (T2D),
       ⊪▤SNOMED:38341003 (HTN),
       ⊪▤RxNorm:6809 (metformin),
       ⊪▤RxNorm:29046 (lisinopril),
       ⊵⊪▤LOINC:62292-8 (25(OH)D) = 22 ng/mL @ ⌗ᵗ⁻³ᵐ,
       ⊵⊪▤LOINC:33914-3 (eGFR) = 78 @ ⌗ᵗ⁻³ᵐ
     }
     ←❡ ⩘[EHR] + ⩘[Labs]

⟦Vitamin D₃⟧ ≡ ⩘[DrugBank:DB00169] ⊕ ⩘[NatMed:929] ⊕ ⩘[USDA-FDC:1102655]
                ⊪▤RxNorm:11253 ⊪▤UNII:1C6V77QF41

⟪claim_deficiency⟫ᴳ:high,ᴾ:adults-25(OH)D<20
   {direction: ⇑, magnitude: large, effect: 25(OH)D ↑}
   ←❡ ※[PMID:21646368, PMID:25527677]
   via 〈EBMonFHIR〉:Evidence

⟪claim_T2D⟫ᴳ:conflicting,ᴾ:T2D-adults
   {direction: ◐, ⟪D2d⟫ᴳ:high → ⊜ overall, subgroup signals ⇡}
   ←❡ ※[NCT01942694, PMID:31173679]

⩐ Applicability(⟪claim_deficiency⟫ᴾ, ☥):
   ☥.⊵25(OH)D = 22 < 20+threshold → ◍ partial applicability (borderline)
   confidence δ = moderate

⩐ Applicability(⟪claim_T2D⟫ᴾ, ☥):
   ☥ ∈ ᴾ → applies but ◐ → cannot resolve direction
   confidence δ = low / conflicting

Interaction scan over ⩘[DrugBank]:
   ⟪⟦VitD⟧ ⩙ ⟦metformin⟧⟫ → ⊜ᵢ
   ⟪⟦VitD⟧ ⩙ ⟦lisinopril⟧⟫ → ⊜ᵢ
   ←❡ ※[DrugBank-6.0]

⊕δ composition:
   benefits = {⟪deficiency-correction⟫ ᴳ:high δ:moderate}
   risks    = {⟪toxicity at >⌈4000IU⌉⟫ ᴳ:high}
   unknowns = {⟪T2D outcome⟫ ◐, ⟪long-term renal in ☥⟫ ⊟}

⫹⫺⫻ trichotomy frame:
   |  ⇑ deficiency correction (ᴳ:high)
   |  ⊠ toxicity above 4000IU/d (ᴳ:high)
   |  ⊟ T2D outcome (we do NOT claim); ⊟ long-term renal in this profile
   ←❡ provenance preserved per row

⊙[card] via 〈CDS Hooks〉
   suggests: 〈FHIR〉 MedicationRequest{D3, 2000IU PO daily, with-fat}
   schedules: ⌬⥯ at +12 wk → re-fetch ⊵25(OH)D from ⩘[Labs]
```

Every claim in the rendered card now traces to a typed source. Every `⩐`
makes its computation explicit. Every `⊟` is structurally distinct from
`⊜`. No `⤳!δ` appears anywhere — uncertainty is preserved end-to-end.

## Meta-Insight

Health informatics is not a single graph but a *layered topological space*.
The dominant standards each carve out one stratum:

- **FHIR** owns the wire format
- **OMOP** owns the analytics back-plane
- **openEHR** owns deep persistence
- **Phenopackets** owns portable patient state
- **UMLS + sub-vocabularies** own concept identity
- **EBMonFHIR** owns computable evidence (under construction)
- **CPIC** owns one slice of personalization (genetic)
- **DrugBank/PrimeKG** own interaction graphs
- **CDS Hooks + CQL** own decision-time delivery
- **MCDA** owns the formal decision frame

Each is sound within its stratum. **The unsolved problems live almost
entirely at the seams** — population→individual applicability, uncertainty
propagation, supplement→drug binding, studied-null vs genuine-unknown,
N-of-1 data modeling.

A new framework's value is measured not by reinventing strata but by
*closing seams*. The seam catalog above is the priority queue.

The substrate notation (`⩏`, `⩎`, `⩐`, `⩑`, `δ`, `⤳δ`, `⤳!δ`, `⫹⫺⫻`,
`←❡`) makes those targets visible — and makes any new design auditable
against them.
