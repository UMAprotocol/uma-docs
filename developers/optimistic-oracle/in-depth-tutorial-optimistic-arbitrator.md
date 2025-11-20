---
description: Wrapping Optimistic Oracle functionality into individual function calls
hidden: true
---

# Optimistic Arbitrator

### The Optimistic Arbitrator

The Optimistic Arbitrator (OA) is a contract built on top of the [Optimistic Oracle V2](https://github.com/UMAprotocol/protocol/blob/master/packages/core/contracts/optimistic-oracle-v2/implementation/OptimisticOracleV2.sol) (OO) in order to abstract some mechanics and make them easier to use and implement with an "assert and ratify" pattern, as opposed to the Optimistic Oracle's typical "request and propose" pattern.

It should be noted that the uses permitted by the OA were always permitted by the OO; the OA merely formalizes these specific modes of use in a more developer-friendly implementation for this particular purpose. This pattern can also be used as inspiration in your projects when you request and propose (or request, propose and dispute) requests with the OO in one function call.&#x20;

In short, the OA allows a user to make an assertion, i.e., propose an answer to a question directly. They can optionally ratify the assertion by escalating the question immediately to [UMA's Data Verification Mechanism](../../protocol-overview/how-does-umas-oracle-work.md#umas-data-verification-mechanism) (DVM) where all UMA token holders vote on it.

An assertion consists of using the `requestPrice` and `proposePrice` methods of the OptimisticOracle to ask and answer a question simultaneously. It has the advantage, compared to a traditional `priceRequest`, of receiving a response immediately and incurring no final cost if the assertion is truthful and undisputed.

To ratify an assertion, an initial assertion must be made and disputed, such that the question is escalated to the [DVM](../../protocol-overview/how-does-umas-oracle-work.md#umas-data-verification-mechanism), where UMA token holders vote on it. Ratifications can be useful when the user prefers to have their assertion voted on, at a higher cost,  rather than optimistically accepted.

The following table shows how the Optimistic Arbitrator differs from the Optimistic Oracle and why using the Optimistic Arbitrator could be interesting:

| Optimistic Oracle (OO)                                                                                                                                                                                                                                                                                                   | Optimistic Arbitrator (OA)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| <p>In the basic scenario, the user will first pose a question (<code>priceRequest</code>), wait for someone to propose (<code>proposePrice</code>) an answer and potentially dispute (<code>disputePrice</code>) a wrong answer.<br></p>                                                                                 | The user makes an assertion by submitting a question (`priceRequest`) to the Optimistic Oracle and simultaneously proposing an answer (`proposePrice`).  Optionally, they can ratify the assertion and present it to the DVM for a vote (`disputePrice`).                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
| The requester, proposer and disputer are generally different people.                                                                                                                                                                                                                                                     | The requester and and proposer are always the same person. The disputer may be another person or the same one.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| The user waits for someone to respond to their question.                                                                                                                                                                                                                                                                 | The user does not wait, answering their own question immediately.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| The user pays only the reward for the question, which is optional and of their choosing (although, if it is 0, proposers would not be interested in answering).                                                                                                                                                          | The user does not have to pay a reward since they are the proposer, but they must lock the bond amount to propose, which they will receive back if the proposal is not disputed (or determined to be correct after a dispute).                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| In the simplest scenario, in which a user merely requests a price (i.e., asks a question), proposals and disputes are made by other users, so the requester is not charged or required to pay any fees beyond the reward. However if the reward is $0, most likely, they will never receive an answer to their question. | <p>When making only an assertion, the user must lock a bond at least equal to the <a href="../../resources/approved-collateral-types.md"><code>finalFee</code></a>; this bond is returned if the assertion was correct (undisputed). Therefore, the final cost of asserting truthfully is $0.</p><p></p><p>When asserting and ratifying, the function has a fixed final cost for the user equal to the <a href="../../resources/approved-collateral-types.md"><code>finalFee</code></a> of the <code>currency</code> used (~1,500 USD). This is the cost of taking a dispute to the <a href="../../protocol-overview/how-does-umas-oracle-work.md#umas-data-verification-mechanism">DVM</a> for vote resolution. </p> |
| Disputes are resolved in the [DVM](../../protocol-overview/how-does-umas-oracle-work.md#umas-data-verification-mechanism) where they are voted on by UMA token holders.                                                                                                                                                  | Disputes are resolved in the [DVM](../../protocol-overview/how-does-umas-oracle-work.md#umas-data-verification-mechanism) where they are voted on by UMA token holders.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |

### Development environment and tests

#### Clone repository and Install dependencies

Clone the [UMA dev-quickstart](https://github.com/UMAprotocol/dev-quickstart) repository and install the dependencies. To install dependencies, you will need to install the long-term support version of nodejs, currently Nodejs v16, and yarn. You can then install dependencies by running yarn with no arguments:

```bash
git clone git@github.com:UMAprotocol/dev-quickstart.git
cd dev-quickstart
yarn
```

#### Compiling your contracts

We will need to run the following command to compile the contracts and make the Typescript interfaces so that they are easy to work with:

```bash
yarn hardhat compile
```

### Contract Implementation

The contract discussed in this tutorial can be found at `dev-quickstart/contracts/OptimisticArbitrator.sol`([here](https://github.com/UMAprotocol/dev-quickstart/blob/main/contracts/OptimisticArbitrator.sol)) within the repo.

#### Contract creation and initialisation

The constructor of the Optimistic Arbitrator contract takes two parameters:

`_finderAddress` the Finder contract stores the addresses of all of the other relevant UMA contracts, which vary from chain to chain.

`_currency` the collateral token used to pay fees.

```solidity
constructor(address _finderAddress, address _currency) {
    finder = FinderInterface(_finderAddress);
    currency = IERC20(_currency);
    oo = OptimisticOracleV2Interface(finder.getImplementationAddress(OracleInterfaces.OptimisticOracleV2));
}
```

As part of initialization, the `oo` variable is set to the address of the `OptimisticOracleV2` implementation as discovered through the `getImplementationAddress` method in the [Finder](https://github.com/UMAprotocol/protocol/blob/master/packages/core/contracts/data-verification-mechanism/implementation/Finder.sol) contract.

#### Making an assertion

The following function allows a user to make an assertion. Behind the scenes it uses an `OO.priceRequest` with a `YES_OR_NO_QUERY` [price identifier](../../resources/approved-price-identifiers.md). This imposes a set of rules on how the assertion is formulated and formatted, as specified in [UMIP-107](https://github.com/UMAprotocol/UMIPs/blob/master/UMIPs/umip-107.md).\
\
The `makeAssertion` function takes the following arguments:&#x20;

* `timestamp` timestamp used to identify the assertion, usually the current timestamp.
* `ancillaryData` the byte-converted question for which we want to assert (e.g. `'q: Was the price of BTC above 18000 USD for the duration of October 10 2022 UTC time considering top 5 volume weighted markets'`)
* `assertedValue` our response to the question. (e.g. `1e18` for yes `0` for no)
* `bond` the bond in the `currency` defined in the constructor that we want to require on top of the [`finalFee`](../../resources/approved-collateral-types.md) (\~1,500 USD worth of the currency). The [`finalFee`](../../resources/approved-collateral-types.md) can be viewed as the minimum bond required by the system, and the bond is any additional amount we require on top of that.
* `liveness` time period during which the proposal can be disputed

Note: The `makeAssertion` function requires the caller to approve the OA to spend the `bond+finalFee` amount of the `currency`.

```solidity
function makeAssertion(
    uint256 timestamp,
    bytes memory ancillaryData,
    int256 assertedValue,
    uint256 bond,
    uint64 liveness
) public {
    _pullAndApprove(bond + _getStore().computeFinalFee(address(currency)).rawValue);
    _makeAssertion(timestamp, ancillaryData, assertedValue, bond, liveness);
}
```

#### Ratifying an assertion

Once an assertion has been made, it can be ratified. This function will, behind the scenes, dispute an existing assertion in the OA, i.e., an OO dispute will be made against the OO proposal made in `makeAssertion`. The method accepts only the two arguments that we utilized in `makeAssertion`:

* `timestamp` timestamp used to identify the assertion, usually the current timestamp.
* `ancillaryData` the byte-converted question which we want to assert.

Note: The `ratifyAssertion` function requires the caller to again approve the OA to spend the `bond+finalFee` amount of the `currency`.

```solidity
function ratifyAssertion(uint256 timestamp, bytes memory ancillaryData) public {
    OptimisticOracleV2Interface.Request memory request = oo.getRequest(
        address(this),
        priceIdentifier,
        timestamp,
        ancillaryData
    );
    _pullAndApprove(request.requestSettings.bond + _getStore().computeFinalFee(address(currency)).rawValue);
    oo.disputePriceFor(msg.sender, address(this), priceIdentifier, timestamp, ancillaryData);
}
```

#### Ratifying and asserting

This function allows the user to both assert and ratify, in a single transaction, hence simplifying the arguments. This is the function to utilize if we wish to initiate ratification immediately. This kind of pattern might be useful if you want to bypass the OO and directly use the UMA DVM when asking/asserting questions. We've seen this as a useful integration path in a number of projects that maintain their own internal escalation games within their contracts and just want to use the UMA DVM for arbitration.

`assertAndRatify` takes three arguments:

* `timestamp` timestamp used to identify the assertion, usually the current timestamp.
* `ancillaryData` the byte-converted question for which we want to assert (e.g. `'q: "Was the price of BTC above 18000 USD for the duration of October 10 2022 UTC time considering top 5 volume weighted markets"'`)
* `assertedValue` our response to the question. (e.g. `1e18` for yes `0` for no)

Note 1: Here, we do not set a `bond` on top of the `finalFee` because it is set to zero by default due to the fact that the `proposer` and `disputer` are the same wallet. Adding a bond would increase the final cost of ratification for users.

Note 2: The `assertAndRatify` function requires the caller to approve the OA to spend the `2*bond+finalFee` amount of the `currency`. In this function we are acting as the Optimistic Oracle requestor, proposer and disputer all in one transaction. Proposing and disputing a price both require the posting of the final fee and so to do these actions in one go we are required to pull 2x the final fee as payment.

```solidity
function assertAndRatify(
    uint256 timestamp,
    bytes memory ancillaryData,
    int256 assertedValue
) public {
    _pullAndApprove(2 * _getStore().computeFinalFee(address(currency)).rawValue);
    _makeAssertion(timestamp, ancillaryData, assertedValue, 0, 0);
    oo.disputePriceFor(msg.sender, address(this), priceIdentifier, timestamp, ancillaryData);
}
```

### Tests and deployment

All the unit tests covering the functionality described above are available [here](https://github.com/UMAprotocol/dev-quickstart/tree/main/test/OptimisticArbitrator). To execute all of them, run:

```bash
yarn test test/OptimisticArbitrator/*
```

Before deploying the contract check the comments on available environment variables in [the deployment script](https://github.com/UMAprotocol/dev-quickstart/blob/main/deploy/004_deploy_optimistic_arbitrator.ts).

In the case of the Görli testnet, the defaults would use the Finder instance that references the [Mock Oracle](https://github.com/UMAprotocol/protocol/blob/master/packages/core/contracts/data-verification-mechanism/test/MockOracleAncillary.sol) implementation for resolving DVM requests. This exposes a `pushPrice` method to be used for simulating a resolved answer in case of disputed proposals. Also, the default Görli deployment would use the already whitelisted `TestnetERC20` currency that can be minted by anyone using its `allocateTo` method.

To deploy the Optimistic Arbitrator contract on Görli, run:

```shell
NODE_URL_5=YOUR_GOERLI_NODE MNEMONIC=YOUR_MNEMONIC yarn hardhat deploy --network goerli --tags OptimisticArbitrator
```

Optionally you can verify the deployed contract on Etherscan:

```bash
ETHERSCAN_API_KEY=YOUR_API_KEY yarn hardhat etherscan-verify --network goerli --license AGPL-3.0 --force-license --solc-input
```

### Interacting with deployed contract

The following section provide instructions on how to interact with the deployed contract from the Hardhat console, though one can also use it for guidance for interacting through another interface (e.g. Remix or Etherscan).

Start Hardhat console with:

```
NODE_URL_5=YOUR_GOERLI_NODE MNEMONIC=YOUR_MNEMONIC yarn hardhat console --network goerli
```

**Initial setup**

You will need to get the libraries and connect to the necessary contracts that we are going to use. In this tutorial we are using the `TestnetERC20` token as the `currency` as described in the deployment script.

```javascript
const { getAbi, getAddress } = require("@uma/contracts-node");
const { ethers } = require("hardhat");
const signer = (await ethers.getSigners())[0];

const optimisticArbitratorDeployment = await deployments.get("OptimisticArbitrator");
const optimisticArbitrator = new ethers.Contract(
  optimisticArbitratorDeployment.address,
  optimisticArbitratorDeployment.abi,
  ethers.provider
);
const finder = new hre.ethers.Contract(
  "0xDC6b80D38004F495861E081e249213836a2F3217", // Finder address used in the deployment
  getAbi("Finder"),
  ethers.provider
);
const store = new hre.ethers.Contract(
  await finder.getImplementationAddress(ethers.utils.formatBytes32String("Store")),
  getAbi("Store"),
  ethers.provider
);
const currency = new hre.ethers.Contract(
  await optimisticArbitrator.currency(),
  getAbi("TestnetERC20"),
  ethers.provider
);
const mockOracle = new hre.ethers.Contract(
  await finder.getImplementationAddress(ethers.utils.formatBytes32String("Oracle")),
  getAbi("MockOracleAncillary"),
  ethers.provider
);
```

**Make an assertion**

First we need to mint the `bond + finalFee` amount and approve the Optimistic Arbitrator to pull the tokens. This amount of `currency` tokens will be used to propose the assertion to the Optimistic Oracle behind the scenes:

```javascript
const bond = ethers.utils.parseUnits("500", 18);
const finalFee = await store.computeFinalFee(currency.address);
const totalAmount = bond.add(finalFee.rawValue);

await (await currency.connect(signer).allocateTo(signer.address, totalAmount)).wait();
await (await currency.connect(signer).approve(optimisticArbitrator.address, totalAmount)).wait();
```

Then we can proceed to make the assertion in the Optimistic Arbitrator:

```javascript
const YES_ANSWER = ethers.utils.parseUnits("1", 18);

let requestTimestamp = await (await ethers.provider.getBlock("latest")).timestamp;

const ancillaryData = ethers.utils.toUtf8Bytes(
  `q: "Was the price of BTC was above 18000 USD for the duration of October 10 2022 UTC time considering top 5 volume weighted markets"`
);

await (
  await optimisticArbitrator.connect(signer).makeAssertion(
    requestTimestamp,
    ancillaryData,
    YES_ANSWER, // We are asserting affirmatively the ancilliaryData yes or no question
    bond,
    60 // 1 minute for test purposes
  )
).wait();
```

Then, wait one minute for the proposal's liveness time to expire. Then, we may settle the assertion to receive back the `bond + finalFee`  as we haven't been disputed. We also can verify the assertion's result that we proposed, `YES_ANSWER`, meaning that the assertion has been accepted.

```javascript
await (await optimisticArbitrator.connect(signer).settleAndGetResult(requestTimestamp, ancillaryData)).wait();
const result = await optimisticArbitrator.connect(signer).getResult(requestTimestamp, ancillaryData); // YES_ANSWER
console.log("The assertion has been accepted :", result.eq(YES_ANSWER));
```

**Make assertion and ratify**

To ratify an assertion and escalate its resolution to the DVM, we will utilize the `assertAndRatify` method, which combines assertion and ratification into a single transaction.

The Optimistic Arbitrator must be approved once more, but this time the fee is double the `finalFee`, as both proposing and disputing in the Optimistic Oracle require placing a bond.&#x20;

At the end of the procedure, we will receive half of the bond, or `1 x finalFee`. The remainder (`1 x finalFee`) will be consumed; asserting and ratifying will cost the caller `1 x finalFee`:

```javascript
const totalAmountAssertAndRatify = ethers.BigNumber.from(2).mul(bond.add(finalFee.rawValue));
requestTimestamp = await (await ethers.provider.getBlock("latest")).timestamp;

await (await currency.connect(signer).allocateTo(signer.address, totalAmount)).wait();
await (await currency.connect(signer).approve(optimisticArbitrator.address, totalAmount)).wait();
```

We can then proceed to assert and verify:

```javascript
let receipt = await (await optimisticArbitrator.connect(signer).assertAndRatify(requestTimestamp, ancillaryData, YES_ANSWER)).wait();
```

As we are in a test environment, we can simulate the vote that will occur after we assert and ratify by pushing a price to the `mockOracle`:

```javascript
const disputedPriceRequest = (
  await mockOracle.queryFilter(mockOracle.filters.PriceRequestAdded(), receipt.blockNumber, receipt.blockNumber)
)[0];

await mockOracle.connect(signer).pushPrice(
    disputedPriceRequest.args.identifier,
    disputedPriceRequest.args.time,
    disputedPriceRequest.args.ancillaryData,
    YES_ANSWER // The original assertion is accepted
  );
```

At this point, we can call `settleAndGetResult` to settle the accepted price request and then confirm that the result validates the assertion:

```javascript
await (await optimisticArbitrator.connect(signer).settleAndGetResult(requestTimestamp, ancillaryData)).wait();
const resultRatify = await optimisticArbitrator.connect(signer).getResult(requestTimestamp, ancillaryData); // YES_ANSWER
console.log("Assert and ratify result is a yes answer :", resultRatify.eq(YES_ANSWER));
```

<br>
