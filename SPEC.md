# TOCTET Specification (v0)
**Phase-Synchronous Field Architecture · 2026**  
Author: Artem Hutvarev

---

## 1. Computational Model

TOCTET defines computation as the evolution of a bounded dynamic field.

State is not stored in registers or memory addresses.  
State is represented as oscillator variables **(θ phase, ω frequency)**  
plus derived metrics computed from the field.

Execution is not an ordered list of instructions.  
Execution is: `field update → measurement → guard → collapse → observation`

---

## 2. Time

Time in TOCTET is **phase progression**, not a global scheduler tick.

A practical implementation may use a discrete integration step `dt`,  
while preserving phase-first semantics throughout.

The RTC (real-time clock) provides the reference step.  
The system chooses its own micro-reset interval — visible as a range, not as a clock.

---

## 3. Coherence Range (Guard)

The system maintains a coherence window `[coherence_min, coherence_max]`.

Reference values:
```
coherence_min = 0.864
coherence_max = 1.44   (GOLDEN_BASE)
```

If measured coherence exits the allowed window, the Guard applies a **micro-reset**:
- local drift correction
- bounded rollback / normalization
- stability restoration without full restart

Micro-reset is minimally invasive. It preserves continuity.  
It is designed to be invisible to the observer — a correction, not a restart.

---

## 4. Primitives

### 4.1 Field
```
θ[i]     — phase of oscillator i
ω[i]     — frequency of oscillator i
R        — coherence (Kuramoto order parameter)
drift    — rate of phase divergence
energy   — aggregate field energy
```

Optional: layered field projections (see §6).

### 4.2 Rift
A parameterized operator that modifies topology, coupling, or local field constraints.

```
rift(type, params, mode)

type:  ring | cluster | spiral | mirror | split | gate | sync | noise
mode:  loop | one-shot | N{count}
```

Rifts do not execute. They **inject constraints** into the field.  
The field evolves according to those constraints.

### 4.3 Guard
```
guard(coherence_min, coherence_max, reset_policy)

reset_policy: local | bounded | normalize
```

### 4.4 Collapse
Maps superposed state layers into a single observable frame.

```
collapse(layers[], mask) → observable_frame
```

What is not selected by collapse remains in superposition.

---

## 5. Canonical Rift Set

```
ring      cyclic coupling baseline; foundational manifold
cluster   community formation; modular islands of coupling
spiral    gradient coupling; structured drift along axis
mirror    symmetry operation; involution on state
split     partition into independent sub-fields
gate      threshold / mask / SDF form-cutting
sync      phase-lock discipline; PLL-analog
noise     controlled entropy injection; prevents crystallization
```

Composition: a scene is an ordered sequence of rift injections followed by collapse.

---

## 6. Layer Stack

```
[ observation layer ]   ← MRT output channels
[ mask / gate layer ]
[ modulation layer n ]
[ ...               ]
[ modulation layer 1 ]
[ base field layer  ]
```

Layers are superposed, not executed sequentially.  
Collapse resolves the superposition into one observable frame.

---

## 7. MRT Observability

Multi-channel simultaneous output of field state.

```
mrt_phase       θ array
mrt_frequency   ω array
mrt_coherence   stability map
mrt_drift       divergence map
mrt_gate        mask / collapse map
mrt_events      phase-zero crossings, threshold events
```

All channels are available simultaneously.  
Observability is not optional — it is the primary mechanism for system validation.

---

## 8. Boogie Woogie Language (v0)

TOCTET's native programming model.

```
BOOGIE  scheduler / mixer
        operates within [coherence_min, coherence_max]
        applies micro-reset when range is approached
        holds the field in its stability window

WOOGIE  ring script
        loop | N-shot | fractal (phase 288)
        the programmable unit of field behavior
```

**Rune** — atomic semantic unit:
```
not a bit (0 or 1)
a square with a bitmask pattern
interpretation + coupling rules + stability intent = valid rune
without all three: noise generator
```

---

## 9. State Storage

TOCTET favors minimal storage:

```
STORE:      seeds, rift patterns, metric anchors
DON'T STORE: raw streams, full state arrays
```

This enables **state-trajectory playback**:
- replay of state evolution
- non-linear scrubbing
- comparison between sessions
- archival of coherence trajectories

---

## 10. Swarm Protocol

Distributed nodes synchronize via minimal packets:

```
packet = { phase_anchor, coherence_window, compact_metrics }
```

No full state replication.  
Each node maintains its own field; synchronization is constraint propagation.

---

## 11. Scope

This document is public technical disclosure and prior art.  
Published: 2026  
Author: Artem Hutvarev
