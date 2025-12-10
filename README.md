# BIP-UTX: The Sovereign Node Upgrade

### 🛑 Stop Deleting History. Start Compressing State.

**BIP-UTX** is a technical standard proposal designed to solve the "State Bloat" crisis in Bitcoin without resorting to censorship or data confiscation.

### The Problem
The Bitcoin UTXO set is growing exponentially (due to Ordinals, Runes, etc.), pricing home users out of running nodes. Radical proposals like **"The Cat"** suggest deleting "spam" data to save the nodes. This destroys miner revenue and introduces censorship.

### The Solution: Mathematics > Politics
Instead of deleting data, we compress it.
By implementing **Utreexo Accumulators**, we shift the burden of storage from the Node (Validator) to the User (Owner).
* **Current Node RAM:** ~16 GB (and growing).
* **BIP-UTX Node RAM:** < 10 Kilobytes (Fixed).

### Contents
* [**Read the Full Proposal (BIP-UTX)**](BIP-UTX.md)
* **Status:** Draft / Discussion
* **Goal:** A clean Soft Fork upgrade path.

---
*"Don't Trust. Verify. Compress."*
