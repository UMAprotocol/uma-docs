---
description: A decentralized truth machine
---

# Welcome to UMA

**An optimistic oracle (OO) that can record any verifiable truth or data onto a blockchain** :crystal\_ball:

UMA is an optimistic oracle and dispute arbitration system that securely allows for arbitrary types of data to be brought onchain. UMA’s oracle system provides verified onchain data for projects including:&#x20;

1. Crosschain bridge
2. Prediction markets
3. Insurance protocols
4. Custom derivatives&#x20;

UMA's optimistic oracle (OO) is designed to be modular and extensible with focus on building real-world use cases. Two main versions of the optimistic oracle (OO) are currently live: **OOv2** and **OOv3**. Choosing the right one depends on your use case.&#x20;

<figure><img src=".gitbook/assets/For Kanishk (1).png" alt=""><figcaption></figcaption></figure>

***

## Core Concepts

Learn the fundamentals of UMA's optimistic oracle.

<table data-view="cards"><thead><tr><th></th><th></th><th data-hidden data-card-cover data-type="files"></th><th data-hidden data-card-target data-type="content-ref"></th></tr></thead><tbody><tr><td><strong>Optimistic Oracle V2</strong></td><td>Build prediction markets and insurance protocols</td><td><a href=".gitbook/assets/OOv2.png">OOv2.png</a></td><td><a href="developers/optimistic-oracle/">optimistic-oracle</a></td></tr><tr><td><strong>Optimistic Oracle V3</strong></td><td>Build data asserters and escalation managers</td><td><a href=".gitbook/assets/OOv3.png">OOv3.png</a></td><td><a href="developers/optimistic-oracle-v3/">optimistic-oracle-v3</a></td></tr><tr><td><strong>Voting Walkthrough</strong></td><td>Learn how to stake tokens, vote, and claim rewards</td><td><a href=".gitbook/assets/voting.png">voting.png</a></td><td><a href="using-uma/voting-walkthrough/">voting-walkthrough</a></td></tr></tbody></table>

### Correct Oracle for Your Use Case

UMA offers OOv2 and OOv3. Based on your idea and product, let's learn which one fits your use case best:

|                                              OOv2                                              |                                                   OOv3                                                  |
| :--------------------------------------------------------------------------------------------: | :-----------------------------------------------------------------------------------------------------: |
|   Integrations must submit data requests that can then be answered by third-party proposers.   | Integrations submit assertions, where they propose data to their own request along with its parameters. |
| The specific parameters for proposers and disputers to follow must be included in the request. |                        The challenge period and dispute process remains the same.                       |
|  Primary use cases: Prediction markets, sports betting applications, and insurance protocols.  |       Primary use cases: Crosschain infrastructure, content moderation, transaction verification.       |

We understand that your product needs can be complex and would require a larger discussion on which optimistic oracle version to use. Feel free to reach out to us [here](resources/links.md#links) and get help.

***

## Developer Quickstart

Having decided the correct optimistic oracle for your use case, let's now dive deeper into how you can ship apps powered by verifiable onchain truth using UMA's optimistic oracle.

{% tabs %}
{% tab title="OOv2" %}
OOv2 prioritizes protocols that need third-parties to propose answers to data requests.&#x20;

Here are some app guides to help you build on UMA OOv2:

<table data-card-size="large" data-view="cards"><thead><tr><th></th><th></th><th data-hidden data-card-cover data-type="files"></th></tr></thead><tbody><tr><td><strong>Prediction Market</strong></td><td>Use OOv2 to identify and settle real-world events in prediction markets.</td><td><a href=".gitbook/assets/Dev Quickstart - Prediction Markets.png">Dev Quickstart - Prediction Markets.png</a></td></tr><tr><td><strong>Insurance</strong></td><td>Use OOv2 to verify and resolve claims in insurance applications.</td><td><a href=".gitbook/assets/Dev Quickstart - Insurance.png">Dev Quickstart - Insurance.png</a></td></tr></tbody></table>
{% endtab %}

{% tab title="OOv3" %}
OOv3 prioritizes protocols that simply needs asserted data to be verified.&#x20;

Here are some app guides to help you build on UMA OOv3:

<table data-card-size="large" data-view="cards"><thead><tr><th></th><th></th><th data-hidden data-card-cover data-type="files"></th></tr></thead><tbody><tr><td><strong>Data Asserter</strong></td><td>Use OOv3 to asserts arbitrary offchain data onchain through flexible assertions</td><td><a href=".gitbook/assets/Dev Quickstart - Data assertion.png">Dev Quickstart - Data assertion.png</a></td></tr><tr><td><strong>Escalation Managers</strong></td><td>Use OOv3 to enable modular functionality via custom escalation managers</td><td><a href=".gitbook/assets/Dev Quickstart - Escalation managers.png">Dev Quickstart - Escalation managers.png</a></td></tr></tbody></table>
{% endtab %}
{% endtabs %}

***

## Community

The UMA token secures UMA’s optimistic oracle through decentralized governance and economic guarantees against corruption. Token holders vote on upgrades, price requests, and disputes, earning rewards for honest participation.

**UMA Improvement Proposals (UMIPs)** are design documents used to propose changes to the UMA ecosystem. They are an important part of UMA's governance processes.&#x20;

The **UMA DAO** accepts proposals for onchain actions that require tokenholders approval. An example would be a request for funding from the UMA treasury.

<table data-view="cards" data-full-width="false"><thead><tr><th></th><th></th><th data-hidden data-card-cover data-type="files"></th></tr></thead><tbody><tr><td><strong>Governance</strong></td><td>Learn about UMA token, UMIPs and voting on UMA's DVM</td><td><a href=".gitbook/assets/01UMA governance.png">01UMA governance.png</a></td></tr><tr><td><strong>UMIP Process</strong></td><td>Learn how UMIPs propose changes to the UMA Ecosystem</td><td><a href=".gitbook/assets/02UMIP process.png">02UMIP process.png</a></td></tr><tr><td><strong>DAO Proposals</strong></td><td>Learn how you can submit onchain proposals to the UMA Ecosystem</td><td><a href=".gitbook/assets/03DAO proposal.png">03DAO proposal.png</a></td></tr></tbody></table>

***

## Resources and Support

We request all community members and developers to [join our discord server](https://discord.umaproject.org/) and gain instant help on any issues they face while voting or submitting proposals to UMA.

You can [find more support resources here](/broken/pages/CGYkRbOnhJ483urKrE2v).

The security of our protocol is extremely crucial. If you notice any bugs in the protocol, [please report them here](resources/audit-and-bug-bounty-programs.md#submissions). You can also find details of our [audits here](resources/audit-and-bug-bounty-programs.md).
