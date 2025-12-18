---
description: The next iteration of the UMA DVM
---

# DVM 2.0

The UMA DVM is the arbitrator for the Optimistic Oracle and other UMA ecosystem contracts. It facilitates dispute resolution wherein UMA token holders vote in a commit reveal schelling point mechanism. For an overview of how the DVM see [here. ](https://docs.umaproject.org/protocol-overview/how-does-umas-oracle-work#umas-data-verification-mechanism)The DVM has been rebuilt with the new iteration being released in Q1 of 2023.&#x20;

At a high level, the upgrade adds a new staking and slashing mechanism wherein voters earn a pro-rata share of emissions for staking and are slashed for voting wrong (or not voting). This upgrade also reworks a number of other DVM contracts such as the governor, proposer contract and a new designated voting contract. Finally, this upgrade adds a new emergency recovery mechanism to the DVM that enables admin proposals to bypass the DVM's schelling point oracle system in the event of an emergency. More detail on the individual changes are broken down in the sections that follow.&#x20;

This doc page outlines some of the changes between DVM 1.0 and DVM 2.0 and some other relevant implementation details.

### Mechanism changes & UMA tokenomics update

The DVM2.0 introduces three key changes to how the UMA token interacts with DVM system:

1. Voters now must **stake UMA in the DVM** to participate in the schelling point and to receive rewards. &#x20;
2. Voters now **earn a pro-rata share of a fixed emission rate** simply for staking their tokens. Staking rewards are emitted at a constant rate per block. This means you can work out the overall UMA inflation over time and stakers can easily work out their APY for staking in the system. In comparison to the previous inflation system, the total inflation rate is no longer a factor of the number of votes held by the DVM. The pro-rata share of tokens received by stakers is independent of their voting performance. The emissions rate is set through UMA governance.&#x20;
3. Voters' staked balances are also now  **susceptible to slashing**. The slashing mechanism redistributes tokens from inactive and wrong voters to the stakers who voted correctly. This hardens the schelling point by adding a more punitive cost function to being wrong. Governance votes are treated as a special price request category where the slashing mechanism is applied to inactive voters, but not wrong voters.

### Vote Delegation

First class vote delegation support enabling a 1 to 1 relationship between the delegator and delegate wallets. This lets a voter delegate from a secure cold storage wallet to a hot wallet. The hot wallet can commit, reveal and claim and re-stake their rewards but cannot access the underlying stake. If desired, more complex delegation systems (pooled UMA staking with delegate voting etc) can be built on top of this externally to the core UMA contracts.

### Emergency recovery system

The DVM 2.0 now has an emergency recovery mechanism where bonded emergency proposals can be executed to short-circuiting and bypass the normal voting mechanism. This enables the DVM to recover from any kind of internal failure mode that could occur (breakage in the commit reveal system, contract issues or other) through a permissionless upgrade flow.

### Other system wide changes

There are a number of other changes made, including:

1. Governance proposals now include ancillary data enabling better identifying information to be passed along to voters.
2. Price requests now contain a unique identifier, enabling easier tracking and support in front ends.
3. A number of gas optimizations were made throughout the protocol.

### Implementation details

The sections below contain some details on nuanced implementation details of the DVM2.0 system.

#### Staking APY

Staking rewards (and the associated APY) consists of two discrete components: a) rewards from being **staked** and b) balance changes from **slashing**. The net staking APY you receive considers both of these values. Let's consider them separately.

**Staking rewards** are a pro-rata share of a fixed emission rate, calculated on a per block basis. The rewards you are entitled to, from the last time you claimed, are calculated as follows:

$$
stakingRewards = \frac{stakedDuration*stake*emissionsPerBlock}{cumulativeStake}
$$

**Slashing rewards & penalties** at the time of DVM 2.0 launch are designed to be as simple as possible, with the ability to update them at a later point through the use of a Slashing Library. At launch, slashing penalties will be approximately equal to the rewards one would have earned for an estimated stake duration. This has been targeted based off historical DVM 1.0 vote frequency, and means that someone who stakes but does not participate in any votes (or gets all votes wrong) should have their slashed stake and accumulated rewards cancel out for a 0% APY. This amounts to losing 0.1% of your staked amount per vote that is incorrect or missed. Note that each voting round (48 hour commit-reveal cycle) can have multiple votes in it (for example multiple price requests or governance actions). The amount you earn for voting correctly is the voters pro-rata share of the slashing of the incorrect voters. This can be calculated per voter per round as follows:

$$
slashPerVoterPerRound = \sum stake * numberOfIncorrectVotes * 0.001
$$

Then, the total amount slashed per round is the sum of all voters slash for that round:

$$
totalSlashPerRound = \sum slashPerVoterPerRound
$$

A voter, who was correct, will then earn a pro-rater share of the totalSlashPerRound as:

$$
positiveSlashForCorrectVotePerRound=\frac{totalSlashPerRound*stake}{cumlativeCorrectVoteStake}
$$

A staker's net APY is therefore the sum of their `stakingRewards` and `positiveSlashForCorrectVotePerRound` annualized over a 1 year period.

#### Rolled votes

Under some situations votes can "roll". We define a rolled vote as a vote that was not resolved in one vote round and was moved into the subsequent voting round due to not successfully reaching the two types of needed quorums. These quorums are known as GAT (God Awful Threshold) and SPAT (Schelling Point Activation Threshold).

The GAT within the DVM 2.0 is a constant amount of UMA that must vote on any given vote for it to not roll. In the DVM 1.0, this was set as a percentage of circulating UMA. At the time of DVM 2.0 initial deployment, the required GAT per vote is a constant 5 million UMA.

The SPAT is a new concept from the DVM 2.0. It is a percentage of staked tokens that must vote and agree in order for a vote to not roll. The required SPAT is 65% currently of staked tokens.

If a vote does not satisfy both of these constraints, it will roll.&#x20;

#### Vote Deletion

There are certain situations in the DVM 2.0 where voters may want to delete votes. A good example of this would be in the case of spam deletion, where needless price requests are submitted in order by an attacker to try to create some sort of desired slashing conditions.

In the DVM 2.0, votes are deleted once they have rolled a certain number of times. So if voters choose to not vote on the resolution of price requests, and either the GAT or SPAT are not met for a set number of voting rounds, those price requests will become deletable. At the time of DVM 2.0 initial deployment, the number of times a vote is rolled before being deleted is 4.

