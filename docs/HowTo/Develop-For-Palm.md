---
Description: How to develop for palm
---

# Develop for Palm

Palm is a fully EVM-compatible Ethereum side chain, which means that any [standard EVM-compatible tools](Use-Supported-Tools/Tools.md)
and workflows should work seamlessly on the Palm network.

## Local development

Develop your dapp and test using standard Ethereum tools such as:

- [Solidity](https://docs.soliditylang.org/en/latest).
- [Truffle](https://www.trufflesuite.com/truffle).
- [Ganache](https://www.trufflesuite.com/ganache).
- [web3.js](https://web3js.readthedocs.io/).
- [ethers.js](https://docs.ethers.io/v5/).

During development, you can connect to your local testing environment with your standard Ethereum tools just as you
would if developing for Ethereum.

If you're new to smart contract development, you can follow standard Ethereum tutorials such as
[Truffle's Pet Shop Tutorial](https://www.trufflesuite.com/tutorial).

## Connecting end users to the Palm network

When developing for Palm, you may need to get end users set up and connected to the Palm network.
Because Palm is a relatively new network, it may not be supported out-of-the-box across all wallet providers.

To be able to transact on the Palm network, users may need to:

- Configure their wallet to recognize the Palm network.
- Get assets onto the Palm network by either purchasing assets on the network directly or
  [moving them onto Palm from Ethereum](../HowTo/Bridge.md).
- [Get PALM](../Get-Started/Tokens.md) to pay for transactions.

Palm provides an app that users can access to complete these onboarding tasks:

- [Palm Mainnet](https://app.palm.io/)
- [Palm Testnet](https://app.palm-uat.xyz/)

You can link directly to the Palm app to help your users onboard.

## Deploying a dapp to Palm

Before you can deploy a dapp to Palm, you must get [access to PALM](../Get-Started/Tokens.md) in order to pay the transaction
fees required for deployment.

You can [deploy using Truffle Migrations](Deploy-using-Truffle.md) or other typical Ethereum deployment processes.
