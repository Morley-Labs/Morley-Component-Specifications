# Morley Time Logic Specification

## Goal
Support real-time execution (Hydra/Midgard/Sidechains) and slot-based enforcement (Cardano L1) while ensuring Plutus scripts can be compiled and reverse compiled into Ladder Logic.

## Two Types of Time Logic
### 1️. Execution-Based Time Logic (Layer 2 - Hydra/Midgard)
- Timers execute off-chain in a high-speed environment.
- Transactions do not wait for Cardano slot validation.
- Timestamp is recorded in a Plutus datum using:
  ```json
  {
      "format": "verifiable",  // "basic", "structured", or "verifiable"
      "timestamp": 1700000000,
      "machine_id": "MORLEY-PLC-001",
      "process_id": "LADDER-SEQ-12",
      "hash": "d5f1a8b0e...7c3e1"  // Only in "verifiable" mode
  }
  ```
- Works well for machine automation, SCADA, PLCs, and instant transactions.
- Not final until it is anchored to Layer 1.

### 2️. Slot-Based Time Logic (Layer 1 - Cardano)
- Transactions are validated within a specific slot range.
- Uses mustValidateIn to enforce slot constraints.
- Timestamped proofs can be anchored on-chain.
- Ensures finality and trustless verification.
- Required for critical state changes or financial settlement.

### **Hybrid Time Flow**
- Layer 2 timestamps are generated and stored in a Plutus datum.
- Later, the timestamp is committed to Layer 1 for validation.
- On L1, the timestamp is checked against slot constraints (`mustValidateIn`).

## Compiler (Ladder Logic → Plutus)
### Features:
- Convert Ladder Logic Timers into Plutus Constraints.
- Support Hydra/Midgard execution without L1 slot constraints.
- Encode timestamps into datums when storing state on-chain.

### Example Transformation:
Ladder Logic Input:
```
TON Timer1, 5000ms
MOV state = state + 1
```
Compiled to Plutus:
```haskell
mustValidateIn (from slotX)
recordedTime `member` validTimeRange
```
If executing in Hydra/Midgard:
The compiler will *skip slot enforcement and instead encode time into a separate structure.*

## Reverse Compiler (Plutus → Ladder Logic)
### Features:
- Extract time logic from Plutus scripts & convert back to Ladder Logic.
- Interpret datums storing timestamps & state data.
- Recognize real-time Hydra execution & adjust output accordingly.

### Example Transformation:
Plutus Input:
```haskell
mustValidateIn (from slotX)
```
Reverse Compiled into Ladder Logic:
```
TON Timer1, 5000ms
```
Plutus Datum Input:
```json
{"timestamp": 1700000000}
```
Reverse Compiled into Ladder Logic:
```
MOV timestamp = 1700000000
```

## Anchoring Mechanism (Connecting Off-Chain to L1)
### Features:
- Hydra transactions store timestamps off-chain and verify them locally.
- When needed, timestamps are committed to an L1 smart contract.
- L1 validates that transactions respect original off-chain timing rules.

### Example Flow:
1. Hydra executes transaction instantly, timestamp recorded off-chain.
2. When final settlement is needed, Hydra submits timestamp proof to L1.
3. L1 contract verifies timestamp against expected constraints before allowing settlement.

## ToDo
### Development Priorities:
- Extend the Morley Compiler to handle Plutus time constraints.
- Extend the Morley Reverse Compiler to extract and interpret timestamps.
- Define a standard format for encoding timestamps in datums for real-time execution.
- Build the anchoring mechanism to connect Hydra/Midgard time logic to L1 slot constraints.
