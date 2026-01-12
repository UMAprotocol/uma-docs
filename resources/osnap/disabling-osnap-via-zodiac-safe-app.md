---
description: Steps to remove oSnap from your DAO Snapshot space and Safe treasury
---

# Disabling oSnap via Zodiac Safe App

Requirements:&#x20;

* The below steps propose a Safe transaction to disable oSnap. Transactions can only be proposed to a Safe by that Safe's Signers or Proposers. You can view these roles on your Safe's settings page.
* Executing the proposed Safe transaction requires your minimum number of Signers to sign the transaction and one Signer to execute the transaction.

Steps:

1. Navigate to your DAO's Safe treasury on the Safe(Wallet) app: [https://app.safe.global/](https://app.safe.global/)
2.  Click "Apps" on the left side bar and open on the "Zodiac" App<br>

    <figure><img src="../../.gitbook/assets/image (42).png" alt=""><figcaption></figcaption></figure>
3.  Under the Modules and Modifiers heading, click on the "UMA OSNAP MODULE" and then the "Remove" button.\
    Note: if no "UMA OSNAP MODULE" shows under the "Modules and Modifiers" heading, your treasury does not an oSnap module.<br>

    <figure><img src="../../.gitbook/assets/image (44).png" alt=""><figcaption></figcaption></figure>


4.  Click "Submit Transactions" in the Zodiac "Bundle Transactions" modal<br>

    <figure><img src="../../.gitbook/assets/image (45).png" alt=""><figcaption></figcaption></figure>


5. This will propose a "disableModule" transaction in the Safe(Wallet) dapp. This transaction can be proposed, reviewed, signed, and executed as per the typical Safe(Wallet) transaction flow.

<figure><img src="../../.gitbook/assets/image (6).png" alt=""><figcaption></figcaption></figure>



6. After this transaction is executed, oSnap is disabled from your Safe treasury. You can verify that your Treasury has no remaining oSnap modules as per the note in step number 3 above.&#x20;
