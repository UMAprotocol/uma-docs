# Optimistic Oracle v2

This section showcases different design patterns for building contracts that integrate with the UMA Optimistic Oracle (OO). These include:

* A [simple deposit box](solidity-examples.md) to showcase the basic OO request lifecycle.
* An [event based prediction market](in-depth-tutorial-event-based-prediction-market.md). In this example, settlement requests are submitted at the time of contract deployment, and the OO proposer network is used as a decentralized keeper service that identifies when settlement events happen and propose the outcomes.
* An ["internal Optimistic Oracle"](internal-optimistic-oracle.md), where an integrating contract handles proposals and state management, and only uses the OO when proposal verification is needed or a dispute is filed.

