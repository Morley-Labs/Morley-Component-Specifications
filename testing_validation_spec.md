# Testing & Validation Specification

## Overview
The **Morley Testing & Validation Framework** ensures the correctness, consistency, and integrity of **Ladder Logic, Structured Text, and Plutus IR transformations**. It verifies instruction mappings, compiler outputs, and reverse compilation functionality.

## Testing Structure
The framework consists of **unit tests** for the following components:

1. **Instruction Set Validation** (`test_instruction_set.py`)
   - Ensures **instruction mappings** exist and conform to OpenPLC standards.
   - Verifies the presence of **Intermediate Representation (IR) mappings**.
   - Prevents **duplicate IR assignments**.

2. **Ladder Logic Validation** (`test_ladder_logic.py`)
   - Confirms that **Ladder Logic instructions** (contacts, coils, timers) map correctly to IR.
   - Checks for missing or duplicate **IR representations**.

3. **PlutusLadder Compiler Testing** (`test_plutusladder_compiler.py`)
   - Validates **IR-to-Plutus Haskell transformation**.
   - Ensures correct handling of **logical conditions, timers, and counters**.
   - Checks arithmetic and comparison operations.

4. **Reverse Compiler Testing** (`test_reverse_compiler.py`)
   - Verifies **Plutus-to-Ladder Logic translation**.
   - Confirms **traceIfFalse** conditions are mapped correctly.
   - Ensures correct handling of **state updates, arithmetic, and control flow**.

5. **Structured Text Validation** (`test_structured_text.py`)
   - Ensures **Structured Text instructions** (logical, comparison, timers) are correctly mapped.
   - Prevents missing mappings or IR duplicates.

## Testing Workflow
1. **Load Mappings** → Fetch instruction mappings from `mappings/`.
2. **Run Unit Tests** → Execute individual test cases via `unittest`.
3. **Verify Outputs** → Ensure expected mappings, no missing IR, and correct logic flow.
4. **Identify & Fix Errors** → If tests fail, debug mapping inconsistencies or compiler logic.

## Features
- **Automated Verification:** Ensures correctness across all components.
- **Comprehensive Testing:** Covers **IR, compilation, and reverse compilation**.
- **Ensures OpenPLC Compatibility:** Guarantees adherence to industry-standard automation logic.
- **Error Prevention:** Identifies **duplicate mappings, incorrect translations, and missing IR elements**.

---
For more details, visit [morleylang.org](https://morleylang.org/) or check the test suite in the [Morley-Labs GitHub](https://github.com/Morley-Labs).

