# TOCTET Specification (v0.3)
**Phase-Synchronous Field Architecture · 2026**  
Author: Artem Hutvarev

Mathematical foundations reviewed and confirmed: 2026

---

## 1. Computational Model

TOCTET defines computation as the evolution of a bounded dynamic field.

State: oscillator variables **(θ phase, ω frequency)** + derived metrics.

Execution cycle:
```
field update → measurement → guard → collapse → observation
```

---

## 2. Time

Time is **phase progression on a torus**, not a global scheduler tick.

θ lives naturally on a torus: `θ ∈ [0, 2π)` — closed, cyclic.  
ω evolves in the orthogonal direction.  
The intersection of these two rotations is the **TOCTET viewport lattice**.

```
First uroboros:   θ (phase) rotates on torus
Second uroboros:  ω (frequency) rotates in opposite direction
Intersection:     viewport grid — where state becomes observable
```

Discrete integration step `dt` is chosen by the system (micro-reset interval),  
not imposed by an external clock.

---

## 3. Coherence — Formal Definition

### 3.1 Global Order Parameter (Kuramoto)

```
R · exp(iψ) = (1/N) · Σ exp(i·θ_k)     k = 1..N

R   ∈ [0, 1]   global coherence magnitude
ψ              mean phase angle
```

```
R = 1.0   perfect synchrony
R = 0.0   complete incoherence
```

### 3.2 Local Order Parameter

For each rift topology (cluster, ring, etc.), local coherence is computed  
over the oscillator neighborhood defined by that rift:

```
R_i = | (1/|N_i|) · Σ_{k ∈ N_i} exp(i·θ_k) |
```

This gives a **coherence field R(x)** — a map of stability across the system,  
not just a single scalar.

State representation: `(R_global, ψ_global)` + field `R(x)` local map.

### 3.3 Coherence Window (Golden Window)

```
coherence_min = 0.864
coherence_max = 1.44   (GOLDEN_BASE)
```

Stable: `R ∈ [0.864, 1.44]`  
Below 0.864: drifting → Guard activates  
Above 1.44: over-synchronized → reseed

---

## 4. Low-Dimensional Reduction (State Compression)

The "springs instead of stones" principle has a mathematical basis:  
large N oscillators can be described by a small number of parameters.

### 4.1 Ott-Antonsen Reduction

For large N with Lorentzian frequency distribution,  
the density of phases ρ(θ,ω,t) evolves on a low-dimensional manifold:

```
da/dt = -Δa + (K/2)(η - a²η*)     (OA ansatz)
```

where `a` is a single complex order parameter capturing collective dynamics.

**In TOCTET terms:** the pyramid (Guard/Boogie) is the OA reduction operator.  
You control the step → the lattice obeys.  
This is the mathematical foundation for state-trajectory playback  
without storing full streams.

### 4.2 Watanabe-Strogatz Constants of Motion

For identical oscillators, WS theory gives constants of motion  
that compress N oscillators into 3 parameters + N-3 constants.

**In TOCTET terms:** these constants are the "seeds" stored per session.  
Not the full state — just the invariants that reconstruct it.

---

## 5. Topology Operators — Spectral Basis

Since "rift = topology operator", the natural state lattice is  
the **eigenmodes of the coupling graph Laplacian**:

```
L = D - A     (Laplacian = degree matrix - adjacency matrix)
L · v_k = λ_k · v_k     (eigenmodes)
```

Field state can be projected onto the first `k` modes →  
stable coordinate basis for the field.

**Rift correspondence:**
```
cluster   →  low eigenmodes (community structure)
split     →  spectral bisection (Fiedler vector λ_2)
mirror    →  symmetric eigenmodes
sync      →  eigenmode locking (all phases align to mode)
ring      →  circulant graph eigenmodes (DFT basis)
```

This is the **mathematical definition of the cube**:  
Laplacian eigenmodes = stable "rooms" inside the state space.  
The torus rotates. The cube organizes.

---

## 6. Guard — Micro-Reset Policy

### 6.1 Trigger Conditions

```
R < 0.864              drift → normalize or rollback
R > 1.44               over-sync → reseed
cluster R_i < threshold  local → local normalize
```

### 6.2 Reset Types

**normalize**  
`θ_k += α · (θ_mean - θ_k)`    α ∈ [0.01, 0.1]  
Gentle pull toward mean phase. Preserves local structure.

**rollback**  
Restore from last stable snapshot (taken every N ticks when R is in window).  
Use when: R falling fast, normalize insufficient.

**reseed**  
`θ_k += noise(σ)` for over-synchronized oscillators.  
Use when: R > 1.44, diversity lost.

### 6.3 Continuity Invariant

No oscillator phase changes by more than `π/4` per reset.  
The observer must not perceive a restart.

---

## 7. Observability — Extended MRT

Standard channels:

```
mrt_phase       θ[N]
mrt_frequency   ω[N]
mrt_coherence   R map (global + local R(x))
mrt_drift       dR/dt
mrt_gate        mask / collapse map
mrt_events      phase-zero crossings, threshold events
```

Extended channels (v1):

```
mrt_recurrence  recurrence plot matrix (trajectory self-similarity)
mrt_poincare    Poincaré section snapshot (discrete map of flow)
mrt_modes       projection onto top-k Laplacian eigenmodes
mrt_homology    persistent homology diagram (topological signature)
```

Recurrence plot and persistent homology provide  
**proof-grade** evidence that mode A ≠ mode B —  
valuable for research publication and grant documentation.

---

## 8. State Storage — Compressed

```
STORE:        seeds (WS constants), rift patterns, metric anchors
DON'T STORE:  raw streams, full θ/ω arrays

ENABLES:      state-trajectory playback
              non-linear scrubbing
              session comparison
              coherence archival
```

Per-user buffer (3 states):
```
State_t      current
State_t-1    previous
State_t-2    two steps back
```

One shared backup 3 steps back.  
No continuous stream storage required.

---

## 9. Primitives

### Field
```
θ[N]       phase array
ω[N]       frequency array
R          global coherence
R_local    per-cluster coherence map R(x)
ψ          mean phase
drift      dR/dt
energy     Σ ω[k]² / N
modes[k]   projection on top-k Laplacian eigenmodes
```

### Rift
```
rift(type, params, mode)
type:  ring | cluster | spiral | mirror | split | gate | sync | noise
mode:  loop | one-shot | N{count}
```

### Guard
```
guard(coherence_min=0.864, coherence_max=1.44,
      reset_policy, snapshot_interval)
```

### Collapse
```
collapse(layers[], mask) → observable_frame
```

---

## 10. Canonical Rift Set

```
ring      circulant graph / cyclic coupling
cluster   community structure / low Laplacian eigenmodes
spiral    gradient coupling along spectral axis
mirror    symmetric eigenmode pair
split     spectral bisection (Fiedler vector)
gate      SDF form-cutting / threshold / mask
sync      eigenmode locking
noise     controlled entropy / prevents crystallization
```

---

## 11. Layer Stack

```
[ observation layer ]   ← MRT outputs (standard + extended)
[ mask / gate layer ]
[ modulation layer(s) ]
[ base field layer  ]
```

---

## 12. Cohort Modeling (State Space Geometry)

User/node states live in a continuous 4-dimensional space:

```
S = [arousal, tempo, stability, predictability]

arousal         energy, variability, jitter
tempo           event density, speech/signal rate
stability       coherence R, phase noise
predictability  similarity to baseline
```

Distance to typical zones replaces hard categories.  
Markov State Models provide transition probabilities between zones.

**Backup policy:** one shared snapshot 3 steps back per cohort.  
Ring buffer in RAM. Periodic aggregate snapshot to disk.

---

## 13. Swarm Protocol

```
packet = { phase_anchor, coherence_window, compact_metrics }
```

No full state replication.  
Laplacian eigenmodes as shared coordinate basis —  
nodes synchronize in eigenmode space, not raw phase space.

---

## 14. External Boundary

See [LINK_WINDOW.md](LINK_WINDOW.md).

---

## 15. Geometric Summary

```
TORUS      time as phase cycle (θ lives on torus)
           first uroboros — phase rotation

CUBE       state space organized by Laplacian eigenmodes
           stable rooms, coordinate basis
           second uroboros — frequency in orthogonal direction

PYRAMID    Guard / Boogie — the will
           Ott-Antonsen reduction
           large N → small parameters
           you choose the step, the lattice obeys
```

The intersection of torus and cube rotations = TOCTET viewport lattice.  
The pyramid makes it dance.

---

## Changelog

```
v0.1  initial
v0.2  coherence defined (Kuramoto R), micro-reset policy formalized
v0.3  OA/WS reduction added, Laplacian spectral basis,
      extended MRT (recurrence, Poincaré, homology),
      cohort geometry, torus/cube/pyramid geometric summary
```

---

*TOCTET Kernel Architecture · Artem Hutvarev · 2026*
