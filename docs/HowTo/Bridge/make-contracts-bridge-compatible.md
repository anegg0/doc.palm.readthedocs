---
description: ensuring your token contract works with the bridge
---

## Make your contracts bridge-compatible

In our previous post we went through how the bridge operates, this article will explain what you need to do to integrate your token contracts with the bridge.

Let's go through these simple steps together:

### Making your token contracts bridge-compatible

#### Original contracts vs Synthetic contracts

In the context of the Palm network's bridge, an original contract sits where tokens are primarily minted.
A synthetic contract is deployed on the chain where tokens will be transferred via the bridge.

Deploying both original and synthetic contracts ensures that tokens can be transferred back and forth between the original and destination chains.

Here are the changes you will need to make to your contracts for them to be bridge-compatible:
The below specifications apply to ERC-721 contracts. Further specifications will be provided for ERC-1155 or ERC-20 contracts in future.

##### Original contract

1. Needs to include the [Enumerable extension from Open Zeppelin libraries](https://docs.openzeppelin.com/contracts/3.x/api/token/erc721#ERC721Enumerable). This is to support enumerability of all the token ids in the contract as well as all token ids owned by each account, so that the bridge UI can help the user to select the correct token from their account.

Aside from `Enumerable`, any custom implementation of ERC-721 is allowed: bulk minting, token ID auto-increment, etc...

##### Synthetic contract

1. Also needs to add the [Enumerable extension from Open Zeppelin libraries](https://docs.openzeppelin.com/contracts/3.x/api/token/erc721#ERC721Enumerable).

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

    We recommend using [role-based access controls](https://docs.openzeppelin.com/contracts/3.x/access-control) to do this, and it also helps to avoid granting full admin functions to the bridge address.

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

You can find a useful list of all the bridge components [in this post](./bridge-components-addresses.md)

!!! question
    Any question? Drop them on our [Discord](https://discord.gg/grcpwNRxVj)
