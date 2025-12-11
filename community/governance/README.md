# Governance

The UMA token is primarily a governance token used to contribute to UMA protocol decisions, such as voting on UMA Improvement Proposals (UMIPs), price requests, and disputes made to UMA's Data Verification Mechanism (DVM).

### Governing the UMA ecosystem <a href="#governing-the-uma-ecosystem" id="governing-the-uma-ecosystem"></a>

The UMA token is an integral part of the UMA ecosystem as it guarantees the economic security of UMA smart contracts and its oracle system. The objective of the UMA token is to enable the optimistic oracle to remain secure utilizing a fully decentralized and permissionless method.

UMA's DVM is designed with an economic guarantee around the cost it would take to corrupt the oracle and the profit someone would receive. The DVM ensures the price to obtain 65% of UMA tokens is greater than the profit from corrupting the DVM, as measured by the collateral stored in UMA's financial contracts. This is achieved through an inflationary reward (currently 0.05% of total network token supply), distributed pro-rata by stake to voters who participate and vote correctly. As long as there is an honest majority, voters will vote correctly.

As the total value of collateral locked in UMA grows, the UMA token is required to increase in value to ensure the security of the DVM. To ensure this inequality holds, the DVM may charge fees to financial contracts which the DVM would use to buy UMA tokens.

### Voting <a href="#voting" id="voting"></a>

The UMA voting process requires tokenholders to commit and reveal their votes in two separate stages. Each stage is open for 24 hours, so each voting period is 48 hours.

* UMA Tokenholders can discuss their votes in the #voting channel of the [UMA Discord](https://discord.umaproject.org/) before voting
* UMA tokenholders can use the [Voter dApp](https://vote.umaproject.org/) to vote on protocol decisions

Examples of governance proposals include:

* Approving new [price identifiers](../../resources/approved-price-identifiers.md) and [collateral currencies](../../resources/approved-collateral-types.md)
* Price requests and disputes
* Upgrading the core DVM protocol and / or modify DVM parameters
* Registering and de-registering contract templates
* Shutting down contract instantiations (in rare circumstances)
