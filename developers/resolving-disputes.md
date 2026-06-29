# Resolving Disputes

If the dispute is on a live network, it will be resolved by the [DVM](https://docs.umaproject.org/protocol-overview/how-does-umas-oracle-work) on Ethereum, and the results returned within 48-72 hours (depending on when the dispute was raised during the DVM voting cycle).

If you are testing the dispute flow on Görli, you will need to manually resolve the dispute through the [Mock Oracle](https://goerli.etherscan.io/address/0x20570e9e27920ac5e2601e0bef7244deff7f0b28#code). This contract stands in for the DVM on Görli and allows you to manually return your own values for testing purposes.

1. Go to the [Mock Oracle contract](https://goerli.etherscan.io/address/0x20570e9e27920ac5e2601e0bef7244deff7f0b28#code) on Görli Etherscan.
2. Click 'Write Contract.'
3. Click 'Connect to Web3' to connect your wallet.
4. Click 'OK' on the pop-up that warns you that writing to contracts on Etherscan is a beta test feature.
5. Connect through your wallet interface, and switch networks to Görli if necessary.
6. Click `pushPrice` to see the parameters you will need to enter.
7. Enter the original request's `identifier`, `time`, and `ancillaryData`, and enter the value you want to return for `price`.
8. Click 'Write' and submit the transaction.
9. Depending on how the contract you are testing was written, you may need to call `settleAndGetPrice` from your contract to the Optimistic Oracle contract to get the value returned from the mock oracle, or you may be able to call `settle` on the Optimistic Oracle and have your contract automatically receive and handle the return value with a `priceSettled` callback function.
