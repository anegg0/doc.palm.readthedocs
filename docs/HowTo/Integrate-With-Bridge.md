---
description: integrating with the palm network bridge.
---

# Integrating with the Palm network bridge

The Palm Network enables NFT trading in a fast, cost-efficient, and eco-friendly manner. However, some users might decide to move their assets from the Palm Network to Ethereum in order to reach specific marketplaces. They can transfer their token(s) using the Palm network bridge.

As a developer you can support those users by ensuring your smart contracts are compatible with the Palm network bridge.

This article explains:

1. What is the Palm network Bridge.
2. How to integrate with the bridge.

## What is the Palm network Bridge?

The Palm network bridge connects the Palm network with Ethereum. It allows transferring assets such as ERC-20 and ERC-721 tokens back and forth between the Palm Network and Ethereum.

The bridge works by locking tokens that have already been minted on one side of the bridge and then minting an equivalent token on the other side, using what we call a “synthetic” version of the ERC-721 contract:

![](/../../Images/token-bridge-flow.png)

Users can send their tokens back to the original side: the bridge will burn the synthetic token and release the original token that will be transferred to the destination wallet address:

![](/../../Images/token-bridge-reverse-flow.png)

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

## How to ensure your token contract works with the bridge?

  All you need to do to integrate with the bridge is to prepare your token contracts so that the Bridge supports them.

### Making your token contracts bridge-compatible

#### Original contracts vs Synthetic contracts

In the context of the Palm network's bridge, an original contract sits where tokens are primarily minted.
A synthetic contract is deployed on the chain where tokens will be transferred via the bridge.

Deploying both original and synthetic contracts ensures that tokens can be transferred back and forth between the original and destination chains.

Here are the changes you will need to make to your contracts for them to be bridge-compatible:
The below specifications apply to ERC-721 contracts. Further specifications will be provided for ERC-1155 or ERC-20 contracts in future.

##### Original contract

1. Needs to include the [Enumerable extension from Open Zeppelin libraries](https://docs.openzeppelin.com/contracts/3.x/api/token/erc721#IERC721Enumerable).

Aside from `Enumerable`, any custom implementation of ERC-721 is allowed: bulk minting, token ID auto-increment, etc...

##### Synthetic contract

1. Needs to add the [Enumerable extension from Open Zeppelin libraries](https://docs.openzeppelin.com/contracts/3.x/api/token/erc721#IERC721Enumerable).

2. Needs to have a custom `mint()` function.

    In order to mint a replica of the original token on the targeted chain, the synthetic contract must be able to mint tokens that have the same `IDs` and `URIs` as the original.

    ???+ note "Custom `mint()` function example:"

        ``` js linenums="1"
        /**
          * @dev Mints the specified token id to the recipient addresses
          * @dev The unused string parameter exists to support the API used by ChainBridge.
          * @dev Mint interface: function mint(address to, uint256 tokenId, string calldata _data) public where _data is the tokenUri
          * @param tokenId tokenId to be minted
          * @param recipient Address that will receive the tokens
          */
        function mint(address recipient, uint256 tokenId, string calldata tokenUri) external onlyMinters {
            _mint(recipient, tokenId);
            _setTokenURI(tokenId, tokenUri);
        }
        ```

3. Needs to grant the bridge minting permission.

    The bridge's handler will need access to the synthetic contract's `mint()` function.

    We recommend using [role-based access controls](https://docs.openzeppelin.com/contracts/3.x/access-control) to avoid granting admin functions to the bridge address.

    You can set granular rights that only set controls on the `mint()`function.

    ???+ note "Granular `mint()` rights example:"

        ``` js linenums="1"
        /**
          * @dev Throws if called by any account other than minters. Implemented using the underlying AccessControl methods.
          */
        modifier onlyMinters() {
            require(hasRole(MINTER_ROLE, _msgSender()), "Caller does not have the MINTER_ROLE");
            _;
        }
        ```

4. Needs to have a `burn()` function.

    Same as for the `mint()` function described above, the bridge will need to be granted permission to the `burn()` function in order to burn synthetic tokens when transferring them back to the original chain.

5. Needs to give the bridge burning permission.

    Same as point 5. but for the `burn()` function.


If you would like to put all those bits into context, here's a contract example that applies for both _original_ and synthetic contracts:

???+ note "ERC-721 contract example:"

    ``` js linenums="1"

    // SPDX-License-Identifier: MIT
    pragma solidity 0.8.6;

    import "@openzeppelin/contracts/access/AccessControl.sol";
    import "@openzeppelin/contracts/token/ERC721/extensions/ERC721Enumerable.sol";
    import "@openzeppelin/contracts/token/ERC721/extensions/ERC721Burnable.sol";
    import "@openzeppelin/contracts/token/ERC721/extensions/ERC721Pausable.sol";

    import "./ERC2981.sol";

    contract NFT is AccessControl, ERC2981, ERC721Enumerable, ERC721Burnable, ERC721Pausable {
        event RoyaltyWalletChanged(address indexed previousWallet, address indexed newWallet);
        event RoyaltyFeeChanged(uint256 previousFee, uint256 newFee);
        event BaseURIChanged(string previousURI, string newURI);

        bytes32 public constant OWNER_ROLE = keccak256("OWNER_ROLE");
        bytes32 public constant MINTER_ROLE = keccak256("MINTER_ROLE");

        uint256 public constant ROYALTY_FEE_DENOMINATOR = 100000;
        uint256 public royaltyFee;
        address public royaltyWallet;

        string private _baseTokenURI;

        /**
        * @param _name ERC721 token name
        * @param _symbol ERC721 token symbol
        * @param _uri Base token uri
        * @param _royaltyWallet Wallet where royalties should be sent
        * @param _royaltyFee Fee numerator to be used for fees
        */
        constructor(
            string memory _name,
            string memory _symbol,
            string memory _uri,
            address _royaltyWallet,
            uint256 _royaltyFee
        ) ERC721(_name, _symbol) {
            _setBaseTokenURI(_uri);
            _setRoyaltyWallet(_royaltyWallet);
            _setRoyaltyFee(_royaltyFee);
            _setupRole(DEFAULT_ADMIN_ROLE, msg.sender);
            _setupRole(OWNER_ROLE, msg.sender);
            _setupRole(MINTER_ROLE, msg.sender);
        }

        /**
        * @dev Throws if called by any account other than owners. Implemented using the underlying AccessControl methods.
        */
        modifier onlyOwners() {
            require(hasRole(OWNER_ROLE, _msgSender()), "Caller does not have the OWNER_ROLE");
            _;
        }

        /**
        * @dev Throws if called by any account other than minters. Implemented using the underlying AccessControl methods.
        */
        modifier onlyMinters() {
            require(hasRole(MINTER_ROLE, _msgSender()), "Caller does not have the MINTER_ROLE");
            _;
        }

        /**
        * @dev Mints the specified token ids to the recipient addresses
        * @param recipient Address that will receive the tokens
        * @param tokenIds Array of tokenIds to be minted
        */
        function mint(address recipient, uint256[] calldata tokenIds) external onlyMinters {
            for (uint256 i = 0; i < tokenIds.length; i++) {
                _mint(recipient, tokenIds[i]);
            }
        }

        /**
        * @dev Mints the specified token id to the recipient addresses
        * @dev The unused string parameter exists to support the API used by ChainBridge.
        * @param recipient Address that will receive the tokens
        * @param tokenId tokenId to be minted
        */
        function mint(address recipient, uint256 tokenId, string calldata) external onlyMinters {
            _mint(recipient, tokenId);
        }

        /**
        * @dev Pauses token transfers
        */
        function pause() external onlyOwners {
            _pause();
        }

        /**
        * @dev Unpauses token transfers
        */
        function unpause() external onlyOwners {
            _unpause();
        }

        /**
        * @dev Sets the base token URI
        * @param uri Base token URI
        */
        function setBaseTokenURI(string calldata uri) external onlyOwners {
            _setBaseTokenURI(uri);
        }

        /**
        * @dev Sets the wallet to which royalties should be sent
        * @param _royaltyWallet Address that should receive the royalties
        */
        function setRoyaltyWallet(address _royaltyWallet) external onlyOwners {
            _setRoyaltyWallet(_royaltyWallet);
        }

        /**
        * @dev Sets the fee percentage for royalties
        * @param _royaltyFee Basis points to compute royalty percentage
        */
        function setRoyaltyFee(uint256 _royaltyFee) external onlyOwners {
            _setRoyaltyFee(_royaltyFee);
        }

        /**
        * @dev Function defined by ERC2981, which provides information about fees.
        * @param value Price being paid for the token (in base units)
        */
        function royaltyInfo(
            uint256, // tokenId is not used in this case as all tokens take the same fee
            uint256 value
        )
            external
            view
            override
            returns (
                address, // receiver
                uint256 // royaltyAmount
            )
        {
            return (royaltyWallet, (value * royaltyFee) / ROYALTY_FEE_DENOMINATOR);
        }

        /**
        * @dev For each existing tokenId, it returns the URI where metadata is stored
        * @param tokenId Token id
        */
        function tokenURI(uint256 tokenId) public view override returns (string memory) {
            string memory uri = super.tokenURI(tokenId);
            return bytes(uri).length > 0 ? string(abi.encodePacked(uri, ".json")) : "";
        }

        function supportsInterface(bytes4 interfaceId)
            public
            view
            override(AccessControl, ERC2981, ERC721, ERC721Enumerable)
            returns (bool)
        {
            return super.supportsInterface(interfaceId);
        }

        function _beforeTokenTransfer(
            address from,
            address to,
            uint256 tokenId
        ) internal override(ERC721, ERC721Enumerable, ERC721Pausable) {
            super._beforeTokenTransfer(from, to, tokenId);
        }

        function _setBaseTokenURI(string memory newURI) internal {
            emit BaseURIChanged(_baseTokenURI, newURI);
            _baseTokenURI = newURI;
        }

        function _setRoyaltyWallet(address _royaltyWallet) internal {
            require(_royaltyWallet != address(0), "INVALID_WALLET");
            emit RoyaltyWalletChanged(royaltyWallet, _royaltyWallet);
            royaltyWallet = _royaltyWallet;
        }

        function _setRoyaltyFee(uint256 _royaltyFee) internal {
            require(_royaltyFee <= ROYALTY_FEE_DENOMINATOR, "INVALID_FEE");
            emit RoyaltyFeeChanged(royaltyFee, _royaltyFee);
            royaltyFee = _royaltyFee;
        }

        function _baseURI() internal view override returns (string memory) {
            return _baseTokenURI;
        }
    }
    ```
[Code GitHub repository](https://github.com/ConsenSys-Palm/palm-drop-contracts/blob/master/contracts/NFT.sol)


### Testing your contracts

You can use the Palm Testnet bridge to live-test your contracts' integration with the bridge.
All you need is to deploy them to Palm testnet and Rinkeby (Rinkeby is one of Ethereum's testnets). For ERC-721 contracts, you will need to initialise it using the constructor, and grant the bridge's ERC-721 handler contract `MINTER_ROLE` (see table below for relevant contract addresses).
Your _original_ and _synthetic_ can be exactly the same as in production.

Read more about how to use the Palm Testnet bridge [here](../HowTo/Bridge.md).

Once you have prepared your contracts for the bridge, feel free to [contact us on discord](https://discord.gg/grcpwNRxVj) to validate your contracts compatibility, they will be tested by our team on the testnet and then set for production.

### Helpful Resources - Address of Bridge Components

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


<table>
  <tr>
   <td>
   </td>
   <td>0xff93B45308FD417dF303D6515aB04D9e89a750Ca
   </td>
   <td>Relayer
   </td>
  </tr>
  <tr>
   <td>
   </td>
   <td>0x8e0a907331554AF72563Bd8D43051C2E64Be5d35
   </td>
   <td>Relayer
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
   <td>Relayer
   </td>
  </tr>
  <tr>
   <td>
   </td>
   <td>0x4CEEf6139f00F9F4535Ad19640Ff7A0137708485
   </td>
   <td>Relayer
   </td>
  </tr>
</table>

!!! question
    Any question? Drop them on our [Discord](https://discord.gg/grcpwNRxVj)
