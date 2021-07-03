---
Description: How to use the bridge.
---

# Use the bridge

The Palm bridge can be used to transfer tokens between the Palm network and Ethereum.
When assets are transferred from Ethereum to Palm, the bridge will top up the depositer's account with a small amount of
PALM.

Palm provides the following environment-specific bridges:

- [Development environment bridge](https://app.palm-dev.xyz/bridge)
- [Testing environment bridge](https://app.palm-uat.xyz/bridge)

## Prerequisites

- To use the development environment bridge, you must be [connected to the Palm development network](../Get-Started/Connect/Development.md).
- To use the testing environment bridge, you must be [connected to the Palm testnet](../Get-Started/Connect/Testnet.md).

## Using the development environment bridge

You can transfer tokens between the Palm development network and Palm Mainnet (Ganache) using
[the development environment bridge](https://app.palm-dev.xyz/bridge).
Ganache is a fork of the Ethereum Mainnet codebase.

### 1. Connect to Palm Mainnet (Ganache)

To use Ganache, add it as a custom network to [MetaMask](https://metamask.io/index.html).
Follow [this guide to add a custom network](https://metamask.zendesk.com/hc/en-us/articles/360043227612-How-to-add-custom-Network-RPC-and-or-Block-Explorer),
using the following values:

- **Network Name:** Palm Mainnet (Ganache)
- **RPC URL:** `http://54.236.92.17:8545`
- **Chain ID:** 1

By default, Ganache will create ten accounts with a lot of test Ether.

!!! Warning

    **Palm Mainnet (Ganache) is a non-production network.
    Do not use the following test accounts on Ethereum Mainnet or any production network.**

    The following accounts' private keys are publicly visible in this documentation.
    They are not secure and everyone can use them.

    **Using test accounts on Ethereum Mainnet and production networks can lead to loss of funds and identity fraud.
    Always secure your Ethereum mainnet and any production account properly.**
    For example, see [MyCrypto "Protecting Yourself and Your Funds" guide](https://support.mycrypto.com/staying-safe/protecting-yourself-and-your-funds).


```text
Available accounts
==================
(0) 0x9d52143668aaAa0D5238B9C98419c0BfE40Ef143 (100 ETH)
(1) 0xf128fAd9Df935dD42dA9B9fFb65E43f9C5A5aD11 (100 ETH)
(2) 0x885E8509a1Bd3d04595f770FDf0940dd781f35c6 (100 ETH)
(3) 0xa930AEd9075a53120321b73E288837456e7d8954 (100 ETH)
(4) 0x5dDBF29b9024537305c1895eC41C4f28031dEFB1 (100 ETH)
(5) 0x882d478B0937446C22e6bE4C0BAB5118a0a9B2F9 (100 ETH)
(6) 0x86d6B119B2337eB717b65F48A408412508B08926 (100 ETH)
(7) 0xc146FDfe42678a0341Ca761a33373663C2A55ff9 (100 ETH)
(8) 0xC656D392E7a3fC2f9eAE14B5c7e552E9A984Becd (100 ETH)
(9) 0x89bf9d13853c75C81e2C13Ec1Ae1155493FE8F87 (100 ETH)

Private keys
==================
(0) 0x16e6570de51218154b8ed58d7d21965f3d80e38b7b766f2ff63bdf4bfc80dfd2
(1) 0x1923e5d6694203af7bd8ef46caddaf6f1c3531ccbb9327c8153773aa6694e9fc
(2) 0xb74be25ca3ee4d5b79a9b6684dbe13b0c4c6b64dd6a444c6ecb5d1054bae2dc0
(3) 0x10650b67f26687ee276d354e6e26c08e6e6f4fa8b033f9f433cdac7b295396fe
(4) 0xc5ca8cc94186a261139df728b5b9901c518155f0ceb4f3ab0b1aa40cb4ce361c
(5) 0x99cb10f36e1a056e4dc0661db88bb8b229b109ac089afd58ab2bae163879aac9
(6) 0xe58510d9e14d8c8cacdd2c64d445656f57123081d817ad9823ac4f1090681ee3
(7) 0xfd0df21daf6cbecc59a857fb0c04310518d8781fe21e7c37866eb39ae3608083
(8) 0x6a3ee61701d9404ec6231c0e2de5262591c53605e283672218125a63e3de1ca9
(9) 0xf29657af30e693efc0ec8b23e2d2839a56d23f7f7c95be1aa716bb669269c144
```

Mnemonic used to create these accounts:

```text
essay portion buddy best outdoor ramp ocean churn service transfer speak found
```

### 2. Get tokens on Ganache

The development environment bridge allows you to transfer DAI, USDC, and MEME tokens between Ganache and the Palm development network.
To get those tokens on Ganache, use [Uniswap](https://app.uniswap.org/#/swap) to swap ETH for those tokens while MetaMask
is connected to the Palm Mainnet (Ganache) network.

### 3. Transfer tokens between Ganache and the development network

Use [the Bridge UI](https://app.palm-dev.xyz/bridge) to transfer tokens between Ganache and the Palm development network, entering:

- The transfer amount.
- The token (DAI, USDC, or MEME).
- The destination address.

## Using the testing environment bridge

You can transfer tokens between the Palm testnet and the Ethereum Rinkeby testnet using
[the testing environment bridge](https://app.palm-uat.xyz/bridge).

### 1. Connect to Rinkeby and create an account

You can access Rinkeby on your MetaMask wallet by selecting "Rinkeby Test Network" under the "Networks" tab.
[Create a new account](https://metamask.zendesk.com/hc/en-us/articles/360015289452-How-to-Create-an-Additional-Account-in-your-MetaMask-Wallet).

### 2. Get tokens on Rinkeby

The testing environment bridge allows you to transfer DAI tokens between Rinkeby and the Palm testnet.
To get DAI on Rinkeby, first get some ETH onto your Rinkeby account by transferring from another account or
through the [Rinkeby faucet](https://faucet.rinkeby.io/).

Next, get DAI on Rinkeby using [Compound](https://app.compound.finance/) as follows:

1. Select "Dai" under "Supply Markets."
2. Select "Withdraw."
3. Select "Faucet" at the bottom of the screen.
   This opens MetaMask and confirms the transaction.
   After a few seconds, DAI is added to your Rinkeby account.
4. To view the DAI on your MetaMask wallet, select "Add Token" on your Rinkeby account in MetaMask and enter the contract
   `0x5592ec0cfb4dbc12d3ab100b257153436a1f0fea`.

### 3. Transfer tokens between Rinkeby and the testnet

Use [the Bridge UI](https://app.palm-uat.xyz/bridge) to transfer tokens between Rinkeby and the Palm testnet, entering:

- The transfer amount.
- The token (DAI).
- The destination address.
