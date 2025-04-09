---
description: 0 to 1 Optimistic Oracle integration by example.
---

# OO Quick Start

The primary integration point into the UMA ecosystem is the Optimistic Oracle (OO). The OO is an _**oracle for arbitrary off-chain data**_ which leverages an interactive escalation game between _requesters_, _proposers_ and _disputers_ and is secured by _economic incentives_.

This getting started tutorial will show you how to go from 0 to 1 with the Optimistic Oracle by executing the simplest possible request flow. Later in the docs you can find more information on [how the Optimistic Oracle works](../../protocol-overview/how-does-umas-oracle-work.md#optimistic-oracle) and dig deeper into its mechanism and [more sophisticated code examples](in-depth-tutorial-event-based-prediction-market.md).&#x20;

{% hint style="info" %}
**If you prefer, you can also watch the following video tutorial in which we follow the step-by-step instructions described below. Please note, the Optimistic Oracle now supports Sepolia testnet.**
{% endhint %}

{% embed url="https://www.youtube.com/watch?v=tyi6PnlIHw8&t=26s" %}

### **A minimum viable Optimistic Oracle integration**

You will be working through a simple smart contract that asks the oracle the question: `Q:Did the temperature on the 25th of July 2022 in Manhattan NY exceed 35c? A:1 for yes. 0 for no.`  After submitting the request, you will propose a solution using the UMA Optimistic Oracle UI.

Once through liveness, you will fetch the price from the smart contract. This shows the full lifecycle of an Optimistic Oracle data request on the Sepolia testnet, interacting with an actual Optimistic Oracle contract deployment, without needing to write any code or clone any repos. It should give you the basic intuition as to how the Optimistic Oracle works without too much overhead and is a great starting point before digging deeper.  Let's get started!

### **Prerequisites**

To complete this tutorial you will nee&#x64;**:**

1. Metamask installed in a Chromium based browser (such as [Google Chrome](https://www.google.com/chrome/)) If you don't have it already, you can get Metamask [here](https://metamask.io/).
2. A wallet set up in Metamask.
3. [Sepolia](https://sepolia.dev/) test ETH to send test transactions. You can get Sepolia ETH from one of these faucets: [Infura](https://www.infura.io/faucet/sepolia), [Alchemy,](https://www.alchemy.com/faucets/ethereum-sepolia) [PoW Faucet](https://sepolia-faucet.pk910.de/),  or [LearnWeb3](https://learnweb3.io/faucets/sepolia/),&#x20;

### Requesting data

First, we will work through the basic flow for _asking the oracle for a piece of data_. In this example, we are asking the Oracle for information on the weather but the request could be much more complex to could power any kind of smart contract system requiring data. See [approved price identifies](../../resources/approved-price-identifiers.md) and the sample projects for more inspiration on what is possible with the OO.

The contract used in this tutorial is meant to be a simple data request flow. The contract exposes a simple `requestData` function which asks the OO a simple question about the weather.

1. [Go to this example contract on Remix](https://remix.ethereum.org/#version=soljson-v0.8.16+commit.07a7930e.js\&optimize=false\&runs=200\&gist=fba5d2812d940759f4f7585741b529a4). This gives you the minimum data request and retrieval flow possible. We'll work through the code in the sections that follow.
2. Click on "gist-fba..." to see the files in the gist, and click `OO_GettingStarted.sol` to open to Solidity file.
3. In the far left hand menu, click the link to deploy and run transactions (which looks like the Ethereum logo and a right arrow).
4. In the "Environment" dropdown, choose "Injected Provider," and connect to your wallet. **Make sure you are connected to Sepolia within your Metamask wallet!** You don't want to deploy to a real network and spend real gas, and the Sepolia Optimistic Oracle address hardcoded into the contract will not work on other networks.
5. Under the "Contract" dropdown, select `OO_GettingStarted`.
6. Click "Deploy" and confirm the transaction with your wallet.
7. You should now see the `OO_GettingStarted` contract under "Deployed Contracts". Clicking the dropdown carrot will reveal buttons for each of the functions in the contract.
8. Click `requestData` to request the data specified in the contract's ancillary data, asking about the temperature in Manhattan on July 25th, 2022. Confirm the transaction in your wallet. This will submit a data request to the Optimistic Oracle.

What we've done in the above steps is: a) deploy a smart contract and b) submit a "price request" to the Optimistic oracle through the call to the Optimistic Oracle's `requestPrice` function.

### Proposing data

Now that we've asked the OO a question, it's time to propose a solution! The Optimistic Oracle works via an _interactive escalation game_ wherein a) requesters ask questions b) proposers provide solutions and c) disputers monitor the proposals and dispute if they are invalid. If no dispute is done then the proposal is taken as valid. In this example, we'll be acting as the proposer who will be answering the question we asked in the previous section

1. Go to the Sepolia[ Optimistic Oracle UI](https://testnet.oracle.umaproject.org/) to see your request, which should be a `YES_OR_NO_QUERY` at the top of the index page. Click on the request to go to the details page.
2. Click "Connect Wallet" and make sure you are connected to the Sepolia testnet so that you can make a proposal. You should see all the information posed in the data request: the requester, identifier, timestamp, ancillary data and a link to the UMIP.
3. Submit your proposal (either `1` or `0`). The actual value you submit is not super important in this case since this is just for testing purposes, but the [correct response](https://www.wunderground.com/history/daily/us/ny/williston-park/KJFK/date/2022-7-25) is `0`. In doing this we are acting as the "proposer" providing a solution.

Once you've provided an answer to the question the proposal enters into a liveness period of 30 seconds. During this time it's up to Disputers to verify the correctness of what the proposer provided.  Note that in a main net price request you'll normally set the liveness to much longer than 30 seconds (say two hours), the proposer will be bonded (if they propose wrong they lose money) and the proposer will be rewarded for providing a valid answer to the question.

### Settling the final answer

Finally, we can fetch the data proposed in the previous step from the smart contract. For this example, we assume that there the proposed data was correct (was not disputed). Head back to your Remix tab and:

1. After the transaction finalizes, wait 30 seconds (the challenge window) and then click `settleRequest` on the `OO_GettingStarted` contract on Remix. Confirm the transaction in your wallet. Because the proposal was not disputed within the challenge window, it can now be settled.
2. Click `getSettledPrice` to get the settled value. It should look like `int256: 0` under `getSettledPrice` if you proposed an answer of `0`. You can also see details in the console logs.

Congratulations! you've successfully integrated with the Optimistic Oracle, requested data, proposed data and fetched it within your smart contract! Note that the incentives in this toy example mean that there is no reason that anyone would provide the _correct_ price (there was no reward for the proposers or disputes and there was no bond). A real world version of this should [leverage custom bonds and more realistic liveness parameters](../setting-custom-bond-and-liveness-parameters.md).

### Next Steps

Hopefully you got a basic understanding of the OO request flow from this getting started guide.  Check out some of the [example tutorials](solidity-examples.md) where we walk through more details on the functions discussed in this guide.
