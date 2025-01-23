---
description: How to stake tokens, vote, and claim rewards
---

# Voting Walkthrough

This tutorial describes how to vote using the [UMA voter dApp](https://vote.uma.xyz/). For context, each voting period is 48hrs. Voting takes 3 steps:

* Commit Vote: the first 24hrs of a voting period allows you to encrypt and commit your vote
* Reveal vote: the second 24hrs of a voting period allows you to decrypt and reveal your vote. Votes are tallied by a DVM smart contract at the end of the reveal period.
* Claim rewards: staking rewards are constantly accrued. You can claim + restake or just claim them to your wallet whenever you like.

The purpose behind using the [Commit/Reveal scheme](https://www.gitcoin.co/blog/commit-reveal-scheme-on-ethereum) is that it allows votes to remain private.

### Instructions:

1. Go to [http://vote.uma.xyz](http://vote.uma.xyz).
2. Connect your wallet.
3. In order to vote, you must stake your tokens. Click "Stake/Unstake" then follow the instructions in the module. Staking your tokens does three things:
   * Your tokens will immediately start to earn a prorated portion of the $UMA token emissions, which is reflected as an APY on the dapp.
   * You will be able to vote with those tokens
   * You will have a 7-day cooldown period in order to unstake those tokens. During that period, your tokens will not be earning any rewards or be eligible for voting.

<figure><img src="../.gitbook/assets/image (3) (2).png" alt=""><figcaption><p>Indicate that you understand the cooldown period, approve, then stake your $UMA</p></figcaption></figure>

4. You can only vote when there is a live vote(s), and during the **Commit period.**&#x20;
5. Choose your vote(s) from the dropdown menu, then click "Commit vote" and send the transaction.&#x20;
6. Each voting round consists of a 24-hour commit period, followed by a 24-hour reveal period. **In order for your vote(s) to count, you need to return and reveal your vote(s) during the reveal period.**
7. During the reveal period, return to the Dapp and choose "Reveal" to reveal your vote(s).&#x20;

Once the dapp shows your vote as "Revealed," you are done with the voting process. The vote(s) will be finalized when the reveal period ends, at which point you will be able to see if you voted correctly. If you did, you will receive a bonus. If you voted inaccurately or failed to vote, you will have a penalty applied to your stake. This penalty amount is redistributed to accurate voters.

**Protocol Parameters (adjustable by governance)**

Emissions: .18 $UMA/second

Missed vote penalty: .05% of staked balance

Withdraw cooldown period: 7 days

### Claim rewards

Emission rewards begin to accrue automatically when you stake $UMA. You may claim these rewards to your wallet, or claim-and-stake to your staked balance.&#x20;

Rewards associated with voting successfully are automatically added to your staked balance after the voting period ends.\
