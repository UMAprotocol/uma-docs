# UMA TVS Methodology

This page outlines UMA’s approach to calculating Total Value Secure or TVS. Calculating TVS is done slightly differently by many projects and popular decentralized statistics projects. At UMA, we pride ourselves on transparency and will always be open about our data calculation methodologies. We are always happy to share the detailed logic for specific queries upon request.&#x20;

### Methodology

The UMA TVS calculation follows one major principle: **UMA TVS = the total value secured by UMA’s optimistic oracle.**&#x20;

UMA’s TVS consists of various sub components for which we can calculate aggregated USD values at a point in time. We use the DeBank Cloud API and the Web3 Package on Python to achieve this.&#x20;

We divide this into four subcategories: Optimistic Oracle integrations, oSnap integrations, LSP contracts, and EMP contracts.&#x20;

#### Optimistic Oracle

The UMA Optimistic Oracle and dispute arbitration system securely allows for arbitrary types of data to be brought on-chain. UMA’s oracle system provides data for projects including a cross-chain bridge, insurance protocols, custom derivatives and prediction markets.

Each protocol integration requires bespoke logic to calculate the total value secured (TVS). We use one of the following sub-methods to calculate TVS:&#x20;

* Use the DeBank API to return the total protocol TVL for that protocol’s address
  * Across, Cozy Finance, Sherlock, Polymarket
* Find all the markets and/or subcontracts for which collateral tokens are used for security. Use DeBank to get the USD value for each subcontract at the time of the call
  * FORE Protocol, Index COOP, Y2K Finance
* Each protocol’s TVS data is then uploaded daily to Dune via the `dune.risk_labs.dataset_uma_oo_tvs dataset`.

#### oSnap

oSnap is a governance tool that integrates with SAFE and Snapshot, allowing DAOs to vote on transactions to be executed onchain. The Optimistic Oracle plays a crucial role by determining whether transactions proposed to the SAFE can be executed.

The methodology for calculating oSnap TVS is straightforward. When a DAO integrates their SAFE with oSnap, we use the Debank Pro API to fetch the total balance of the SAFE to attribute to the oSnap TVS. In the event the Safe has control over another contract’s mint function we count the total supply of the tokens that Safe controls.

Some statistics projects, such as [DeFi Llama](https://docs.llama.fi/list-your-project/readme), do not count funds in smart contract wallets, such as Gnosis Safe. However, UMA is securing these funds as we determine the validity of a proposed transaction. For oSnap, protocol native tokens are included in TVS.

We calculate the USD value for a collection of Safe’s on which oSnap has been deployed:

#### Long/Short Pairs (LSP)

We use a very similar process to the following [methodology](https://github.com/DefiLlama/DefiLlama-Adapters/blob/main/projects/uma/index.js\)) for calculating the total value locked (TVL) of LSP contracts. We fetch the USD values for the collateral for each LSP contract. This data is made available on Dune via the `dune.risk_labs.dataset_uma_oo_tvs dataset`.&#x20;

#### Expiring Multi Party (EMP)

We use a very similar process to the following [methodology](https://github.com/DefiLlama/DefiLlama-Adapters/blob/main/projects/uma/index.js) for calculating the total value locked (TVL) of EMP contracts. We fetch the USD values for the collateral for each EMP contract. This data is made available on Dune via the `dune.risk_labs.dataset_uma_oo_tvs dataset`.&#x20;

### Dashboards

The UMA team has created two dashboards to showcase our TVS for oSnap and the OO. The wallet balances for this dashboard and retrieved from the [DeBank Cloud API](https://docs.cloud.debank.com/en/readme/api-pro-reference/user) and data is refreshed everyday by 7AM UTC.&#x20;

[https://dune.com/risk\_labs/osnap-total-value-secured](https://dune.com/risk\_labs/osnap-total-value-secured)

[https://dune.com/risk\_labs/uma-total-value-secured](https://dune.com/risk\_labs/uma-total-value-secured)
