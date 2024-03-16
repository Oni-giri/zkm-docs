---
title: Deploying the rollup contracts
nextjs:
  metadata:
    title: Deploying the rollup contracts
    description: Deployng the rollup contracts
---


## Deployments

### Devnet / Stage / Testnet Deployments

The CI/CD pipeline will use the script in `deploy/deploy_contracts.sh` to orchestrate deployments.

### Local Deployments

To quickly bootstrap an anvil fork with our entire suite of mainnet contracts deployed run `scripts/start_e2e_fork.sh`.

**Environment Variables**
When deploying to dev or testnet from a local machine, there are some required environment variables.

Required:

- ETHEREUM_HOST - The rpc url you are targeting
- PRIVATE_KEY - The private key you are deploying from

Optional:

- DEPLOYER_ADDRESS - The address of the key you are deploying from [default: the address matching the PRIVATE_KEY]
- ROLLUP_PROVIDER_ADDRESS - The address to be added as a sequencer [default: DEPLOYER_ADDRESS]
- FAUCET_CONTROLLER - The address that will be given superOperator privileges when the faucet is deployed [default: DEPLOYER_ADDRESS]
- SAFE_ADDRESS - The address to own the deployed system [default: DEPLOYER_ADDRESS]
- VK - The verification key type (VerificationKey1x1 | VerificationKey28x32 | MockVerifier) [default: MockVerifier]
- UPGRADE - Flag to upgrade rollup to use the newest implementation [default: true]

### How are deployments triggered?

Redeploying the testnet contracts can be done in one click with the `redeploy-mainnet-fork` circle ci workflow.
To force deployments through ci there are override files for each environment inside the `deploy` folder (`dev`, `testnet`. `stage`). The deployment script it will check whether there is a diff in the target environment's file. E.g. if you want to force a redeploy in dev, changing the dev file will trigger it.  
Please exercise caution in your commits that these files have not changed by accident.

### How do downstream services consume the contract addresses?

#### e2e

For e2e tests the contracts service will serve deployed addresses using `socat` - see `serve.sh`. It will serve the deployment script output (`/serve/deployment_addresses.json`) on the defined port (8547 by default). When downstream services (`kebab`, `falafel`) boot they run an `export_addresses` script that consumes the contract addresses.

#### Deployments

At the end of the deployments script, all critical addresses will be saved into terraform variables. They are consumed by downstream services as env vars at deploy time.