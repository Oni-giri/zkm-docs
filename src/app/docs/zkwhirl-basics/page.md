---
title: How does zkWhirl works
nextjs:
  metadata:
    title: Basics
    description: How does zkwhirl works
---

## Understanding zkWhirl's Unique System

zkWhirl operates differently from Ethereum's account-based system, mainly because encrypting account states would hinder the ability to verify important aspects like double spending and the overall integrity of transactions.

### The UTXO System in zkWhirl

#### Digital Cash Analogy
zkWhirl adopts the Unspent Transaction Output (UTXO) model, akin to how digital cash works. Unlike an account balance that updates with each transaction, a UTXO system calculates an account's balance based on the sum of previous transactions linked to that account's address. These transactions are kept private, decryptable only by the address owner, which significantly enhances user privacy.

#### Transaction Process
When user A wants to transfer assets to user B, user A creates a "note" that signifies asset ownership and passes this ownership to user B. Concurrently, user A destroys the previous note (indicating their former ownership) and generates a new one that shows their updated balance of unspent assets.

### Ensuring Integrity and Privacy with Zero-Knowledge Proofs

#### Example Scenario
Imagine user A holds two notes, each for 50 ETH, and intends to send 20 ETH to user B. User A will create a new 20 ETH note for user B and another 80 ETH note for themselves. To prevent double spending, user A must destroy the original 50 ETH notes.

#### Role of Zero-Knowledge Proofs
To record the change in note ownership accurately and prevent fraud, user A generates a zero-knowledge proof. This proof verifies to the rollup (without revealing the details) that no double spending occurred and the transaction is legitimate.

#### Finalizing the Transaction
After the zero-knowledge proof is created and validated by the rollup, the old notes are nullified. The new notes' ownership is then added to a Merkle tree, symbolizing the transaction's completion. The rollup system does not know the notes' specifics, ensuring only the recipient can access and decrypt the notes.