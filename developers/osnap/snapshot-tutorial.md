---
description: Snapshot Space Set-up and Transaction Builder
---

# ⚡ Snapshot Proposal + Transaction Tutorial

Welcome to the Snapshot Proposal + Transaction Tutorial. This guide provides detailed, step-by-step instructions on how configure proposals and transactions in your Snapshot space using oSnap.&#x20;

## Create a Proposal in Snapshot with oSnap

In your Snapshot space:

1. Click **'New Proposal'**
2. Fill in the title, description, and discussion link
3. Click **'Continue'**
4. Click the checkbox to use oSnap. _Please note, this will restrict to basic voting._
5. Set the voting period to meet the Safe oSnap module's minimum requirements.
6. Click **'Continue'**

<figure><img src="../../.gitbook/assets/Screenshot 2023-11-14 at 1.48.32 PM.png" alt="" width="474"><figcaption></figcaption></figure>

For more information on creating a proposal, please refer to the [Snapshot documentation. ](https://docs.snapshot.org/user-guides/proposals/create)

## Add Transactions to Your Snapshot Proposal

The oSnap transaction builder allows DAOs to add transactions when creating a Snapshot proposal. The transaction builder has forms for transferring funds, transferring collectables, contract interactions or raw transaction data.\
\
The example proposal below proposes a funds transfer of 0.000005 ETH.

<figure><img src="../../.gitbook/assets/image (14).png" alt=""><figcaption></figcaption></figure>

## Automatic Transaction Execution on Mainnets

For Snapshot proposals that pass on mainnets, UMA bots send a transaction requesting execution of the transactions and post a bond to UMA's Optimistic.&#x20;

After the Optimistic Oracle verifies the transactions and associated transactions are valid, UMA bots will execute this transaction for oSnap modules with default settings. \
\
For testnet transaction execution, see the section below.

## Manual Transaction Execution on Testnets

After the voting period has ended, if the proposal passes and meets the criteria set in the rules of the oSnap module, the 0.000005 ETH transfer can be proposed. Anyone can propose the transactions by clicking the 'Request execution' button.&#x20;



<figure><img src="../../.gitbook/assets/image (3) (2) (1).png" alt=""><figcaption></figcaption></figure>

Requesting execution requires sending a transaction along with a bond (bonds are not required on testnets). This starts the Optimistic Oracle liveness period, during which anyone can dispute the proposal.&#x20;

After requesting execution, the Snapshot proposal displays the date and time when the liveness period will expire. If the request is not disputed by this time, the requester's bond will be returned and the transactions will be able to be executed.

<figure><img src="../../.gitbook/assets/image (12).png" alt=""><figcaption></figcaption></figure>

After the challenge period has been completed, the Snapshot proposal gives the user the option to 'Execute transaction batch'. Signing this transaction will execute the transactions and return the bond to the requester.

<figure><img src="../../.gitbook/assets/image (2).png" alt=""><figcaption></figcaption></figure>

After executing our example proposal, the below shows the 0.000005 ETH transfer being executed.

<figure><img src="../../.gitbook/assets/image (2) (1).png" alt=""><figcaption></figcaption></figure>
