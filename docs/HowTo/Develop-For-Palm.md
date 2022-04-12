---
description: how to develop for the palm network
---

# Develop for Palm

Palm is a fully EVM-compatible Ethereum side chain, which means that any [standard EVM-compatible tools](Use-Supported-Tools/Tools.md)
and workflows should work seamlessly on the Palm network.

## Local development

Develop your dapp and test using standard Ethereum tools such as:

- [Solidity](https://docs.soliditylang.org/en/latest).
- [Truffle](https://www.trufflesuite.com/truffle).
- [Hardhat](https://hardhat.org/).
- [Ganache](https://www.trufflesuite.com/ganache).
- [web3.js](https://web3js.readthedocs.io/).
- [ethers.js](https://docs.ethers.io/v5/).

During development, you can connect to your local testing environment with your standard Ethereum tools just as you would when developing for Ethereum.

If you're new to smart contract development, you can follow standard Ethereum tutorials such as:

* [Truffle's Pet Shop Tutorial](https://www.trufflesuite.com/tutorial).

* [HardHat's quick start](https://hardhat.org/getting-started/#quick-start).

## Connecting to the Palm network

### Developers

As a developer, read the following quick guides to quickly connect to [Palm testnet](../Get-Started/Connect/Testnet.md) as well as [Palm mainnet](../Get-Started/Connect/Mainnet.md).

### End users

When developing for Palm, you may need to get end users set up and connected to the Palm network.
Because Palm is a relatively new network, it may not be supported out-of-the-box across all wallet providers.

To be able to transact on the Palm network, users may need to:

- [Configure their wallet](https://app.palm.io/configure) to recognize the Palm network.
- Get assets onto the Palm network by either purchasing assets on the network directly or [moving them onto Palm from Ethereum](Bridge.md).
- [Get PALM](../Get-Started/Tokens.md) to pay for transactions.

Palm provides an app that users can access to complete these onboarding tasks:

- [Palm Mainnet](https://app.palm.io/)
- [Palm Testnet](https://app.palm-uat.xyz/)

You can link directly to the [Palm app](https://app.palm.io/) to help your users onboard.

## Deploying a Dapp to Palm

To deploy a Dapp to Palm, you will need [access to PALM](../Get-Started/Tokens.md) in order to pay the transaction fees required for deployment.

You can [deploy using Truffle Migrations](Deploy-using-Truffle.md), [deploy using Hardhat](Deploy-using-Hardhat.md) or other typical Ethereum deployment processes.

## Minting NFTs on Palm

You can also quickly learn how to [mint NFTs on the Palm network using HardHat](Mint-NFT-using-Hardhat.md).

## Integrating with the bridge

The Palm network offers a bridge enabling users to transfer their assets to and from to Ethereum mainnet.
You can [learn how to integrate with it](Integrate-With-bridge.md).

## Indexing your contracts

Tools like Covalent and The Graph are deployed on the Palm network to facilitate pulling data from the chain.

You can learn how to use those protocols on the Palm network:

* [Covalent API](./Use-Supported-Tools/Covalent.md)

* [Using The Graph on the Palm Network](./Use-Supported-Tools/TheGraph.md)
