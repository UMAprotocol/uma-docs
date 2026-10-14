# Overview

Polymarket is migrating to new V2 contracts. The migration includes the following changes:&#x20;

* Polymarket’s all new V2 smart contracts connect to an upgraded Managed Optimistic Oracle V2 (MOOV2) through the new PolymarketOOReporter contract.
* Rule updates (aka clarifications) will now be stored on the MOOV2 contract
* Request ancillary data will now be json encoded
* Risk Labs is launching a Proposer API alongside this migration to solve toxic MEV proposal front running. **Proposers must submit Polymarket V2 proposals via the API. Onchain proposal transactions will be blocked.** Learn more [here](https://docs.risklabs.co/docs).

The Explorer and Voter dApps will be updated to provide minimal changes to a similar UX throughout the migration. However, the below changes should be reviewed by programmatic proposers, disputers, and third-party builders should review the below docs.

#### Timeline

Polymarket is planning to create canary markets (production markets, but not displayed on UIs) that have associated production optimistic oracle requests until October 30th, with a tentative switchover of net new markets on November 11th.&#x20;

#### Other Resources:

* [UMA Discord Migration Updates](https://discord.com/channels/718590743446290492/1123505654305390643)
* [Risk Labs Proposer API docs](https://docs.risklabs.co/docs)
* [Relevant UMA contracts](https://github.com/UMAprotocol/managed-oracle/tree/886c23b88e7210dc3eac16f03a900ac36811862d/src)
* [Polymarket Migration Docs](https://docs.polymarket.com/migrate/polymarket-v2/overview)
* [Polymarket V2 oracle contracts](https://github.com/Polymarket/polymarket-v2-external/tree/main/src/oracle)
