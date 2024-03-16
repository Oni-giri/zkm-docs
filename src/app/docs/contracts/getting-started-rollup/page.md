---
title: Getting started with rollup contracts
nextjs:
  metadata:
    title: Getting started with rollup contracts
    description: Getting started with rollup contracts
---

As per usual, running `./bootstrap.sh` should get you up to speed. It will install any submodules / frameworks required.
This project uses foundry as it's testing framework, before getting started / if you have any issues consult the [book](https://book.getfoundry.sh/).

## Submodules

Forge modules:

- [forge-std](https://github.com/foundry-rs/forge-std) (Testing)
- [uniswap v2 core](https://github.com/uniswap/v2-core) (Fee Distributor dependency)
- [uniswap v2 periphery](https://github.com/uniswap/v2-periphery) (Fee Distributor dependency)
- [openzeppelin contracts](https://github.com/openzeppelin/openzeppelin-contracts)
- [openzeppelin contracts upgradable](https://github.com/openzeppelin/openzeppelin-contracts-upgradable)
- [rollup encoder](https://github.com/AztecProtocol/rollup-encoder) (Test harness to encode rollup calldata)
- [aztec connect bridges](https://github.com/AztecProtocol/aztec-connect-bridges) (Bridges repository)

Use `forge update --no-commit` if submodules have changed and you already have some installed. If submodules are causing issues and errors are occurring while installing. Deleting the `/lib` folder then running `forge install --no-commit` will generally resolve the issues.

## Directory Structure

```bash
src
├── core
│   ├── Decoder.sol
│   ├── DefiBridgeProxy.sol
│   ├── processors
│   │   ├── RollupProcessor.sol
│   │   └── RollupProcessorV2.sol
│   ├── reference
│   │   └── RollupProcessorV2Reference.sol
│   └── verifier
│       ├── BaseStandardVerifier.sol
│       ├── instances
│       │   └── ... Contract Verifiers
│       └── keys
│           └── ... Contract Verification Keys
├── periphery
│   ├── AztecFaucet.sol
│   ├── AztecFeeDistributor.sol
│   ├── PermitHelper.sol
│   └── ProxyDeployer.sol
├── script
│   └── ... Deployment scripts
└── test
    └── ... Test suite
```

## Tests

`forge test` will run the test suite. See the forge book linked above about how to target specific tests. To use the reference implementation set `export REFERENCE=true`. 

### Running tests in a Docker container:

```bash
# In root run
docker build --no-cache .
```

## Generating new verification keys

It is possible to generate new verification keys by running the `generate_vks.sh` script that is put in `verification-keys`. This generates the keys and their matching solidity contracts. For example the 28x32 key (used in our production rollup) verifies the circuit which validates 28 recursive proofs of 32 smaller inner proofs.