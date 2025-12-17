---
description: oSnap Proposal Verification with UMA's Optimistic Oracle
---

# oSnap Proposal Verification

## Verifying Proposals&#x20;

After transactions have been requested for execution, the challenge period begins. The challenge period length is set by the oSnap Safe module advanced settings. Invalid requests must be disputed within the challenge period. oSnap requests can be reviewed in the [Optimistic Oracle dApp](https://oracle.umaproject.org/)'s Verify tab.&#x20;

<figure><img src="../../.gitbook/assets/image (22).png" alt=""><figcaption></figcaption></figure>

After selecting a request, a sidebar will show additional request details including the bond amount, when the challenge period ends, a dispute button (when a wallet is connected), a link to the corresponding Snapshot proposal, the oSnap rules that the request should be reviewed against (including minimum quorum and voting period for the Snapshot proposal), and links to relevant block explorer pages.

The explanation includes an IPFS hash that is a unique identifier for the Snapshot proposal and can be used to retrieve details of the proposal. For example, [https://ipfs.io/ipfs/bafkreie2hujdeenwz7dcbqcvwq37vjgu6g3alvkk5ysxbm4yqpeahm37xa](https://ipfs.io/ipfs/bafkreie2hujdeenwz7dcbqcvwq37vjgu6g3alvkk5ysxbm4yqpeahm37xa).

<figure><img src="../../.gitbook/assets/image (23).png" alt=""><figcaption></figcaption></figure>

## oSnap Proposal Pass Criteria

An oSnap proposal's voting results must meet both criteria below to be considered passed:

1. Quorum: FOR + AGAINST + ABSTAIN ≥ Required Quorum
2. 'FOR' majority: FOR > 50% \* (FOR + AGAINST + ABSTAIN)

## Disputing Proposals&#x20;

If the oSnap request does not meet the Rules, it should be disputed. Disputed requests can not be executed no matter how UMA resolves the dispute. This [section](../../using-uma/disputing-oracle-data.md) outlines the steps to dispute an invalid oSnap request, or any other invalid assertions to UMA's Optimistic Oracle.
