---
title: Architecture
nextjs:
  metadata:
    title: Architecture
    description: Helicopter view of zkWhirl's architecture
---

zkWhirl is based on the Aztec Connect architecture, used by zk.money. Here is the diagram summarizing the architecture.

![image](/diagram.png)

Let's disassemble the components:

## Aztec SDK

The Aztec SDK is privacy's main character and is responsible for computing "inner proofs" that anonymize transactions. By interacting with the SDK, a user or a dev can check the state of his account, compute proofs and send transactions on the network.

It is worth noting that the user has two kind of keys: a **view key**, that allows him to decrypt past transactions and know his account state, and a **spending key** that lets him construct proofs and send transactions on the network.

- Read the Aztec SDK docs here
- [Aztec SDK repo](https://github.com/AztecProtocol/aztec-connect/tree/master/yarn-project/sdk)

## Falafel

Once the transaction is created, it’s sent to Falafel. Falafel is a Typescript implementation of Aztec’s client — think of it like Go Ethereum (geth) or any other Ethereum client that forms an interface between the user and the actual blockchain.

Falafel is offchain software that accepts user proofs (encrypted transactions), aggregates them, creates a rollup proof, and then sends rollup proofs to the rollup contract for validation. This large proof comprised of many user transactions is what we call an “outer proof” (the inner proofs being the proofs containing each individual user transaction).

Falafel is just a batching mechanism that creates a big mega proof that adds all the inner proofs together proving that all the underlying state updates are valid.

- [Falafel repo](https://github.com/AztecProtocol/aztec-connect/tree/master/yarn-project/falafel)

## Halloumi & Barretenberg

Halloumi and Barretenberg are libraries used by Falafel to generate proofs for the rollup contract.

- [Halloumi repo](https://github.com/AztecProtocol/aztec-connect/tree/master/yarn-project/halloumi)

### Rollup contract

Once the outer proof is constructed, it’s sent to the on-chain rollup contract, a smart contract published on an EVM chain.

The outer proof sent to the rollup contract can contain a variety of encrypted user actions: withdrawals, deposits, new account registrations, or Aztec Connect bridge transactions.

The outer proof is sent to the rollup contract for validation and state updates. The rollup contract then runs through each user transaction and executes the necessary logic on Ethereum Layer 1.

- **Deposit**: Users move funds to the Aztec rollup contract, which locks funds and then credits the user with an L2 representation of those funds once the rollup is published to Ethereum.

- **Withdrawal**: Users indicate they want to send funds out of Aztec, the rollup contract confirms the value of the singular note the user is spending, nullifies internal Aztec funds, and sends its L1 funds to the specified withdrawal address.

- **DeFi interaction**: the rollup contract calls out to an Aztec Connect Bridge Contract, which serves as an interface to Layer 1 smart contracts.

### L1 bridge contract

Bridge contracts are all fundamentally structured as two-input, two-output swaps: the rollup contract sends 1–2 assets into an L1 protocol, and atomically receives 1–2 assets back (a synchronous bridge like a swap) or returns token assets back at a later date (an asynchronous bridge like a timed vault). In the next block, it subsequently generates a claim note for the user to claim the returned asset at a later date.

Virtual assets are an Aztec-specific representation of token positions that can’t immediately be returned to the rollup — think vault positions, fixed term positions, or anything else that is escrowed for redemption at a later date.

