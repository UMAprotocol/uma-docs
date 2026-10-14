# Smart Contract Overview

Polymarket’s all new V2 smart contracts connect to an upgraded Managed Optimistic Oracle V2 (MOOV2) through the new PolymarketOOReporter contract.

These smart contracts can be reviewed here:

* [Relevant UMA contracts](https://github.com/UMAprotocol/managed-oracle/tree/886c23b88e7210dc3eac16f03a900ac36811862d/src)
* [Polymarket V2 oracle contracts](https://github.com/Polymarket/polymarket-v2-external/tree/main/src/oracle)

Below is a summary of contracts and changes that may be relevant to programmatic proposers and disputers and 3rd party builders.

### New and Upgraded Contracts

#### OOReporterModule

The [OOReporterModule](https://github.com/Polymarket/polymarket-v2-external/blob/main/src/oracle/modules/OOReporterModule.sol) is Polymarket’s adapter between Polymarket's contracts and UMA’s contracts. It registers new markets and submits rule updates to the PolymarketOOReporter and reports UMA outcomes back to Polymarket.

#### PolymarketOOReporter

The [PolymarketOOReporter](https://github.com/UMAprotocol/managed-oracle/blob/886c23b88e7210dc3eac16f03a900ac36811862d/src/reporters/OOReporter.sol) is an adaptor for receiving Polymarket registrations. When Polyamrket creates a new market it is registered on the PolymarketOOReporter. An authorized intializer later creates a MOOV2 request for the registered market. The reporter also forwards rule updates, stores final UMA outcomes, re-requests after initial disputes and on too early settlements.&#x20;

#### ManagedOptimisticOracleV2 Upgrades

The existing MOOV2 deployment has been upgraded to this [version](https://github.com/UMAprotocol/managed-oracle/blob/886c23b88e7210dc3eac16f03a900ac36811862d/src/optimistic-oracle-v2/implementation/ManagedOptimisticOracleV2.sol) for the V2 migration.

The existing propose and dispute function signatures remain unchanged. For reporter-backed requests, the MOOV2 requester is the PolymarketOOReporter contract.

The relevant additions are:

* `updateRequestRules(identifier, ancillaryData, updatedRules)`: allows an authorized requester to record rule updates or clarifications.
* `getRequestRulesUpdates(requester, identifier, ancillaryData)`: returns the complete recorded clarification history.
* `getLatestRequestRulesUpdate(requester, identifier, ancillaryData)`: returns the latest recorded update; reverts if no update exists.
* `requestManagerSetReward(requester, identifier, timestamp, ancillaryData, newReward)`: allows an authorized request manager to change the reward while a concrete request is still in the Requested state, before a proposal is made.

Clarifications are stored separately from the original ancillary data. They do not change the bytes used to identify the request, and their history is shared across re-requests using the same requester, identifier, and ancillary data. Proposers and disputers must review any applicable updates when reviewing a request. Updates are full replacement of the initial market rules (aka anicallary data) and earlier updates.

### Mapping Polymarket Request IDs to MOOV2 Requests Onchain

Programmatic proposers and disputers may want to map a MOOV2 request to an associated Polymarket request ID which is used to identify resolution requests in Polymarket's contracts.

A concrete MOOV2 request is identified by `(requester, identifier, timestamp, ancillaryData)`. Polymarket’s requestId is a separate identifier.

#### From MOOV2 to Polymarket

Confirm that the MOOV2 `requester` is the expected PolymarketOOReporter address. Call `getRequestId(identifier, ancillaryData)` on that reporter, using the original ancillary-data bytes. This returns the canonical Polymarket request ID associated with those rules.

#### From Polymarket to MOOV2

Call `getRequest(requestId)` on the PolymarketOOReporter. Once initialized, its current MOOV2 request is:

| MOOV2 field     | Value                        |
| --------------- | ---------------------------- |
| `requester`     | PolymarketOOReporter address |
| `identifier`    | Returned `priceIdentifier`   |
| `timestamp`     | Returned `requestTimestamp`  |
| `ancillaryData` | Returned `requestRules`      |

Note: the MOOV2 requester is the reporter address, not the Polymarket OOReporterModule address.

#### Shared requests and replacement attempts

Multiple Polymarket request IDs can share one MOOV2 lifecycle. `getRequestId` returns the canonical ID, not every associated ID. Integrations needing the complete association should index the reporter’s `RequestRegistered` events and group registrations by reporter address, identifier, and original rules.

`getRequest` returns the current shared state. Each re-request advances the MOOV2 request timestamp while preserving the original identifier and ancillary data. Track `RequestInitialized` and `RequestRerequested` events, and retain the full original tuple when processing a historical proposal or dispute. A timestamp mismatch indicates an older attempt; it does not invalidate that historical request.

Always use the original ancillary data for identity lookups, rather than update text or re-serialized JSON.
