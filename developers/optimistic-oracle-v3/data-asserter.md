---
description: Using the Optimistic Oracle V3 to assert arbitrary off-chain data on-chain.
---

# 💾 Data Asserter

This section covers the [DataAsserter contract](https://github.com/UMAprotocol/dev-quickstart-oov3/blob/master/src/DataAsserter.sol), which is available in the Optimistic Oracle V3 [quick-start repo](https://github.com/UMAprotocol/dev-quickstart-oov3). This tutorial shows an example of how to build an arbitrary data asserter contract, using the UMA [Optimistic Oracle V3](https://github.com/UMAprotocol/protocol/blob/master/packages/core/contracts/optimistic-oracle-v3/implementation/OptimisticOracleV3.sol) contract. This enables you to assert to the correctness of some off-chain process and store the output on-chain. This tutorial acts as a starting point to show how the OOv3 can be flexibility used in a sample integration.

### Data Asserter

This smart contract allows data providers to assert the correctness of a data point, within any arbitrary data set, and bring the result on-chain. This contract is designed to be maximally open ended, enabling _any kind of data to be asserted_. For example, and for illustrative purposes of this tutorial, we will assert weather data on-chain. Note, though, that this kind of assertion flow and the associated OOv3 integration could build a wide range of assertions with arbitrary complexity. For example you could use the same structure to bring on-chain complex data associated with Beacon chain validator set to build a Liquid staking derivative, enable complex off-chain computation with on-chain outputs or anything that has a defined deterministic input output relationship.

Anyone can assert a data point against the DataAsserter contract, with a known dataId and datapoint. Independent 3rd parties can then verify the correctness of this data against the dataId.&#x20;

For our example we will be asserting the output of a very simple numerical computation: `69+420`. This is meant to show the basic idea without overcomplicating the off-chain part. Note, though, that the `dataId` prop used here could represent any kind of unique data identifier (a row in a database table, a validator address or a complex structure used to associate any kind of data to a given value.

### Development environment and tests

#### Clone repository and Install dependencies&#x20;

Clone the UMA [Optimistic Oracle V3 quick-start repository](https://github.com/UMAprotocol/dev-quickstart-oov3) and install the dependencies. To install dependencies, you will need to install the long-term support version of nodejs and yarn. You can then install package dependencies by running `yarn` with no arguments:

<pre class="language-bash"><code class="lang-bash"><strong>git clone https://github.com/UMAprotocol/dev-quickstart-oov3.git
</strong>cd dev-quickstart-oov3
yarn
</code></pre>

This project uses [forge](https://github.com/foundry-rs/foundry/tree/master/forge) as the Ethereum testing framework. You will also need to install Foundry, refer to [Foundry installation documentation](https://book.getfoundry.sh/getting-started/installation) if you don’t have it already.

Instructions below for interacting with the contracts also assumes `bash` shell and `jq` tool is installed in order to parse transaction outputs.

### Contract implementation

The contract discussed in this tutorial can be found at `dev-quickstart-oov3/src/DataAsserter.sol` ([here](https://github.com/UMAprotocol/dev-quickstart-oov3/blob/master/src/DataAsserter.sol)) within the repo.

#### Contract creation and initialization

`_defaultCurrency` parameter in the constructor identifies the token used for bonds of data assertions. This token should be approved as whitelisted UMA collateral. Please check [Approved Collateral Types](../../resources/approved-collateral-types.md) for production networks or call `getWhitelist()` on the [Address Whitelist](https://github.com/UMAprotocol/protocol/blob/master/packages/core/contracts/common/implementation/AddressWhitelist.sol) contract for any of the test networks. Note that the deployed Optimistic Oracle V3 instance already has its `defaultCurrency` added to the whitelist, so it can also be used by the Insurance contract. Alternatively, you can approve a new token address with `addToWhitelist` method in the Address Whitelist contract if working in a sandboxed UMA environment.

`_optimisticOracleV3` is used to locate the address of UMA Optimistic Oracle V3. Address of `OptimisticOracleV3` contact can be fetched from the relevant [networks](https://github.com/UMAprotocol/protocol/tree/master/packages/core/networks) file, if you are on a live network, or you can provide your own contract instance if deploying UMA Oracle contracts in your own sandboxed testing environment.

<pre class="language-solidity"><code class="lang-solidity"><strong>    constructor(address _defaultCurrency, address _optimisticOracleV3) {
</strong>        defaultCurrency = IERC20(_defaultCurrency);
        oo = OptimisticOracleV3Interface(_optimisticOracleV3);
        defaultIdentifier = oo.defaultIdentifier();
    }
</code></pre>

#### Asserting data

`assertDataFor` method allows any data provider to assert some value of `data` for the associated `dataId`. The caller sets the `asserter` as the address that will receive bonds at resolution of the data assertion (this could be their address as well).

```solidity
   function assertDataFor(
        bytes32 dataId,
        bytes32 data,
        address asserter
    ) public returns (bytes32 assertionId) { ...
```

Internally, this function pulls the associated assertion bond (global for all assertions within the DataAssertion contract) from the caller, makes an assertion call to the Optimistic Oracle V3 and stores the assertion within the contracts internal `assertionsData` mapping. Lastly, an event is emitted.

For the context of this example we will be considering the `dataId` simply as the URL that would be called to fetch the weather data from `wttr.in` and the `data` value as the result from this call. Both are bytes32 encoded.

The call to the Optimistic Oraclve V3 looks as follows:

```solidity
assertionId = oo.assertTruth(
            abi.encodePacked(
                "Data asserted: 0x", // in the example data is type bytes32 so we add the hex prefix 0x.
                ClaimData.toUtf8Bytes(data),
                " for dataId: 0x",
                ClaimData.toUtf8Bytes(dataId),
                " and asserter: 0x",
                ClaimData.toUtf8BytesAddress(asserter),
                " at timestamp: ",
                ClaimData.toUtf8BytesUint(block.timestamp),
                " in the DataAsserter contract at 0x",
                ClaimData.toUtf8BytesAddress(address(this)),
                " is valid."
            ),
            asserter,
            address(this), // Callback recipient
            address(0), // No sovereign security.
            assertionLiveness,
            defaultCurrency,
            bond,
            defaultIdentifier,
            bytes32(0) // No domain.
        );
```

Lets break this down step by step:

* First, the value for the `claim` filed is constructed to be a human readable string as the concatination of a number of different data points. This is meant to be parsable by the validation network and enable both Humans and software verifiers alike to assess the correctness of the claim. Note that within the Optimistic Oracle V3 the `claim` field (this prop in this function call) is _not_ stored on-chain; it is simply emmitted and is used by off-chain actors to verify the corretness of the claim.
* Next, the `asserter` field is passed in from this function call. This is who will recive the bonds back at resolution of the assertion, assuming the asserter was correct.
* Next, we have the callback recipient set to `address(this)`The OOv3 has the concept of callbacks wherein at the settlement of an assertion the asserter can choose to optionally call out from the OO into another contract. In this context, we want the OOv3 to call _back_ into the DataAsserter contract when the assertion is settled (this will become clearer in the following section).
* We have no Sovereign security enabled here (this is an advanced topic and does not need to be covered here).
* Default liveness, bond and identifier are set.
* No domain is set (this is an external concept that helps 3rd parties identify assertions).

#### Resolving and subsequently fetching data assertions

Once data has been asserted, the act of verifying it is passed over to the Optimistic Oracle V3 and the associated network of data verifiers. Due to the presence of a bond (reward for catching invalid assertions), we can be confident that in real world someone would verify this assertion (if the bond is high enough, and the liveness is long enough we have high guarantees of this).

If the assertion passes the `assertionLiveness` without dispute then it can be settled. The data assertion contract showcases the OOV3's concept of `assertionResolvedCallback` which enables the OOv3 to settle downstream contracts that are dependent on the resolution of an assertion. Once an assertion is settleable (it has: a) passed liveness and b) not been disputed) anyone can call [`settleAssertion`](https://github.com/UMAprotocol/protocol/blob/922e5da60d1e71f64de50acdf9bf83a07be9449e/packages/core/contracts/optimistic-oracle-v3/implementation/OptimisticOracleV3.sol#L250) on the OOv3. This function acts to settle the assertion within the OO _and_ call into the callback recipient, set during the `assertDataFor` function call.

Looking within the DataAsserter contract, we can see what happens when the `assertionResolvedCallback` is hit from the OOv3 on settlement:

```solidity
// OptimisticOracleV3 resolve callback.
    function assertionResolvedCallback(bytes32 assertionId, bool assertedTruthfully) public {
        require(msg.sender == address(oo));
        // If the assertion was true, then the data assertion is resolved.
        if (assertedTruthfully) {
            assertionsData[assertionId].resolved = true;
            DataAssertion memory dataAssertion = assertionsData[assertionId];
            emit DataAssertionResolved(dataAssertion.dataId, dataAssertion.data, dataAssertion.asserter, assertionId);
            // Else delete the data assertion if it was false to save gas.
        } else delete assertionsData[assertionId];
    }
```

This function effectively:

* enforce the inbound caller is the OO. Else, revert
* If the assertion was resolved as truthful (it would not be if it was disputed and the dispute proved to be correct) then:
  * set the data assertion status to true,
  * remove the data from the temporary `dataAssertion` structure
* if it was resolved as not truthful (deleted) then simply delete it from the temp structure `assetionsData`

Once settled, we can now call the `getData` function with the `assertionId.`This will return the data value and the acomponing resolution status (was it resolved truthfully, or not).

#### Disputing data assertions

If we want to dispute an asserted data point one must simply call the [`disputeAssertion(bytes32 assertionId, address disputer)`](https://github.com/UMAprotocol/protocol/blob/922e5da60d1e71f64de50acdf9bf83a07be9449e/packages/core/contracts/optimistic-oracle-v3/implementation/OptimisticOracleV3.sol#L220) method on the OOv3. In here, you provide the associated assertionId (the same field that was returned when calling `assertData`and the `disputer` address (the address that will recive the bonds back at the resolution of the dispute).

### Tests and deployment

All the unit tests covering the functionality described above are available [here](https://github.com/UMAprotocol/dev-quickstart-oov3/blob/master/test/Insurance.t.sol). To execute all of them, run:

```bash
forge test --match-path *DataAsserter*
```

#### Deployment

Before deploying and interacting with the contracts export the required environment variables. Note that there are a number of ways to interact with these contracts. The simplest setup is to fork Goerli testnet into a local development environment, using Anvil, and run the steps against this the fork. To create an Anvil fork run the following:

```bash
anvil --fork-url https://goerli.infura.io/v3/xxx
```

Replacing `xxx` with your infura key. This will create a local fork of the Goerli testnet, running on `127.0.0.1:8545`. It will also expose a test mnemonic that you can use which has pre-loaded Eth within the fork.

(**Note:** to set enviroment variables, as those listed below, use the `export` syntax in your shell. for example `export ETH_RPC_URL=XXX`

* `ETHERSCAN_API_KEY`: your secret API key used for contract verification on Etherscan if deploying on a public network (not required if you want to do this against a local testnet).
* `ETH_RPC_URL`: your RPC node used to interact with the selected network. For this tutorial it will be easist to do this against the Goerli fork by setting `ETH_RPC_URL=http://127.0.0.1:8545`
* `MNEMONIC`: your passphrase used to derive private keys of deployer (index 0) and any other addresses interacting with the contracts. The simplest setup with this is to re-use the Anvil default unlocked accounts, if you are using the local testnet enviroment, using the `MNEMONIC=test test test test test test test test test test test junk`
* `FINDER_ADDRESS`: address of the [Finder](https://github.com/UMAprotocol/protocol/blob/master/packages/core/contracts/data-verification-mechanism/implementation/Finder.sol) contract used to locate other UMA ecosystem contracts (in order to resolve disputes you would need to use the one from a sandboxed environment). For Goerli, if you are using the local fork, set this to `0xE60dBa66B85E10E7Fd18a67a6859E241A243950e`

Use `cast` command from Foundry to locate the address of Optimistic Oracle V3 and we will also use its default bonding token for settling insurance claims:

<pre class="language-bash" data-overflow="wrap"><code class="lang-bash"><strong>export OOV3_ADDRESS=$(cast call $FINDER_ADDRESS "getImplementationAddress(bytes32)(address)" $(cast --format-bytes32-string "OptimisticOracleV3"))
</strong><strong>export DEFAULT_CURRENCY_ADDRESS=$(cast call $OOV3_ADDRESS "defaultCurrency()(address)")
</strong></code></pre>

You should be able to see both of these values set in your env with `echo $OOV3_ADDRESS` and `echo $DEFAULT_CURRENCY_ADDRESS`

To deploy the Insurance contract, run `forge create` command.&#x20;

(**Note** if you hit an error like `Is a directory (os error 21)`update your forge with `foundryup`). Also note that if you get an error like `Error accessing local wallet. Did you set a private key, mnemonic or keystore?` then you might be hitting a foundry related issue. We've found changing the version is effective. To do this use: `foundryup -v nightly-a44159a5c23d2699d3a390e6d4889b89a0e5a5e0`.

```bash
export DATA_ASSERTER=$(forge create src/DataAsserter.sol:DataAsserter --mnemonic "$MNEMONIC" --json --constructor-args $DEFAULT_CURRENCY_ADDRESS $OOV3_ADDRESS | jq -r .deployedTo)
```

Finally, we can verify the deployed (if deployed to a public network) contract with `forge verify-contract`:

```
forge verify-contract --chain-id $(cast chain-id) --constructor-args $(cast abi-encode "constructor(address,address)" $DEFAULT_CURRENCY_ADDRESS $OOV3_ADDRESS) $DATA_ASSERTER
```

### Interacting with deployed contract

The following section provides instructions on how to interact with the deployed contract from the foundry `cast` tool, though one can also use it for guidance for interacting through another interface (e.g. Remix or Etherscan).

#### Initial setup

Export required user addresses and their derivation indices. This includes setting up the bytes32 encoding for the `dataId` and `data` , as discussed in the [Asserting Data](https://app.gitbook.com/o/MZ06Z7ucpJtTO46hUUQr/s/KdaoNjf9AzgWFNHyPo5b/\~/changes/130/developers/optimistic-oracle-v3/data-asserter#asserting-data) section. We set the `DATA_ID` env as the bytes32 encoded version of the URL we are asserting. We set the `DATA`env as the output of the curl command. Run the curl command and then set the env as the output value.

```bash
# The DATA_ID field will be the simplest command that will be re-producable
# by independent verifiers. we run the linux bc command with the input 69+420.
export DATA_ID=$(cast --format-bytes32-string "echo '69+420' | bc")
echo $DATA_ID # Show the set value.
> 0x6563686f202736392b34323027207c2062630000000000000000000000000000

# Set the value of DATA as the output of this command.
export DATA=$(cast --format-bytes32-string $(echo '69+420' | bc))
echo $DATA # Show the set value.
0x3438390000000000000000000000000000000000000000000000000000000000

# Set the ASSERTER_ADDRESS to the address of the unlocked wallet to use in the assertions.
export ASSERTER_ADDRESS=$(cast wallet address --mnemonic "$MNEMONIC")
```

Note that at any point you can look at your env by running `printenv` within your consol to see the things we've set up to this point.

#### Asserting the data point

Asserting a data point. Next, we will call the data assertion and assert the data point and data ID that we set within our environment. Note that on the Goerli testnet the default bond for the default currency is set to 0. This is done to somewhat simplify the interactions: you dont need to mint yourself any USDC and you dont need to approve the Data Asserter to pull it from your account. Clearly, this is different to how this would be structured in reality but for demonstration purposes it keeps thing easier.

```
export ASSERTION_TX=$(cast send --mnemonic "$MNEMONIC" --json $DATA_ASSERTER "assertDataFor(bytes32,bytes32,address)" $DATA_ID $DATA $DATA_ASSERTER | jq -r .transactionHash)
```

What we are doing here is calling the `assertDataFor(bytes32,bytes32,address)`with the props `DATA_ID`, `DATA` and `DATA_ASSERTER`, that we set eariler. We are taking the output of the transaction and storing the `transactionHash` within the `ASSERTION_TX` variable.

We can fetch the associated `assertionId` from the Assertion event. The emitted event has the structure:

```solidity
event DataAssertionResolved(
 bytes32 indexed dataId,
 bytes32 indexed data,
 address asserter,
 bytes32 indexed assertionId);
```

We can fetched indexed props from cast by using the `receipt` function in conjunction with `jq`. To Fetch the 3rd indexed prop, the `assertionId` we would run:

```
export ASSERTION_ID=$(cast receipt --json $ASSERTION_TX | jq -r .logs[-1].topics[3])
```

Next, we can view the data asserted object using the `--abi-decode` call. This will return the props in the `assertionData` mapping, relating to this structure:

```solidity
struct DataAssertion {
        bytes32 dataId; // The dataId that was asserted.
        bytes32 data; // This could be an arbitrary data type.
        address asserter; // The address that made the assertion.
        bool resolved; // Whether the assertion has been resolved.
    }
```

```bash
cast --abi-decode "assertionsData(bytes32)(bytes32,bytes32,address,bool)" \
    $(cast call $DATA_ASSERTER "assertionsData(bytes32)" $ASSERTION_ID)
> 0x6563686f202736392b34323027207c2062630000000000000000000000000000 # dataId
> 0x3438390000000000000000000000000000000000000000000000000000000000 # data
> 0x7255f42D5c7433C9297D31fFB301613681C68190 # asserter
> false # not yet resolved
```

#### Settling the assertion

The default liveness is 7200 seconds. We need to advance time past this to enable the assertion to be resolved. Advance time then call the OOv3 to settle the assertion:

```bash
cast rpc evm_increaseTime 7200 # Default time 
cast rpc evm_mine # Mine the next block
```

Now, we can submit the settlement TX to the OOV3 as we've passed liveness:

```bash
cast send --mnemonic $MNEMONIC $OOV3_ADDRESS "settleAssertion(bytes32)" $ASSERTION_ID
```

Now, if we call the `assertionData` maping agin we'll see that the assertion has settled!

```bash
cast --abi-decode "assertionsData(bytes32)(bytes32,bytes32,address,bool)" \
    $(cast call $DATA_ASSERTER "assertionsData(bytes32)" $ASSERTION_ID)
> 0x6563686f202736392b34323027207c2062630000000000000000000000000000 # dataId
> 0x3438390000000000000000000000000000000000000000000000000000000000 # data
> 0x7255f42D5c7433C9297D31fFB301613681C68190 # asserter
> true # Assertion has now settled!
```

We can now call the `getData` method directly as:

<pre class="language-bash"><code class="lang-bash">cast --abi-decode "getData(bytes32)(bool,bytes32)" \
    $(cast call $DATA_ASSERTER "getData(bytes32)" $ASSERTION_ID)
<strong>> true
</strong>> 0x3438390000000000000000000000000000000000000000000000000000000000
</code></pre>

Where the `true` represents that it is now settled and the second field is the data value. This can be decoded with cast as:

```bash
cast --parse-bytes32-string "0x3438390000000000000000000000000000000000000000000000000000000000"
> 489
```

Which gives us back the original data point asserted.

### Conclusion

This tutorial has shown you how to use the Data Asserter sample tutorial, along with the Optimistic Oracle V3. This is meant to act as a starting point for further contracts to be built on. Next steps should be looking at the other OOv3 tutorials or trying to build your own intergration! if you have questions please message us on Discord or create a Github issue and we'll happily help you with your integration.
