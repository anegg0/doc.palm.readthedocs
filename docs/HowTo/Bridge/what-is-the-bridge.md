---
description: what is the palm network bridge?
---

## What is the Palm network Bridge?

The Palm Network enables NFT trading in a fast, cost-efficient, and eco-friendly manner. However, some users might decide to move their assets from the Palm Network to Ethereum in order to reach specific marketplaces. They can transfer their token(s) using the Palm network bridge.

As a developer you can support those users by ensuring your smart contracts are compatible with the Palm network bridge.

The Palm network bridge connects the Palm network with Ethereum. It allows transferring assets such as ERC-20 and ERC-721 tokens back and forth between the Palm Network and Ethereum.

The bridge works by locking tokens that have already been minted on one side of the bridge and then minting an equivalent token on the other side, using what we call a “synthetic” version of the ERC-721 contract:

![](../../static/img/token-bridge-flow.png)

Users can send their tokens back to the original side: the bridge will burn the synthetic token and release the original token that will be transferred to the destination wallet address:

![](../../static/img/token-bridge-reverse-flow.png)

A fee is required for transferring assets from Palm to Ethereum (to cover gas costs of minting and a carbon offset). Moving assets back to Palm will top up the depositor's account with a small amount of PALM tokens.

!!! abstract "What does an end-user see when she uses the bridge?"

    The Palm network [provides a user-friendly Dapp](https://app.palm.io/nftbridge) where users can initiate the transfer and give their approval to pay transfer fees (in DAI).

Now, let's dive a bit deeper into how the bridge operates:

The Palm bridge runs on [ChainBridge](https://chainbridge.chainsafe.io/), a communication protocol where events on the source chain are used to send messages routed to the destination chain, where they will be submitted as transactions.

A few concepts specific to ChainBridge:

* **Relayers** — Off-chain servers that listen for particular events on the source chain and submit signed proposals to the destination chain.
* **Bridge contracts** — Delegate calls to the handler contracts for deposits, start a transaction on the source chain, and execute the proposals on the target chain.
* **Handler contracts** — Palm's handler contracts send mint/burn transactions depending on the user input.
* **Target contracts** — On Palm, target contracts are ERC-20, ERC-721 and ERC-1155 on each side of the bridge.
* **Deposit() function** — Here, a deposit is simply the initiation of a transfer of a piece of data, often representing instructions to lock a token in the bridge. In the reverse direction, the deposit is an instruction to burn a token.
* **Resource ID** — Identifier for the transferring token's smart contract. Resource ID is used to link the equivalent contracts on both sides of the bridge.
* **Chain ID** — Identifier of the chain, for example, Palm Network or Ethereum
* **Calldata** — Payload contained by an event/proposal. The calldata represents a function to be executed on the targeted chain. On Palm, calldata represent the mint() functions.

### Transfer flow

!!! abstract "What actually happens when an end-user uses the bridge?"

    Here’s the workflow occurring when a user transfers an ERC-721 token from the Palm Network to Ethereum:

    1. The user calls the `deposit()` function on Palm Network’s bridge contract. The user must provide the `target chain`, the `resource ID`, and the `calldata`, which represent a token transfer to be executed on Ethereum.
    2. The ERC-721 handler’s `deposit()` function is called, which verifies the data provided by the user. The bridge then locks the token on the ERC-721 contract.
    3. Proposal -  Palm’s bridge contract then emits a `Deposit` event containing the data that will be executed on Ethereum. On ChainBridge, this type of event is called a `proposal`.
    4. Once the bridge's first relayer detects the event on Ethereum, it executes the proposal on Ethereum via the bridge. Effectively, the proposal delegates an `executeDeposit `call to the ERC-721 handler contract.
    6. The ERC-721 handler’s `executeDeposit` function validates the parameters provided by the user and makes a call to the target ERC-721 contract to mint the token with the original ID (a custom `mint` function on the target contract is passed the token ID as part of the calldata to ensure this). The token is transferred to the recipient's account on Ethereum.

### Bridge Fees

A fee is required to transfer assets from the Palm network to Ethereum.
Part of this charge covers the cost of gas and the other part is used to purchase carbon offsets to make these operations carbon neutral.

#### Bridge's fee calculation

 The Palm network bridge automatically calculates and adjusts the fees based on average Ethereum gas prices. It updates every three hours, so the fees reflect the cost of gas incurred by the bridge.

 The bridge is designed to be cost-neutral, and the Palm network does our best to set the price in a fair and predictable way that doesn’t change too much, even when the underlying gas prices do.

![ethereum gas prices over 3 days](../../Images/gas-prices-3-days-700x335.png "Chart 1: Gas prices over the past 3 days showing significant volatility")

More on the technical aspects of how this will work:

When transferring to Ethereum, there are two transactions executed by the bridge on Mainnet.

The first transaction confirms that the tokens on the Palm network side are definitely locked in the bridge (when moving NFTs) or are burned (when moving DAI or other ERC-20 tokens back to Ethereum).

The second transaction will transfer the token to the recipient wallet address. Depending on the token type, the gas consumed by these transactions is usually just under 400,000 units total. The gas cost is calculated by multiplying gas usage by gas price, ie. `400,000 x 100 gwei = 0.04 Ether`. When gas is 50 gwei, consumption will be 0.02 Ether.

Designed to be cost-neutral, the median gas prices for the past 72 hours are calculated based on snapshots taken of price at 2 minute intervals. This helps smooth the fluctuation of prices during periods of network congestion.

Given that fees are charged in DAI, but are charged to the bridge in Ether, we then calculate an moving average exchange rate using the previous 7 days ETH/DAI exchange rates, with an adjustment factor of one standard deviation to reduce the effects of exchange rate volatility.

Once we have calculated the fee in DAI, we update the relevant fee information in the bridge smart contracts.

![bridge fees](../../Images/bridge-fees-700x267.png "Chart 2: Theoretical bridge fees for the past 7 days calculated using moving-median gas prices (50th percentile)")

The chart above shows what the automated fee would have been for the past week. There may be times where you will pay a little more than the actual transaction costs, and other times when you’ll pay a little less. But given that the prices are being updated more frequently, they should be pretty close to the actual cost.

We’re currently evaluating and planning the migration steps to integrate updated contracts that use less gas, which will lead to lower fees in the future.


In the [***next article***](./make-contracts-bridge-compatible.md) we will explain how to make your smart contracts bridge-compatible.

!!! question
    Any question or suggestion about the Palm network bridge? Drop them on our [Discord](https://discord.gg/grcpwNRxVj)
