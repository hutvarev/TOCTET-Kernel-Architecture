# Demo Concept — Vertical Slice
**Raw State-Driven World · TOCTET Kernel Architecture · 2026**  
Author: Artem Hutvarev

---

## Goal

Prove the architecture through a single vertical slice.

Not content-heavy. Not polished. **Feature-heavy.**

The demo must show one thing clearly:  
field dynamics produce a living world —  
not scripted, not pre-baked, not smooth CGI.

---

## Visual Principle

**24 frames. Not smooth. Physical weight.**

Micro-reset = frame boundary.  
Every state jump is visible as a discrete step.  
The field breathes. Not dead pixels. Living state.

Like Jurassic Park dinosaurs — weight, inertia, cost per step.  
Like T-1000 — discrete state changes, not interpolation.

The difference between living and dead graphics  
is not resolution. It is **discreteness**.

---

## Scene Structure

```
Layer 0: base field
    WOOGIE(ring, {N:288, K:0.5}, loop)
    → ocean / atmospheric field

Layer 1: modulation
    WOOGIE(cluster, {islands:3}, loop)
    → wave regions, weather zones

Layer 2: gate
    WOOGIE(gate, {sdf: vessel_hull}, loop)
    → vessel form cuts the field
    → ripples at SDF boundary

Layer 3: local stability pocket
    WOOGIE(cluster, {local: cabin}, loop)
    → calm zone inside the vessel

Layer 4: signal
    WOOGIE(sync, {channel: radio}, loop)
    → minimal SDR/phase anchor packets
    → external communication channel
```

---

## Agents

Not scripted NPCs. Not behavior trees.

**Dual-loop field agents:**

```
Agent = two coupled oscillator sub-fields

Left loop:   abstract / generative (long-range coupling)
Right loop:  form-cutting / decisive (short-range, gate-dominant)

Thalamus:    coupling between loops (sync rift)
```

Agent behavior emerges from field dynamics —  
the same Kuramoto synchronization that runs the world  
also runs the agents inside it.

They are not programmed to walk around obstacles.  
They synchronize with the field and obstacles are gate rifts.

---

## MRT Overlay (Debug / Research Mode)

```
mrt_coherence   →  visible as color field beneath scene
mrt_drift       →  velocity vectors on surface
mrt_gate        →  glyph/form boundaries highlighted
mrt_events      →  flash at phase-zero crossings
```

In demo mode: overlays visible to show the field is real.  
In product mode: overlays hidden, field is invisible infrastructure.

---

## Typography in Scene

Ship name on hull. Navigation readouts. Radio frequency display.  
All rendered through `ToctetFontTables` — 5×7 matrix or 14-segment.  
Direct framebuffer output. No separate text renderer.  
Text breathes with the field at 24 frames.

---

## What the Demo Proves

| Claim | Demo evidence |
|---|---|
| Field produces emergent behavior | Agents navigate without scripts |
| Topology operators work | Vessel hull as gate rift creates ripples |
| Guard maintains stability | Field never collapses during demo |
| MRT observability works | Overlay shows field truth in real time |
| Typography is field-native | Text stable without separate renderer |
| 24 frames feels alive | Physical weight visible in movement |

---

## What the Demo Does Not Need

- High polygon count
- Texture art assets
- Pre-baked animations
- Scripted sequences
- Sound (optional, not required for proof)

The field is the content.

---

## Status

Concept defined. Implementation: future work.  
Target platform: bare metal Linux + `/dev/fb0`.  
No GPU required for proof-of-concept.

---

*TOCTET Kernel Architecture · Artem Hutvarev · 2026*
