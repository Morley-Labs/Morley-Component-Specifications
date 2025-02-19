# Reverse Compiler Specification (Plutus to Ladder Logic)

## Overview
The **Morley Reverse Compiler** translates **Plutus smart contracts** back into **Ladder Logic**, enabling industrial automation engineers to verify and audit Plutus-based control logic in a familiar format. The tool supports a **wide range of Ladder Logic constructs**, ensuring full compatibility with **real-world PLC programming.**

## Architecture
The reverse compilation process consists of three key components:

1. **Plutus Script Parsing**
   - Extracts logical conditions, state updates, arithmetic operations, bitwise operations, and control flow instructions from Morley-specific Plutus scripts.

2. **Intermediate Representation (IR) Extraction**
   - Maps extracted Plutus logic to **LadderCore IR**, ensuring correct instruction mappings.
   - Validates conditions before conversion.

3. **Ladder Logic Code Generation**
   - Converts IR-based conditions and operations into fully formatted **Ladder Logic (.ll) files**.
   - Supports **timers, counters, bitwise operations, and control flow mechanisms.**

## Core Components

### 1. Reverse Compiler (`reverse_compiler.py`)
- Parses Plutus scripts, extracting:
  - **Logical conditions** (`traceIfFalse`, boolean expressions)
  - **Timers & counters** (e.g., TON, TOF conversions)
  - **Arithmetic & state updates** (MOV, ADD, SUB operations)
  - **Bitwise operations** (SHL, SHR, etc.)
  - **Control flow** (JMP, LBL, MCR handling)
- Converts extracted logic into **Ladder Logic IR**.

### 2. Utility Functions (`utils.py`)
- **save_to_file**: Saves generated Ladder Logic into `.ll` files.
- **load_plutus_script**: Loads Morley-specific Plutus scripts from files.

### 3. Initialization (`__init__.py`)
- Exposes the `reverse_compile_plutus_to_ll` function for external use.

## Reverse Compilation Workflow
1. **Plutus Script Input** → Passed to `reverse_compiler.py`.
2. **Parsing & Condition Extraction** → Extract logic components.
3. **Intermediate Representation (IR) Formation** → Validate and structure mappings.
4. **Ladder Logic Generation** → Convert IR into `.ll` format.

## Features
- **Full Ladder Logic Support:** Supports standard PLC instructions.
- **Bidirectional Compatibility:** Enables verification of Plutus smart contracts in Ladder Logic.
- **Timer & Counter Handling:** Converts timing constraints into PLC-executable formats.
- **Control Flow Reconstruction:** Maintains logical flow from Plutus to Ladder Logic.
- **Expandable Design:** Supports additional instruction mappings and optimizations.

---
For more details, visit [morleylang.org](https://morleylang.org/) or check the source code in the [Morley-Labs GitHub](https://github.com/Morley-Labs).

