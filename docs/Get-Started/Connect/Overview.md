---
Description: Connecting to networks overview.
---

# Connect to the Palm network

Use [Infura](https://infura.io/) to connect to the Palm network.
Infura is a blockchain development suite that provides access to the Ethereum network over HTTPS and WebSockets.
Although the Palm network is a separate network from Ethereum, your Infura account and project information is used to
authenticate you on the Palm network.

The instructions in this section allow you to connect to the target Palm network environment.
You can connect to:

- [The Palm development network](Development.md): Palm's development environment.
  This is useful for quick testing during development.
  This network may reset periodically, meaning data may not persist.
- [The Palm testnet](Testnet.md): Palm's testing environment.
  This is a persistent environment that will not be reset.
  This is useful as a staging environment and for long-term testing.
  A bridge exists on the Ethereum Rinkeby testnet for transferring assets to and from the Palm testnet.
- [Palm Mainnet](Mainnet.md): Palm's production environment.
  Contracts and accounts in this network hold real value and assets can be bridged across to Ethereum's Mainnet.
