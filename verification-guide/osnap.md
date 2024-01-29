---
description: How to verify oSnap Proposals
---

# oSnap

#### What is the ASSERT\_TRUTH identifier?

oSnap uses the `ASSERT_TRUTH` identifier can be found [here](https://github.com/UMAprotocol/UMIPs/blob/master/UMIPs/umip-170.md). `ASSERT_TRUTH` is intended to be used as a default price identifier for UMA's [Optimistic Oracle V3 contract](https://github.com/UMAprotocol/protocol/blob/master/packages/core/contracts/optimistic-oracle-v3/implementation/OptimisticOracleV3.sol) that allows asserters to make claims about the state of the world.&#x20;

Price settlement can happen only in one of two ways:

* Return the 1 value if the claim is true.
* Return the 0 value if the claim is false or cannot be resolved.

**Part 1: Snapshot Proposal Created**

When a Snapshot proposal is created, a Discord ticket is created with a Tenderly simulation of the transaction payload. Here is an example:

<figure><img src="../.gitbook/assets/image (29).png" alt=""><figcaption></figcaption></figure>

The main objective of verifying the Snapshot proposal before voting is to ensure:

* The transaction payload can be executed
* Comparing the intent of the proposal against the transaction payload
* The transaction payload doesn't have any unintended consequences or malicious intent

In the Tenderly simulation, if the `Status` value is not `Success`, that is an indicator the transaction payload is unable to be executed and requires further review.

The input shows the transactions array that will be executed if the Snapshot proposal passes:

<figure><img src="../.gitbook/assets/image (30).png" alt=""><figcaption></figcaption></figure>

**Verifying the transaction data**

1. Go to [https://lab.miguelmota.com/ethereum-input-data-decoder/example/](https://lab.miguelmota.com/ethereum-input-data-decoder/example/).
2. Input the contract ABI into the ABI section
   1. Take the ‘to’ value from the transaction object and input it into etherscan (or the block explorer based on the chainID. For etherscan, you would go to [https://etherscan.io/address/{to](https://etherscan.io/address/%7Bto)}.&#x20;
   2. Click the ‘contract’ tab
   3. Scroll down to the ‘Contract ABI’ section and copy everything
   4. Paste it into the ABI section

Here is what the example would look like:

![](https://lh6.googleusercontent.com/X6YmY3KqrdHNzD7PO3dvNWOSFl374VguSZsXs0xZv\_-L9npKaa7AcxgrzzUwXQveeJzyeILCIdX2f1FPvyhOIHlMkIfncnZStgrTe-lF85PU2woo3XfH3taxibvPDrh0HHL4pgBJK8ctGUOyZOd7MRg)

The ‘Output’ for a Transfer should look as the below:

```
{
  "method": "transfer",
  "types": [
    "address",
    "uint256"
  ],
  "inputs": [
    "9040e41eF5E8b281535a96D9a48aCb8cfaBD9a48",
    {
      "type": "BigNumber",
      "hex": "0x14adf4b7320334b9000000"
    }
  ],
  "names": [
    "to",
    "amount"
  ]
}
```

Verify the following:

* For any BigNumber values, go to [https://playground.ethers.org/](https://playground.ethers.org/) and input `BigNumber.from("{INPUT_VALUE}").toString()`. For example, `BigNumber.from("0x14adf4b7320334b9000000").toString()` would be `25000000000000000000000000`. To scale this value, go to the token contract address and check decimals. For the ACX token, the decimals are 18 so the scaled value is `25000000`. You can use [https://eth-converter.com/](https://eth-converter.com/) if the decimals are 18.
* Confirm that the above value is mentioned in the actual Snapshot proposal.
* Search for the token in coingecko.com with the token address and confirm there is liquidity.\
  ![](https://lh4.googleusercontent.com/XQTD64G3zp1doTkN\_-u\_pmsuIEHsV-f6ODkyvXLvJD7veH2Q85S4FEYAQFnZwW00PcjzEFFITi0E4plYWownM2iqxM2V\_NHct720eOqfSE\_4Cksj5dxFfJ\_i353cm0eL8zhGCkDG2HHaW0u8YKAQCsE)

**Part 2: oSnap Transactions Proposed**

After a Snapshot proposal is completed, if the proposal is valid the transactions are proposed which creates another Discord ticket. [Here](https://discord.com/channels/718590743446290492/1133220903728189460) is an example thread in the UMA Discord.

![](https://lh4.googleusercontent.com/p4P55O8VFf\_1HLBZCG50Vrrj-78M92Uz4y1KtGMjgKtvO8WaFEdmIgh9Sdh5MZkzOdSd0wiZRMJ5BlDwu9Kvd9IJ6g2SjWyLwUr4KK2R5m\_cjtX2Qotft0i-B-CxCT1\_E5psYixzpEHnY-OIMD4XsM8)

The first objective is to verify the oSnap rules against Snapshot Proposal. Click the Snapshot space URL, in the above example that is [https://snapshot.org/#/acrossprotocol.eth](https://snapshot.org/#/acrossprotocol.eth). Go through the list of proposals and confirm the explanation from the Discord message matches the Snapshot space.

Validators should do the following to verify the oSnap proposal:

* Check the Snapshot space referenced in the rules string.
* Check that the IPFS hash passed as the explanation lines up with an actual vote in that Snapshot space. They should be able to cross-reference the value with a link from a specific Snapshot proposal.
* Check the Snapshot proposal tied to the IPFS explanation value met the minimum voting period and quorum.&#x20;
* Check that the Snapshot proposal passed according to the Snapshot strategy.

**Compare Snapshot IPFS Transaction data against Proposed Transactions**

1. Go to the Snapshot proposal and click the ‘View Details’ link in the Transactions container:

<img src="https://lh3.googleusercontent.com/d7Pzv8pFjEhTtBKVHjhr12HL8ZhJ034va1Z03jEip7UAYYlKxwNKLKEeFTaKfHONDxnMnHDdCrVFwLk71nsP901VvEjiDP99Ms2Sv7gdEBqetZsZDViU_E7UbF7n60eT7mhw6wA7vKiAGUkzhpxX0eo" alt="" data-size="original">

2. Copy the “transactions” array into [https://jsonformatter.org/](https://jsonformatter.org/). You should use Firefox or Brave so that backslashes “\” aren’t included in the array which will give you an error message when trying to parse:

![](https://lh6.googleusercontent.com/18HVGLKM\_PbypT61kEVmo5kFFm9FMn5WMm27TeWe7YlmhAAJ49c-62tkyPMpKCc-l4kcv893PjL-KXFYYK7p9DTj8LYzbAsRIUjloqvwN3A4vfkkTjZ4X8od9W6qzCJpYXlrdjas7iEqcOiRLjKqPoQ)

3\. In the Discord post, click the transaction:’

![](https://lh6.googleusercontent.com/aEUx5sRSrWj3KsjwW0N96jkssfv0kXGu8xbN8Q\_9iVBLr0bsWa1nH\_64NslFmKx0hRTs1RYlAonI6Bk93birxHPgULuzZD9wzNclr9rDigm215x0g1qkzXEoKwcJFZcCij2osytYbOMF1dwC8ImLZ90)

Then copy the ‘Transaction Hash’:

![](https://lh3.googleusercontent.com/0suGhWmXC8Xyo1zVkH-g7rtAr519ochAJlsA4rVrUlfsVdiIcuGJxMgMFNyAQLWYODtVM7TWjF5JIT3RF3UyE3WQ-KyXkIdcWlZ2McnOFSL8U22J22uE7k7skb7ONFZ1VwWN8mGae7Suwlhyv5z-ewk)

Go to [https://dashboard.tenderly.co/](https://dashboard.tenderly.co/) and paste the ‘Transaction Hash’. Then click ‘Show’ to check the input data.

![](https://lh3.googleusercontent.com/TSrHDZePeo\_v\_ZbjAyuA4EkKjD3bYjf57fL7WlCVX6E7cdTYTayuthJfbTHTjx9c70\_CRLhbz5s\_pMezclorNTYLSdL7KF2xObANrin1OTXu0\_i9veszCebY2SnHBSw4LUOkzOm1VUMlctA8oO4\_h7k)

In the Discord Ticket, paste the screenshots of Tenderly and json formatter and also paste the transaction data into another thread message as below:

```
Tenderly:
[
  {
    "to": "0x44108f0223a3c3028f5fe7aec7f9bb2e66bef82f",
    "operation": 0,
    "value": "0",
    "data": "0xa9059cbb0000000000000000000000009040e41ef5e8b281535a96d9a48acb8cfabd9a4800000000000000000000000000000000000000000014adf4b7320334b9000000"
  }
]

Snapshot IPFS:
[
  {
    "operation": "0",
    "data": "0xa9059cbb0000000000000000000000009040e41ef5e8b281535a96d9a48acb8cfabd9a4800000000000000000000000000000000000000000014adf4b7320334b9000000",
    "to": "0x44108f0223A3C3028F5Fe7AEC7f9bb2E66beF82F",
    "value": "0"
  }
]
```

The following should be verified:

* ‘data’ from Tenderly matches the transaction data from Snapshot.
* ‘value’ from Tenderly matches the transaction data from Snapshot.
* ‘to’ from Tenderly matches the transaction data from Snapshot.
* Verify the explanation:
  * Go to [https://playground.ethers.org/](https://playground.ethers.org/) and input the following explanation to ethers.utils.toUtf8String(“[{explanation](https://snapshot.4everland.link/ipfs/%7Bexplanation)}”).toString()
  * Take the returned value and input it into the following url ‘[https://snapshot.4everland.link/ipfs/{explanation](https://snapshot.4everland.link/ipfs/%7Bexplanation)} and verify that the data matches from Snapshot
