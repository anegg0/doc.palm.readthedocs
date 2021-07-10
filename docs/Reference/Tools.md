---
description: Supported tools
---

# Supported tools

The Palm network supports several tools, including:

- **Fiat on-ramps.**
  [Wyre](https://www.sendwyre.com/) is integrated into the [Palm Mainnet portal](https://app.palm.io/) and [Palm Testnet portal](https://app.palm-uat.xyz/).
- **Block explorers.**
  View the [Palm Mainnet block explorer](https://explorer.palm.io/) and [Palm Testnet block explorer](https://explorer.palm-uat.xyz/).
- **Wallets.** Palm supports any Ethereum-compatible wallet. The Palm portal only supports [MetaMask](https://metamask.io/).
- **Asset bridges.**
  The [Palm Mainnet bridge](https://app.palm.io/bridge) connects Palm Mainnet to Ethereum Mainnet.
  See [how to use the Palm Mainnet bridge](../HowTo/Bridge.md#using-the-palm-mainnet-bridge).
  The [Palm Testnet bridge](https://app.palm-uat.xyz/bridge) connects Palm Testnet to the Ethereum Rinkeby testnet.
  See [how to use the Palm Testnet bridge](../HowTo/Bridge.md#using-the-palm-testnet-bridge).
- **Developer tools.**
  Palm supports any Ethereum-compatible tools and contracts, including [Truffle](https://www.trufflesuite.com/),
  [web3](https://web3js.readthedocs.io/en/v1.3.4/), and [OpenZepellin](https://openzeppelin.com/).
- **APIs.** Palm uses [Infura Ethereum APIs](https://infura.io/docs).
  See [how to connect to the Palm Infura endpoints](../Get-Started/Connect/Overview.md).

Some of these tools may require the following chain IDs (which have been registered at
[Chainlist](https://chainlist.org/)) to use the Palm network:

| Network | Chain ID |
| :-----: | :------: |
| Palm Testnet | 11297108099 |
| Palm Mainnet | 11297108109 |

For example, these can be used in web3 code to help users add or switch to the Palm network in their wallets (as
demonstrated on the Palm portal).
