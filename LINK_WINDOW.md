# LINK_WINDOW
**External Boundary Protocol · TOCTET Kernel Architecture · 2026**  
Author: Artem Hutvarev

---

## Premise

TOCTET is a closed field system.  
The legacy internet is an open, unverified, scheduler-based network.

These are **diametrically opposed systems**.

Direct connection between them is not an architectural option.  
It is a security collapse.

The Link Window is the explicit, controlled gate between them.

---

## Architecture

```
[ legacy internet ]
        |
        ▼
┌───────────────────┐
│   LINK_WINDOW     │  ← boundary node (bare metal)
│                   │
│  TLS termination  │
│  AVD handshake    │
│  rate control     │
│  logging (SD)     │
│  watchdog         │
└───────────────────┘
        |
        ▼ (internal protocol only)
[ TOCTET field network ]
  COM / RS-485 / internal Ethernet
  no raw internet packets inside
```

The legacy internet does **not** enter the lab.  
Only verified, translated, rate-controlled data crosses the boundary.

---

## Link Window Node Properties

- Bare metal (no OS complexity to exploit)
- Single function: boundary translation
- Accepts: HTTPS from outside
- Emits: internal protocol packets only
- Logs: all boundary events to local SD
- Watchdog: resets on anomaly, not on command

---

## Boundary Handshake

Entry into TOCTET network requires AVD resonance verification:

```
1. External client presents Vector_Identity
2. Link Window forwards to AVD node
3. AVD: Phase_Identity.VerifyResonance
4. Golden Window check: 0.864 < interference < 1.44
5. PASS → packet translated and forwarded
   FAIL → Shooter trap, connection dropped, logged
```

Unverified traffic does not reach the field.  
It does not reach any internal node.  
It terminates at the boundary.

---

## Data Direction

```
INBOUND   external → Link Window → verified packet → TOCTET node
OUTBOUND  TOCTET node → Link Window → sanitized response → external
```

The Link Window is **not** a router.  
It is a **translator** between two incompatible network philosophies.

---

## Implementation Reference

Hardware: IEI 2660 (Celeron 600MHz, bare metal)  
Interface out: HTTPS (wolfSSL / mbedTLS)  
Interface in: RS-485 / COM / internal Ethernet  
Storage: CF card (boot) + SD (event log)  
Watchdog: hardware, independent of software state

---

## Why This Is a Separate Document

The boundary is not a feature of TOCTET.  
It is a **precondition** for TOCTET to remain what it is.

A field system that accepts arbitrary external packets  
is no longer a field system.  
It is legacy infrastructure with extra steps.

---

*TOCTET Kernel Architecture · Artem Hutvarev · 2026*
