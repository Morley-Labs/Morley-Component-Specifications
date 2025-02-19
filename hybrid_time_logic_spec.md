# Morley Time Logic Specification

## Goal
The objective is to support both real-time execution (Hydra/Midgard/Sidechains) and slot-based enforcement (Cardano L1) while ensuring that Plutus scripts can be compiled and reverse-compiled into Ladder Logic. This approach provides flexibility in timing mechanisms, enabling both high-speed off-chain execution and reliable on-chain finality.

---

## Two Types of Time Logic  

### 1. Execution-Based Time Logic (Layer 2 - Hydra/Midgard)  
- Timers execute off-chain in a high-speed environment, allowing for rapid and real-time processing.
- Transactions are not delayed by Cardano slot validation, enabling low-latency interactions.
- Timestamps are recorded in a Plutus datum using the following structure:  
    ```json
    {
        "format": "verifiable" | "basic" | "structured",
        "timestamp": 1700000000,
        "machine_id": "MORLEY-PLC-001",
        "process_id": "LADDER-SEQ-12",
        "hash": "d5f1a8b0e...7c3e1"
    }
    ```
- Suitable for machine automation, SCADA, PLCs, and instant transactions.
- Transactions are not final until anchored to Layer 1.

### 2. Slot-Based Time Logic (Layer 1 - Cardano)  
- Transactions are validated within a specific slot range using `mustValidateIn`.
- Timestamps can be anchored on-chain for finality and trustless verification.
- Ensures deterministic finality required for critical state changes or financial settlements.

---

## Hybrid Time Flow
- Layer 2 timestamps are generated and stored in a Plutus datum.
- Later, the timestamp is committed to Layer 1 for validation.
- On L1, the timestamp is checked against slot constraints using `mustValidateIn`.

---

## Anchoring Mechanism (Connecting Off-Chain to L1)
### Storing L2 Timestamps on L1  
- Supports both immediate and finality-based anchoring.
  - **Immediate Anchoring:** Submits transactions as events occur.
  - **Finality Anchoring:** Holds timestamps off-chain until a trigger event.

### Triggering the Anchoring Mechanism
#### Option 1: On-Chain Anchoring (Plutus Transaction)  
- A Plutus transaction includes:
  - L2 Timestamp stored in a Plutus datum.
  - Verification logic to ensure slot constraints are met.
  - A reference input proving the execution context.
- Used when full decentralization and automated smart contract enforcement are needed.

#### Option 2: Off-Chain Anchoring (External Service)  
- An off-chain service monitors L2 timestamps and submits them to L1 as needed.
  - Can batch multiple timestamps into a single transaction.
  - Uses CIP-68 metadata for lightweight timestamp anchoring.
  - Submits proofs verifying timestamps before finalization.
- Suitable for cost efficiency and low-latency execution.

---

## Verifiable Hash Anchoring for Hybrid Time Logic
### Overview  
Verifiable Hash Anchoring uses Blake2b hashing to ensure data integrity and tamper resistance by:
- Hashing the timestamp and storing the hash on-chain.
- Anchoring the timestamp with `mustValidateIn` using the hashed value.
- Allowing reverse compilation to restore Ladder Logic with the verified timestamp.

### Implementation
#### In `plutusladder_compiler.py`
```python
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
```

#### In `reverse_compiler.py`
```python
# Detect and Handle Verifiable Hash Anchoring
if "Verifiable Hash" in plutus_code:
    hash_match = re.search(r'-- Verifiable Hash: (\w+)', plutus_code)
    if hash_match:
        hash_value = hash_match.group(1)
        # Preserve Verifiable Hash as a comment in Ladder Logic
        if f"// Verifiable Hash: {hash_value}" not in ladder_logic_lines:  # Check to avoid duplicates
            ladder_logic_lines.append(f"// Verifiable Hash: {hash_value}")
            print(f"Reverse Compiled Verifiable Hash: {hash_value}")
```
- The Verifiable Hash is preserved as a comment in Ladder Logic to maintain integrity.

### Specification
- **Format:** `"verifiable"`
- **Example Structure:**  
    ```json
    {
        "format": "verifiable",
        "timestamp": 1700000000,
        "machine_id": "MORLEY-PLC-001",
        "process_id": "LADDER-SEQ-12",
        "hash": "d5f1a8b0e...7c3e1" 
    }
    ```

### Example Transformation:
Plutus Input:
```haskell
mustValidateIn (from slot1700000000)
-- Verifiable Hash: cc5f0d2ea144196379c332b7ce6553b26e6eef381fb3d1e9095533b49a915a4d
```
Reverse Compiled into Ladder Logic:
```
TON TimerX, 1700000000ms
// Verifiable Hash: cc5f0d2ea144196379c332b7ce6553b26e6eef381fb3d1e9095533b49a915a4d
```

---

## Development Priorities
- Implement Verifiable Hash Anchoring. (Done)
- Implement Reverse Compilation for Verifiable Hash Anchoring. (Done)

