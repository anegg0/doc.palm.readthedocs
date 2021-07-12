---
description: Supported tools
---

# Supported tools

The Palm network is fully compatible with Ethereum and supports any EVM-compatible tools, including:

- **Fiat on-ramps.**
  [Wyre](https://www.sendwyre.com/) is integrated into the [Palm Mainnet portal](https://app.palm.io/) and [Palm Testnet portal](https://app.palm-uat.xyz/).
- **Block explorers.**
  A block explorer is available for [Palm Mainnet](https://explorer.palm.io/) and [Palm Testnet](https://explorer.palm-uat.xyz/).
- **Wallets.** Palm supports any EVM-compatible wallet. The Palm portal only supports [MetaMask](https://metamask.io/).
- **Asset bridges.**
  The [Palm Mainnet bridge](https://app.palm.io/bridge) can be used to
  [transfer tokens between Palm Mainnet and Ethereum Mainnet](../HowTo/Bridge.md#using-the-palm-mainnet-bridge).
  The [Palm Testnet bridge](https://app.palm-uat.xyz/bridge) can be used to
  [transfer tokens between Palm Testnet and the Ethereum Rinkeby testnet](../HowTo/Bridge.md#using-the-palm-testnet-bridge).
- **Developer tools.**
  Palm supports any EVM-compatible tools and contracts, including [Truffle](https://www.trufflesuite.com/),
  [web3](https://web3js.readthedocs.io/en/v1.3.4/), and [OpenZeppelin](https://openzeppelin.com/).
- **APIs.** Palm uses [Infura Ethereum APIs](https://infura.io/docs).
  You must [connect to the Palm Infura endpoints](../Get-Started/Connect/Overview.md) or
  [run a transaction node](../HowTo/Run-a-Transaction-Node.md) to read transaction history, or send transactions on the
  Palm network without performing [validator duties](../Concepts/Validators.md).

Some of these tools may require the following chain IDs (which have been registered at
[Chainlist](https://chainlist.org/)) to use the Palm network:

| Network | Chain ID |
| :-----: | :------: |
| Palm Testnet | 11297108099 |
| Palm Mainnet | 11297108109 |

For example, these can be used in web3 code to help users add or switch to the Palm network in their wallets (as
demonstrated on the Palm portal).

Some Ethereum-based tooling may require additional set up to integrate into the Palm network.
If you require any additional tooling that is not yet available, feel free to [contact us] to request support.

<!-- links -->
[contact us]: https://share.hsforms.com/1_sBreu7XTMWZtH9n1xTP3g2urwb
