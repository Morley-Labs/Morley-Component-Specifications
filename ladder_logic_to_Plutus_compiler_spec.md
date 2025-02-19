# Ladder Logic to Plutus Compiler Specification

## Overview
The **Ladder Logic to Plutus Compiler (PlutusLadder Compiler - PLC)** is designed to transform **Ladder Logic** programs into **Plutus smart contracts**. This enables deterministic and verifiable execution of industrial automation logic on the **Cardano blockchain** while supporting both **real-time execution** (Hydra/Midgard/Sidechains) and **slot-based enforcement** (Cardano L1).

## Architecture
The compilation process consists of three key stages:

1. **Ladder Logic Parsing (LL-Parser)**
   - Converts raw Ladder Logic into an **Intermediate Representation (IR)** called **LadderCore IR**.
   - Supports OpenPLC components, including timers, counters, function blocks, and logical operations.
   
2. **Validator-Based IR Transformation**
   - Ensures the IR maintains structural integrity and contains required elements before compilation.
   - Validates the presence of essential Ladder Logic constructs.

3. **PlutusLadder Compiler**
   - Converts **LadderCore IR** into a structured **Plutus Haskell** script.
   - Implements **validation logic** to enforce conditions, timers, and counters using **traceIfFalse**.
   - Produces **Plutus Core (PLC)** output for execution on the blockchain.

## Core Components

### 1. LL-Parser (`ll_parser.py`)
- Parses Ladder Logic code into an **Intermediate Representation (IR)**.
- Captures logical operations (`AND`, `OR`, `NOT`), timers (`TON`, `TOF`), counters (`CTU`, `CTD`), math operations, comparators, and jump instructions.
- Outputs **structured IR JSON** format.

### 2. Validator IR Transform (`validator_ir_transform.py`)
- Ensures **IR structural correctness** before compilation.
- Verifies required elements (`instructions`, `timers`, `counters`, etc.) are present.
- Prevents compilation errors due to missing components.

### 3. PlutusLadder Compiler (`plutusladder_compiler.py`)
- Converts **LadderCore IR** into **Plutus Haskell** and **Plutus Core (PLC)**.
- Implements **logical constraints** using `traceIfFalse`.
- Supports conversion of Ladder Logic timers into **Plutus validation constraints**.
- Generates **structured Plutus scripts** for execution.

## Compilation Workflow
1. **Ladder Logic Input** → Passed to `LL-Parser`.
2. **Intermediate Representation (IR)** → Validated by `Validator IR Transform`.
3. **Plutus Haskell Generation** → Transformed by `PlutusLadder Compiler`.
4. **Plutus Core Compilation** → Generates executable **Plutus smart contract**.

## Features
- **Complete Ladder Logic Support:** OpenPLC-compatible components.
- **Smart Contract Execution:** Converts automation logic into blockchain-enforceable constraints.
- **Real-Time & Slot-Based Execution:** Supports Hydra/Midgard for real-time, and Cardano L1 for slot-based constraints.
- **Validation & Error Handling:** Ensures IR correctness before Plutus compilation.
- **Expandable Design:** Future support for additional automation logic formats.

---
For more details, visit [morleylang.org](https://morleylang.org/) or check the source code in the [Morley-Labs GitHub](https://github.com/Morley-Labs).

