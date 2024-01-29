# oSnap Deployment Tutorial

Welcome to the oSnap Deployment Tutorial. In this tutorial, you'll find detailed, step-by-step instructions on deploying oSnap for fully decentralized governance.

Before deploying oSnap, you must [set-up a Snapshot space](https://docs.snapshot.org/user-guides/spaces/create) and [Safe](https://help.safe.global/en) with Multi-Sig (or Safe with on-chain governance).&#x20;

## Create a Safe

Visit [https://app.safe.global/](https://app.safe.global/) to deploy and access your Safe.&#x20;

If you do not have a Safe created, select the ‘Create new Safe’ button and follow [these](https://help.gnosis-safe.io/en/articles/3876461-creating-a-safe-on-a-web-browser) instructions to deploy a safe. Select ‘Add existing Safe’ if you would like to use an existing safe with your oSnap module.

:exclamation:_Note: Confirm your wallet is set to the appropriate network if the safe isn’t displayed._

## Create a Snapshot Space

Visit [https://snapshot.org/](https://snapshot.org/#/) to access your Snapshot space.

&#x20;If you do not have a Snapshot space created, follow [these instructions](https://docs.snapshot.org/spaces/create) to get started.&#x20;

## Deploy oSnap

Now that your Safe and Snapshot space are set up, you can deploy oSnap.

In your Snapshot space, click **'Settings'** then '**Advanced'** on the left sidebar.

<div align="center">

<figure><img src="../../.gitbook/assets/Screenshot 2023-11-13 at 3.35.40 PM.png" alt="" width="563"><figcaption></figcaption></figure>

</div>

&#x20;In the Plugins container, click **'Add plugin'.** In the modal that opens, click the **'oSnap by UMA'** option.

<div align="center">

<figure><img src="../../.gitbook/assets/Screenshot 2023-11-13 at 3.31.05 PM.png" alt="" width="563"><figcaption></figcaption></figure>

</div>

Next, in the treasury container, click **'Add Treasury'**. Then, select the network, and enter the name and contract address of your Safe treasury.

:exclamation:_Note: Make sure you have the **correct chain selected** or oSnap installation will not work. See supported networks_ [_here._](../../resources/network-addresses.md)

<div align="center">

<figure><img src="../../.gitbook/assets/Screenshot 2023-11-13 at 3.39.14 PM.png" alt="" width="473"><figcaption></figcaption></figure>

</div>

Scroll down, click **'Save'** and sign the message.

<div align="center">

<figure><img src="../../.gitbook/assets/Screenshot 2023-11-13 at 3.32.13 PM.png" alt="" width="563"><figcaption></figcaption></figure>

</div>

Once saved, in the treasury container, click **'Activate oSnap'.**

<div align="center">

<figure><img src="../../.gitbook/assets/Screenshot 2023-11-13 at 3.55.05 PM.png" alt=""><figcaption></figcaption></figure>

</div>

In the modal that opens, click **'Activate oSnap'.**

<div align="center">

<figure><img src="../../.gitbook/assets/Screenshot 2023-11-13 at 3.55.20 PM.png" alt="" width="440"><figcaption></figcaption></figure>

</div>

Next, you will be redirected to the Safe App, click **'Activate oSnap'.**

<div align="center">

<figure><img src="../../.gitbook/assets/Screenshot 2023-11-13 at 3.56.18 PM.png" alt="" width="486"><figcaption></figcaption></figure>

</div>

Execute the generated transaction on your Safe.&#x20;

<div align="center">

<figure><img src="../../.gitbook/assets/Screenshot 2023-11-13 at 4.02.30 PM.png" alt="" width="527"><figcaption></figcaption></figure>

</div>

Once the transaction is confirmed, you should see that oSnap is activated in two places:

First, confirm it's activated in the Safe App.&#x20;

<figure><img src="https://lh7-us.googleusercontent.com/4FfjTorCvSnDlA4d8hu6kGCOOooUUD_HMEvTx1otiq3Lw1W83Hmps3ZWibU0T_PLFP-3ZicQ9U3HjTNLGz0QtDVWk_k4wywRc5OSYaMEAQ7xSYSILnuuOdDXZXjtEiB1VN3pRASeOYLdA-WquF5sEZsc2A=s2048" alt="" width="563"><figcaption></figcaption></figure>

Next, confirm it is activated in the Snapshot advanced settings in the treasury container.

<figure><img src="../../.gitbook/assets/image (21).png" alt=""><figcaption></figcaption></figure>

You're finished! Time to create your [first proposal.](snapshot-tutorial.md)&#x20;
