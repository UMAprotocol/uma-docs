# Network Information

| Network                                      | UMA Contract Addresses                                                                                                                                                                              | Fully Permissionless Settlement | Bot Support | UI Support |
| -------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------- | ----------- | ---------- |
| [Ethereum](https://ethereum.org/)            | [Mainnet](https://github.com/UMAprotocol/protocol/blob/master/packages/core/networks/1.json), [Göerli](https://github.com/UMAprotocol/protocol/blob/master/packages/core/networks/5.json)           | Yes                             | Yes         | Yes        |
| [Polygon](https://polygon.technology/)       | [Mainnet](https://github.com/UMAprotocol/protocol/blob/master/packages/core/networks/137.json), [Mumbai](https://github.com/UMAprotocol/protocol/blob/master/packages/core/networks/80001.json)     | Yes                             | Yes         | Yes        |
| [Boba Network](https://boba.network/)        | [Mainnet](https://github.com/UMAprotocol/protocol/blob/master/packages/core/networks/288.json)                                                                                                      | Yes                             | Yes         | Yes        |
| [Optimism](https://www.optimism.io/)         | [Mainnet](https://github.com/UMAprotocol/protocol/blob/master/packages/core/networks/10.json)                                                                                                       | Yes                             | Yes         | Yes        |
| [Arbitrum](https://arbitrum.io/)             | [Mainnet](https://github.com/UMAprotocol/protocol/blob/master/packages/core/networks/42161.json), [Rinkeby](https://github.com/UMAprotocol/protocol/blob/master/packages/core/networks/421611.json) | Yes                             | Yes         | Yes        |
| [Gnosis Chain](https://www.gnosischain.com/) | [Mainnet](https://github.com/UMAprotocol/protocol/blob/master/packages/core/networks/100.json)                                                                                                      | Multi-sig relay                 | No          | Yes        |
| [Avalanche](https://www.avax.network/)       | [C-Chain](https://github.com/UMAprotocol/protocol/blob/master/packages/core/networks/43114.json)                                                                                                    | Multi-sig relay                 | No          | Yes        |
| [Base](https://docs.base.org/)               | [Goerli](https://github.com/UMAprotocol/protocol/blob/master/packages/core/networks/84531.json)                                                                                                     | n/a                             | n/a         | no         |
| _Deprecated_                                 | [Kovan](https://github.com/UMAprotocol/protocol/blob/master/packages/core/networks/42.json), [Rinkeby](https://github.com/UMAprotocol/protocol/blob/master/packages/core/networks/4.json)           | n/a                             | n/a         | No         |

{% hint style="info" %}
UMA's data verification mechanism (DVM), which is used to resolve disputes, is on Ethereum mainnet. Where possible, UMA uses a chain's native messaging bridge to relay dispute and governance information between that chain and Ethereum.

On chains where no such native messaging bridge exists, UMA uses a multi-sig controlled by UMA core engineers at Risk Labs to relay disputes to the DVM, to return data from the DVM to requesters on that chain, and to execute governance actions (for instance, adding new identifiers).

UMA is researching decentralized cross-chain messaging systems to make these chains fully permissionless.
{% endhint %}

{% hint style="info" %}
Anyone can propose and dispute on any chain, and run bots to monitor contracts or propose and dispute automatically. For convenience, Risk Labs runs monitoring bots connected to UMA Discord channels, as well as proposer and disputer bots, on chains that secure significant value.

If you plan to launch on a chain not currently supported by the bots, we recommend you contact Risk Labs to add bot support, which will make it easier for third-party proposers and disputers to monitor your contracts. We also recommend running your own bots to provide additional resiliency.
{% endhint %}
