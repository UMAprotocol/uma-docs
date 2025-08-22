# Proposing Programmatically

Proposing and disputing ManagedOptimisticOracleV2 requests is largely the same as OptimisticOracleV2 requests with the following differences:

#### Adding the ManagedOptimisticOracleV2 Addresses

Programmatic proposers and disputers will be used to listening for OptimisticOracleV2 events and sending proposing and disputing transactions to these addresses. To add support to new ManagedOptimisticOracleV2 deployments, programmatic proposers and disputers will have to add these new deployment addresses to their bot configs.&#x20;

Note: the first ManagedOptimisticOracleV2 deployment is managed by Polymarket and can be found [here](https://polygonscan.com/address/0x2C0367a9DB231dDeBd88a94b4f6461a6e47C58B1).

#### Viewing a Request's Proposer Whitelist

ManagedOptimisticOracleV2 requests have defined proposer whitelists. The whitelist for a given request can be found by calling the [`getProposerWhitelistWithEnforcementStatus`](https://github.com/UMAprotocol/managed-oracle/blob/fc03083eca91c880efa8918c6d9532af9362f00d/src/optimistic-oracle-v2/implementation/ManagedOptimisticOracleV2.sol#L381C14-L381C55) function with the request's `requester` , `identifier` , and `ancillaryData`. The function will return  an `isEnforced` boolean that defines whether a whitelist is enforced and an address array of `allowedProposers` . If `isEnforced` is true, then only `allowedProposers` may propose the request. If `isEnforced` is false, any address will be able to propose.&#x20;

When a new request is created on ManagedOptimisticOracleV2 it will default to the `defaultProposerWhitelist` unless overridden by the `requestManager` . This default whitelist can be viewed with the following steps:

1. Call the [`defaultProposerWhitelist`](https://github.com/UMAprotocol/managed-oracle/blob/fc03083eca91c880efa8918c6d9532af9362f00d/src/optimistic-oracle-v2/implementation/ManagedOptimisticOracleV2.sol#L94) view function on a deployed ManagedOptimisticOracleV2 contract to get the address of the default whitelist.
2. Call [`getWhitelist`](https://github.com/UMAprotocol/managed-oracle/blob/fc03083eca91c880efa8918c6d9532af9362f00d/src/common/implementation/AddressWhitelist.sol#L79) on the default whitelist address to view all address on the whitelist. Alternatively, call [`isOnWhitelist`](https://github.com/UMAprotocol/managed-oracle/blob/fc03083eca91c880efa8918c6d9532af9362f00d/src/common/implementation/AddressWhitelist.sol#L67)  to check if a given address is on the whitelist.

#### Proposing on ManagedOptimisticOracleV2

Proposing on ManagedOptimisticOracleV2 can be done by calling the same [`proposePrice`](https://github.com/UMAprotocol/managed-oracle/blob/fc03083eca91c880efa8918c6d9532af9362f00d/src/optimistic-oracle-v2/implementation/OptimisticOracleV2.sol#L382) or [`proposePriceFor`](https://github.com/UMAprotocol/managed-oracle/blob/fc03083eca91c880efa8918c6d9532af9362f00d/src/optimistic-oracle-v2/implementation/ManagedOptimisticOracleV2.sol#L327)  functions with the same arguments  as proposing on OptimisticOracleV2. Proposal transactions from addresses that are not whitelisted will revert with the following error messages, `"Sender not whitelisted"` or, `"Proposer not whitelisted"`.

#### Listening for ManagedOptimisticOracleV2 Events

The emitted events that are relevant to oracle participants (`RequestPrice, ProposePrice and DisputePrice, SettlePrice`) are unchanged from the OptimisticOracleV2 contract. These events can be queried via RPCs or by using our [deployed subgraph](https://github.com/UMAprotocol/subgraphs/blob/master/README.md#managed-optimistic-oracle-v2-events-and-calls) that indexes ManagedOptimisticOracleV2 events.
