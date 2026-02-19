# Typography in Dynamic SDF Fields
**TOCTET Kernel Architecture · 2026**  
Author: Artem Hutvarev

---

## The Problem

Standard SDF text pipelines assume static glyph geometry.  
Dynamic fields are not static. This creates three failure modes:

**Aliasing** — scale changes destabilize SDF edges  
**Drift** — field modulation deforms glyph boundaries  
**Layout instability** — kerning and spacing break when geometry moves

The standard solution: fight the field with static overrides.  
The TOCTET solution: make text **part of the field**.

---

## The Insight

SDF wants static geometry.  
TOCTET has dynamic geometry.

Resolution: load all glyph logic into a single cube.  
Glyphs become **pattern injections** — runes, masks —  
stabilized by the same range policy as any other rift.

Text is not special. Text is a `gate` rift with a glyph mask.

---

## Implementation Model

### Glyph as Gate Rift

```
glyph_mask = SDF(character_form)
inject(gate, glyph_mask, position)
```

The glyph mask acts as a `gate` rift:  
field inside the glyph boundary behaves differently  
from field outside.

### Stabilization

Glyph layers have their own Guard instance:

```
guard(
  coherence_min = 0.864,
  coherence_max = 1.44,
  reset_policy  = normalize,
  scope         = glyph_layer_only
)
```

Edge stability is maintained by the same mechanism  
that stabilizes the entire field — not by a separate renderer.

### MRT Integration

Typography layer exposes dedicated MRT channels:

```
mrt_gate        →  glyph mask boundaries
mrt_coherence   →  edge stability map
mrt_events      →  glyph boundary crossings
```

This makes typography **measurable and debuggable**,  
not a visual layer that "looks about right".

---

## Reference Implementation

`ToctetFontTables` provides two tables:

**14-segment display** (`Segment14[128]`)  
- uint16 per character, ASCII 32–127  
- Bit mapping: A(top) B(top-right) ... M(bot-right diag)  
- Hardware display compatible

**5×7 dot matrix** (`Matrix5x7[128][5]`)  
- 5 bytes per character, ASCII 0–127  
- Column format, LSB = top pixel  
- Full upper and lower case

Direct framebuffer output via `/dev/fb0`:  
no GPU, no X11, no dependencies, no drivers.  
Text rendered at the filesystem level.

---

## Visual Behavior

At 24 frames — not smooth.  
Glyph edges are field boundaries.  
When the field jumps (micro-reset = frame boundary),  
the glyph edge jumps with it.

This is not a bug. This is **field-native typography**.  
Text breathes with the field.  
Not dead smooth pixels. Living state.

---

## Why This Matters

Typography is one of the hardest problems in dynamic SDF environments.  
Every major real-time SDF renderer has a "font pain" section.

TOCTET resolves it architecturally:  
by refusing to treat text as a special case,  
and instead making it a first-class field citizen.

The cube contains everything — including the alphabet.

---

*TOCTET Kernel Architecture · Artem Hutvarev · 2026*
