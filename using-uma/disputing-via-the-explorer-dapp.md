# Disputing via the Explorer Dapp

Disputing through a UI is done on the **Explorer dapp** at [explorer.uma.xyz](https://explorer.uma.xyz). The Explorer replaces the legacy Oracle dapp (`oracle.uma.xyz`), which is being deprecated.

{% hint style="warning" %}
**The Explorer only serves Polymarket, Predict.fun and Outcome requests.** Optimistic Oracle requests from any other integration are not shown in the Explorer and will be unsupported through a UI once the Oracle dapp is deprecated. Disputers on other requests will need to dispute programmatically by calling `disputePrice` on the oracle contract directly.
{% endhint %}

Onchain disputing in the Explorer is supported on **Polygon**, for requests on the Managed Optimistic Oracle V2 (MOOv2) and Optimistic Oracle V2 (OOv2) contracts listed in [Proposing via the Explorer Dapp](proposing-via-the-explorer-dapp.md).

#### Finding a proposal to dispute

1. Go to [explorer.uma.xyz](https://explorer.uma.xyz).
2. Select the **Proposed** tab. These are requests whose proposal has been submitted and whose challenge period is still active — the set you can dispute. The header also shows **Proposals in Challenge Period** and **Volume in Challenge Period** as a running count and notional.
3. Narrow with the integration filters (Polymarket, Predict.fun, Outcome), the tag and date-range filters, or the search box.
4. Click a request to expand its detail panel. The **Proposed value** panel shows the proposed answer, the proposer's address, the **Bond** and the **Reward if correct**.

#### Disputing a proposal

Before disputing, confirm the proposal is actually incorrect by checking the question, the request's resolution rules and ancillary data, and the instructions in the [UMIP](../resources/approved-price-identifiers.md) for the request's identifier. An incorrect dispute loses your bond.

1. Tick the acknowledgement checkbox: _"I understand that disputing requires posting a bond, and that an incorrect dispute results in losing it."_ The dispute button stays disabled until this is ticked.
2. Connect your wallet using the **Connect to Dispute Proposal** button, then click **Dispute Proposal**.
3. The **Dispute this proposal** dialog confirms that disputing escalates the request to UMA's dispute resolution and requires posting a bond. Review the proposed outcome and the **Bond required**. If your balance is short, the dialog shows _"Insufficient balance to cover the bond."_ and blocks submission.
4. Submit and confirm in your wallet. Where your existing token allowance does not already cover the bond, your wallet will request an unlimited token approval for the oracle contract first — only the required bond is transferred, and the approval applies only to that oracle. The button narrates progress as **Preparing…**, **Switching network…**, **Approving unlimited allowance…**, then **Submitting dispute…**.

If the dispute window has already closed, the button is replaced by a disabled **Dispute window closed** state — the proposal is past its challenge window or under extended review and can no longer be disputed.

How long that window stays open differs by oracle:

* **OptimisticOracleV2** — disputes revert once the challenge period lapses, so an expired proposal can only be settled, not disputed.
* **ManagedOptimisticOracleV2** — settlement is permissioned and an expired proposal is still treated as proposed, so it remains disputable after the challenge period lapses right up until it is actually settled.

#### After a dispute

A disputed request moves to the **Disputed** tab and its detail panel switches to an **Under dispute** view: the proposed value struck through, the dispute timestamp and disputer address, and a **View vote on UMA ↗** link to the [voter dapp](https://vote.uma.xyz/) where the dispute is resolved by UMA token-holder vote.

Disputes escalate to UMA's [Data Verification Mechanism](../protocol-overview/how-does-umas-oracle-work.md) (DVM) on Ethereum mainnet. Once resolved, the request moves to the **Settled** tab and shows a **Final answer** panel with the settlement time and whether it resolved **via UMA vote** or with **no dispute**.

#### Requests without UI actions

Some requests are shown for visibility only. On those, the detail panel reads _"Manual actions are unavailable for this request."_, or links out to the legacy Oracle UI while it remains available. Dispute these by calling `disputePrice` on the oracle contract directly.
