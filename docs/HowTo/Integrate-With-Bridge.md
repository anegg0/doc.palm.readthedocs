----
-Description: integrating with the palm network bridge.
----

# Integrating With The Palm Network Bridge

The Palm Network enables NFT trading in a fast, cost-efficient, and eco-friendly manner. However, some users might prefer to move their assets from the Palm Network to Ethereum in order to reach marketplaces such as [OpenSea](https://opensea.io/).

As a developer, you might want to support those users wishing to transfer tokens from the Palm Network to Ethereum, and so you will want to know how to prepare your ERC-721 smart contracts to ensure that the NFT Bridge supports them.

This article contains two sections:



1. A high-level description of how the Palm Network bridge operates.
2. Instructions on how to integrate your ERC-721 contracts with Palm Network bridge.

Note: In this article, the term “Palm” designates the Palm network.


## What Does The Palm Bridge Do?

The bridge allows transferring assets such as ERC-20 and ERC-721 tokens back and forth between the Palm network and Ethereum.

In the case of NFTs, the bridge works by locking tokens that have already been minted on one side of the bridge and then minting an equivalent token on the other side, using what we call a “synthetic” version of the ERC-721 contract.

![alt_text](images/image1.png "image_tooltip")


Users can send their tokens back to the original side: the bridge will burn the synthetic token and release the original token that will be transferred to the destination wallet address.

![alt_text](images/image2.png "image_tooltip")


A fee is required for transferring assets from Palm to Ethereum (to cover gas costs of minting and a carbon offset). Moving assets back to Palm will top up the depositor's account with a small amount of PALM tokens.

Now let's dive a bit deeper into how the bridge operates:


## How Does The Palm Bridge Work?

The Palm bridge runs on [ChainBridge](https://chainbridge.chainsafe.io/), a communication protocol where events on the source chain are used to send messages routed to the destination chain, where they will be submitted as transactions.

A few concepts specific to ChainBridge:



* **Relayers** — Off-chain servers that listen for particular events on the source chain and submit signed proposals to the destination chain.
* **Bridge contracts** — Delegate calls to the handler contracts for deposits, start a transaction on the source chain, and execute the proposals on the target chain.
* **Handler contracts** — Palm's handler contracts send mint/burn transactions depending on the user input.
* **Target contract**s — On Palm, target contracts are ERC-721 and ERC-1155 on each side of the bridge.
* **Deposit() function** — Here, a deposit is simply the initiation of a transfer of a piece of data, often representing instructions to execute a mint() function.
* **Resource ID** — Identifier for the ERC-721 smart contract. Resource ID is used to link the contracts on both sides of the bridge.
* **Chain ID** — Identifier of the chain, for example, Palm network or Ethereum
* **Calldata** — Payload contained by an event/proposal. The calldata represents a function to be executed on the targeted chain. On Palm, calldata represent the mint() functions.


### Transfer flow

Here’s the workflow occurring when a user transfers an ERC-721 token from the Palm network to Ethereum :



1. The user calls the _deposit()_ function on Palm network’s bridge contract. The user must provide the target chain, the resource ID, and the _calldata, which represent a token transfer to be executed on Ethereum_.  \
 \
The user is triggering the deposit on the Bridge UI \
	* fees \
	* approval from the user to spend DAI or other currency \
Include bridge video (blog post) \
 \
	*
2. The ERC-721 handler’s _deposit()_ function is called, which verifies the data provided by the user. The bridge then locks the token on the ERC-721 contract.
3. Proposal -  Palm’s bridge contract then emits a _Deposit_ event containing the data that will be executed on Ethereum. On ChainBridge, this type of event is called a _proposal_.
4. Vote - Once a relayer detects the event on Ethereum, it triggers a vote on the proposal.  The vote happens between relayers on Ethereum’s bridge contract.
5. If a quorum is met, a relayer executes the proposal on Ethereum via the bridge. Effectively, the proposal delegates an `executeDeposit `call to the ERC-721 handler contract.
6. The ERC-721 handler’s `executeDeposit` function validates the parameters provided by the user and makes a call to the target ERC-721 contract to mint the token with the original ID and transfers it to its owner’s account on Ethereum.


## Helpful Information - Palm Mainnet

The following table contains the contracts and addresses of the main bridge components.


<table>
  <tr>
   <td>Chain
   </td>
   <td>Address
   </td>
   <td>Description
   </td>
  </tr>
  <tr>
   <td>Palm
   </td>
   <td><code><a href="https://explorer.palm.io/address/0xB3C62Aed3be8e0577D4724C40a01379dbf895C01/transactions">0xB3C62Aed3be8e0577D4724C40a01379dbf895C01</a></code>
   </td>
   <td>Bridge Contract - main bridge smart contract
   </td>
  </tr>
  <tr>
   <td>Palm
   </td>
   <td><code><a href="https://explorer.palm.io/address/0x97FAcbF880e47c27cafbA6bE3d677A50d536813e/transactions">0x97FAcbF880e47c27cafbA6bE3d677A50d536813e</a></code>
   </td>
   <td>ERC-20 Handler - account given permissions to mint/burn ERC-20 tokens on Palm network
   </td>
  </tr>
  <tr>
   <td>Palm
   </td>
   <td><code><a href="https://explorer.palm.io/address/0x317bc33A442AA0f6C8235cb2487f0Bb338eD27E4/transactions">0x317bc33A442AA0f6C8235cb2487f0Bb338eD27E4</a></code>
   </td>
   <td>ERC-721 Handler - account given permissions to mint/burn ERC-721 tokens on Palm network
   </td>
  </tr>
  <tr>
   <td>Palm
   </td>
   <td><code><a href="https://explorer.palm.io/address/0x22887Af68E57A76692f2686020FF563aC873eA24/transactions">0x22887Af68E57A76692f2686020FF563aC873eA24</a></code>
   </td>
   <td>Primary relayer - tx for assets minted/burned on Palm network can be found here
   </td>
  </tr>
  <tr>
   <td>Palm
   </td>
   <td><code><a href="https://explorer.palm.io/address/0x2A84F0c208872184c9dfcd57B6cd7bF63BcF829E/transactions">0x2A84F0c208872184c9dfcd57B6cd7bF63BcF829E</a></code>
   </td>
   <td>Secondary relayer - this account periodically sweeps for transactions that did not successfully complete by the primary relayer
   </td>
  </tr>
  <tr>
   <td>Palm
   </td>
   <td><code><a href="https://explorer.palm.io/address/0x8993D834b036913E25f35ed1Cbb288F26779e16a/transactions">0x8993D834b036913E25f35ed1Cbb288F26779e16a</a></code>
   </td>
   <td>Tollbooth account that collects fees in DAI from bridge users. Dai fees are regularly transferred to the Ethereum network using the bridge, swapped for Ether, and added to the Ethereum Relayer accounts. This way, it can pay for gas fees incurred by the bridge when it mints a token on behalf of the user. Carbon offset fees are also collected in this account and withdrawn for direct payment to carbon offset projects.
   </td>
  </tr>
  <tr>
   <td>Ethereum Mainnet
   </td>
   <td><code><a href="https://etherscan.io/address/0x7d0e63736aeb136acd44c70d6e1a0f27fb897679">0x7D0e63736aEb136aCd44C70D6e1A0f27fb897679</a></code>
   </td>
   <td>Bridge Contract - main bridge smart contract.
   </td>
  </tr>
  <tr>
   <td>Ethereum Mainnet
   </td>
   <td><code><a href="https://etherscan.io/address/0xf4684eb75659bec9c3c3b19f075a6fd5aba34b87">0xf4684EB75659Bec9C3c3b19f075a6fd5ABa34b87</a></code>
   </td>
   <td>ERC-20 Handler - account given permissions to  mint/burn ERC-20 tokens on Ethereum.
   </td>
  </tr>
  <tr>
   <td>Ethereum Mainnet
   </td>
   <td><code><a href="https://etherscan.io/address/0x4B4473093d98F0daA39e60406333B019c3A29D36">0x4B4473093d98F0daA39e60406333B019c3A29D36</a></code>
   </td>
   <td>ERC-721 Handler - account given permissions to mint/burn ERC-721 tokens on Ethereum.
   </td>
  </tr>
  <tr>
   <td>Ethereum Mainnet
   </td>
   <td><code><a href="https://etherscan.io/address/0x22887af68e57a76692f2686020ff563ac873ea24">0x22887Af68E57A76692f2686020FF563aC873eA24</a></code>
   </td>
   <td>Primary relayer - tx for assets minted/burned on Ethereum can be found here.
   </td>
  </tr>
  <tr>
   <td>Ethereum Mainnet
   </td>
   <td><code><a href="https://etherscan.io/address/0x2a84f0c208872184c9dfcd57b6cd7bf63bcf829e">0x2A84F0c208872184c9dfcd57B6cd7bF63BcF829E</a></code>
   </td>
   <td>Secondary relayer - this account periodically sweeps for transactions that did not successfully complete on the primary relayer.
   </td>
  </tr>
</table>



<table>
  <tr>
   <td>Palm testnet
   </td>
   <td><a href="https://explorer.palm-uat.xyz/address/0xdeD098F762456D4BEA387AcadcB1eAeA63E8e954/transactions">0xdeD098F762456D4BEA387AcadcB1eAeA63E8e954</a>
   </td>
   <td>bridgeAddress
   </td>
  </tr>
  <tr>
   <td>Palm testnet
   </td>
   <td><a href="https://explorer.palm-uat.xyz/address/0x0113A208409505470d3F9b4a7c43488e57564bD7/transactions">0x0113A208409505470d3F9b4a7c43488e57564bD7</a>
   </td>
   <td>erc20HandlerAddress
   </td>
  </tr>
  <tr>
   <td>Palm testnet
   </td>
   <td><a href="https://explorer.palm-uat.xyz/address/0x0114a4A5604f88076D6CDD5607115CE42812e404/transactions">0x0114a4A5604f88076D6CDD5607115CE42812e404</a>
   </td>
   <td>erc721HandlerAddress
   </td>
  </tr>
  <tr>
   <td>Palm testnet
   </td>
   <td><a href="https://explorer.palm-uat.xyz/address/0x5105F2e61A5139589c02e557bd4A61A5a22B2676/transactions">0x5105F2e61A5139589c02e557bd4A61A5a22B2676</a>
   </td>
   <td>erc1155HandlerAddress
   </td>
  </tr>
</table>



<table>
  <tr>
   <td>Rinkeby
   </td>
   <td><a href="https://rinkeby.etherscan.io/address/0x21bE213d63e9F5CE1F93D2758F132817A41874e1">0x21bE213d63e9F5CE1F93D2758F132817A41874e1</a>
   </td>
   <td>bridgeAddress
   </td>
  </tr>
  <tr>
   <td>Rinkeby
   </td>
   <td><a href="https://rinkeby.etherscan.io/address/0x515C78a5737093Dd8a2638800b93445C77e5bE1D">0x515C78a5737093Dd8a2638800b93445C77e5bE1D</a>
   </td>
   <td>erc20HandlerAddress
   </td>
  </tr>
  <tr>
   <td>Rinkeby
   </td>
   <td><a href="https://rinkeby.etherscan.io/address/0x3bd23d02a76804e839c4b73e978ce05a406e964b">0x3bD23d02a76804E839c4B73E978Ce05a406e964b</a>
   </td>
   <td>erc721HandlerAddress
   </td>
  </tr>
  <tr>
   <td>Rinkeby
   </td>
   <td><a href="https://rinkeby.etherscan.io/address/0x91429e6686348645cc9b17c382f991d900966d7d">0x91429E6686348645cc9B17C382F991d900966D7D</a>
   </td>
   <td>erc1155HandlerAddress
   </td>
  </tr>
</table>


TODO: relayer addresses


<table>
  <tr>
   <td>
   </td>
   <td>0xff93B45308FD417dF303D6515aB04D9e89a750Ca
   </td>
   <td>relayer
   </td>
  </tr>
  <tr>
   <td>
   </td>
   <td>0x8e0a907331554AF72563Bd8D43051C2E64Be5d35
   </td>
   <td>relayer
   </td>
  </tr>
  <tr>
   <td>
   </td>
   <td>0x24962717f8fA5BA3b931bACaF9ac03924EB475a0
   </td>
   <td>relayer
   </td>
  </tr>
  <tr>
   <td>
   </td>
   <td>0x148FfB2074A9e59eD58142822b3eB3fcBffb0cd7
   </td>
   <td>relayer
   </td>
  </tr>
  <tr>
   <td>
   </td>
   <td>0x4CEEf6139f00F9F4535Ad19640Ff7A0137708485
   </td>
   <td>relayer
   </td>
  </tr>
</table>



## Making your token contracts bridge compatible

TODO: explain how to make your contract bridge-compatible: how to [approve](https://github.com/ChainSafe/chainbridge-deploy/blob/a8ced79fec2b4b1bf9250ad392a6a50df9f80479/cb-sol-cli/docs/erc721.md#approve) the handler to cal lock/mint on your contract?

transferFrom() - The user approves the handler to move the tokens before initiating the transfer. The handler will call transferFrom() as part of the transfer initiation. For the inverse, the handler will call transfer() to release tokens from the handler’s ownership.

mint()/burn() - The user approves the handler to move the tokens before initiating the transfer. The handler will call burnFrom() as part of the transfer initiation. For the inverse, the handler will call mint() to release tokens to the recipient (and must have privileges to do so).

[https://github.com/Palm-Network/nft-contracts/blob/43a404fe35956fb009ad51e5017ad7f3b150261b/contracts/NFT.sol#L71](https://github.com/Palm-Network/nft-contracts/blob/43a404fe35956fb009ad51e5017ad7f3b150261b/contracts/NFT.sol#L71)

Bridge contracts require minting, burning, and transfer permission. They have to implement the Enumerable (Open Zeppelin).

Contact us on discord to validate your contracts compatibility, they will be tested by our team on the testnet and then set for production.
![alt_text](images/image3.png "image_tooltip")


We recommend the following for the synthetic contract:

* It should have a `mint` function, and the bridge will expect to be able to set the Token ID when minting.
* It should have a `burn` function that the bridge will call when sending tokens back to the original side.
* The bridge ERC-721 handler will need access to call the contract mint and burn functions. We recommend using [role-based access controls](https://docs.openzeppelin.com/contracts/3.x/access-control) to avoid granting admin functions to the bridge address.


# Registering your ERC-721 contracts with the Palm NFT Bridge

This page contains example steps as a guideline. Steps are valid for the UAT environment. Users need to update configuration parameters correctly when performing the steps in PRD.

References

[Deploying Live (EVM EVM)](https://chainbridge.chainsafe.io/live-evm-bridge/) - ChainBridge Docs

Notes
