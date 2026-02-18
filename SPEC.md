# TOCTET Specification (v0.2)
**Phase-Synchronous Field Architecture · 2026**  
Author: Artem Hutvarev

---

## 1. Computational Model

TOCTET defines computation as the evolution of a bounded dynamic field.

State is represented as oscillator variables **(θ phase, ω frequency)**  
plus derived metrics computed from the field.

Execution cycle:
```
field update → measurement → guard → collapse → observation
```

---

## 2. Time

Time is **phase progression**, not a global scheduler tick.

A practical implementation uses discrete integration step `dt`,  
while preserving phase-first semantics.

The system selects its own micro-reset interval —  
expressed as a coherence range, not as a clock period.

---

## 3. Coherence — Formal Definition (v0)

Coherence `R` is the **global order parameter** of the oscillator field,  
defined as the magnitude of the mean complex phase vector:

```
R = | (1/N) · Σ exp(i·θ_k) |    k = 1..N

R = 1.0   → perfect synchrony (all oscillators in phase)
R = 0.0   → complete incoherence (uniform phase distribution)
```

This is the Kuramoto order parameter.  
In TOCTET, `R` is the primary health metric of the field.

**Coherence window (Golden Window):**
```
coherence_min = 0.864
coherence_max = 1.44   (GOLDEN_BASE)
```

The field is considered **stable** when `R ∈ [0.864, 1.44]`.  
Below 0.864: field is drifting → Guard activates.  
Above 1.44: field is over-synchronized → entropy injection recommended.

**Local coherence** (per-cluster):  
Computed as the order parameter within a subset of oscillators.  
Used for cluster-level Guard decisions independent of global R.

**Spectral coherence** (optional, v1):  
Frequency-domain measure of phase stability over time.  
Not required for v0 implementation.

---

## 4. Guard — Micro-Reset Policy

The Guard monitors `R` and applies corrections when the field exits the stability window.

### 4.1 Trigger Conditions

```
R < coherence_min (0.864)   → drift detected → reset type: normalize or rollback
R > coherence_max (1.44)    → over-sync detected → reset type: reseed
```

### 4.2 Reset Types

**normalize**  
Gently pull oscillator phases toward the mean phase.  
Proportional correction: `θ_k += α · (θ_mean - θ_k)`  
Strength `α ∈ [0.01, 0.1]` — weak, preserves local structure.  
Use when: mild drift, R slightly below threshold.

**rollback**  
Restore field state from the last known stable snapshot.  
Snapshot is taken every N ticks when R is within window.  
Use when: rapid drift, R falling fast, normalize insufficient.

**reseed**  
Inject controlled noise into over-synchronized regions.  
`θ_k += noise(σ)` for oscillators with |θ_k - θ_mean| < ε  
Use when: R > coherence_max, field crystallizing, diversity lost.

### 4.3 Reset Policy Selection

```
R falling, rate slow     → normalize
R falling, rate fast     → rollback
R > coherence_max        → reseed
cluster R < threshold    → local normalize (cluster only)
global R stable but      → local rollback (affected cluster)
  cluster diverging
```

### 4.4 Continuity Invariant

Micro-reset must preserve the continuity invariant:  
**the observer must not perceive a restart.**

Reset magnitude is bounded: no oscillator phase changes by more than `π/4` per reset.  
If correction requires more than `π/4`: use rollback, not normalize.

---

## 5. Primitives

### 5.1 Field
```
θ[N]     phase array
ω[N]     frequency array
R        global coherence (Kuramoto order parameter)
R_local  per-cluster coherence map
drift    dR/dt — rate of coherence change
energy   Σ ω[k]² / N
```

### 5.2 Rift
```
rift(type, params, mode)

type:  ring | cluster | spiral | mirror | split | gate | sync | noise
mode:  loop | one-shot | N{count}
```

Rifts inject constraints. The field evolves according to constraints.

### 5.3 Guard
```
guard(
  coherence_min,     // 0.864
  coherence_max,     // 1.44
  reset_policy,      // normalize | rollback | reseed
  snapshot_interval  // ticks between stable snapshots
)
```

### 5.4 Collapse
```
collapse(layers[], mask) → observable_frame
```

---

## 6. Canonical Rift Set

```
ring      cyclic coupling baseline
cluster   modular community formation
spiral    gradient coupling along axis
mirror    symmetry / involution
split     independent sub-field partition
gate      SDF form-cutting / threshold / mask
sync      phase-lock discipline
noise     controlled entropy injection
```

---

## 7. Layer Stack

```
[ observation layer ]   ← MRT outputs
[ mask / gate layer ]
[ modulation layer(s) ]
[ base field layer  ]
```

Collapse resolves superposition → one observable frame.

---

## 8. MRT Observability

```
mrt_phase       θ[N]
mrt_frequency   ω[N]
mrt_coherence   R map (global + local)
mrt_drift       dR/dt map
mrt_gate        mask / collapse map
mrt_events      phase-zero crossings, threshold events
```

---

## 9. Boogie Woogie (v0)

```
BOOGIE   scheduler within [coherence_min, coherence_max]
WOOGIE   ring script: loop | N-shot | fractal phase 288

RUNE     bitmask pattern square
         requires: interpretation + coupling rules + stability intent
         without all three: noise generator
```

---

## 10. State Storage

```
STORE        seeds, rift patterns, metric anchors
DON'T STORE  raw streams, full state arrays
ENABLES      state-trajectory playback, non-linear scrubbing
```

---

## 11. Swarm Protocol

```
packet = { phase_anchor, coherence_window, compact_metrics }
```

No full state replication.  
Synchronization = constraint propagation, not state copy.

---

## 12. External Boundary

TOCTET does not accept raw internet traffic.  
All external data enters through the Link Window boundary node.  
See [LINK_WINDOW.md](LINK_WINDOW.md).

---

## Changelog

```
v0.1  initial release
v0.2  §3 coherence formally defined (Kuramoto R, local R, spectral v1)
      §4 micro-reset policy formalized (normalize / rollback / reseed)
      §12 external boundary reference added
```

---

*TOCTET Kernel Architecture · Artem Hutvarev · 2026*
