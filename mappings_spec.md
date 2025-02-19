# Mappings Specification (Ladder Logic, Structured Text, and IR)

## Overview
The **Mappings System** defines the translation layer between industrial automation languages (**Ladder Logic and Structured Text**) and the **Intermediate Representation (IR)** used in Morley. This system ensures seamless bidirectional compilation between traditional automation programming languages and **Plutus smart contracts.**

## Architecture
Mappings are structured into three key categories:

1. **Instruction Set Mapping** (`instruction_set.json`)
   - Defines bitwise, arithmetic, logical, and comparison operations.
   - Converts standard automation commands into **IR representations.**

2. **Ladder Logic Mapping** (`ladder_logic.json`)
   - Maps **Ladder Logic elements** (contacts, coils, timers, counters) to **IR**.
   - Provides symbol definitions and execution behavior.

3. **Structured Text Mapping** (`structured_text.json`)
   - Maps **Structured Text functions** (logical operators, math functions, timers, counters) to **IR**.
   - Supports real-time execution in automation systems.

## Core Components

### 1. Instruction Set Mapping (`instruction_set.json`)
- **Defines all fundamental operations in Morley’s IR.**
- Covers bitwise (`AND_BIT`, `OR_BIT`), arithmetic (`ADD`, `SUB`), logical (`AND`, `OR`), comparison (`EQU`, `NEQ`), and movement operations (`MOV`, `SWAP`).
- Includes **timer and counter mappings** (`TON`, `TOF`, `CTU`, `CTD`).

### 2. Ladder Logic Mapping (`ladder_logic.json`)
- **Provides Ladder Logic-specific mappings.**
- Defines contacts (`XIC`, `XIO`), coils (`OTE`, `OTL`, `OTU`), timers (`TON`, `TOF`, `RTO`), and counters (`CTU`, `CTD`).
- Includes **comparison (`EQU`, `NEQ`, `LES`, `GRT`) and logical operations (`AND`, `OR`, `XOR`).**
- Encodes jump and subroutine handling (`JMP`, `LBL`, `JSR`, `RET`).

### 3. Structured Text Mapping (`structured_text.json`)
- **Defines mappings for Structured Text programming.**
- Covers logical operators (`AND`, `OR`, `XOR`), comparison (`=`, `<`, `>`), timers (`TON`, `TOF`), counters (`CTU`, `CTD`), and math functions (`SQRT`, `LOG`, `EXP`).
- Includes **bitwise operations (`SHL`, `SHR`, `ROR`, `ROL`) and selection functions (`SEL`, `MUX`, `MAX`, `MIN`).**

## Workflow
1. **Automation Code Input** → Written in **Ladder Logic** or **Structured Text**.
2. **Mapping System** → Translates operations into **IR format**.
3. **Compilation Pipeline** → Converts IR into **Plutus smart contracts**.
4. **Reverse Compilation** → Transforms **Plutus scripts back into Ladder Logic or Structured Text.**

## Features
- **Seamless Conversion:** Allows automation engineers to deploy industrial logic as blockchain-enforced contracts.
- **Industry Standard Compliance:** Supports **OpenPLC-compatible Ladder Logic and IEC 61131-3 Structured Text.**
- **Bidirectional Compilation:** Enables **Plutus-to-Ladder Logic and vice versa.**
- **Optimized Execution:** Maps instructions efficiently for **on-chain and off-chain execution.**

---
For more details, visit [morleylang.org](https://morleylang.org/) or check the source code in the [Morley-Labs GitHub](https://github.com/Morley-Labs).

