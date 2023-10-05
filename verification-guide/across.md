---
description: How to verify Across Bundles
---

# Across

**Step 1: Install Required Dependencies:**

git:

* Documentation: [https://github.com/git-guides/install-git](https://github.com/git-guides/install-git)

yarn:

* Documentation: [https://classic.yarnpkg.com/lang/en/docs/install/#mac-stable](https://classic.yarnpkg.com/lang/en/docs/install/#mac-stable)

node.js

* Documentation: [https://nodejs.org/en/download](https://nodejs.org/en/download)
* Run the following command to check what version of node you are running: `node -v`\
  Make sure it is greater than 16.18.0.&#x20;

redis:

* Documentation: [https://redis.io/docs/getting-started/installation/](https://redis.io/docs/getting-started/installation/)
* Note: Follow the [relayer-v2 README](https://github.com/across-protocol/relayer-v2/tree/master) instructions

ts-node

* You can use the following to install globally: `npm install -g ts-node`

Nvm (optional)

* Documentation: [https://github.com/nvm-sh/nvm#installing-and-updating](https://github.com/nvm-sh/nvm#installing-and-updating)
* Note: Optional, but nvm allows you to easily install and switch between node versions

VS Code (optional)

* Highly recommend using a code editor like VS code.

**Step 2: Clone the relayer-v2 repo:**

* If using VS code, open the terminal and run the following command:

`git clone https://github.com/across-protocol/relayer-v2`

**Step 3: Run Redis**

Open a terminal window and run:

`redis-server`

`From the README, run the three commands:`

`cd relayer-v2`

`yarn install`

`yarn build`

**Step 4: Environment Variables**

* Change the .env.example file to .env
* In line 8 and 9, uncomment either the MNEMONIC or PRIVATE\_KEY and input a private key that has no money on it.
  * I used [https://vanity-eth.tk/](https://vanity-eth.tk/) to create a private key to use for the demo but again, do not use one that has any money on it as it is an unnecessary risk.
* Sign up for an Infura account [https://www.infura.io/](https://www.infura.io/) and click ‘Create New API Key’. After you create the key, you should see a list of endpoints for each network.
* In the .env file, update the following env variables to the URL for your infura account. The underscore number at the end of the variable represents the chain ID. So RPC\_PROVIDER\_ALCHEMY\_1 is mainnet.&#x20;
  * You can use [https://chainlist.org/](https://chainlist.org/) to find the ID by network.&#x20;
  * Include the NODE\_URL\_324 for zksync below even though it hasn’t been added to the .env.example file yet:

`RPC_PROVIDER_ALCHEMY_1=https://eth-mainnet.g.alchemy.com/v2/{API_KEY}`

`RPC_PROVIDER_ALCHEMY_10=https://opt-mainnet.g.alchemy.com/v2/{API_KEY}`

`RPC_PROVIDER_ALCHEMY_137=https://polygon-mainnet.g.alchemy.com/v2/{API_KEY}`

`RPC_PROVIDER_ALCHEMY_42161=https://arb-mainnet.g.alchemy.com/v2/{API_KEY}`

`NODE_URL_324=https://mainnet.era.zksync.io`

**Step 5: Run the script**

The below is an example, however the two changes that need to be made based on the request are:

* REQUEST\_TIME=CHANGE\_THIS\_TO\_THE\_TIMESTAMP
* I used privateKey for the below, however, if you used mnemonic in your env variables, it should be changed to –wallet mnemonic

Note: The first time you run this, it is going to take a long time.

REQUEST\_TIME=1692110627 ts-node ./src/scripts/validateRootBundle.ts --wallet privateKey

When the script finishes running, check the Validation results as shown below. The below shows an example of an invalid bundle as it shows “valid”: false:

```
{
  "at": "RootBundleValidator",
  "message": "Validation results",
  "rootBundle": {
    "poolRebalanceRoot": "0x7005c47a8e476a2551d5325a6c53cd5b71f308b2ac670742ad7cceb8078a193f",
    "relayerRefundRoot": "0x9edefc2fc000e71fa1ea1c6ce666a0f8cb2d6da422c45a372585da5e3b738713",
    "slowRelayRoot": "0x0000000000000000000000000000000000000000000000000000000000000000",
    "proposer": "0xf7bAc63fc7CEaCf0589F25454Ecf5C2ce904997c",
    "unclaimedPoolRebalanceLeafCount": 5,
    "challengePeriodEndTimestamp": 1692117191,
    "bundleEvaluationBlockNumbers": [
      17920824,
      108255396,
      46338509,
      977154,
      121659934,
      11305323
    ],
    "proposalBlockNumber": 17920860
  },
  "valid": false,
  "invalidReason": "Disputed pending root bundle:\n.....
// removed the rest of the invalidReason for the example
```
