# Topology Library (Rifts)
**TOCTET Kernel Architecture · 2026**  
Author: Artem Hutvarev

---

## Concept

A rift is a topology/field operator.

A scene is a composition of rifts — analogous to a node graph or blueprint —  
but nodes operate on **field state**, not on imperative code.

The rift does not execute. It injects constraints.  
The field evolves according to those constraints.

---

## Why Topology First

Instead of shipping stones (heavy stacks, runtime dependencies),  
TOCTET ships springs (patterns + constraints).

Patterns are cheap. The field makes them complex.

A bitmap pattern costs nothing.  
Its emergent behavior can be arbitrarily rich.

---

## Canonical Rift Set

### `ring`
**Cyclic coupling baseline.**  
Oscillators arranged in a closed loop. Each node coupled to neighbors.  
Spectral basis: circulant graph eigenmodes (DFT basis).  
Foundation for all other topologies.

```
coupling: θ_k → θ_{k+1} → ... → θ_k
spectral: λ_j = 2(1 - cos(2πj/N))
```

---

### `cluster`
**Modular community formation.**  
Field partitioned into islands of strong internal coupling,  
weak external coupling between islands.  
Spectral basis: low Laplacian eigenmodes (community structure).

```
within cluster:   K_internal >> K_external
between clusters: K_external → min
result:           semi-independent coherence pockets
```

---

### `spiral`
**Structured coupling gradient.**  
Coupling strength varies along an axis — strong at center, weak at periphery,  
or progressive along a direction.  
Creates structured drift and wave propagation.

```
K(r) = K_0 · f(r)     r = distance from axis
result: rotating phase waves, gradient synchronization
```

---

### `mirror`
**Symmetry / involution.**  
Field folded along an axis. Symmetric oscillator pairs coupled.  
Creates standing waves and stable symmetric attractors.

```
θ_k ↔ θ_{N-k}    (mirror pairing)
result: symmetric eigenmodes, stable reflection patterns
```

---

### `split`
**Sub-field partitioning.**  
Field divided into independent regions with minimal cross-coupling.  
Spectral basis: Fiedler vector (λ_2 eigenvector of Laplacian).

```
partition: sign(v_2[k])    v_2 = Fiedler vector
result:    two weakly coupled sub-fields
           each with independent Guard
```

---

### `gate`
**Form-cutting / threshold / masking.**  
SDF (signed distance function) applied to field topology.  
Oscillators outside the form boundary are decoupled or suppressed.  
Creates stable "physical" shapes in the field.

```
mask[k] = SDF(position[k]) > 0 ? active : inactive
result:   field takes the shape of the SDF form
          boundary produces natural phase discontinuity
```

---

### `sync`
**Discipline + phase-lock.**  
Strong coupling pulls all oscillators toward a target phase.  
Analog: PLL (phase-locked loop).  
Spectral basis: eigenmode locking — all phases align to dominant mode.

```
dθ_k/dt += K_sync · sin(θ_target - θ_k)
result:    rapid convergence to coherent state
           use carefully — can crystallize the field (R > 1.44)
```

---

### `noise`
**Controlled entropy injection.**  
Prevents crystallization (over-synchronization, R > 1.44).  
Maintains diversity in the field without destroying coherence.

```
θ_k += ξ_k · σ    ξ_k ~ N(0,1)
σ calibrated to keep R in [0.864, 1.44]
result:    living field — not frozen, not chaotic
```

---

## Composition Rules

Rifts are composed sequentially as injections into the field:

```
inject(ring)
inject(cluster, params)
inject(gate, sdf_form)
→ collapse
→ observation
```

A rift composition is a **scene**.  
A scene with agents is a **world**.

---

## Stability Requirements

Every rift must specify three things to remain signal — not noise:

1. **Topology intent** — what structure it creates
2. **Coupling intent** — how it modifies ω, θ, K
3. **Stability intent** — its range policy (does it push toward sync or toward noise?)

Without all three: noise generator.  
With all three: controllable field behavior.

---

## Spectral Correspondence Summary

| Rift | Graph structure | Spectral basis |
|---|---|---|
| `ring` | Circulant | DFT eigenmodes |
| `cluster` | Block diagonal | Low eigenmodes λ_1..k |
| `spiral` | Gradient weighted | Weighted Laplacian |
| `mirror` | Symmetric | Symmetric eigenmodes |
| `split` | Bipartite | Fiedler vector λ_2 |
| `gate` | SDF masked | Boundary modes |
| `sync` | Complete (K→∞) | Dominant eigenmode |
| `noise` | Random perturbation | Flat spectrum |

---

*TOCTET Kernel Architecture · Artem Hutvarev · 2026*
