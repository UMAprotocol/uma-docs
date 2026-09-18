# Proposing via the Explorer Dapp

Proposing through a UI is done on the **Explorer dapp** at [explorer.uma.xyz](https://explorer.uma.xyz). The Explorer replaces the legacy Oracle dapp (`oracle.uma.xyz`), which is being deprecated.

{% hint style="warning" %}
**The Explorer only serves Polymarket, Predict.fun and Outcome requests.** Optimistic Oracle requests from any other integration are not shown in the Explorer and will be unsupported through a UI once the Oracle dapp is deprecated. Proposers on other requests should use the [programmatic flow](proposing-programmatically.md).
{% endhint %}

Onchain proposing and disputing in the Explorer is supported on **Polygon** for two oracle contracts:

| Oracle                                | Polygon address                                                                                                         | Proposer whitelist |
| ------------------------------------- | ----------------------------------------------------------------------------------------------------------------------- | ------------------ |
| Managed Optimistic Oracle V2 (MOOv2)  | [`0x2C0367a9DB231dDeBd88a94b4f6461a6e47C58B1`](https://polygonscan.com/address/0x2C0367a9DB231dDeBd88a94b4f6461a6e47C58B1) | Enforced           |
| Optimistic Oracle V2 (OOv2)           | [`0xeE3Afe347D5C74317041E2618C49534dAf887c24`](https://polygonscan.com/address/0xeE3Afe347D5C74317041E2618C49534dAf887c24) | Not enforced       |

#### Finding a request to propose on

1. Go to [explorer.uma.xyz](https://explorer.uma.xyz). The landing page is the request list.
2. Use the lifecycle tabs to narrow by stage:
   * **Requested** — awaiting a first proposal. These are the requests you can propose on.
   * **Proposed** — proposal submitted; challenge period active.
   * **Disputed** — disputed; awaiting DVM resolution.
   * **Settled** — completed, including DVM-resolved disputes.
   * **All** — all lifecycle stages.
3. Narrow further with the integration filters (Polymarket, Predict.fun, Outcome), the tag and date-range filters, or the search box, which matches on title, market, event, exact question ID, or exact transaction hash.
4. Click a request to expand its detail panel and review the question, resolution rules and request payload before proposing.

#### Proposing an answer

The expanded detail panel of an unproposed request shows a **Propose answer** panel with the request's **Minimum challenge period**, **Bond** and **Reward if correct**.

1. Confirm the event has concluded and that you have determined the correct answer from the request's resolution rules. Also check the instructions in the [UMIP](../resources/approved-price-identifiers.md) for the request's identifier.
2. Tick the acknowledgement checkbox: _"I've confirmed the event has concluded and my answer is correct. Proposing posts a bond that's lost if the answer is wrong or disputed successfully."_ The propose button stays disabled until this is ticked.
3. Connect your wallet using the **Connect to Propose** button, then click **Propose**.
4. In the **Propose a price** dialog, select the outcome you are proposing. Binary, incremental negative risk and atomic negative risk requests each show their own preset outcomes. Where presets apply you can instead choose **Enter a custom value** and supply a **Raw int256 value**; the dialog shows which outcome that value resolves to. Requests whose market type cannot be determined hide the presets and take a raw value only.
5. Review **Bond required** against your **Wallet balance**. If the balance is short, the dialog shows _"Insufficient balance to cover the bond."_ and blocks submission.
6. Submit and confirm in your wallet. Where your existing token allowance does not already cover the bond, your wallet will request an unlimited token approval for the oracle contract first — only the required bond is transferred, and the approval applies only to that oracle. The button narrates progress as **Preparing…**, **Switching network…**, **Approving unlimited allowance…**, then **Submitting proposal…**.

Once the proposal is submitted the request moves to the **Proposed** tab and its challenge period begins. If it is not successfully disputed within that period, the answer settles and the bond is returned along with any reward.

#### Proposer whitelist on Managed Optimistic Oracle V2

MOOv2 requests enforce a proposer whitelist. Risk Labs manages the default whitelist — see [Default Proposer Whitelist](default-proposer-whitelist.md) for the criteria and update cadence.

The Explorer checks your connected address against the request's effective whitelist and replaces the propose button with a disabled state explaining why you cannot propose:

| Button state                   | Meaning                                                      |
| ------------------------------ | ------------------------------------------------------------ |
| **Checking whitelist…**        | The whitelist read is in flight.                             |
| **Not whitelisted to propose** | The connected wallet is not on the request's whitelist.      |
| **Whitelist unavailable**      | The whitelist could not be read; retry.                      |
| **Whitelist not configured**   | No proposer whitelist is configured for the request.         |

OOv2 requests do not enforce a whitelist, so any address may propose on them.

#### Requests without UI actions

Some requests are shown for visibility only. On those, the detail panel reads _"Manual actions are unavailable for this request."_, or links out to the legacy Oracle UI while it remains available. Propose on these [programmatically](proposing-programmatically.md) instead.
