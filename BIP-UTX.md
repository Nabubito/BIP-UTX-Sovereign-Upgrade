---
BIP: UTX  
Title: Compact State Verification via Utreexo Accumulators (The Sovereign Node Upgrade)  
Author: Chron  
Status: Draft  
Type: Standards Track  
Layer: Consensus (Soft Fork) / P2P  
Created: 2025-12-10
---

## 1. Abstract

This proposal introduces a method for **Compact State Verification** using cryptographic accumulators (Utreexo). It fundamentally alters the resource requirements for running a fully verifying Bitcoin node.

Currently, nodes must store the entire Unspent Transaction Output (UTXO) set in RAM to validate new blocks. This proposal shifts the storage burden from the **Validator (Node)** to the **Asset Owner (Wallet)**. By representing the global state as a cryptographic "fingerprint" (Merkle Root) rather than a full database, we reduce the RAM requirements for a full node from gigabytes to kilobytes.

This upgrade renders "State Bloat" irrelevant to node operators, negating the need for destructive proposals (like "The Cat") that seek to delete or censor historical data to save resources.

## 2. Motivation

### The "Industrialization" Crisis
The Bitcoin UTXO set has grown exponentially due to new data protocols (Ordinals, Runes, Inscriptions). As detailed in recent network analysis, this growth threatens to centralize the network by pricing out home-operated nodes. When the hardware cost to verify the chain exceeds consumer capabilities, the network's consensus becomes controlled solely by industrial data centers.

### The Rejected Alternative: "The Cat" (State Confiscation)
Proposals have emerged to solve this crisis by "pruning" or "deleting" specific UTXOs deemed "non-monetary" or "spam." This approach is rejected for three reasons:
1.  **Censorship:** It violates the principle of uncensorable money.
2.  **Subjectivity:** It requires a political consensus to define "spam."
3.  **Miner Economics:** It destroys a critical source of fee revenue that sustains miners as the block subsidy declines.

### The Solution: Mathematics over Politics
BIP-UTX solves the resource constraint without censoring data. By compressing the state, we allow the blockchain to grow indefinitely while keeping verification costs fixed and minimal.

## 3. Specification

### 3.1 The Utreexo Accumulator (Merkle Forests)
We propose replacing the node's local UTXO database with a **Dynamic Merkle Accumulator**.
* **Current Architecture:** Nodes store millions of "leaves" (individual coins).
* **New Architecture:** Nodes store only the **Merkle Roots** of the forest.
* **Impact:** The memory footprint for the Bitcoin State is reduced to **< 10 Kilobytes**, regardless of how many millions of Inscriptions or UTXOs exist.

### 3.2 Proof of Inclusion (The Transaction Standard)
To spend a UTXO, the transaction verification rules are updated:
* **The Burden Shift:** The sender must provide a **Cryptographic Proof** (a Merkle path) connecting their specific UTXO to the current Accumulator Root held by the nodes.
* **Verification Logic:** The node computes `Hash(UTXO + Proof)` and compares the result to its local Root. If they match, the transaction is valid.
* **Statelessness:** The verifying node requires zero knowledge of the transaction history or the specific UTXO prior to the moment of spending.

### 3.3 The "Smart Wallet" Protocol
Wallet software must be upgraded to maintain these proofs.
* **Dynamic Updates:** As new blocks are mined (adding/removing leaves), the Merkle Forest changes shape.
* **Client Behavior:** Wallet software acts as a "Light Node," watching block headers and automatically applying mathematical updates to its local proofs to keep them valid against the current chain tip.

---

## 4. Operational Roadmap

### Phase 1: The Bridge Era (Transition)
*Duration: 6–12 Months*
To ensure backward compatibility and prevent user friction:
* **Bridge Nodes:** Large infrastructure providers (Exchanges, Explorers) run "Archive Nodes" that hold the full database.
* **Translation:** Legacy wallets route transactions through Bridge Nodes. The Bridge attaches the necessary Utreexo Proof and broadcasts it to the compact network.
* **Cold Storage:** Users bringing cold wallets online connect to Bridge Nodes to receive a "Proof Update" (calculating the difference between the old state and current state).

### Phase 2: Client Standards (Adoption)
*Duration: 12–18 Months*
* **Smart Wallets:** Major wallet providers (Mobile/Desktop) implement local proof storage.
* **Sovereignty:** Users verify their own assets locally. The reliance on centralized indexers for Ordinals/Runes decreases, as the wallet holds the mathematical proof of ownership.

### Phase 3: The Sovereign Renaissance (Maturity)
* **Hardware Accessibility:** The minimum requirement for a Full Node effectively drops to low-power devices (e.g., Raspberry Pi Zero, older smartphones).
* **Spam Neutrality:** The debate over "State Bloat" ends. Since "spam" UTXOs no longer consume node resources, the political pressure to ban them evaporates.

---

## 5. Economic Security Analysis (Miner Incentives)

A critical flaw in deletion proposals ("The Cat") is the destruction of Miner Revenue. BIP-UTX preserves the "Security Budget."

### 5.1 The Fee-Revenue Lifeline
With the block subsidy currently at 3.125 BTC and halving every 4 years, miners are increasingly dependent on transaction fees. Data from the 2023-2024 cycle demonstrates that **Inscriptions provided a critical revenue floor** during bear markets.
* **"The Cat" Impact:** By deleting or banning this data, the network destroys 20-30% of potential miner revenue, forcing small miners out of business and centralizing hashrate into industrial pools.
* **BIP-UTX Impact:** By allowing Inscriptions to exist efficiently, we preserve this revenue stream. Miners get paid to include the data, but Nodes are not punished for storing it.

### 5.2 Alignment of Incentives
BIP-UTX creates a fair market:
* **The Spammer:** Pays the fee to mint data and bears the cost of storing the Proof.
* **The Miner:** Receives the fee for security.
* **The Node:** Verifies the rules without bearing the cost.

## 6. Rationale & Alternatives Considered

### Why not Pruning?
Current "Pruned Nodes" discard history but still maintain the full UTXO set in RAM. They do not solve the RAM bottleneck caused by State Bloat. Utreexo solves the RAM bottleneck directly.

### Why not "The Cat" (Deletion)?
Deleting UTXOs destroys the fungibility of Bitcoin. If a "consensus of nodes" can vote to delete one type of UTXO today, they can vote to delete a political adversary's UTXO tomorrow. This transforms Bitcoin from a **Protocol of Truth** into a **Protocol of Opinion**.

## 7. Conclusion

BIP-UTX represents the evolution of Bitcoin from a "Replicated Database" (where everyone stores everything) to a "Verifiable Proof System" (where users prove ownership to a lean decentralized court).

It resolves the conflict between **Ordinals growth** and **Node decentralization** without compromising the security model or the economic viability of the mining sector. It allows Bitcoin to scale globally while remaining verifiable on a device that fits in a pocket.
