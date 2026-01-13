# Voting Gas Rebates

Risk Labs rebates the Ethereum gas fees voters pay to vote to encourage more participation in securing the Optimistic Oracle. Voters with ≥ 1,000 UMA staked are eligible.

**Voter eligibility:**&#x20;

* Voters must have at least 1000 UMA staked at the start of a commit phase for that round’s commit and reveal transactions to be eligible.

**Rebate Payments**

* Rebates are calculated for one calendar month and are sent out within the first half of the following month. Announcements are made in the UMA Discord at the time each rebate is sent out.
* Rebates are made in ETH.&#x20;
* Commit or reveal transactions sent via a delegate will be rebated to to the delegate address.

**Transaction eligibility:**

* Rebates cover commit and reveal transactions.&#x20;
  * Committed votes must be revealed to be rebated.&#x20;
  * If a voter commits more than once on a single vote, only the first commit will be rebated.
* Rebates do not cover token approval, stake/unstake, or rewards claiming transactions.

**What is covered:**

Rebates are calculated according to the two components of Ethereum gas fees:

* Base Fee: a network fee that is determined by block congestion. The Base Fee is calculated using a base price (gwei per gas) that is the same for all transactions within a given block.
  * Rebate coverage: full Base Fee with no cap.
* Priority Fee: an extra tip based on the max priority fee price set by the transaction sender. This tip determines how quickly a transaction is included onchain.
  * Rebate coverage: **Priority fees will only be rebated up to a price cap of 0.001 gwei per gas.** Eligible transactions that use a higher priority fee price will only be rebated up to 0.001 gwei per gas.\
    Note: the voting dapp suggests a priority fee of 0.001 gwei when sending a commit or reveal transaction to your wallet. Most wallets use this suggested fee as a default. Please review your wallet's gas fee settings when voting. &#x20;

**Reviewing your wallet's Priority Fee price:**

_Note: the below tutorial uses the desktop Metamask browser extension. UX will vary by wallet._

* When reviewing a commit or reveal transaction sent to your wallet by the voter dapp, your wallet should default to gas prices suggested by the voter dapp. This is shown by the red markup in the below screenshot. These suggestions will set the priority fee to 0.001 gwei per gas which will be fully rebated.&#x20;
* If you want to change these settings or verify the actual gas settings used, click the edit symbol (yellow markup in screenshot below).&#x20;

<figure><img src="../../.gitbook/assets/image (47).png" alt=""><figcaption></figcaption></figure>

* This will display different preset gas fee options (see screenshot below). Hover over the tooltip to see the gas price details for each option. In the below screenshot, the site suggested gas fee settings use a Priority Fee price of 0.001 gwei per gas.
* Click "Advanced" to set your own custom gas settings.

<figure><img src="../../.gitbook/assets/image (48).png" alt=""><figcaption></figcaption></figure>

* Here you can set custom gas prices. To be rebated for all gas spent, ensure the Priority Fee is not greater than 0.001 gwei.&#x20;

<figure><img src="../../.gitbook/assets/image (50).png" alt=""><figcaption></figcaption></figure>
