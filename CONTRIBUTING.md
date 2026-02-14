# CONTRIBUTING TO TOCTET

**Status:** Open Interfaces / Closed Kernel  
**Architecture:** Hutvarev Baremetal Protocol

Thank you for your interest in the Geometric Ontology. Before you commit, you must understand the **boundary** between the Public Interface and the Ontological Kernel.

## 1. THE "BLACK BOX" DOCTRINE
This repository contains the **SDK, Drivers, and Header Definitions** for the TOCTET environment.
It does **NOT** contain the compiled Kernel or the Source of Truth (The Resonance Parameters).

### What is Hidden (Trade Secrets):
The following components are strictly proprietary and reside in the private `TOCTET-Core` repository:
* **The Kuramoto Coupling Constants ($K$):** The exact values that allow phase synchronization.
* **The Pyramid Gradient Weights:** The mathematical constants defining the "Will" vector.
* **The Torus Metric Solver:** The exact implementation of `GetTorusDistanceSq`.

**DO NOT** submit Pull Requests that attempt to:
1.  "Guess" or hardcode these magic numbers in the public headers.
2.  Reverse-engineer the Kernel binary.
3.  Implement "mock" kernels that violate the Hutvarev Protocol specifications.

Such PRs will be closed, and the user may be banned from the organization.

## 2. CONTRIBUTOR LICENSE AGREEMENT (CLA)
By submitting code to this repository, you agree to the following:

1.  **Grant of Rights:** You grant Artem Hutvarev (The Architect) a perpetual, irrevocable, worldwide, royalty-free license to use, modify, and distribute your contribution as part of the TOCTET project (both public and private versions).
2.  **Originality:** You certify that the code is your original work or you have the right to submit it.
3.  **No Patents:** You waive any patent claims against the TOCTET project regarding your contribution.

## 3. HOW TO JOIN THE INNER CIRCLE
Access to the **Private Kernel Repository** (where the actual physics lives) is by **INVITATION ONLY**.

We invite contributors who:
* Write high-performance, baremetal-compatible C++20 drivers.
* Optimize the SDF rendering pipeline in the public SDK.
* Demonstrate a deep understanding of the Phase-Synchronous paradigm.

If you are invited to the Core, you will be required to sign a strict **Non-Disclosure Agreement (NDA)** protecting the Trade Secrets mentioned in Section 1.

## 4. CODING STYLE
* **Language:** C++20 Standard.
* **Format:** `Clang-Format` (Google Style).
* **Forbidden:** `std::shared_ptr` (bloat), RTTI, Exceptions.
* **Required:** Raw pointers, Templates, `constexpr`.
* **Philosophy:** If it allocates memory at runtime, justify it or delete it.

---

**"We do not need more code. We need more resonance."**
