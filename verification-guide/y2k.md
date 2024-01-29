# Y2K

Y2K uses UMA's oracle to assert the price of Chainlink’s crvUSD price on Ethereum mainnet. [Here](https://oracle.uma.xyz/settled?oracleType=Optimistic+Oracle+V3\&chainName=Arbitrum\&transactionHash=0xbb8f21f912c86276bce13be17c0e600f2e7de2d9a8107480adb46a37c2461cac\&eventIndex=9) is an example Y2K assertion that will be used for reference:

> As of assertion timestamp 1705089029. Using the following resources for validation {chain: Ethereum, dataSource: Chainlink, assetIdOnDataSource: CRVUSD/USD, methodologyUrl: https://shorturl.at/cIP39}. The price of crvUSD/USD is 99908022000000000000000000 for roundId 18446744073709551762.

The first step is to use `methodologyUrl` to retrieve the assertion methodology. Included in the file are steps to recalculate the price of crvUSD/USD and compare the value against the asserted price of `99908022000000000000000000` at roundId `18446744073709551762`.

To fetch the relevant price for the query, follow these steps from the methodology:

1\. Fetch the address for Chainlink’s EACAggregatorProxy contract for crvUSD on Ethereum mainnet (address: 0xEEf0C605546958c1f899b6fB336C20671f9cD49F)

2\. Query the getRoundData(\<roundId>) on this contract which will return a struct with the following information → {roundId, answer, startedAt,updatedAt, answeredInRound}

3\. The answer should be scaled by 10 \*\* 18 (e.g. answer \* 10 \*\*18) and the scaled value should be compared against the value for \<AssertionPrice> - both should be the same

The contract for crvUSD on mainnet can be found [here](https://etherscan.io/address/0xEEf0C605546958c1f899b6fB336C20671f9cD49F) or by going to [https://etherscan.io/](https://etherscan.io/) and searching for address 0xEEf0C605546958c1f899b6fB336C20671f9cD49F. To query the roundId:

1. Click on the "Contract" tab
2. Click on the "Read Contract" tab
3. Input `RoundId` referenced in the assertion into the `getRoundId` input

<figure><img src="../.gitbook/assets/image (28).png" alt=""><figcaption></figcaption></figure>

Inputting the `roundId` and clicking "Query" provides an `answer` of `99908022`.

<figure><img src="../.gitbook/assets/image (27).png" alt=""><figcaption></figcaption></figure>

This value needs to be scaled by 10 \*\* 18. https://eth-converter.com/ can be used to scale the value by inputting the `answer` into the "Ether" input and the "Wei" value can be used to compare against the assertion price.&#x20;

For this assertion, `99908022000000000000000000` matches the asserted value of `99908022000000000000000000` . Therefore, this assertion should not be disputed.
