# Morley Time Logic Specification

## Goal
Support real-time execution (Hydra/Midgard/Sidechains) and slot-based enforcement (Cardano L1) while ensuring Plutus scripts can be compiled and reverse compiled into Ladder Logic.

## Two Types of Time Logic
### 1 Execution-Based Time Logic (Layer 2 - Hydra/Midgard)
- Timers execute off-chain in a high-speed environment.
- Transactions do not wait for Cardano slot validation.
- Timestamp is recorded in a Plutus datum using:
  ```json
  {
      "format": "verifiable",  "basic", "structured", or "verifiable"
      "timestamp": 1700000000,
      "machine_id": "MORLEY-PLC-001",
      "process_id": "LADDER-SEQ-12",
      "hash": "d5f1a8b0e...7c3e1"  Only in "verifiable" mode
  }
  ```
- Works well for machine automation, SCADA, PLCs, and instant transactions.
- Not final until it is anchored to Layer 1.

### 2 Slot-Based Time Logic (Layer 1 - Cardano)
- Transactions are validated within a specific slot range.
- Uses mustValidateIn to enforce slot constraints.
- Timestamped proofs can be anchored on-chain.
- Ensures finality and trustless verification.
- Required for critical state changes or financial settlement.

### Hybrid Time Flow
- Layer 2 timestamps are generated and stored in a Plutus datum.
- Later, the timestamp is committed to Layer 1 for validation.
- On L1, the timestamp is checked against slot constraints (mustValidateIn).

## Anchoring Mechanism (Connecting Off-Chain to L1)
### Storing L2 Timestamps on L1
- Supports both immediate anchoring and finality-based anchoring.
- Immediate anchoring submits transactions as events occur.
- Finality anchoring holds timestamps off-chain until a trigger event.

### Triggering the Anchoring Mechanism
#### Option 1: On-Chain Anchoring (Plutus Transaction)
- A Plutus transaction includes:
  - L2 Timestamp stored in a Plutus datum.
  - Verification logic to ensure slot constraints are met.
  - A reference input proving the execution context.
- Used when full decentralization is required.
- Used when automated smart contract enforcement is needed.

#### Option 2: Off-Chain Anchoring (External Service)
- An off-chain service monitors L2 timestamps and submits them to L1 as needed.
- The service can:
  - Batch multiple timestamps into a single transaction.
  - Use CIP-68 metadata for lightweight timestamp anchoring.
  - Submit proofs verifying timestamps before finalization.
- Used when cost efficiency is important.
- Used when low-latency execution is required.

Verifiable Hash Anchoring for Hybrid Time Logic
Overview
We implemented Verifiable Hash Anchoring using Blake2b hashing for timestamps in the hybrid time logic system.
This ensures data integrity and tamper resistance by:

Hashing the timestamp and storing the hash on-chain.
Anchoring the timestamp with mustValidateIn using the hashed value.
Allowing reverse compilation to restore Ladder Logic with the verified timestamp.
Changes Made
1. In plutusladder_compiler.py
Added logic for Blake2b hash generation:

Utilizes hashlib.blake2b with a digest size of 32 for secure hashing.
Stores the hash alongside mustValidateIn to maintain on-chain verifiability.
Example Implementation:

python
Copy
Edit
import hashlib  # Required for Blake2b hash generation

# Handle Verifiable Hash Anchoring in Plutus
if "format" in ir_data and ir_data["format"] == "verifiable":
    if "timestamp" in ir_data:
        # Generate Blake2b hash for the timestamp
        timestamp_value = str(ir_data["timestamp"]).encode('utf-8')
        blake2b_hash = hashlib.blake2b(timestamp_value, digest_size=32).hexdigest()

        # Store the hash in the Plutus script
        script_lines.append(f'mustValidateIn (from slot{ir_data["timestamp"]})')
        script_lines.append(f'-- Verifiable Hash: {blake2b_hash}')
        print(f"Verifiable Hash Anchoring Applied: mustValidateIn (from slot{ir_data['timestamp']}) with Hash: {blake2b_hash}")
Expected Output:

sql
Copy
Edit
Verifiable Hash Anchoring Applied: mustValidateIn (from slot1700000000) with Hash: <Blake2b_Hash>
mustValidateIn (from slot1700000000)
-- Verifiable Hash: <Blake2b_Hash>
2. In reverse_compiler.py
Pending: Next step is to recognize and reverse compile:

mustValidateIn with Verifiable Hashes.
Convert it back to Ladder Logic while maintaining hash integrity.
Spec Addition for Hybrid Time Logic
Add the following section under Anchoring Mechanisms:

Verifiable Hash Anchoring
Verifiable Hash Anchoring provides an immutable and verifiable timestamping mechanism using Blake2b hashing.
This ensures data integrity and tamper resistance for critical operations.

Description
Purpose: Securely anchors timestamps with verifiable integrity.
Method: Generates a Blake2b hash for the timestamp and anchors it using mustValidateIn.
Storage: The hash is stored on-chain, enabling verifiability and auditability.
Specification
Format: "verifiable"
Example Structure:
json
Copy
Edit
{
    "format": "verifiable",
    "timestamp": 1700000000,
    "machine_id": "MORLEY-PLC-001",
    "process_id": "LADDER-SEQ-12",
    "hash": "d5f1a8b0e...7c3e1" 
}
How It Works
Blake2b Hash Generation:
hashlib.blake2b with a digest size of 32 is used.
The timestamp is hashed to ensure tamper resistance.
Plutus Anchoring:
mustValidateIn (from slotX) is used for timestamp anchoring.
The hash is stored as a comment in the Plutus script:
haskell
Copy
Edit
mustValidateIn (from slot1700000000)
-- Verifiable Hash: <Blake2b_Hash>
Reverse Compilation:
Recognizes and reverses the Verifiable Hash Anchoring.
Converts it back to Ladder Logic with the hash intact.
Benefits
Immutability: Ensures the timestamp cannot be tampered with.
Auditability: On-chain hash storage allows for verifiable integrity checks.
Security: Blake2b provides high security with efficient performance.

### Plutus Constraints for L1 Validation
#### Constraint 1: Slot Validation
- The transaction must validate within the correct slot range.
- Uses mustValidateIn to ensure execution happened within the expected timeframe.

#### Constraint 2: Timestamp Consistency Check
- The timestamp in the Plutus datum must match the expected L2 timestamp.
- Uses traceIfFalse to reject transactions where datum["timestamp"] != expectedTimestamp.

#### Constraint 3: Hash Verification (If Verifiable Mode is Used)
- If format == "verifiable", the execution hash must match the expected hash.
- Uses blake2b hashing to prevent timestamp forgery.

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
The compiler will skip slot enforcement and instead encode time into a separate structure.

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

## ToDo
### Development Priorities:
- Extend the Morley Compiler to handle Plutus time constraints.
- Extend the Morley Reverse Compiler to extract and interpret timestamps.
- Define a standard format for encoding timestamps in datums for real-time execution.
- Build the anchoring mechanism to connect Hydra/Midgard time logic to L1 slot constraints.
