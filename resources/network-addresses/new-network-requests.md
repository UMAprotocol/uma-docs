# New Network Requests

The following are the evaluation criteria UMA considers when adding Optimistic Oracle support for a new network. UMA aims to support as many networks as possible to deliver the best experience for our integrating protocols and users. However, integrating a new network requires a significant investment. Therefore, the evaluation criteria were established to ensure that the benefits to UMA's integrating protocols and users outweigh the development, security, and operational costs of adding a new network.

Please note that these requirements are subject to change as the EVM (Ethereum Virtual Machine) and Layer 2 ecosystem continue to evolve. Meeting these requirements is the initial step in our evaluation process and does not guarantee support.

### Non-Negotiable Criteria:

**The below are a pre-requisite to any consideration:**

* EVM-Compatible Network
* ≥ 2 or more Independent RPC Node Providers (without block range limits for log queries)
* 30-Day Moving Average TVL on the network > $75M
* Block Explorer with Etherscan-Compatible API, contract verification and ABI fetching
* Availability of token mappings data source with Ethereum mainnet
* Dedicated support channel containing technical members of the network with priority support commitment to advise on contract development and debug data quality
* Subgraph support for testnet and mainnet
* Wallet connector libraries support for  testnet and mainnet

**Additional Criteria for oSnap Support:**

* Snapshot network support for testnet and mainnet
* SAFE App support for testnet and mainnet&#x20;
  * _Requires app opener with deep linking and transaction service_
* Zodiac support for testnet and mainnet&#x20;
* Support for [Covalent service](https://www.covalenthq.com/docs/networks/)&#x20;
  * _Required for balance fetching in transaction builder_

### Get in Touch with UMA

* Drop a message in the [UMA Discord Server ](https://discord.com/invite/jsb9XQJ)
* Send an email to integrations@umaproject.org
