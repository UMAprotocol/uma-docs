---
description: Using the Optimistic Oracle to allow for verification of insurance claims
---

# Insurance Claim Arbitration

This section covers the [insurance claims arbitration contract](https://github.com/UMAprotocol/dev-quickstart/blob/main/contracts/InsuranceArbitrator.sol), which is available in the [developer's quick-start repo](https://github.com/UMAprotocol/dev-quickstart). This tutorial shows an example of how insurance claims can be resolved and settled through the [Optimistic Oracle V2](https://github.com/UMAprotocol/protocol/blob/master/packages/core/contracts/optimistic-oracle-v2/implementation/OptimisticOracleV2.sol) contract.

You will find out how to test and deploy this smart contract and how it integrates with the Optimistic Oracle.

### Insurance Arbitrator Contract

This smart contract allows insurers to issue insurance policies by depositing the insured amount, designating the insured beneficiary, and describing the insured event.

Anyone can submit a claim that the insured event has occurred at any time. Insurance Arbitrators resolve the claim through the Optimistic Oracle by passing a question with a description of the insured event in ancillary data using the `YES_OR_NO_QUERY` price identifier specified in [UMIP-107](https://github.com/UMAprotocol/UMIPs/blob/master/UMIPs/umip-107.md).

If the claim is confirmed and settled through the Optimistic Oracle, this contract automatically pays out insurance coverage to the beneficiary. If the claim is rejected, the policy continues to be active and ready for subsequent claim attempts.

### Development environment and tests

#### Clone repository and Install dependencies&#x20;

Clone the UMA dev-quickstart repository and install the dependencies. To install dependencies, you will need to install the long-term support version of nodejs, currently Nodejs v16, and yarn. You can then install dependencies by running yarn with no arguments:

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

### Contract implementation

The contract discussed in this tutorial can be found at `dev-quickstart/contracts/InsuranceArbitrator.sol` ([here](https://github.com/UMAprotocol/dev-quickstart/blob/main/contracts/InsuranceArbitrator.sol)) within the repo.

#### Contract creation and initialization

`_finder` parameter in the constructor points the Insurance Arbitrator to the Finder contract that stores the addresses of the rest of the UMA contracts. The Finder address can be fetched from the relevant [networks](https://github.com/UMAprotocol/protocol/tree/master/packages/core/networks) file, if you are on a live network, or you can provide your own `Finder` instance if deploying UMA [protocol](https://github.com/UMAprotocol/protocol) in your own sandboxed testing environment.

`_currency` parameter in the constructor identifies the token used for settlement of insurance claims, as well as the bond currency for proposals and disputes. This token should be approved as whitelisted UMA collateral. Please check [Approved Collateral Types](../../resources/approved-collateral-types.md) for production networks or call `getWhitelist()` on the [Address Whitelist](https://github.com/UMAprotocol/protocol/blob/master/packages/core/contracts/common/implementation/AddressWhitelist.sol) contract for any of the test networks.

Alternatively, you can approve a new token address with `addToWhitelist` method in the Address Whitelist contract if working in a sandboxed UMA environment.

`_timer` is used only when running unit tests locally to simulate the advancement of time. For all the public networks (including testnets) the zero address should be used.

```solidity
    constructor(
        FinderInterface _finder,
        address _currency,
        address _timer
    ) Testable(_timerAddress) {
        finder =_finderAddress;
        currency = IERC20(_currency);
        oo = OptimisticOracleV2Interface(finder.getImplementationAddress(OracleInterfaces.OptimisticOracleV2));
    }
```

As part of initialization, the `oo` variable is set to the address of the `OptimisticOracleV2` implementation as discovered through `getImplementationAddress` method in the [Finder](https://github.com/UMAprotocol/protocol/blob/master/packages/core/contracts/data-verification-mechanism/implementation/Finder.sol) contract.

#### Issuing insurance

`issueInsurance` method allows any insurer to deposit `insuredAmount` of `currency` tokens by designating an insurance beneficiary (`insuredAddress`) and defining the insured event (`insuredEvent`). Before calling this method, the insurer should have approved this contract to spend the required amount of `currency` tokens.

```solidity
    function issueInsurance(
        string calldata insuredEvent,
        address insuredAddress,
        uint256 insuredAmount
    ) external returns (bytes32 policyId) ...
```

Internally, the issued policy is stored in the `insurancePolicies` mapping using the calculated `policyId` key that is generated by hashing the current block number with the provided insurance parameters in the internal `_getPolicyId` function.

After pulling `insuredAmount` from the caller in the `issueInsurance` method, the contract emits a `PolicyIssued` event including the `policyId` parameter that should be used when claiming insurance.

#### Submitting insurance claim

Anyone can submit an insurance claim on the issued policy by calling the `submitClaim` method with the relevant `policyId` parameter. This method will initiate both a data request and proposal with the Optimistic Oracle. A proposal bond is required, hence the caller should have approved this contract to spend the required amount of `currency` tokens for the proposal bond.

```solidity
    function submitClaim(bytes32 policyId) ...
```

After checking that the `policyId` represents a valid unclaimed insurance policy, the contract gets the current `timestamp` and composes `ancillaryData` that will be required for making requests and proposals to the Optimistic Oracle:

```solidity
        uint256 timestamp = getCurrentTime(); // note that `getCurrentTime` is exported from testable to enable easy time manipulation.
        bytes memory ancillaryData = abi.encodePacked(ancillaryDataHead, claimedPolicy.insuredEvent, ancillaryDataTail);
        bytes32 claimId = _getClaimId(timestamp, ancillaryData);
        insuranceClaims[claimId] = policyId;
```

The resulting `timestamp` and `ancillaryData` parameters are hashed in the internal `_getClaimId` method that is used as a key when storing the linked `policyId` in the `insuranceClaims` mapping. This information will be required when receiving a callback from the Optimistic Oracle.

The concatenated `ancillaryData` will have a valid question as specified in [UMIP-107](https://github.com/UMAprotocol/UMIPs/blob/master/UMIPs/umip-107.md) for `YES_OR_NO_QUERY` price identifier.

An Optimistic Oracle data request is initiated without providing any proposer reward since the proposal will be done within the `submitClaim` method:

```solidity
        oo.requestPrice(priceIdentifier, timestamp, ancillaryData, currency, 0);
```

Before the proposal is made, the Optimistic Oracle allows the requesting contract (this Insurance Arbitrator) to set additional parameters like bonding, liveness, and callback settings. This requires passing the same `priceIdentifier`, `timestamp`, and `ancillaryData` parameters to identify the request.

Total bond to be pulled from the claim initiator consists of the Optimistic Oracle proposer bond and final fee for the relevant `currency` token. This contract sets the proposer bond as a fixed percentage (constant `oracleBondPercentage`) from `insuredAmount`. When calling the `setBond` method, the Optimistic Oracle calculates and returns the total bond that would be pulled when making the proposal:

```solidity
        uint256 proposerBond = (claimedPolicy.insuredAmount * oracleBondPercentage) / 1e18;
        uint256 totalBond = oo.setBond(priceIdentifier, timestamp, ancillaryData, proposerBond);
```

Optimistic Oracle liveness is set by calling the `setCustomLiveness` method. This contract uses 24 hours so that verifiers have sufficient time to check the claim, but one can adjust the `optimisticOracleLivenessTime` constant for testing. (You probably don't want to wait a full day to resolve your test requests!)

```solidity
        oo.setCustomLiveness(priceIdentifier, timestamp, ancillaryData, optimisticOracleLivenessTime);
```

In contrast to earlier versions, the Optimistic Oracle V2 by default does not use callbacks and requesting contracts have to explicitly subscribe to them if intending to perform any logic when a data request has changed state. Here, in calling `setCallbacks`, this contract only subscribes to a callback for settlement, as implemented in the `priceSettled` method. (Note: Subscribing to any other callbacks that are not implemented in the requesting contract would make data requests unresolvable.)

```solidity
        oo.setCallbacks(priceIdentifier, timestamp, ancillaryData, false, false, true);
```

After `totalBond` amount of `currency` token is pulled from the claim initiator and approved to be taken by Optimistic Oracle, this contract proposes `1e18` representing an answer of `YES` to the raised question. Requesting and proposing affirmative answers atomically allows us to reduce the number of steps taken by end users and it is most likely expected that the insured beneficiary would be initiating the claim.

```solidity
        oo.proposePriceFor(msg.sender, address(this), priceIdentifier, timestamp, ancillaryData, int256(1e18));
```

#### Disputing insurance claim

For the sake of simplicity this contract does not implement a dispute method, but the disputer can dispute the submitted claim directly through Optimistic Oracle before the liveness passes by calling its `disputePrice` method:

```solidity
    function disputePrice(
        address requester,
        bytes32 identifier,
        uint256 timestamp,
        bytes memory ancillaryData
    ) ...
```

The disputer should pass the address of this Insurance Arbitrator contract as `requester` and all the other parameters from the original request when the claim was initiated as emitted by the Optimistic Oracle in its `RequestPrice` event.

If the claim is disputed, the request is escalated to the UMA DVM and it can be settled only after UMA voters have resolved it. To learn more about the DVM, see the docs section on the DVM: [how does UMA's DVM work](../../protocol-overview/how-does-umas-oracle-work.md#umas-data-verification-mechanism).

#### Settling insurance claim

Similar to disputes, claim settlement should be initiated through the Optimistic Oracle contract by calling its `settle` method with the same parameters:

```solidity
    function settle(
        address requester,
        bytes32 identifier,
        uint256 timestamp,
        bytes memory ancillaryData
    ) ...
```

In case the liveness has expired or a dispute has been resolved by the UMA DVM, this call would initiate a `priceSettled` callback in the Insurance Arbitrator contract:

```solidity
    function priceSettled(
        bytes32, // identifier passed by Optimistic Oracle, but not used here as it is always the same.
        uint256 timestamp,
        bytes memory ancillaryData,
        int256 price
    ) ...
```

Based on the received callback parameters, this contract can identify the relevant `claimId` that is used to get the stored insurance policy:

```solidity
        bytes32 claimId = _getClaimId(timestamp, ancillaryData);
```

Importantly, all callbacks should be restricted to accept calls only from the Optimistic Oracle to avoid someone spoofing a resolved answer:

```solidity
        require(address(oo) == msg.sender, "Unauthorized callback");
```

Depending on the resolved answer, this contract would either pay out the insured beneficiary and delete the insurance (in case of `1e18` representing the answer `YES`, the insurance claim was valid) or reject the payout and re-open the policy for any subsequent claims:

```solidity
        // Deletes insurance policy and transfers claim amount if the claim was confirmed.
        if (price == 1e18) {
            delete insurancePolicies[policyId];
            currency.safeTransfer(claimedPolicy.insuredAddress, claimedPolicy.insuredAmount);

            emit ClaimAccepted(claimId, policyId);
            // Otherwise just reset the flag so that repeated claims can be made.
        } else {
            insurancePolicies[policyId].claimInitiated = false;

            emit ClaimRejected(claimId, policyId);
        }
```

### Tests and deployment

All the unit tests covering the functionality described above are available [here](https://github.com/UMAprotocol/dev-quickstart/tree/main/test/InsuranceArbitrator). To execute all of them, run:

```bash
yarn test test/InsuranceArbitrator/*
```

Before deploying the contract check the comments on available environment variables in [the deployment script](https://github.com/UMAprotocol/dev-quickstart/blob/main/deploy/003\_deploy\_insurance\_arbitrator.ts).

In the case of the Görli testnet, the defaults would use the Finder instance that references the [Mock Oracle](https://github.com/UMAprotocol/protocol/blob/master/packages/core/contracts/oracle/test/MockOracleAncillary.sol) implementation for resolving DVM requests. This exposes a `pushPrice` method to be used for simulating a resolved answer in case of disputed proposals. Also, the default Görli deployment would use the already whitelisted `TestnetERC20` currency that can be minted by anyone using its `allocateTo` method.

To deploy the Insurance Arbitrator contract on Görli, run:

```bash
NODE_URL_5=YOUR_GOERLI_NODE MNEMONIC=YOUR_MNEMONIC yarn hardhat deploy --network goerli --tags InsuranceArbitrator
```

Optionally you can verify the deployed contract on Etherscan:

```bash
ETHERSCAN_API_KEY=YOUR_API_KEY yarn hardhat etherscan-verify --network goerli --license AGPL-3.0 --force-license --solc-input
```

### Interacting with deployed contract

The following section provide instructions on how to interact with the deployed contract from the Hardhat console, though one can also use it for guidance for interacting through another interface (e.g. Remix or Etherscan).

Start Hardhat console with:

```bash
NODE_URL_5=YOUR_GOERLI_NODE MNEMONIC=YOUR_MNEMONIC yarn hardhat console --network goerli
```

#### Initial setup

From the Hardhat console, start by adding the required `getAbi` dependency for interacting with UMA contracts and use the first two accounts as insurer and insured beneficiary:

```javascript
const { getAbi } = require("@uma/contracts-node");
const [insurer, insured] = await ethers.getSigners();
```

Grab the deployed Insurance Arbitrator contract:

```javascript
const insuranceArbitratorDeployment = await deployments.get("InsuranceArbitrator");
const insuranceArbitrator = new ethers.Contract(
	insuranceArbitratorDeployment.address,
	insuranceArbitratorDeployment.abi,
	ethers.provider
);
```

#### Issue insurance

Assuming `TestnetERC20` was used as `currency` when deploying, mint the required insurance amount (e.g. 10,000 TEST tokens) and approve the Insurance Arbitrator to pull them:

```javascript
const insuredAmount = ethers.utils.parseEther("10000");
const currency = new ethers.Contract(await insuranceArbitrator.currency(), getAbi("TestnetERC20"), ethers.provider);
await (await currency.connect(insurer).allocateTo(insurer.address, insuredAmount)).wait();
await (await currency.connect(insurer).approve(insuranceArbitrator.address, insuredAmount)).wait();
```

Issue the insurance policy and grab the resulting `policyId` from the emitted `PolicyIssued` event:

```javascript
const issueReceipt = await (await insuranceArbitrator.connect(insurer).issueInsurance(
	"Bad things have happened",
	insured.address,
	insuredAmount
)).wait();
const policyId = (await insuranceArbitrator.queryFilter(
	"PolicyIssued",
	issueReceipt.blockNumber,
	issueReceipt.blockNumber
))[0].args.policyId;
```

#### Submit insurance claim

First calculate the expected proposer bond:

```javascript
const proposerBond = insuredAmount.mul(await insuranceArbitrator.oracleBondPercentage()).div(ethers.utils.parseEther("1"));
```

Fetch the expected final fee from the `Store` contract (which is discovered through the `Finder`):

```javascript
const finder = new ethers.Contract(await insuranceArbitrator.finder(), getAbi("Finder"), ethers.provider);
const store = new ethers.Contract(await finder.getImplementationAddress(
	ethers.utils.formatBytes32String("Store")),
	getAbi("Store"),
	ethers.provider);
const finalFee = (await store.computeFinalFee(currency.address)).rawValue;
```

Calculate the expected total bond and provide funding/approval for the insured claimant:

```javascript
const totalBond = proposerBond.add(finalFee);
await (await currency.connect(insured).allocateTo(insured.address, totalBond)).wait();
await (await currency.connect(insured).approve(insuranceArbitrator.address, totalBond)).wait();
```

Now initiate the insurance claim and grab request details from the `RequestPrice` event emitted by the Optimistic Oracle:

```javascript
const oo = new ethers.Contract(await insuranceArbitrator.oo(), getAbi("OptimisticOracleV2"), ethers.provider);
const claimReceipt = await (await insuranceArbitrator.connect(insured).submitClaim(policyId)).wait();
const request = (await oo.queryFilter("RequestPrice", claimReceipt.blockNumber, claimReceipt.blockNumber))[0].args;
```

#### Dispute insurance claim

Before liveness passes, the insurer can dispute the claim through the Optimistic Oracle. First, they must fund and approve with the same bonding amount:

```javascript
await (await currency.connect(insurer).allocateTo(insurer.address, totalBond)).wait();
await (await currency.connect(insurer).approve(oo.address, totalBond)).wait();
```

If you are on a testnet like Göerli, in order to simulate UMA voting on a testnet, you can use the [Mock Oracle](https://github.com/UMAprotocol/protocol/blob/master/packages/core/contracts/data-verification-mechanism/test/MockOracleAncillary.sol):

```javascript
const mockOracle = new ethers.Contract(await finder.getImplementationAddress(
	ethers.utils.formatBytes32String("Oracle")),
	getAbi("MockOracleAncillary"),
	ethers.provider);
```

Now initiate the dispute and grab the vote request details from the `PriceRequestAdded` event emitted by the Mock Oracle:

```javascript
const disputeReceipt = await (await oo.connect(insurer).disputePrice(
	request.requester,
	request.identifier,
	request.timestamp,
	request.ancillaryData
)).wait();
const voteRequest = (await mockOracle.queryFilter(
	"PriceRequestAdded",
	disputeReceipt.blockNumber,
	disputeReceipt.blockNumber
))[0].args;
```

#### Settle insurance claim

Before settling the claim, we can take a look at the vote request as seen by UMA voters:

```javascript
console.log("identifier:", ethers.utils.parseBytes32String(voteRequest.identifier));
console.log("time:", Number(voteRequest.time));
console.log("ancillaryData:", ethers.utils.toUtf8String(voteRequest.ancillaryData));
```

The `ancillaryData` should start with `q:"Had the following insured event occurred as of request timestamp: Bad things have happened?"`. It is then followed by the `ooRequester` key with our Insurance Arbitrator address in its value.

In order to simulate `YES` as the resolved answer we would pass `1e18` as the `price` parameter in the Mock Oracle `pushPrice` method:

```javascript
await (await mockOracle.connect(insured).pushPrice(
	voteRequest.identifier,
	voteRequest.time,
	voteRequest.ancillaryData,
	ethers.utils.parseEther("1")
)).wait();
```

Now we can settle the request through the Optimistic Oracle and observe the emitted `ClaimAccepted` from our Insurance Arbitrator contract:

```javascript
const settleReceipt = await (await oo.connect(insured).settle(
	request.requester,
	request.identifier,
	request.timestamp,
	request.ancillaryData
)).wait();
const claimSettlementEvent = (await insuranceArbitrator.queryFilter(
	"ClaimAccepted",
	settleReceipt.blockNumber,
	settleReceipt.blockNumber
))[0];
console.log(claimSettlementEvent);
```

The above settlement transaction should also transfer `insuredAmount` tokens to the insured beneficiary as well as return the proposer bond to the claim initiator.

Alternatively, if `0` value was resolved, the settlement transaction should emit the `ClaimRejected` event without paying out the `insuredAmount` and returning the bond to the disputer, along with half of the proposer's bond.
