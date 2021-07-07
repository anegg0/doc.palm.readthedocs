---
Description: How to use the bridge.
---

# Use the bridge

The Palm bridge can be used to transfer assets between the Palm network and Ethereum.

A small fee is required to move assets across the bridge.
This fee covers the cost of gas and is used to purchase carbon offsets in order to make these operations carbon neutral.
When assets are transferred onto the Palm network, the bridge will top up the depositor's account with a small amount of PALM.

Palm provides the following environment-specific bridges:

- [Palm Testnet bridge](https://app.palm-uat.xyz/bridge)
- [Palm Mainnet bridge](https://app.palm.io/bridge)

## Using the Palm Testnet bridge

You can use [the Palm Testnet bridge](https://app.palm-uat.xyz/bridge) to transfer tokens between Palm Testnet and
the Ethereum Rinkeby testnet.
You must be [connected to Palm Testnet](../Get-Started/Connect/Testnet.md).

### 1. Create a Rinkeby account

Access Rinkeby on your Metamask wallet by selecting **Rinkeby Test Network** under the **Networks** tab.
[Create a new account](https://metamask.zendesk.com/hc/en-us/articles/360015289452-How-to-Create-an-Additional-Account-in-your-MetaMask-Wallet).

### 2. Get tokens on Rinkeby

The bridge allows you to transfer DAI tokens between Rinkeby and Palm Testnet.
To get DAI on Rinkeby, first get some ETH onto your Rinkeby account by transferring from another account or
through the [Rinkeby faucet](https://faucet.rinkeby.io/).

Next, get DAI on Rinkeby using [Compound](https://app.compound.finance/) as follows:

1. Select **Dai** under **Supply Markets**.
2. Select **Withdraw**.
3. Select **Faucet** at the bottom of the screen.
   This opens Metamask and confirms the transaction.
   After a few seconds, DAI is added to your Rinkeby account.
4. To view the DAI on your Metamask wallet, select **Add Token** on your Rinkeby account in Metamask and enter the contract
   `0x5592ec0cfb4dbc12d3ab100b257153436a1f0fea`.

### 3. Transfer tokens between Rinkeby and Palm Testnet

Use [the Palm Testnet bridge](https://app.palm-uat.xyz/bridge) to transfer tokens between Rinkeby and Palm Testnet, entering:

- The transfer amount.
- The token (DAI).
- The destination address.

## Using the Palm Mainnet bridge

You can use [the Palm Mainnet bridge](https://app.palm.io/bridge) to transfer tokens between Palm Mainnet and Ethereum Mainnet.
You must be [connected to Palm Mainnet](../Get-Started/Connect/Mainnet.md)

### 1. Create an Ethereum Mainnet account

Access Ethereum Mainnet on your Metamask wallet by selecting **Ethereum Mainnet** under the **Networks** tab.
[Create a new account](https://metamask.zendesk.com/hc/en-us/articles/360015289452-How-to-Create-an-Additional-Account-in-your-MetaMask-Wallet).

### 2. Get tokens on Ethereum Mainnet

The bridge allows you to transfer DAI tokens between Ethereum Mainnet and Palm Mainnet.
You can get DAI by trading or selling it on exchanges such as [Coinbase](https://www.coinbase.com/), or taking out a
loan on [MakerDAO](https://makerdao.com/en/).

Transfer DAI onto your Ethereum Mainnet account on your Metamask wallet.

### 3. Transfer tokens between Ethereum Mainnet and Palm Mainnet

Use [the Palm Mainnet bridge](https://app.palm.io/bridge) to transfer tokens between Ethereum Mainnet and Palm Mainnet, entering:

- The transfer amount.
- The token (DAI).
- The destination address.
