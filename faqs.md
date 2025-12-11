---
description: General questions about UMA OO
---

# FAQs

## General Questions

<details>

<summary><strong>What is UMA?</strong></summary>

UMA is a decentralized protocol designed to verify real-world information onchain in a trustless and secure manner. It consists of an **optimistic oracle (OO)** and a **data verification mechanism (DVM)** to help apps trustlessly verify real-world data using economic guarantees and community resolution.

</details>

<details>

<summary><strong>Why does UMA matter?</strong></summary>

Decentralized applications need trustworthy data to function. However, blockchains and smart contracts are inherently disconnected from real-world data. UMA bridges this gap by providing a scalable, secure, and cost-effective way to verify offchain (and onchain) information without relying on a centralized oracle.

</details>

<details>

<summary><strong>How does UMA compare to Chainlink?</strong></summary>

Chainlink streams data onchain at regular intervals. UMA is a request-based, dispute-driven system that verifies data only when needed, offering cost savings and support for broader data types.

</details>

<details>

<summary><strong>What makes UMA different from other oracles?</strong></summary>

Most oracles can only process purely objective data (e.g. price feeds). UMA is uniquely built to process and verify objective _and_ intersubjective data (e.g. sports scores, election outcomes, cultural events, etc.).

UMA’s OO enables smart contracts to verify intersubjective claims such as “Did the Eagles win the Super Bowl?”, “Did Donald Trump say ‘Tesla’ in the debate last night?” or “Did xyz proposal pass?" All these claims are verified using a decentralized dispute resolution system backed by economic guarantees.

Additionally, rather than pushing data onchain by default, UMA uses an optimistic system where proposed data is assumed correct unless challenged. This design allows for fast verification through the OO, while still enabling secure dispute resolution through the DVM. If a dispute is raised, a decentralized network of UMA token stakers vote on the correct answer using offchain information. This layered structure is fast when there’s consensus and robust when there’s conflict, making UMA more flexible, cost-effective, and secure than traditional oracles.

</details>

<details>

<summary><strong>Can UMA verify data about real-world events?</strong></summary>

Yes. UMA can verify outcomes of real-world events like elections, sports results, weather outcomes, or legal rulings inasmuch as there's public, credible evidence that voters can independently verify.

</details>

{% include ".gitbook/includes/untitled (1).md" %}

<details>

<summary><strong>Is UMA only for DeFi?</strong></summary>

No. UMA can be used anywhere trustless verification is needed. While its roots are in DeFi, UMA supports use cases across prediction markets, DAO governance, crosschain interop, \
insurance, gaming, and much more.

</details>

***

## How UMA Works

<details>

<summary><strong>What is the optimistic oracle?</strong></summary>

UMA’s [optimistic oracle (OO)](https://blog.uma.xyz/articles/what-is-umas-optimistic-oracle) is a decentralized verification protocol that lets anyone propose data onchain. If no one disputes the data within a predefined liveness period, it's considered verified. If disputed, the claim goes to a community vote via the DVM.

<figure><img src=".gitbook/assets/image (7).png" alt=""><figcaption></figcaption></figure>

</details>

<details>

<summary><strong>What is the DVM?</strong></summary>

The [Data Verification Mechanism (DVM)](https://docs.uma.xyz/protocol-overview/dvm-2.0) is UMA’s dispute resolution system. It uses a commit-reveal voting process where UMA tokenholders vote on the correct answer to a disputed data request. Honest voters earn rewards; incorrect or absent voters are penalized.

</details>

<details>

<summary><strong>How long does it take to verify data with UMA’s system?</strong></summary>

If undisputed, data is verified after the liveness period, which is configurable and should be set based on your use case. Currently, integrations set their liveness period between 2 hours to 2 days. If undisputed, the data is verified after the liveness period. In the rare case of a dispute, the DVM vote resolves the case within 2-4 days.

</details>

<details>

<summary><strong>What is the economic model behind UMA?</strong></summary>

UMA relies on the principle of economic guarantees. Economic incentives are in place for every participant, from proposers, disputers, and voters—to ensure that everyone behaves honestly. This economic model ensures that corrupting the protocol (e.g., by submitting false data or voting dishonestly) is more expensive than the potential profit from doing so.

</details>

<details>

<summary><strong>Can I trust UMA without understanding how it works?</strong></summary>

Yes, UMA is designed to be trustless. The economic game theory ensures that data is highly likely to be accurate, even if you don’t personally verify each step.

</details>

***

## oSnap and DAO Tooling

<details>

<summary><strong>What is oSnap?</strong></summary>

[oSnap](https://docs.uma.xyz/developers/osnap) is UMA’s governance automation tool. It connects Snapshot votes with onchain execution, letting DAOs run offchain votes and trustlessly execute them onchain with UMA’s OO securing the results. Notable users include Across, Developer DAO, Nexus Mutual, and many more.

<figure><img src=".gitbook/assets/image (2) (1).png" alt=""><figcaption></figcaption></figure>

</details>

<details>

<summary><strong>Does oSnap ensure the integrity of governance execution?</strong></summary>

Yes. By anchoring Snapshot votes to UMA’s decentralized verification process, oSnap ensures that governance outcomes cannot be tampered with between voting and execution.

</details>

<details>

<summary><strong>How does oSnap ensure secure governance execution?</strong></summary>

oSnap posts a bond-backed proposal to the OO after a Snapshot vote concludes. If no one disputes the validity of the vote outcome, the proposal is executed onchain automatically. If disputed, the DVM resolves it through tokenholder voting.

</details>

***

## UMA Participants (Proposers, Disputers, and Voters)

### Proposing Data

<details>

<summary><strong>What kinds of data can be proposed?</strong></summary>

Any peice of information that can be verified with public evidence or consensus: asset prices, sporting event outcomes, governance results, hack confirmations, crosschain transfers, and more.

</details>

<details>

<summary><strong>How do I propose data?</strong></summary>

Use the [UMA OO dApp](https://oracle.uma.xyz/propose) to submit data to the OO. Under the “Propose” tab, you can view the full list of live queries and choose which one you want to propose an answer to. You’ll need to post a bond and specify details for the request. If undisputed during the liveness window, the data is finalized and published onchain.

<figure><img src=".gitbook/assets/image (3) (1).png" alt=""><figcaption></figcaption></figure>

</details>

<details>

<summary><strong>What are the incentives for honest behavior as a proposer?</strong></summary>

When a proposer is correct, they receive a posted financial reward. However, when a proposer is incorrect, they lose their bond. This encourages honest participation. Dishonest participation can result in financial losses.

</details>

<details>

<summary><strong>What are the risks of participating as a proposer?</strong></summary>

If you propose incorrectly or too early, you risk losing your bond.

</details>

<details>

<summary><strong>Can I automate participation as a proposer?</strong></summary>

Yes. Bots can be written to monitor for proposals based on predefined logic. UMA encourages automation for higher participation, but does not provide these services.

</details>

### Disputing Data

<details>

<summary><strong>How do I dispute incorrect data?</strong></summary>

Submit a dispute through the [UMA OO dApp](https://oracle.uma.xyz/) within the liveness period. Under the “Verify” tab, you can view the full list of live proposals currently in their challenge period and choose which one you want to dispute. You'll need to post a bond and the request will escalate to the DVM for resolution.

<figure><img src=".gitbook/assets/image (4) (1).png" alt=""><figcaption></figcaption></figure>

</details>

<details>

<summary><strong>What are the incentives for honest behavior as a disputer?</strong></summary>

When a disputer is correct, they receive half of the proposer’s bond. However, when a disputer is incorrect, they lose their bond. This encourages honest participation. Dishonest participation can result in financial losses.

</details>

<details>

<summary><strong>What are the risks of participating as a disputer?</strong></summary>

If you dispute incorrectly, you risk losing your bond.

</details>

<details>

<summary><strong>Can I automate participation as a disputer?</strong></summary>

Yes. Bots can be written to monitor for proposals and auto-dispute based on predefined logic. UMA encourages automation for higher participation, but does not provide these services.

</details>

### Voting

<details>

<summary><strong>How do I vote on disputes?</strong></summary>

To vote on disputes and earn rewards, you must stake $UMA in the [UMA Voter dApp](https://vote.uma.xyz/). From there, you can commit and reveal your votes. You cal also view upcoming votes, your $UMA rewards, and voting history.

<figure><img src=".gitbook/assets/image (5) (1).png" alt=""><figcaption></figcaption></figure>

</details>

<details>

<summary><strong>What is the minimum UMA token amount needed to vote?</strong></summary>

There is no minimum. Your influence is proportional to the amount of tokens staked, but participation is open and inclusive.

{% hint style="info" %}
You need to stake a minimum of 1000 $UMA to receive voting gas rebates.
{% endhint %}

</details>

<details>

<summary><strong>How does DVM voting work?</strong></summary>

DVM voting uses a commit-reveal system:

1. **Commit**: Submit a hashed vote (24 hour window).
2. **Reveal**: Later, reveal your vote to show your answer (24 hour window).
3. **Resolve**: Rewards and slashing is based on voting accuracy.

Votes are shielded, meaning that voters cannot see each other's votes until after the reveal phase starts.

</details>

<details>

<summary><strong>How do voting rewards work?</strong></summary>

Rewards are paid in $UMA tokens. When you stake $UMA in the [UMA Voter dApp](https://vote.uma.xyz/), you start earning rewards automatically. Your staked tokens receive a share of UMA emissions, displayed as an APY in the dApp (\~17%).

Disputes come through the dApp in voting rounds, in which you are responsible for participating. If you vote incorrectly or miss a vote, you lose 0.1% of your staked $UMA per incorrect vote. If you vote accurately, you earn extra rewards in proportion to your staked $UMA. These rewards are collected from inaccurate and absent voters, and are redistributed to accurate voters at the end of each voting round.

**Note**: You must successfully commit and reveal your vote on time in order for your vote to count.

_For more info: read our full_ [_Voting FAQ_](https://blog.uma.xyz/articles/uma-voting-faq-vote-and-earn-like-a-pro)_._

</details>

<details>

<summary><strong>How is the correct answer decided in UMA’s voting system</strong></summary>

In UMA’s DVM, the correct vote is determined by majority consensus. During the voting process, $UMA token stakers participate in a shielded commit-reveal scheme to cast their votes on disputed assertions. After the reveal period, token-weighted votes are tallied, and if any single voting option reaches the consensus threshold (currently 65% of staked UMA), it is considered the correct outcome. This reflects the collective judgment of informed participants, incentivized to vote accurately through rewards and penalties.

Note that UMA uses the **Schelling Point** principle: the assumption that rational, well-informed voters will independently converge on the same answer. There’s no single source of truth, just the consensus of honest participants acting in their own best interest.

</details>

<details>

<summary><strong>How much can I earn for voting correctly?</strong></summary>

Voter APYs typically range between 16% and 21%, depending on emissions and participation. If you restake your rewards, you can capitalize on compound interest. Additionally, $UMA penalties from inaccurate and absent voters are redistributed as extra rewards to accurate voters at the end of each round.

</details>

<details>

<summary><strong>What happens if I vote incorrectly?</strong></summary>

Your staked $UMA tokens are slashed whenever you vote incorrectly. You lose approximately 0.1% of your staked amount of $UMA per vote.

</details>

<details>

<summary><strong>What happens if I miss a vote?</strong></summary>

Your staked $UMA tokens are slashed whenever you miss a vote. You lose approximately 0.1% of your staked amount of $UMA per vote

</details>

<details>

<summary><strong>What is P4?</strong></summary>

P4 is the voting option for when a proposal is submitted too early.

Many proposals get disputed for being too early. For example, someone may submit the Eagles as the winner of the Super Bowl five minutes _before_ the game ends. Even if the Eagles end up winning, they weren’t the winners at the time the proposal was submitted, therefore, the proposal is too early. In these situations, you should vote P4.

Always make sure to check the proposal timestamp and take it into consideration when voting.

_Learn more about P4_ [_here_](https://blog.uma.xyz/articles/what-is-p4)_._

<figure><img src=".gitbook/assets/image (7) (1).png" alt=""><figcaption></figcaption></figure>

</details>

<details>

<summary><strong>Voting gas is expensive. Do I get a refund?</strong></summary>

Yes! As long as you are staking a minimum of 1000 $UMA, you will receive an ETH gas rebate at the end of each month paid in full, gwei for gwei. See more details [here](https://docs.uma.xyz/using-uma/voting-walkthrough/voting-gas-rebates).

</details>

<details>

<summary><strong>Are there community discussions when voting on disputes?</strong></summary>

Yes! You can find live discussions in the [UMA Voter dApp](https://vote.uma.xyz/) and in the [UMA Discord server](https://discord.com/invite/39GFUAk5bA) in the #evidence-rationale channel.

</details>

<details>

<summary><strong>What are the incentives for honest behavior as a voter?</strong></summary>

Incorrect voters are slashed, losing 0.1% of their staked $UMA per incorrect vote. Correct voters receive the slashed $UMA as a reward (in addition to their emissions APY). Therefore voters are incentivized to vote with the majority. Since votes are committed in secret, voters cannot copy other voters, and instead vote for the best answer where other voters will congregate.

</details>

<details>

<summary><strong>What prevents a consensus of stakers corrupting an oracle result?</strong></summary>

Simply put, economic incentives. All voters have staked $UMA with a one week lock up. If they corrupt an oracle result, the value of their staked $UMA will fall drastically before the can unstake and sell. This aligns voters with the long-term success of UMA and the integrity of the Optimistic Oracle.

</details>

<details>

<summary><strong>What are the risks of voting?</strong></summary>

If you vote incorrectly or miss voting rounds, you lose approximately 0.1% of your staked amount of $UMA per vote.

</details>

<details>

<summary><strong>Can I automate participation as a voter?</strong></summary>

Yes. Bots can be written to cast votes based on predefined logic. UMA encourages automation for higher participation, but does not provide these services.

</details>

***

## Technical Questions

<details>

<summary><strong>What is intersubjective data</strong></summary>

Intersubjective data is information that may not be purely objective, but is still publicly verifiable and widely agreed upon by informed observers. It’s the sweet spot between “hard facts” and “personal opinion.” Things like election outcomes, sports results, or whether a protocol got hacked fall into this category.

</details>

<details>

<summary><strong>What is a liveness period, and how is it determined?</strong></summary>

The liveness period, also known as the “challenge period”, is a window of time after a data proposal during which someone can raise a dispute. This duration is configurable and often depends on the use case’s sensitivity or urgency.

The most common liveness period ranges from **2 hours to 2 days**. Many prediction markets, like Polymarket, use this configuration. Across Protocol also uses a 2-hour liveness period because it best suits its crosschain interop use case.

</details>

<details>

<summary><strong>What chains does UMA support?</strong></summary>

UMA is currently deployed and fully supported (including Oracle UI, DVM support, and oSnap functionality) on the following mainnets:

* Ethereum Mainnet
* Polygon Mainnet
* Optimism Mainnet
* Arbitrum Mainnet
* Base Mainnet
* Blast Mainnet
* Core Mainnet
* Story Mainnet

For the most up-to-date list of supported networks, including testnets and partially supported chains, you can find UMA's [network information here](/broken/pages/CGYkRbOnhJ483urKrE2v).

</details>

***

## Recent Optimistic Oracle Upgrades

<details>

<summary><strong>What does UMIP-183 do, and why is it useful?</strong></summary>

[UMIP-183](https://blog.uma.xyz/articles/what-is-umas-optimistic-oracle) introduced the MULTIPLE\_VALUES price identifier, allowing a single data request to return up to **seven values**. This is a major win for prediction markets, sports betting apps, and any protocol resolving multiple outcomes at once.

By batching outcomes into one transaction, protocols can:

* Drastically reduce gas costs.
* Simplify resolution logic.
* Settle multiple events faster and more efficiently.

Use cases like Polymarket and sports betting platforms directly benefit from this added scale and flexibility.

<figure><img src=".gitbook/assets/image (8).png" alt=""><figcaption></figcaption></figure>

</details>

<details>

<summary><strong>What is UMIP-185, and how does it improve performance?</strong></summary>

[UMIP-185](https://x.com/UMAprotocol/status/1914819386369573070) allows proposers to submit **hashed or offchain-referenced data** instead of full ancillary strings. This reduces the size of onchain transactions, which:

* Lowers gas fees.
* Speeds up proposal submissions.
* Makes oracle usage more cost-effective in congested markets.

This is especially helpful for integrations that involve complex or large datasets.

</details>

<details>

<summary><strong>What is the “slow proposer penalty,” and why was it added?</strong></summary>

To discourage latency in oracle activity, UMA introduced a **penalty for proposers who wait until the end of the liveness window** to submit data. This incentivizes faster responses and ensures that applications relying on the oracle don’t get bottlenecked by late proposals.

Faster proposers = faster oracle = better UX for users.

</details>

<details>

<summary><strong>How do these improvements affect the developer experience?</strong></summary>

All of these upgrades are designed to make UMA’s oracle more composable, efficient, and scalable. Whether you're resolving crosschain messages, prediction markets, or governance proposals, these tools help reduce cost and complexity while preserving trustlessness.

</details>

***

## Security

<details>

<summary><strong>How secure is UMA’s economic model?</strong></summary>

UMA’s security comes from game theory, not just code. The protocol is designed so that honest behavior is always the most profitable strategy and dishonest actions are economically irrational.

At the core of this is UMA’s use of the **Schelling Point principle**: rational voters, acting independently, are incentivized to report the truth because they know others will too. Those who vote correctly earn rewards. Those who vote incorrectly or don’t vote at all are penalized via slashing.

For an attacker to corrupt UMA’s oracle, they’d need to acquire **65% of all UMA tokens**, then convince a majority of that voting power to agree on a false outcome. Not only would this be prohibitively expensive, but it would also damage the value of the token they control.

This structure creates a powerful economic feedback loop:

* Truthful participation is rewarded.
* Dishonesty is costly.
* Corruption is uneconomical.

That’s how UMA turns incentive design into a security system at scale.

</details>

<details>

<summary><strong>How secure is the DVM?</strong></summary>

To corrupt the DVM, an attacker would need to acquire 65% of all UMA tokens and convince a majority of voting power to support a dishonest resolution. This would be extremely costly, and would cost far more than the potential upside of submitting incorrect data.

</details>

<details>

<summary><strong>What security audits has UMA undergone?</strong></summary>

At UMA, we take security seriously. The protocol has undergone multiple audits by reputable firms. Notably, OpenZeppelin conducted a comprehensive audit of UMA's Across V2 contracts, providing insights and recommendations to enhance security. Additionally, UMA maintains a bug bounty program to encourage responsible disclosure of vulnerabilities. Detailed information about UMA's security audits and bug bounty program can be found in the.

</details>

<details>

<summary><strong>How does UMA handle potential oracle manipulations or attacks?</strong></summary>

UMA employs a combination of economic incentives and community consensus to mitigate the risk of oracle manipulation:

* Participants proposing data are required to post bonds, which they forfeit if their data is successfully disputed. On the other hand, they earn rewards for proposing accurate data.
* Disputers must also post bonds, which they forfeit if their dispute is deemed inaccurate by the DVM. On the other hand, they earn rewards for disputing inaccurate proposals.
* Voters stake $UMA, which can be slashed if they vote inaccurately or are absent for voting rounds. On the other hand, they earn staking rewards for participating and voting accurately.
* $UMA stakers participate in securing the DVM by voting on oracle resolutions. While small slashing and reward mechanisms exist (\~0.1% of stake), the real security comes from the economic incentive to protect the system. If the DVM were corrupted, UMA’s value would likely collapse, meaning attackers would lose far more than they could gain.

This economic structure creates a financial deterrent against dishonest behavior. The system is built in such a way where honesty is profitable and lying is expensive.

</details>

***

## Compatibility and Limitations

<details>

<summary><strong>Are there any limitations on the types of data UMA can verify?</strong></summary>

UMA's oracle is designed to verify any data that is publicly available and independently verifiable. In particular, the OO and DVM are best built to process objective and intersubjective data. This includes a wide range of data types, such as asset prices, event outcomes, and real-world occurrences.

However, the OO is not suitable for data that is purely subjective or lacks a clear, verifiable source. For instance, personal opinions or unverifiable claims fall outside the scope of what UMA can accurately verify.​

</details>

<details>

<summary><strong>Is UMA compatible with other oracle systems?</strong></summary>

Yes, UMA is designed to be complementary to other oracle systems. It can function alongside traditional oracles, providing an additional layer of verification or serving as a fallback mechanism.

</details>

***

## Operational Edge Cases

<details>

<summary><strong>What happens if a vote in the DVM is inconclusive or tied?</strong></summary>

In the rare event of an inconclusive or tied vote within the DVM, the dispute is rolled over into the next voting round.

A rolled vote happens when a dispute doesn’t get resolved in the current voting round and gets pushed to the next one. This only happens if the vote doesn’t meet two quorum thresholds: **GAT** and **SPAT**.

* **GAT (God Awful Threshold)** is a fixed minimum amount of UMA that must vote for the result to count. In DVM 2.0, that number is 5 million UMA.
* **SPAT (Schelling Point Activation Threshold)** is a new rule in DVM 2.0. It requires at least 65% of staked $UMA to vote _and_ agree on the same outcome.

If either threshold isn’t met, the vote rolls into the next round. It's designed this way to make sure there's both broad participation and meaningful agreement before a dispute is finalized.​

</details>

<details>

<summary><strong>How does UMA handle network congestion or high gas periods?</strong></summary>

UMA is built to handle network congestion by operating across multiple L2s like Arbitrum and Optimism, reducing gas fees. On top of that, upgrades like **UMIP-185** improve efficiency by letting proposers use hashes or pointers instead of submitting full data onchain, cutting gas costs further. UMA also offers gas rebates for voters, so participation stays affordable even in high-fee environments.

</details>

***

## Building and Integrating with UMA

<details>

<summary><strong>What developer resources are available to get started?</strong></summary>

Visit[ docs.uma.xyz](https://docs.uma.xyz/) for integration guides, tutorials, contract templates, and walkthroughs. [UMA’s Discord](https://discord.com/invite/39GFUAk5bA) is also active for dev support.

</details>

<details>

<summary><strong>How do I integrate UMA’s optimistic oracle into my project?</strong></summary>

You can use UMA's OOV3 smart contracts to submit and verify data. Integration info and quick-start guides are available on UMA's [developer docs](https://docs.uma.xyz/developers/quick-start).

</details>

<details>

<summary><strong>Are there different versions of the optimistic oracle?</strong></summary>

Yes. UMA’s OO is constantly being improved to evolve and serve in-demand use cases over time. Currently, there are two versions of the OO available:

1. **OOV2**: Implements the use of data requests. Integrations must submit data requests that can then be answered by third-party proposers. Rules must be included in the request to establish the specific parameters for proposers and disputers to follow.
2. **OOV3**: Unlike OOV2, OOV3 doesn’t implement data requests. Instead, integrations skip that step and submit their own data proposals along with the rules. The challenge period and dispute process remains the same.

</details>

<details>

<summary><strong>Which version of UMA’s optimistic oracle should I use?</strong></summary>

The primary use cases for **OOV2** include prediction markets, sports betting applications, and insurance protocols.

The primary use cases for **OOV3** include crosschain infrastructure, content moderation, transaction verification, governance, and insurance protocols.

</details>

<details>

<summary><strong>How customizable is the UMA oracle’s logic for unique applications?</strong></summary>

Very. UMA was built with flexibility in mind. You can customize nearly every parameter of a request to fit your specific use case, including:

* **Question format:** You decide what you're asking and how it's phrased.
* **Liveness period:** Set how long others have to challenge your data.
* **Bond size:** Increase the required stake for proposers or disputers to raise the cost of dishonest participation.
* **Resolution criteria:** Add detailed instructions or requirements to help voters resolve disputes clearly and accurately.
* **Data format:** You define the format of the data to be verified (e.g. booleans, multiple-choice,  integers, or encoded data using upgrades like UMIP-185).

</details>

<details>

<summary><strong>Can I test UMA locally or on testnet?</strong></summary>

Yes. UMA offers contracts on major Ethereum testnets. Developers can simulate oracle requests, disputes, bonding, and DVM voting in staging environments. Here is the full list of supported testnet chains:

* Ethereum Sepolia
* Arbitrum Sepolia
* Optimism Sepolia
* Polygon Mumbai
* Polygon Amoy
* Base Sepolia
* Blast Sepolia
* Core Testnet
* Story Odyssey

You can view the full list of supported chains [here](https://docs.uma.xyz/resources/network-addresses).

</details>

<details>

<summary><strong>What are common use cases for UMA?</strong></summary>

* Prediction markets (e.g. Polymarket)
* Crosschain bridges (e.g. Across Protocol)
* DAO governance (e.g. oSnap with Nexus Mutual, Developer DAO)
* IP dispute verification (e.g. Story Protocol)
* AI agent verification (e.g. Axal)
* Insurance protocol

</details>

<details>

<summary><strong>Why would I use UMA instead of a traditional price feed oracle?</strong></summary>

Price feed oracles can only handle one type of data: prices. UMA is built to handle any publicly verifiable information.

That’s why UMA is the go-to oracle for onchain applications such as prediction markets, crosschain interop protocols, IP ownership layers, and many more—because each of these rely on access to verified **intersubjective data**. Price feed oracles simply can’t provide that. UMA can.

Additionally, most traditional price feed oracles constantly push data onchain, even when it isn’t needed. This wastes gas and introduces attack vectors. UMA’s OO only verifies data when requested, making it more gas-efficient and censorship-resistant. It also supports **any** type of data, not just prices, allowing for use cases like governance execution, crosschain messages, and real-world event verification.

There’s also a major cost advantage. Setting up and maintaining a price feed on traditional oracles is expensive. It often requires upfront coordination, whitelisting, and ongoing fees. UMA is **fully permissionless and free to use**. There are no fees to access the oracle. You only pay for gas and bonding if you interact with it directly.

</details>

<details>

<summary><strong>How does Polymarket use UMA?</strong></summary>

Polymarket leverages UMA's OOV2 to resolve the outcomes of its prediction markets in a decentralized and trustless manner. When Polymarket creates a prediction market, a data query is sent to the OO. Anyone can then propose the outcome to UMA's OO, which assumes the proposed result is correct unless challenged within a 48-hour predefined dispute window.

If a dispute arises, it is escalated to UMA's DVM, where $UMA token holders vote to determine the accurate outcome. This system allows Polymarket to handle a wide range of markets, including those based on natural language and intersubjective data, which traditional oracles cannot process.

<figure><img src=".gitbook/assets/image (9).png" alt=""><figcaption></figcaption></figure>

</details>

<details>

<summary><strong>How does Across use UMA?</strong></summary>

Across is a [crosschain bridge](https://across.to/) that uses a unique variation of UMA’s OOV3 to verify crosschain Intents, which are statements of a user’s bridging transaction, before relayers are repaid. This gives Across security guarantees without needing to validate every transaction in real-time.

</details>

***

## Governance

<details>

<summary><strong>How is UMA governed?</strong></summary>

UMA is governed by its community of $UMA tokenholders.

Major protocol upgrades, parameter changes, and funding decisions are proposed, discussed, and voted on by the UMA community, primarily through offchain Snapshot votes and confirmed onchain when needed.

Proposal discussions are hosted on [UMA’s Discourse page](https://discourse.uma.xyz/top?period=all).

</details>

<details>

<summary><strong>How can I participate in UMA governance?</strong></summary>

To participate, hold $UMA, stake it to vote, and take part in governance proposals through the [UMA Voter dApp](https://vote.uma.xyz/) and [UMA’s Snapshot Space](https://snapshot.box/#/s:uma.eth). Important proposals are discussed in the UMA Discord and forum before voting begins.

</details>

<details>

<summary><strong>What kinds of decisions are made through governance?</strong></summary>

Governance decisions include protocol upgrades, parameter changes (such as inflation rates or bond sizes), budget allocations, grants, and strategic initiatives for UMA’s growth and development.

</details>

<details>

<summary><strong>How does UMA handle proposals to change protocol parameters?</strong></summary>

Changes to UMA’s core protocol, such as inflation rates, contract upgrades, or bond sizes, are governed directly by $UMA tokenholders. Proposals are typically discussed in the community forum and Discord, then voted on through the [UMA Voter dApp](https://vote.uma.xyz/), which uses the same secure commit-reveal process as oracle dispute resolution.

While Snapshot can be used for informal temperature checks, it is **not** where binding decisions happen. Final decisions are made through onchain voting via the DVM, ensuring that changes are trustlessly approved by the UMA community.

</details>

<details>

<summary><strong>What is the difference between dispute resolutions and governance votes on the Voter Dapp?</strong></summary>

Unlike dispute resolutions, voters who vote against the consensus on governance votes are not slashed. If a voter does not submit a vote on a governance vote, they are still slashed. This allows voters to vote their personal opinions without the economic disincentive of slashing.

</details>

<details>

<summary><strong>What is the difference between protocol governance and oSnap execution?</strong></summary>

Protocol governance refers to changing the UMA protocol itself (e.g. inflation rate, contract upgrades, etc.). oSnap is a tool built with the OO and used to execute DAO votes trustlessly. oSnap can be used by any DAO; the UMA DAO and many other notable DAOs currently uses oSnap.

</details>

<details>

<summary><strong>What is the role of the DVM in UMA governance?</strong></summary>

The DVM is the venue where **all binding governance votes take place**. It’s the same secure, commit-reveal voting system used to resolve oracle disputes, except instead of resolving data proposals, it’s used to approve or reject protocol upgrades, parameter changes, and funding decisions.

In UMA, the DVM is more than a dispute resolution system. It’s the foundation of onchain governance. Every major decision made by the DAO goes through the DVM, giving $UMA tokenholders full control over the protocol’s future.

</details>

***

## Tokenomics

<details>

<summary><strong>What is the $UMA token used for?</strong></summary>

$UMA is the core utility and governance token of the protocol. It’s used to stake, vote, and secure the oracle. Tokenholders help verify data, resolve disputes, and steer the protocol’s direction by participating in governance. In short: UMA aligns incentives around truth.

The value of the $UMA token is directly tied to its role in maintaining honest oracle outcomes and decentralized governance.

</details>

<details>

<summary><strong>What is the total supply and inflation schedule of the UMA token?</strong></summary>

The supply of $UMA isn’t fixed. It’s dynamic and governed by the DAO. The protocol uses controlled inflation to incentivize participation and long-term security. The current inflation rate is set at **0.05% of the total token supply**, distributed to active, correct voters in the DVM.

_Learn more about the $UMA token supply_ [_here_](https://blog.uma.xyz/articles/uma-token-supply-disclosure)_._

</details>

<details>

<summary><strong>How are new UMA tokens distributed?</strong></summary>

Token emissions are controlled by governance. UMA emissions are used to provide staking rewards to stakers who vote on disputes to secure the protocol.

</details>

<details>

<summary><strong>Where can I track UMA's token metrics?</strong></summary>

The current supply of $UMA and emission rates are defined onchain and can be found [here](https://etherscan.io/address/0x7b292034084A41B9D441B71b6E3557Edd0463fa8#code)**.**&#x20;

</details>

***

## Community and Ecosystem

<details>

<summary><strong>How can I contribute to UMA beyond voting?</strong></summary>

There are tons of ways to get involved! You can build integrations, write code, create content, improve the docs, join governance discussions, or just hang out in the [UMA Discord](https://discord.gg/uma) and vibe with the community. Whether you're technical or not, there's space for you to contribute.

</details>

<details>

<summary><strong>Are there developer grants available?</strong></summary>

Yes. UMA offers grants to developers and teams building tools, integrations, or initiatives that advance the ecosystem. To apply, you’ll start by posting a proposal in the[ Funding Proposals](https://discourse.uma.xyz/c/uma-protocol/funding-proposals/41) category of UMA’s governance forum. From there, the community will review and provide feedback.

If the proposal gains traction, it can proceed to a Snapshot vote, and if successful, be submitted onchain with help from Risk Labs or directly by the proposer. All grant proposals are evaluated based on their technical feasibility, ecosystem impact, and funding requirements. If you have any questions about our grant program, please reach out to us in our [Discord](https://discord.gg/uma).

Yes. UMA offers grants to support developers and teams building tools, integrations, or infrastructure that strengthens the ecosystem.

To apply, start by posting your idea in the [Funding Proposals](https://discourse.uma.xyz/c/uma-protocol/funding-proposals/41) category in the UMA governance forum. The community will review and provide feedback. If your proposal gains traction, it can move to a Snapshot vote, and if approved, it’ll execute onchain with help from Risk Labs or directly by the proposer.

Grants are evaluated based on:

* Technical feasibility
* Ecosystem impact
* Budget/funding needs

Have questions? Come chat with us in [Discord](https://discord.gg/uma).

</details>

<details>

<summary><strong>What community resources exist?</strong></summary>

Here’s where you can plug into UMA’s community and stay up to date:

* [UMA Discord](https://discord.gg/uma) – Join conversations, ask questions, or just vibe.
* [UMA Discourse](https://discourse.uma.xyz/) – Home of governance discussions and funding proposals.
* [UMA Snapshot Space](https://snapshot.box/#/s:uma.eth) – Where tokenholders vote on proposals.
* [UMA Docs](https://docs.uma.xyz/) – Developer documentation, guides, and protocol deep dives.
* [UMA Blog](https://blog.uma.xyz/) – Insights, announcements, and ecosystem updates.

</details>
