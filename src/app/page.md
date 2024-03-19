---
title: What is ZkWhirl ?
---

ZkWhirl is a private zk rollup on top of an EVM chain. It allows users to deposit assets, such as ETH, a withdraw on another address, anonymously. It uses Aztec Connect tech, built by the Aztec team, leading the efforts in zero-knowledge privacy.

It is different from solutions such as Tornado Cash, since it allows users to withdraw a lower amount than their initial deposit, but also to send each others assets anonymously, on the rollup. Users can transact, but no one can see their transaction history.

As a result, the anonymity set of zkWhirl is better. Indeed, when trying to deanonymize a withdrawal, one should consider now that it may also come from a deposit lower than the amount withdrawn. This increases drastically the set of potential transactions and creates a near-perfect privacy for users.

---

{% quick-links %}

{% quick-link title="Set-up" icon="installation" href="docs/setup/getting-started" description="Step-by-step guides to setting up your system and installing ZkWhirl." /%}

{% quick-link title="Architecture guide" icon="presets" href="docs/basics/zkwhirl-architecture" description="Get an helicopter-view of the network's architecture" /%}

{% quick-link title="Aztec SDK" icon="plugins" href="docs/sdk/overview" description="Learn how to use the Aztec SDK to interact with ZkWhirl" /%}

{% quick-link title="Learn about zero knowledge" icon="theming" href="docs/basics/zkwhirl-basics" description="Quick and easy intro to the world of zero knowledge" /%}

{% /quick-links %}



