---
description: integrating with the bridge series: Part 3
---

# Helpful Resources - Addresses of Bridge Components

The following table contains the contracts and addresses of the main bridge components.
If you would like to understand what these addresses are used for, you can read: [***What is the Palm network bridge?***](./what-is-the-bridge.md) and [***How to make your smart contracts bridge-compatible?***](./make-contracts-bridge-compatible.md)

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
   <td>Palm Mainnet
   </td>
   <td><code><a href="https://explorer.palm.io/address/0xB3C62Aed3be8e0577D4724C40a01379dbf895C01/transactions">0xB3C62Aed3be8e0577D4724C40a01379dbf895C01</a></code>
   </td>
   <td>Bridge Contract - main bridge smart contract
   </td>
  </tr>
  <tr>
   <td>Palm Mainnet
   </td>
   <td><code><a href="https://explorer.palm.io/address/0x97FAcbF880e47c27cafbA6bE3d677A50d536813e/transactions">0x97FAcbF880e47c27cafbA6bE3d677A50d536813e</a></code>
   </td>
   <td>ERC-20 Handler - account given permissions to mint/burn ERC-20 tokens on Palm Network
   </td>
  </tr>
  <tr>
   <td>Palm Mainnet
   </td>
   <td><code><a href="https://explorer.palm.io/address/0x317bc33A442AA0f6C8235cb2487f0Bb338eD27E4/transactions">0x317bc33A442AA0f6C8235cb2487f0Bb338eD27E4</a></code>
   </td>
   <td>ERC-721 Handler - account given permissions to mint/burn ERC-721 tokens on Palm Network
   </td>
  </tr>
  <tr>
   <td>Palm Mainnet
   </td>
   <td><code><a href="https://explorer.palm.io/address/0x22887Af68E57A76692f2686020FF563aC873eA24/transactions">0x22887Af68E57A76692f2686020FF563aC873eA24</a></code>
   </td>
   <td>Primary relayer - transactions for assets minted/burned on Palm Network can be found here
   </td>
  </tr>
  <tr>
   <td>Palm Mainnet
   </td>
   <td><code><a href="https://explorer.palm.io/address/0x2A84F0c208872184c9dfcd57B6cd7bF63BcF829E/transactions">0x2A84F0c208872184c9dfcd57B6cd7bF63BcF829E</a></code>
   </td>
   <td>Secondary relayer - this account periodically sweeps for transactions that did not successfully complete by the primary relayer
   </td>
  </tr>
  <tr>
   <td>Palm Mainnet
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
   <td>Primary relayer - transactions for assets minted/burned on Ethereum can be found here.
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
   <td>Bridge Address
   </td>
  </tr>
  <tr>
   <td>Palm testnet
   </td>
   <td><a href="https://explorer.palm-uat.xyz/address/0x0113A208409505470d3F9b4a7c43488e57564bD7/transactions">0x0113A208409505470d3F9b4a7c43488e57564bD7</a>
   </td>
   <td>ERC-20 Handler Address
   </td>
  </tr>
  <tr>
   <td>Palm testnet
   </td>
   <td><a href="https://explorer.palm-uat.xyz/address/0x0114a4A5604f88076D6CDD5607115CE42812e404/transactions">0x0114a4A5604f88076D6CDD5607115CE42812e404</a>
   </td>
   <td>ERC-721 Handler Address
   </td>
  </tr>
  <tr>
   <td>Palm testnet
   </td>
   <td><a href="https://explorer.palm-uat.xyz/address/0x5105F2e61A5139589c02e557bd4A61A5a22B2676/transactions">0x5105F2e61A5139589c02e557bd4A61A5a22B2676</a>
   </td>
   <td>ERC-1155 Handler Address
   </td>
  </tr>
</table>

<table>
  <tr>
   <td>Rinkeby
   </td>
   <td><a href="https://rinkeby.etherscan.io/address/0x21bE213d63e9F5CE1F93D2758F132817A41874e1">0x21bE213d63e9F5CE1F93D2758F132817A41874e1</a>
   </td>
   <td>Bridge Address
   </td>
  </tr>
  <tr>
   <td>Rinkeby
   </td>
   <td><a href="https://rinkeby.etherscan.io/address/0x515C78a5737093Dd8a2638800b93445C77e5bE1D">0x515C78a5737093Dd8a2638800b93445C77e5bE1D</a>
   </td>
   <td>ERC-20 Handler Address
   </td>
  </tr>
  <tr>
   <td>Rinkeby
   </td>
   <td><a href="https://rinkeby.etherscan.io/address/0x3bd23d02a76804e839c4b73e978ce05a406e964b">0x3bD23d02a76804E839c4B73E978Ce05a406e964b</a>
   </td>
   <td>ERC-721 Handler Address
   </td>
  </tr>
  <tr>
   <td>Rinkeby
   </td>
   <td><a href="https://rinkeby.etherscan.io/address/0x91429e6686348645cc9b17c382f991d900966d7d">0x91429E6686348645cc9B17C382F991d900966D7D</a>
   </td>
   <td>ERC-1155 Handler Address
   </td>
  </tr>
</table>


!!! question
    Any question? Drop them on our [Discord](https://discord.gg/grcpwNRxVj)
