# SPECIFICATION: S.Y.N.T.H (Series V)
**Designation:** Synthetic Vector Man (SVM-01)  
**Kernel:** TOCTET v1.0 Native  
**Architecture:** Bicameral / Tetra-Torus / Hardware-Abstracted  

> "The anthropomorphic form is a legacy constraint. The Synth is a modular vector execution unit."

---

## 1. Hardware Abstraction Layer (HAL)
The Synth is not an NPC or a software agent. It is defined as a **Virtual Hardware Unit** operating within the TOCTET substrate. It bypasses traditional OS layers to communicate directly with the Geometry Ring.

**Core Components:**
1.  **Chassis:** Procedural Inverse Kinematics (IK) rig driven by physics, not animation clips.
2.  **Sensors:** Ray-marching Lidar (SDF-based vision) instead of rasterized cameras.
3.  **Actuators:** Torque-based muscle simulation driven by the Physics Node (GPU 2).

## 2. The Bicameral Architecture (Brain Topology)
The control system mimics a biological hemisphere split, implemented on distributed GPU nodes via the **Ouroboros Bus**.

### Left Hemisphere (Control & Safety)
**Hardware Allocation:** GPU 1 (Geometry) + GPU 2 (Physics)  
**Function:** Hard Real-Time Control Loop (1000Hz).
* **Proprioception:** Real-time state estimation of limb position.
* **Collision Avoidance:** SDF-based spatial hashing.
* **Safety Protocols:** The "Asimov Governor" — a hardware-level limiter preventing unauthorized force application.

### Right Hemisphere (Planner & Resonance)
**Hardware Allocation:** GPU 3 (Sociology) + GPU 4 (Observer)  
**Function:** High-Level Cognitive Planner (Async).
* **Path Planning:** Navigation mesh generation via flow fields.
* **Social Weighting:** Evaluation of external entity intent (Friend/Foe/Obstacle).
* **Pattern Recognition:** Optical signature analysis.

---

## 3. Modular Deployment (Roles)
The Synth firmware supports hot-swappable "Personality Cartridges" (Kernel Modules) that define the operational vector:

* **TYPE-A (LOGISTICS):** Optimized for pathfinding and payload transport. High torque, low social latency.
* **TYPE-B (SECURITY):** High-frequency sensor polling. Locking target tracking.
* **TYPE-C (INTERFACE):** Maximum social resonance. Voice synthesis and gesture mirroring enabled.

## 4. Input/Output (The Mushroom Array)
External control is achieved via the **I2C Expansion Bus** (see main spec).
* **Remote Override:** The operator can inject direct torque commands via Rotary Encoders.
* **Telemetry:** The Synth broadcasts its internal state (Stress, Heat, Intent) via the Observer Node.

---

## 5. Neuromorphic Subsystems (Bio-Analogues)
To achieve autonomous survival, the Synth architecture implements hardware analogues of critical biological organs within the GPU Ring.

### 5.1 The Thalamic Gate (Input Multiplexer)
**Function:** Sensory Noise Filtering & Attention Routing.
**Location:** I2C Expansion Bus / STM32 Pre-processor.
* **Mechanism:** Before visual or tactile data reaches the Core (VRAM), it passes through the Thalamic Gate.
* **Role:** It ignores 90% of "static" data (background noise) and only forwards "events" (movement, heat, collision vectors) to the Right Hemisphere.
* **Result:** Drastic reduction in bandwidth consumption. The Synth only "sees" what matters.

### 5.2 The Amygdala Interrupt (Threat Heuristics)
**Function:** Latency-Critical Survival Reflexes.
**Location:** GPU 3 (High-Priority Thread).
* **Mechanism:** A hard-coded "Fight or Flight" loop that bypasses the main logic stack.
* **Trigger:** Sudden high-velocity objects in the SDF field or rapid thermal spikes.
* **Action:** Immediate torque override to actuators (Dodge/Brace) *before* the conscious planner realizes what happened.

### 5.3 The Cerebellar Loop (Motion Stabilizer)
**Function:** Fine Motor Control & Gyroscopic Balance.
**Location:** GPU 2 (Physics Node).
* **Mechanism:** Handles the "dirty math" of walking. The Conscious Brain says "Go there," and the Cerebellum calculates the thousands of micro-adjustments needed to keep the chassis upright.
* **Physics:** Real-time Inverse Kinematics (IK) solving at the hardware level.

### 5.4 The Hippocampal Buffer (Spatial Hash)
**Function:** Short-term Localization & Mapping (SLAM).
**Location:** VRAM Address Range 0x0000-0x1FFF (The Grid).
* **Mechanism:** Stores a rolling 3D buffer of the immediate environment.
* **Role:** Allows the Synth to navigate in total darkness by "remembering" the geometry it scanned 10 seconds ago.

> **CONFIDENTIALITY NOTICE:** This specification describes a proprietary robotic control architecture. Unauthorized replication of the Bicameral Topology is prohibited under the **Hippocratic License 3.0**.
