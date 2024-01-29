---
description: Get Started with oSnap in 2 Minutes
---

# oSnap Quick Start

{% embed url="https://www.youtube.com/watch?v=tj_m6XMoPO4" %}
oSnap Deployment Speed Run
{% endembed %}

Welcome to the oSnap Quick Start guide: a high-level overview to start using oSnap!

“oSnap” is short for **O**ptimistic **Snap**shot Execution.&#x20;

oSnap enables DAOs to propose transactions, conduct an off-chain governance vote, and have the transaction data executed in a trustless fashion.&#x20;

<figure><img src="../../.gitbook/assets/oSnap Deployment Graphic.png" alt="" width="375"><figcaption></figcaption></figure>

## Prerequisites

* Snapshot Space&#x20;
* Safe with Multi-Sig (or Safe with on-chain governance)

## **Step-by-step instructions for using oSnap**

1. Create a Safe and Snapshot Space (or use existing accounts)
2. Navigate to Snapshot Space Advanced Settings
3. Add 'oSnap by UMA' plugin
4. Add your Safe treasury
5. Save revised Snapshot space settings by signing a message
6. Click 'Activate oSnap' from Treasury to access the oSnap Safe App
7. Sign transaction in Safe

## Create Your First Proposal

* Create a new Snapshot proposal with the transactions to execute if the proposal passes and is verified by UMA.
  * &#x20;:exclamation:_Note: Currently, oSnap supports basic voting._&#x20;
* Invite the community to vote on the proposal.
* If the proposal passes and default settings are enabled, UMA bots will execute a transaction proposing execution of the transactions and post a bond to UMA's Optimistic.&#x20;
* After the Optimistic Oracle verifies that the transactions and associated transactions are valid, UMA bots will execute these transactions for oSnap modules using default settings, provided they require less than 500,000 gas.&#x20;

## Get in Touch with UMA

* Drop a message in the [UMA Discord Server](https://discord.com/invite/jsb9XQJ)
* Send an email to integrations@umaproject.org
* Or drop your details in [this form](https://uma.xyz/osnap?modal=try-osnap) and we'll reach out
