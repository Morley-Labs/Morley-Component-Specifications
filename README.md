# Morley Component Specifications

This repository serves as a reference hub for detailed specifications of various Morley components. It is intended as a personal guide while developing and refining different aspects of the Morley ecosystem.

## Structure
Each component has its own dedicated markdown file outlining its specifications, functionality, and technical details. These specifications will evolve as development progresses.

## Current Specifications

### **ArkWriter & Related Components**
- **[ArkWriter General Specification](https://github.com/Morley-Labs/Morley-Component-Specifications/blob/main/ArkWriter_general_spec.md)** – High-level overview of ArkWriter’s functionality and purpose.
- **[ArkWriter Plugin System](https://github.com/Morley-Labs/Morley-Component-Specifications/blob/main/ArkWriter_plugin_system_basic.md)** – Initial architecture for extending ArkWriter with custom plugins.
- **[ArkTracer Debugging Specification](https://github.com/Morley-Labs/Morley-Component-Specifications/blob/main/ArkTracer_spec.md)** – Defines ArkTracer’s role as a debugging tool for Morley-Plutus contracts.
- **[ArkRedeem UTXO Migration System](https://github.com/Morley-Labs/Morley-Component-Specifications/blob/main/ArkRedeem.md)** – Describes how ArkWriter handles UTXO migrations for smart contract upgrades.
- **[ArkHunter Simulation Specification](https://github.com/Morley-Labs/Morley-Component-Specifications/blob/main/ArkHunter_spec)** – Defines how ArkHunter models and evaluates Cardano network parameter changes.

### **Compiler & Execution Models**
- **[Ladder Logic to Plutus Compiler](https://github.com/Morley-Labs/Morley-Component-Specifications/blob/main/ladder_logic_to_Plutus_compiler_spec.md)** – Details the compiler pipeline that translates Ladder Logic into Plutus smart contracts.
- **[Reverse Compiler](https://github.com/Morley-Labs/Morley-Component-Specifications/blob/main/reverse_compiler_spec.md)** – Specifies how Plutus scripts can be converted back into Ladder Logic.
- **[Mappings Specification](https://github.com/Morley-Labs/Morley-Component-Specifications/blob/main/mappings_spec.md)** – Defines instruction mappings for Ladder Logic, Structured Text, and IR.
- **[Testing & Validation](https://github.com/Morley-Labs/Morley-Component-Specifications/blob/main/testing_validation_spec.md)** – Covers test cases and validation frameworks for Morley execution models.
- **[Morley Time Logic Specification](https://github.com/Morley-Labs/Morley-Component-Specifications/blob/main/hybrid_time_logic_spec.md)** – Defines the hybrid 2-layer methodology for handling forward and reverse time logic across L1 and other execution environments.

### **API & Infrastructure**
- **[Morley API Plan](https://github.com/Morley-Labs/Morley-Component-Specifications/blob/main/API_plan.md)** – High-level outline of potential Morley API endpoints and usage scenarios.

### **Multi-Environment Support**
- **[Morley-Plutus Dual-Script Architecture](https://github.com/Morley-Labs/Morley-Component-Specifications/blob/main/Morley-Plutus_Dual-Script_Architecture_spec.md)** – Defines how Morley-Plutus contracts interact across L1 and sidechains, ensuring adaptability.
- **[Morley Bridge Specification](https://github.com/Morley-Labs/Morley-Component-Specifications/blob/main/Morley_Bridge_spec.md)** – Details the bridging mechanism for Morley execution between L1, Hydra, and sidechains.
- **[Morley-Hydra Execution Specification](https://github.com/Morley-Labs/Morley-Component-Specifications/blob/main/Morley-Hydra_spec.md)** – Defines how Morley contracts interact with Hydra for high-speed execution.
- **[Morley-Midgard Sidechain Specification](https://github.com/Morley-Labs/Morley-Component-Specifications/blob/main/Morley_Midgard_prelim_spec.md)** – Preliminary outline for deploying Morley contracts on a Midgard-based sidechain.
- **[Morley Execution Simulator](https://github.com/Morley-Labs/Morley-Component-Specifications/blob/main/Morley_Execution_Sim_spec.md)** – A specification for simulating Morley-Plutus execution to test contract logic and performance.

### **Website Specification (Placeholder)**
- **[Morley Website Specification](https://github.com/Morley-Labs/Morley-Component-Specifications/blob/main/Morley_Website_spec.md)** – Documentation for managing, updating, and improving the Morley website.

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

For more information about the Morley ecosystem, visit [morleylang.org](https://morleylang.org/).

