# Index

Index uses UMA's optimistic oracle to bring rebalance methodology onchain. The methodology determines whether a re-weighting proposal is valid and also specifies auction parameter values related to rebalancing execution.

[Here](https://oracle.uma.xyz/settled?oracleType=Optimistic+Oracle+V3\&project=Unknown\&transactionHash=0x6b25d1b813ba004543d0eff46de6e435bd166b1380e4e50de9e84a4141dd6ba0\&eventIndex=153) is an example assertion for Index:

> proposalHash:624170ec238408ef83180e50a7be13836a80ffb01c3e9090e32b7dfda37b5662,rules:"I assert that this transaction proposal is valid according to the rules stored on IPFS under hash: QmaQmtteNydU2c6H9MJoKwVaspAsgMYJ44YE9mkNNuJmGL"

The IPFS hash can be appended to [https://ipfs.io/ipfs/](https://ipfs.io/ipfs/QmdHftq7GK552HHVoLdU41DTzxSyFhhPnPoEEuySKM7nWK?filename=guidelines.md). So for the example above, https://ipfs.io/ipfs/QmdHftq7GK552HHVoLdU41DTzxSyFhhPnPoEEuySKM7nWK can be used to view the methodology.

Index has provided a reference query [https://github.com/IndexCoop/dseth-methodology-reference](https://github.com/IndexCoop/dseth-methodology-reference) to make verifying assertions easier.

The first step is to clone the repo by running:

```
git clone https://github.com/IndexCoop/dseth-methodology-reference.git
```

After cloning, create a .env file with the following variables:

```
MAINNET_RPC_URL="https://eth-mainnet.g.alchemy.com/v2/{INFURA_URL}"
RATED_API_URL="https://api.rated.network/v0"
RATED_API_ACCESS_TOKEN="{RATED_API_TOKEN}"
MOCK_RATED_API="false"
```

You can sign up for a Rated API key here: https://www.rated.network/signUp. Once you receive the API key, input it as the `RATED_API_ACCESS_TOKEN.`

Run the following command once the .env variables are set:

```
yarn calculate-auction-rebalance-params
```

Check the input data of the assertion transaction and compare it against the script output. [Here](https://etherscan.io/tx/0x6b25d1b813ba004543d0eff46de6e435bd166b1380e4e50de9e84a4141dd6ba0) is the assertion transaction for the example above.
