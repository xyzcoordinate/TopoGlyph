# MechGlyph: A Constraint-Topology Module for Simple Machines

## Why mechanical machines need their own module

Base TopoGlyph models **cognitive** topology — information states, transformations, gestalt shifts. Simple machines live in a different topological category: they are **physical constraint manifolds** that route force and motion. Re-using `■`, `→`, `⟳` here would conflate "knowledge transformation" with "force transmission" and lose the conservation law that defines mechanics: input work equals output work.

What we actually need:

- A way to name **carriers** of mechanical signal (force, displacement, torque).
- A vocabulary for **constraints** (pivots, axles, ropes, slopes, helices) — the topological features that make a machine a machine.
- A **coupling operator** that enforces work conservation across a constraint.
- Invariants — **mechanical advantage**, **velocity ratio**, **efficiency** — readable directly off the glyph.

## Core elements

### 1. Carriers (what flows)

| Glyph | Meaning |
|---|---|
| `⇒F` | Applied linear force (effort) |
| `⇐W` | Resistive load (weight, output force) |
| `⇢d` | Linear displacement |
| `↻τ` | Applied torque (input) |
| `↺Τ` | Resistive torque (output) |
| `∿T` | Tension along a cable (scalar, signed pull only) |

Direction of the arrowhead is the physical direction; magnitude is a label.

### 2. Constraint primitives (what holds)

| Glyph | Constraint | Degrees of freedom removed |
|---|---|---|
| `⊙` | Pivot / fulcrum | Point fixed; rotation free |
| `⊚` | Axle | Line fixed; rotation about it free |
| `⊡` | Rigid anchor | All DOF removed |
| `═` | Rigid bar | Transmits force along its length, preserves shape |
| `∿` | Cable / rope | Transmits tension along path; cannot push |
| `⟋θ` | Inclined surface at angle θ | Motion restricted to slope |
| `⟳p` | Helical constraint, pitch p | Couples rotation and translation: Δz = p · (Δφ / 2π) |
| `▣θ` | Wedge surface (two `⟋θ/2` back to back) | Redirects axial push to lateral splay |

### 3. Coupling operators (how flow crosses constraints)

| Glyph | Meaning |
|---|---|
| `⇌` | Ideal work coupling: F_in · d_in = F_out · d_out |
| `⇋` | Lossy coupling: F_in · d_in = F_out · d_out + Q (heat) |
| `↯` | Direction reversal (sign flip along a path) |
| `⊥` | Perpendicular redirection |
| `×n` | n-fold replication of a path (parallel cable runs) |

### 4. Topological invariants (annotations on the glyph)

| Annotation | Meaning |
|---|---|
| `MA:n` | Mechanical advantage = F_out / F_in (ideal) |
| `VR:n` | Velocity ratio = d_in / d_out (= MA when η=1) |
| `η`    | Efficiency = (F_out · d_out) / (F_in · d_in) |

Conservation law (the only physics axiom this module needs):

```
⇒F ⇌ ⇐W   ⟺   F · d_in = W · d_out   ⟺   MA · VR = 1
```

## The six classical simple machines in MechGlyph

### Lever (three classes)

The lever is a rigid bar `═` constrained by one pivot `⊙`. The MA is the ratio of perpendicular distances from the pivot to the two force lines-of-action.

**1st class** (fulcrum between effort and load):

```
⇒F ═══════[⊙]═══ ⇐W
    ←─ a ─→  ←b→     MA = a / b
```

**2nd class** (load between fulcrum and effort):

```
[⊙]═══⇐W═══════⇒F
   ←b→         ←a→   MA = a / b  (always > 1)
```

**3rd class** (effort between fulcrum and load):

```
[⊙]═══⇒F═══════⇐W
   ←b→         ←a→   MA = b / a  (always < 1; trades force for speed)
```

### Wheel and axle

A degenerate continuous lever: pivot becomes axle `⊚`, lever sweeps a full circle.

```
↻τ ─R─⊚─r─ ⇌ ⇐W
                    MA = R / r
```

### Pulley

Cables `∿` route tension; a pulley redirects the cable around a pivot.

**Fixed pulley** (pivot anchored to world): direction change only.

```
⊡⊙             MA = 1
 ∿↯∿
⇒F        ⇐W
```

**Movable pulley** (pivot attached to load): load supported by two segments.

```
⊡∿        ⇒F∿
   ⊙─⇐W           MA = 2
```

**Block and tackle** (n supporting segments via `∿×n`):

```
∿×n ─⊙─⇐W         MA = n
```

### Inclined plane

A 1D constraint slope `⟋θ` embedded in 2D. Push along the slope, gravity-component of load is what you fight.

```
⇒F ─⟋θ─ ⇌ ⇐W       MA = 1 / sin θ
```

Visual: the longer/shallower the slope, the higher the MA, at the cost of distance traveled.

### Wedge

A moving inclined plane. Axial input becomes perpendicular splay; the constraint is `▣θ`.

```
⇒F ──→ ▣θ ──→ ⊥⇐W       (force above)
              ⊥⇐W       (force below)
                          MA ≈ 1 / (2 tan(θ/2)) ≈ L / t
                          (L = length, t = thickness)
```

The `↯` is implicit: input direction is redirected by 90° (split into ±⊥).

### Screw

An inclined plane `⟋` wrapped around a cylinder, i.e. the helical constraint `⟳p`. Rotation transduces to axial translation.

```
↻τ ─R─⟳p─ ⇌ ⇢d=p per turn      MA = 2πR / p
```

Reading the glyph: torque applied at lever-radius R drives helical constraint with pitch p; one full rotation advances p, so the displacement ratio is `2πR : p`.

## Composition: every compound machine is a glyph composition

Compound machines are exactly the products of these primitives, with mechanical advantages multiplying.

**Wheelbarrow** = 2nd-class lever + wheel-and-axle (the wheel is the fulcrum that also rolls):

```
[⊚]═══⇐W═══════⇒F
```

**Scissors / pliers** = two 1st-class levers sharing a pivot, with wedges `▣` at the tips:

```
⇒F ═══[⊙]═══ ▣θ ⊥⇐W
⇒F ═══[⊙]═══ ▣θ ⊥⇐W       MA_total = MA_lever × MA_wedge
```

**Car jack** = screw + lever (the crank):

```
↻τ ─R_crank─⟳p─ ⇌ ⇐W       MA = (2π · R_crank) / p
```

**Bicycle drivetrain** = pedal-lever + wheel-and-axle₁ + chain (`∿`-like, inextensible) + wheel-and-axle₂:

```
⇒F_pedal ═══[⊚₁]r₁ ─∿─ r₂[⊚₂]═══R_wheel─ ⇢d_ground
```

**Differential pulley** (Weston) = two coaxial wheels of nearly equal radius with a loop of `∿` over both:

```
↻τ ─R₁⊚R₂─ ∿×2 ─⇐W         MA = 2R₁ / (R₁ − R₂)
```

Mechanical advantage is unbounded as R₁ → R₂ — a beautiful topological singularity falls right out of the glyph.

## Topological classification of simple machines

The deepest pattern: each simple machine is a **transducer** between motion types, indexed by which carrier sits on input vs. output.

|                    | Output: linear `⇢` | Output: rotational `↻` |
|---|---|---|
| **Input: linear `⇢`** | Pulley (↯), Lever-as-small-angle, Inclined plane, Wedge (⊥) | Crank / capstan |
| **Input: rotational `↻`** | Screw `⟳`, Rack-and-pinion, Wheel-and-axle (rim out) | Wheel-and-axle (axle ↔ rim), Gear train |

The "kind" of a simple machine is fully specified by:

1. **(input carrier, output carrier)** — which cell of the table.
2. **The constraint primitive** — `⊙`, `⊚`, `∿`, `⟋`, `▣`, `⟳`.
3. **The geometric parameter that sets MA** — arm ratio, radius ratio, segment count, sin θ, pitch.

This is the entire classification. The classical "six" are not a coincidence — they are exactly the cells × constraint-primitives that have a single geometric parameter.

## Meta-insight: simple machines are gauge transformations on work

Every simple machine factors the same equation `F·d = const` through a different geometric route. The MA is a **topological invariant of the constraint manifold** — it depends only on the geometry of the constraint (arm ratio, slope, pitch), never on the magnitude of the input.

In MechGlyph terms:

```
⦗⇒F · ⇢d_in⦘ ⇌ ⦗⇐W · ⇢d_out⦘
       ↑                  ↑
       └── same work, routed through ─┘
            constraint topology K

       MA(K) = invariant of K alone
```

Where base TopoGlyph captured `⟦■*⟧` as the protected invariant of a *cognitive* transformation, MechGlyph captures `MA` as the protected invariant of a *physical* transformation. Both are read off the topology of the diagram.

Two consequences this module makes immediate:

- **No free lunch** is a literal reading of `⇌`: the two sides are equal, so increasing force shrinks displacement.
- **Composition is multiplicative**: chain two `⇌`s and the MAs multiply, because the glyph is just one constraint feeding another.

## Suggested next extensions

- **Friction/efficiency module** — turn every `⇌` into `⇋` with explicit Q (heat) labels; model self-locking screws (η < 0.5 ⇒ won't back-drive).
- **Fluid power module** — add `⇝Q` (volumetric flow) and `⇝P` (pressure) carriers; hydraulic press becomes the fluid analogue of a lever (`A_in/A_out` = MA).
- **Elastic module** — springs as constraints `≈k≈` storing work; useful for ratchets, escapements, and any machine with a non-rigid bar.
