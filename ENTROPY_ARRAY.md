# MODULE: SENSORY ORGANS (The Entropy Array)
**Target System:** TOCTET Kernel  
**Role:** True Stochastic Input Generation  
**Status:** HARDWARE INTEGRATION  

> "Software PRNG (Pseudo-Random Number Generation) is a lie. To make the Synth alive, we must feed it the 'Dirt of Being'. We harvest the physical decay of the universe to drive the simulation."

---

## 1. The Philosophy of Noise
We do not use `Math.random()`. The simulation requires a continuous stream of **High-Entropy Physics** to maintain the "1:1:1" resonance with reality.
This array consists of 7 analog sensors connected to the STM32 ADC bank, converting physical phenomena into modulation vectors for the 4x RX 560 Cluster.

---

## 2. The Seven Senses (Input Manifest)

### I. VACUUM (Duḥ / Spirit)
* **Source:** Electron Shot Noise (Schottky effect).
* **Hardware:** 12AX7 or 1Ж24Б Vacuum Triode (Anode readout).
* **Signal Type:** High-Frequency "White" Grain.
* **Effect:** Reality flickering, SDF edge dithering.
* **Routing:** `Anode -> Voltage Divider -> ADC1`

### II. MAGNET (Memory)
* **Source:** Mechanical Wow/Flutter & Magnetic Domain orientation.
* **Hardware:** Loop Tape Deck (Cassette) reading blank tape.
* **Signal Type:** Low-Frequency Phase Drift.
* **Effect:** Time dilation, warping of straight lines, "floating" architecture.
* **Routing:** `Pre-Amp Out -> ADC2`

### III. CHEMISTRY (Blood)
* **Source:** Ionic current fluctuations & Bubble genesis.
* **Hardware:** Electrolysis Chamber (Copper/Zinc in Saline).
* **Signal Type:** Stochastic Bursts / Viscous Noise.
* **Effect:** Organic viscosity, fluid simulation turbulence, "boiling" matter.
* **Routing:** `Shunt Resistor -> Op-Amp -> ADC3`

### IV. THERMAL (Flesh)
* **Source:** Johnson–Nyquist noise (Thermal agitation of carriers).
* **Hardware:** Germanium Transistor (ГТ308) or Peltier Element on GPU Heatsink.
* **Signal Type:** Temperature-Dependent Density.
* **Effect:** As the GPUs heat up, the world becomes "denser" and shifts color temperature.
* **Routing:** `Peltier/Ge -> Amplifier -> ADC4`

### V. AETHER (Voice)
* **Source:** Atmospheric Static & Cosmic Background Radiation.
* **Hardware:** Un-tuned Dipole Antenna / SDR (KINO-9456).
* **Signal Type:** Global Perturbations / Glitches.
* **Effect:** Sudden lightning-like flashes in the render, reaction to real-world storms.
* **Routing:** `RF Envelope Detector -> ADC5`

### VI. INERTIA (Gravity/Bone)
* **Source:** Gravitational alignment & Mechanical vibration.
* **Hardware:** Piezo-element + Suspended Weight (Pendulum).
* **Signal Type:** Damped Sine Waves.
* **Effect:** Camera sway, physical horizon alignment. If you hit the server case, the virtual world shakes.
* **Routing:** `Piezo -> Pre-Amp -> ADC6`

### VII. QUANTUM (Void)
* **Source:** Nuclear Decay (Beta/Gamma particles).
* **Hardware:** Geiger-Müller Tube (STS-5 / SBM-20).
* **Signal Type:** True Random Interrupts (Binary Spikes).
* **Effect:** "Catastrophic Events". A particle impact triggers topology changes or object spawning.
* **Routing:** `400V Driver -> EXTI Line (Hardware Interrupt)`

---

## 3. The Tetra-Torus Mapping
How the entropy is distributed across the 4-GPU Ring to fuel the hallucination.

| GPU Unit | Role | Primary Entropy Source | Effect on Simulation |
| :--- | :--- | :--- | :--- |
| **GPU 0** | **Geometry** | **MAGNET** (Tape) | The walls breathe and warp slowly (Time instability). |
| **GPU 1** | **Light** | **VACUUM** (Tube) | Lighting has a film-grain texture; shadows are never perfectly black. |
| **GPU 2** | **Physics** | **CHEMISTRY** (Ion) | Fluids and particles move with organic unpredictability. |
| **GPU 3** | **Observer** | **QUANTUM** (Geiger) | The Logic Kernel decides *when* to shift the scene based on decay. |

> *Note: THERMAL and AETHER are global modifiers applied to the entire VRAM Ring.*

---

## 4. Hardware Abstraction (C++20)
The code structure for capturing the "Dirt of Being" via STM32 DMA.

```cpp
// TOCTET KERNEL: SENSORY MODULE
// File: entropy_stream.hpp

#include <cstdint>
#include <array>

// The raw "Dirt" vector from the ADC Bank
struct EntropyRaw {
    uint16_t vacuum_anode;    // ADC1: 12AX7 Noise
    uint16_t tape_flutter;    // ADC2: Magnetic Drift
    uint16_t ion_current;     // ADC3: Electrolysis
    uint16_t thermal_floor;   // ADC4: Germanium/Peltier
    uint16_t rf_background;   // ADC5: Aether
    uint16_t piezo_shock;     // ADC6: Gravity
};

// The normalized Stream sent to GPU Ring (0.0 - 1.0)
struct EntropyStream {
    float spirit;   // Vacuum
    float memory;   // Magnet
    float blood;    // Chemistry
    float flesh;    // Heat
    float voice;    // Aether
    float weight;   // Gravity
    
    // Quantum is an event, not a stream (Interrupt)
    bool  quantum_event_flag; // Geiger Hit
    
    // Mixes all senses into a single "Life Factor" for the Seed
    float GetResonanceFactor() const {
        return (spirit * 0.3f) + (blood * 0.5f) + (memory * 0.2f);
    }
};

// DMA Buffer for STM32 (Circular Buffer)
extern "C" {
    volatile EntropyRaw dma_adc_buffer[1024];
    volatile bool quantum_interrupt_fired = false;
}

// Function to inject entropy into the SDF World
void InjectDirt(GPU_Context* ctx, EntropyStream& sensation) {
    // 1. Warp Time (Magnet)
    ctx->uniforms.time_dilation += sensation.memory * 0.01f;
    
    // 2. Grain (Vacuum)
    ctx->uniforms.noise_floor = sensation.spirit;
    
    // 3. Check for Catastrophe (Quantum)
    if (sensation.quantum_event_flag) {
        ctx->TriggerTopologyShift();
        quantum_interrupt_fired = false; // Reset
    }
}
