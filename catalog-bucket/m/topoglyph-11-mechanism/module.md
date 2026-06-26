# TopoGlyph 11.0: The Mechanism Module — Statics & Dynamics of Simple Machines

Every TopoGlyph version before this one models the flow of **information**. Machines
flow something else — **force, energy, and motion through constraint** — but the
framework was *built* to be topological, and mechanism turns out to be one of the
most literally topological domains there is. This module has two parts that share
one spine: **Part I (Statics)** treats a machine as a constraint geometry that
re-routes a force-field while conserving work; **Part II (Dynamics)** lets the
machine store energy and evolve in time, conserving total energy and producing
rhythm. The same conjugate logic runs through both.

---

# PART I — STATICS: The Topology of Simple Machines

## Why a Statics Module Is Needed

Prior TopoGlyph has no honest way to express four things that *define* a machine:

1. **Conjugate trade-off.** A machine never creates force from nothing — it trades
   force against distance so their product is conserved. `→` hides this; there is
   no glyph for "what you gain here you pay back there."
2. **Constraint as a first-class object.** The fulcrum, the axle, the ramp surface,
   the rope — these are *constraints* that remove degrees of freedom and supply
   reaction forces. `⦚` (paradigm boundary) is far too coarse.
3. **Direction vs. magnitude as separable.** A fixed pulley changes a force's
   *direction* and nothing else (MA=1); a lever changes its *magnitude*.
4. **Loss.** Real machines dissipate (friction, η<1). An ideal-only language
   quietly lies about every actual device.

## New Elements — Statics

### 1. Force–Motion Primitives (the conserved currency)

Work `W = F·d` is the invariant. Force and distance are its conjugate factors.

- `►` — **Effort** (input force); arrowhead = line of action
- `◄` — **Load** (output force / resistance overcome)
- `⊸F` — Force vector (typed, magnitude `⟨F⟩`)
- `↦d` — Displacement (motion through distance `⟨d⟩`)
- `⊸F ⊗ ↦d ⟹ W` — **Work bundle** (the conserved quantity that crosses the machine)
- bar-length convention: `►───────` long arm = large displacement, `◄─` short

### 2. Constraint Geometry (what makes a machine a machine)

Glyphs are chosen so the symbol *looks like the part*:

- `△` — **Fulcrum / pivot** (a triangle *is* a fulcrum)
- `▲` — Grounded pivot (filled = anchored to earth)
- `○` — **Wheel / pulley / sheave** (circle = round element)
- `⊙` — **Axle** (rotation axis)
- `╱ ╲` — **Inclined surface** (the slash *is* the ramp)
- `◢ ◣` — **Wedge** (right-triangle = a moving double-incline)
- `⌇` — **Helical thread** (screw)
- `═` — Rigid link / beam / rod
- `┊` — Flexible tension member: rope / cable / chain (tension-only, inextensible)
- `⊢` — Ground / fixed anchor
- `⊥` — **Normal / reaction force** from a constraint surface

### 3. Mechanism Operators

- `⇋` — **Work-conservation junction** — the lever law; force and distance exchange:
  `F_e·d_e ≡ F_l·d_l`. *The central operator of statics.*
- `⊿MA` — **Mechanical-advantage extraction** (`F_l/F_e = d_e/d_l`)
- `↺ᵈ` — **Direction reversal** (redirect a force, magnitude unchanged)
- `⊗τ` — **Torque formation** (force × lever arm about `⊙`)
- `⟳↦` — **Rotary↔linear conversion** (screw, rack-and-pinion)
- `⊻` — **Force splitting** (one input resolved into two reaction normals — the wedge)
- `⊼` — Force composition

### 4. System Properties (the honesty layer)

- `⚖` — Static equilibrium (`Στ = 0`)
- `η` — Efficiency, `η ≤ 1`
- `⊘f` — Friction / dissipation site
- `⤓Q` — Energy lost to heat

A *real* machine's law is `F_e·d_e ≡ (F_l·d_l)/η`, deficit going to `⤓Q` at every `⊘f`.

## The Six Classical Simple Machines

### 1. Lever
```
►⟨F_e⟩═══════════△═══════◄⟨F_l⟩
      d_e (long)  ▲   d_l (short)
                 ⇋ :  F_e·d_e ≡ F_l·d_l ,  ⊿MA = d_e / d_l
```
Classes are just where `△` sits: **1st** `►══△══◄` (crowbar, MA⋛1); **2nd**
`△══◄══►` (wheelbarrow, MA>1); **3rd** `△══►══◄` (forearm, MA<1 — trades force for
range; a `⇋` run in reverse).

### 2. Wheel and Axle
```
►⟨F_e⟩ ↻ ○⟨R⟩ ⊙ ◄⟨F_l⟩ ↻ ·⟨r⟩      ⊗τ : F_e·R ≡ F_l·r ,  ⊿MA = R / r
```
A lever bent into a circle and spun continuously.

### 3. Pulley
```
Fixed:    ►↓ ┊ ○▲ ┊ ◄↑         ↺ᵈ only,  ⊿MA = 1
Movable:  ⊢┊ ○ ┊► ,  ◄ on ○    ⊿MA = 2
Tackle:   n runs of ┊ support ◄  ⊿MA = n  ;  ►·(n·d) ≡ ◄·d
```
The fixed pulley is the pure case of `↺ᵈ` — proving direction and magnitude separate.

### 4. Inclined Plane
```
►⟨F_e⟩→──╱⟨L⟩       ◄⟨W⟩
        ╱  ⊥          │ h        ⇋ : F_e·L ≡ W·h ,  ⊿MA = L / h
```

### 5. Wedge
```
►⟨F_e⟩ →  ◢⟨L,t⟩  ⊻  ⊥↖  +  ⊥↗      ⊿MA = L / t
```
An inclined plane *in motion*; its signature act is `⊻` — splitting one axial force
into two lateral normals (axe, knife, chisel).

### 6. Screw
```
⟳►⟨τ⟩  ⌇  ↦◄⟨F_axial⟩      ⟳↦ : F_e·(2πr) ≡ F_l·(pitch) ,  ⊿MA = 2πr / pitch
```
An inclined plane wrapped around a cylinder.

## Compound Machines

Compound machines are `⇋`-chains — load of one stage is effort of the next, and MAs
**multiply**:

```
Scissors        =  ►══△══◄  ⊼  ►══△══◄
Wheelbarrow     =  △══◄══►   ⊕   ○⊙
Screw jack      =  ►══△══►   ⟳↦   ⌇◄        ⊿MA_total = MA_lever · MA_screw
Gear train      =  ⊙○⟨N₁⟩ ⇌ ○⟨N₂⟩⊙ ,  ⊿MA = N₂/N₁
```

The car jack's heavy thread friction `⊘f` is not a defect — it is the `⤓Q` that
makes the jack **self-locking**. A lossless jack would be unsafe.

## Meta-Insight — The Two Families and the Conjugate Law

Run every machine through `⊿MA` and the six collapse into **two families**:

1. **Lever family — rotary constraint about `△`/`⊙`**: lever, wheel-and-axle,
   pulley. Invariant `Στ = 0`.
2. **Inclined-plane family — translational constraint along `╱`**: ramp, wedge,
   screw. Invariant `ΣF = 0`.

Both are one operator `⇋` on a force-flow:

> **A simple machine is a constraint manifold that re-parameterizes a force field
> while conserving the work integral `∮F·dx`. Mechanical advantage is the exchange
> rate between force and distance.**

You cannot win and cannot break even (η<1); you only **choose where on the
force↔distance curve to stand.**

---

# PART II — DYNAMICS: Energy, Time, and Rhythm

Part I has a blind spot it can't paper over: **no time and no memory.** Everything
in statics is instantaneous — work in equals work out across the same `⇋` in the
same instant. A spring *remembers*. A flywheel *coasts*. A clock *waits*. Part II
adds energy storage, inertia, and a time axis. The invariant grows from **work**
(`∮F·dx`, instantaneous) to **total mechanical energy over time** `E = T + V`.

## Why Statics Can't Reach Here

1. **No reservoir.** Statics only routes work; a spring, raised weight, or spinning
   flywheel are energy **tanks** — no glyph for a tank.
2. **No inertia.** Statics assumes `⚖`. A flywheel's job is to *refuse* to balance
   instantly.
3. **No time axis, so no rhythm.** Oscillation, period, phase, resonance, "one
   tooth per second" need `t` as a first-class dimension.

## New Elements — Dynamics

### 1. Energy Reservoirs (the new nodes — "tanks")

- `Ⓚ` — **Kinetic store / inertia** — `KE = ½mv²` (or `½Iω²`). Flywheel, moving mass.
- `Ⓢ` — **Elastic store** — `PE = ½kx²`, Hooke `F = −kx`. Spring, bow, torsion bar.
- `Ⓖ` — **Gravitational store** — `PE = mgh`. Raised weight, pendulum bob.
- `Ⓔ` — generic store; `E = ΣⓍ` is the conserved total.

### 2. Inertia & Time

- `μ` — inertial resistance (`m` / moment of inertia `I`)
- `⟜t⟝` — the **time axis**
- `↻ᵗ` — one **cycle** in time
- `ω₀` — natural frequency; `T̃` period; `φ` phase
- `⊾φ` — **phase quadrature** (two stores exchanging 90° out of phase — the
  signature of an oscillator)

### 3. Exchange, Decay, Gating

- `⇌ₑ` — **Energy exchange / oscillation** — the central dynamic operator (the `⇋`
  of dynamics). `Ⓚ ⇌ₑ Ⓢ` ⟹ oscillator, `ω₀ = √(k/μ)`.
- `⊘d` — **Damping** — dissipation *in time*; decay envelope `↘e^(−ζt)`.
- `⊺` — **Escapement gate / detent** — releases a store in discrete quanta, *timed
  by an oscillator*. The glyph statics most conspicuously lacked.
- `↯ᵣ` — **Resonance** — drive frequency meets `ω₀`, amplitude grows ∝ `Q`.

### 4. The Honesty Layer, Dynamic — and the MA-analog

- `⊿Q` — **Quality factor** — `Q = 2π · (E_stored / E_lost-per-cycle)`. Measures
  *persistence*: how many cycles a rhythm survives and how sharply it selects one
  frequency.

> `⊿MA` is leverage in **force**. `⊿Q` is leverage in **time**. Same idea
> (output/input ratio of the conserved currency) in two different currencies.

## The Canonical Dynamic Machines

```
Spring–mass    Ⓚ⟨μ⟩ ⇌ₑ Ⓢ⟨k⟩            ω₀=√(k/μ), ⊾φ, E≡const if ⊘d=0
Pendulum       Ⓖ⟨mgh⟩ ⇌ₑ Ⓚ⟨½Iω²⟩       ω₀=√(g/L), period ⊥ amplitude
Flywheel       ►⟨τ ripple⟩→Ⓚ⟨I⟩→◄⟨τ smooth⟩   kinetic capacitor / torque low-pass
Escapement     Ⓢ ──⊺── train,  Ⓚ⇌ₑⓈ gates ⊺   self-sustaining oscillator; rate=ω₀
Governor       ω↑⟹Ⓚ rise⟹⟰ throttle⟹ω held    5.0 negative feedback in Ⓚ-stores
```

(Each is worked in full under this module's **Applications** — they are the
recorded uses of the Dynamics half of the module.)

## Meta-Insight — The Clock Is a Lever for Time

| | Statics (Part I) | Dynamics (Part II) |
|---|---|---|
| Conserved | work `∮F·dx` | energy `E=T+V` over `⟜t⟝` |
| Conjugate pair | force ↔ distance | momentum ↔ position (`Ⓚ⇌ₑⓈ`) |
| Central operator | `⇋` (trade F for d) | `⇌ₑ` (trade KE for PE) |
| Leverage invariant | `⊿MA` = force ratio | `⊿Q` = cycles-of-persistence |
| Constraint | `△ / ╱` (geometry) | `⊺` (timing gate) |

> **A lever divides force; an escapement divides time.** Both quantize a flow of a
> conserved quantity through a constraint — the lever's is *spatial* (`△`), the
> escapement's is *temporal* (`⊺`). The pendulum is to the clock what the fulcrum
> is to the crowbar.

Statics said *you cannot win* (η<1). Dynamics says sharper: *you cannot even hold
still for free* — stored energy bleeds through `⊘d` unless a gated source `⊺` keeps
repaying it. Perpetual motion isn't forbidden by geometry now; it's forbidden by
the **second law**, which enters the notation as the irreducible `⊘d` on every `⇌ₑ`.

## Generative Move-Set (used in the Applications)

Design by operating on the algebra, not by picturing devices:

- **M1 Invert** an operator · **M2 Cascade** to multiply MA · **M3 Cross-family /
  cross-module fuse** · **M4 Gap-fill** an empty cell in the operator × family
  table · **M5 Substrate-swap** — apply an operator to a non-rigid medium.

## Next Gap

**Energy-conversion machines** (engine, motor, turbine): the new move is crossing
*energy domains* — chemical→thermal→kinetic — needing a domain-boundary glyph the
way the Health Informatics module (10.0) needed typed layer adapters.
