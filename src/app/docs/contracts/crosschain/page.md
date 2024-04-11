---
title: crosschain deposits
---

Using a trusted custom keeper, it is possible to do cross-chain deposits into the protocol.

## Deposits

![Cross-chain-deposit](/cross-chain-deposit.png)

### Prerequisite:

User already has an aztec-connect account. If not, he should make one.

### Deposit flow

1. The user notifies the zkWhirl bridging keeper of the intent to bridge. The keeper generates an address for the deposit.
2. The user sends the money on the bridge to the destination address.
3. The bridge send the money to the address
4. Upon reception, the keeper uses AztecSdk.createDepositController with the sender's address as recipient to make a deposit on his behalf. See the [deposits](/docs/sdk/deposit) doc.

## Withdrawals

![Cross-chain-witdrawals](/cross-chain-withdrawals.png)

### Withdraw flow

1. The user notifies the zkWhirl bridging keeper of the intent for bridging out. The keeper generates an address for the reception of the funds from the rollup.
2. The user sends the money to the target address.
3. Upon reception, the keeper sends the money to the target address of the user, using the bridge.
4. Bridge sends the money to the user.