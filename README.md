# Morley Component Specifications

This repository serves as a reference hub for detailed specifications of various Morley components. It is intended as a personal guide while developing and refining different aspects of the Morley ecosystem.

## Structure
Each component will have its own dedicated markdown file outlining its specifications, functionality, and technical details. These specifications will evolve as development progresses.

## Current Specifications
- **[Morley Time Logic Specification](https://github.com/Morley-Labs/Morley-Component-Specifications/blob/main/hybrid_time_logic_spec.md)** – Defines the hybrid 2-layer methodology for handling forward and reverse time logic across the Cardano L1 and other execution environments with Morley scripts.

## Goal
Support real-time execution (Hydra/Midgard/Sidechains) and slot-based enforcement (Cardano L1) while ensuring Plutus scripts can be compiled and reverse compiled into Ladder Logic.

## Compiler (Ladder Logic → Plutus) Features
- Convert Ladder Logic Timers into Plutus Constraints
- Support Hydra/Midgard execution without L1 slot constraints
- Encode timestamps into datums when storing state on-chain

## Usage
This repository is primarily for internal reference and iterative documentation. Updates and additions will be made as components are refined.

## Future Additions
Planned specifications for additional components will be added as needed to support the ongoing development of Morley.

---

For more information about the Morley ecosystem, visit [morleylang.org](https://morleylang.org/).

