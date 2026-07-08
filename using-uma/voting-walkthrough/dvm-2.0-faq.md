---
description: Frequently asked questions about the new UMA DVM 2.0
---

# DVM 2.0 FAQ

### What kind of APY can I expect to receive for staking?

To learn more about how exact APY is calculated, you can refer [here](dvm-2.0.md#staking-apy).

At a high level though, all UMA voters on average will receive an APY determined by the annual UMA emissions amount divided by the average total UMA staked over the year.

Before the DVM 2.0 upgrade, approximately 18-20mm UMA was voting on average for each vote. If this holds relatively constant, it means that average UMA voter APYs will be approximately 28-32%. This can of course fluctuate on an individual voter basis dependent on voter performance.

### How do I maximize my APY?

Maximizing your APY should be simple. To maximize APY you should:

* Remain staked within the system.
* Claim and restake your rewards at times when it makes sense based off balancing your increased stake’s additional future rewards received against gas costs of claiming and restaking.
* Vote consistently and carefully. Incorrect or absent voter stakes are slashed and distributed to correct / participating voters after each voting round.

### What is the current UMA voter rewards emission rate?

The UMA emission rate is currently 0.155 UMA/second. This will be distributed pro-rata to all stakers within the UMA system on a per second basis. The UMA emissions rate is controlled by UMA governance and can be updated at any time by a DVM vote.

### What is the unstake timer? Why does this exist?

The unstake timer is a set amount of time that a staker must wait before they can execute a request to unstake from the UMA system. With the initial DVM 2.0 upgrade, this unstake timer was set to 7 days.

The unstake timer exists to make the UMA voting system more hardened against attempted manipulation attacks. UMA has always had an assumption that if a malicious attacker could manage to either vote with or bribe a majority of voters to vote incorrectly on a dispute, the UMA token would go to zero in value. This is known as the UMA cost of corruption.

In practice, this is probably not entirely true as markets are not entirely efficient. Attackers could likely dump voted with tokens for some amount of value after a successful attack. Having a required 7 hold period more strongly enforces that UMA’s cost of corruption is enforced by the market if a manipulation attempt is successful.

### What kind of things does the DVM vote on?

The Optimistic Oracle & DVM can provide all kinds of different data, from crypto prices, to sports, to verifying if a bridging transaction was valid, with new use cases being added all the time, so its hard to anticipate precisely what you might be asked to vote on, however all questions will be asking something about the state of the world at a particular time, indicated by the timestamp.

### How should I determine what I should vote?

There are two primary data sources to assist a voter in determining what value they should vote with.

First, the UMIP that the DVM request references. This will tell you general information about how that question should be answered. The second place to check is the ancillary data of the question. This will provide more context for the question and is likely to include a source, which a link that will take you to a place where that data is likely to be published.

Disputes are also discussed at length in the [UMA Discord](https://discord.com/invite/jsb9XQJ) #voting-chat and #evidence-rationale channels.

### Why do I have to commit and reveal votes?

The commit/reveal cycle prevents people from seeing how others have voted by encrypting all initial votes and only decrypting them once all votes are cast.  The oracle is built on Schelling Point theory, which indicates that the most likely consensus of non-colluding participants is the truth.  By obscuring how other people have voted, this ensures that each tokenholder votes independently.&#x20;

### Can I vote with tokens that haven’t been staked?&#x20;

No. Only tokens that have been staked can be voted with. If you have already initiated the unstaking process for any or all of your coins, they also will not receive rewards or be able to be voted with. However if you have unstaked coins when a vote starts, you can stake your coins any time up to the end of the commit period, and they will be eligible to vote with.

### Can I vote while I am unstaking?

Unstaked tokens cannot vote. This includes tokens that are in the “cooldown period” after an unstake request has been submitted.

### Can I cancel an unstake request while it is pending?

You cannot. Unstake requests cannot be canceled once submitted. If you once again wish for those tokens to be staked, you can wait for the unstake period to finish, claim your tokens and restake them.

### Can I get someone else to vote on my behalf?

It is possible to delegate your staked UMA in one wallet to a different wallet, allowing voters to keep their UMA on a cold wallet and vote with hot wallet, however there is a 1-1 relationship between voting wallets and staking wallets, therefore it is not possible for one person to vote on behalf of multiple other people.

### Do I still get gas rebates?

Yes! Risk Labs will still be continuing with the existing voting gas rebate program.

### I still don't know how this works - where can I get help?&#x20;

Jump into our [discord](https://discord.com/invite/jsb9XQJ) and ask questions about anything that you are unsure of.
