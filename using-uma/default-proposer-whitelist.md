# Default Proposer Whitelist

Risk Labs manages the default proposer whitelist for the ManagedOptimisticOracleV2 contract. The current whitelist can be viewed [here](https://polygonscan.com/address/0x9f35885ce8f67a942d7b2f4fbf937987da08c463#readContract#F1) and is subject to updates and changes in guidelines.

Note: the whitelist is not used to restrict proposals to known or trusted actors. The whitelist is used to only allow proposers who have been accurate in the past.&#x20;

### Current Proposer Whitelist Criteria

Proposer addresses must meet the two below criteria over the last 6 months:

* 5 or more Polymarket or Predict.fun proposals on OptimisticOracleV2 or ManagedOptimisticOracleV2
* Proposal accuracy greater than 95% in the last 6 months.

Proposers who base their proposal timing on other proposer's transactions in the public mempool (i.e. front run) will be removed from the whitelist.

### How do I get on the whitelist?

By proposing [OOV2 Polymarket requests](https://oracle.uma.xyz/propose?oracleType=Optimistic+Oracle+V2\&project=Polymarket) that are not whitelisted to meet the criteria for the next update.

### Who is on the whitelist? How many address are on the whitelist?

The current whitelist can be viewed [here](https://polygonscan.com/address/0x9f35885ce8f67a942d7b2f4fbf937987da08c463#readContract#F1)

### When will the whitelist be updated?

Updates are based on data pulled on the 2nd of each month for the preceding 6 months (e.g. June 2nd to December 2nd). The whitelist is updated within the following week.

### Are addresses removed from the whitelist?

Yes, if a whitelisted address no longer meets the criteria of the most recent update it will be removed from the whitelist.
