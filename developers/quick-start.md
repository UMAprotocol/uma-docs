---
description: 0 to 1 integration with the Optimistic Asserter.
---

# Quick start

The primary integration point into the UMA ecosystem is the Optimistic Asserter (OA). The OA is an _ **oracle for arbitrary off-chain data** _ which leverages an interactive escalation game between _asserters_ and _disputers_ and is secured by _economic incentives_. It differs from the Optimistic Oracle (getting started can be found [here](optimistic-oracle/getting-started.md)) by being easier to reason about and simpler in integration points.

This getting started tutorial will show you how to go from 0 to 1 with the Optimistic Asserter by executing the simplest possible assertion flow. Later in the docs you can find more information on how the Optimistic Asserter works and dig deeper into its mechanism and more sophisticated code examples.&#x20;

### **A minimum viable Optimistic Asserter integration**

The Optimistic Asserter works by making a truth claim about the world, stating that something has happened or is true. Once asserted, the assertion enters a challenge period wherein someone can disagree with the assertion, by disputing it. If no one disputes it during the challenge window the statement is taken as true. If disputed, the outcome is arbitrated using the UMA DVM (more info on how this works here).&#x20;

In this tutorial you will be working through a simple smart contract that asserts the following truth: `Statement: Argentina won the 2022 Fifa world cup in Qatar.`&#x20;

Once through the challenge window, you will use the assertion in your resolution contract. This shows the full non-dispute lifecycle of an Optimistic Asserter data assertion. It will give you the basic intuition as to how the Optimistic Asserter works without much overhead and is a great starting point before digging deeper.  Let's get started!

### **Prerequisites**

To complete this tutorial you will need**:**

1. Metamask installed in a Chromium based browser (such as [Google Chrome](https://www.google.com/chrome/)) If you don't have it already, you can get Metamask [here](https://metamask.io/).
2. A wallet set up in Metamask.
3. [Görli](https://goerli.net/) test ETH to send test transactions. You can get GETH from one of these faucets: [Telegram Authenticated](https://goerli-faucet.com/), [Twitter Authenticated](https://goerli-faucet.mudit.blog/), [Alchemy](https://goerlifaucet.com/), [slock.it](https://goerli-faucet.slock.it/).

### Asserting a truth

First, we will work through the basic flow for _asserting a truth to the oracle_. In this example, we are making a statement about the outcome of a sports event, but the statement could be much more complex; it could power any kind of smart contract system requiring data.&#x20;

The contract used in this tutorial is meant to be a simple data assertion flow. The contract exposes a simple `assertTruth` function which asserts the truth to the OA about the outcome of the world cup.

1. [Go to this example contract on Remix](https://remix.ethereum.org/#version=soljson-v0.8.16+commit.07a7930e.js\&optimize=false\&runs=200\&gist=4f86070d14226d61b4ff54c7927f426b). This gives you the minimum assertion flow. Read the contract and the comments listed within.
2. Click on "gist-4f86..." to see the files in the gist, and click `OA_GettingStarted.sol` to open to Solidity file.
3. In the far left hand menu, click the link to deploy and run transactions (which looks like the Ethereum logo and a right arrow).
4. In the "Environment" dropdown, choose "Injected Provider," and connect to your wallet. **Make sure you are connected to Görli within your metamask wallet, that has Görli test ETH!** You don't want to deploy to a real network and spend real gas, and the Görli Optimistic Oracle address hardcoded into the contract will not work on other networks.
5. Under the "Contract" dropdown, select `OA_GettingStarted`.
6. Click "Deploy" and confirm the transaction with your wallet.
7. You should now see the `OA_GettingStarted` contract under "Deployed Contracts". Clicking the dropdown carrot will reveal buttons for each of the functions in the contract.
8. Click `assertClaim` to request the data specified in the contract's ancillary data, asserting the truth about the outcome of the Fifa world cup. This will submit a data assertion to the Optimistic Asserter and the assertion will enter the challenge period.

What we've done in the above steps is: a) deploy a smart contract and b) submit a "data assertion" to the Optimistic Asserter through the call to the Optimistic Asserters `assertTruthWithDefaults` function. Now, the assertion has entered the challenge window and can be disputed by someone who disagrees with the assertion. If you click the `getAssertion` you can see some of the fields that are associated with the assertion. For more details on what these fields mean you can look at the OA interface [here](https://github.com/UMAprotocol/protocol/blob/9ce0c62c0f4b5681619c1da042498705d9badfb0/packages/core/contracts/optimistic-asserter/interfaces/OptimisticAsserterInterface.sol#L20).

The default liveness for an assertion is 120 seconds (2 minutes). Wait this time before going to the next step.

### Settling the assertion

Once the assertion has settled, and assuming no one has disputed it, we can settle it! Do the following:

1. Click `settleAndGetAssertion` in remix. This will send a transaction to settle the assertion and return the settlement value.
2. Wait for the transaction to mine.
3. Once mined you can expand the transaction output block inline with the green tick arrow. If you scroll down here you can find some information about the assertion, such as the settlement value (which should be `true` as it was not disputed).

<figure><img src="../.gitbook/assets/2023-02-02 at 14.39.21@2x.png" alt=""><figcaption></figcaption></figure>

4. Now, you can call `getAssertion` and `getAssertionResult` from remix and see the outputs. `getAssertion` has changed such the `settled` value is `true` and `getAssertionResult` now returns `true` as the assertion was deemed valid as it was not disputed.

### Next Steps

Hopefully you got a basic understanding of the Optimistic Asserter request flow from this getting started guide. Note that this kind of example would not really work on mainnet as: a) there was no bonds for the asserter (and therefore no rewards for disputers) b) the challenge window was too short. The Optimistic Asserter works due to economic incentives between the asserter and disputers, which was absent in this example to keep things simple. For some incentive compatible examples check out some of the example tutorials where we walk through more details on the functions discussed in this guide.
