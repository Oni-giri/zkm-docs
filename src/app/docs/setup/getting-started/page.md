---
title: Setting up zkWhirl
---

ZkWhirl has many different components that need to be set up correctly for the whole solution to work.

You can look at the [architecture](/docs/basics/zkwirl-architecture) page to understand the structure.

![image](/diagram.png)

1. [Set up the proof-generation server](halloumi), known as "Halloumi"

2. [Set up the rollup management server](falafel), aka "Falafel"

3. [Set up the contracts for the rollup](/docs/contracts/getting-started-rollup)

4. [Set up the SDK to interact with the rollup](/docs/sdk/setup)

5. Set up the front end

Regarding the front-end, two ways are possible: 

- Starting from the [zk.money](https://zk.money/) code, stored [here](https://github.com/AztecProtocol/zk-money)
- Using the boilerplate code provided [here](https://github.com/AztecProtocol/aztec-frontend-boilerplate)

You can also interact with zkWhirl using the [cli](/docs/sdk/cli)
