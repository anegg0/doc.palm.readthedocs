---
Description: How to deploy a smart contract to the Palm network using Truffle.
---

# Deploy using Truffle

This guide walks you through using [Truffle migrations](https://www.trufflesuite.com/docs/truffle/getting-started/running-migrations)
to deploy an NFT smart contract to the Palm network.

## Prerequisites

- [Access to PALM](../Get-Started/Tokens.md) in order to pay the deployment transaction fees
- [Access to a Palm Infura endpoint](../Get-Started/Connect/Overview.md)
- [MetaMask](https://metamask.io/) wallet set up
- [Node.js](https://nodejs.org/en/download/) installed

## Steps

1. Initialize your npm project:

    ```bash
    npm init
    ```

2. Install Truffle on your project folder:

    ```bash
    npm install --save truffle
    ```

3. Initialize Truffle on your project folder:

    ```bash
    npx truffle init
    ```

4. Install Truffle's [`HDWalletProvider`](https://github.com/trufflesuite/truffle/tree/develop/packages/hdwallet-provider):

    ```bash
    npm install --save @truffle/hdwallet-provider
    ```

5. Write your contract:

    We'll use a contract based on the OpenZeppelin library’s [ERC-721 implementation](https://docs.openzeppelin.com/contracts/4.x/erc721).

    First, install the Open Zeppelin library in order to inherit its classes:

    ```bash
    npm install --save @openzeppelin/contracts
    ```

    Next, add the following smart contract to the "contracts" folder and name it "NFT.sol":

    ```js
    //Contract based on [https://docs.openzeppelin.com/contracts/4.x/erc721](https://docs.openzeppelin.com/contracts/4.x/erc721)
    // SPDX-License-Identifier: MIT
    pragma solidity ^0.8.0;

    import "@openzeppelin/contracts/token/ERC721/ERC721.sol";
    import "@openzeppelin/contracts/token/ERC721/extensions/ERC721URIStorage.sol";
    import "@openzeppelin/contracts/utils/Counters.sol";
    import "@openzeppelin/contracts/access/Ownable.sol";

    contract NFT is ERC721URIStorage, Ownable {
        using Counters for Counters.Counter;
        Counters.Counter private _tokenIds;

        constructor() ERC721("NFT", "NFT") {}

        function mintNFT(address recipient, string memory tokenURI)
            public
            returns (uint256)
        {
            _tokenIds.increment();

            uint256 newItemId = _tokenIds.current();
            _mint(recipient, newItemId);
            _setTokenURI(newItemId, tokenURI);

            return newItemId;
        }
    }
    ```

6. Set environment variables with [`dotenv`](https://www.npmjs.com/package/dotenv).

    Install `dotenv`:

    ```bash
    npm install --save dotenv
    ```

    Create a `.env` file in your project's root folder, and set environment variables in the file as follows:

    ```text
    PRIVATE_KEY = // The private key of the account you intend to use on Palm
    INFURA_PROJECT_ID = // Your infura project id
    ```

    !!! note
        [Follow the link to see how your private key can be accessed on MetaMask](https://metamask.zendesk.com/hc/en-us/articles/360015289632-How-to-Export-an-Account-Private-Key)

    !!! critical "Security warning"

        **Keep your private keys secret.**

        Private keys must be kept secret and not committed to any code repository. In the example of this tutorial, the `.env` file should be added to your `.gitignore` file and kept local.

        For example, see [MyCrypto's Protecting Yourself and Your Funds guide](https://support.mycrypto.com/staying-safe/protecting-yourself-and-your-funds).

7. Edit `truffle-config.js` with the following text:

    ```js
    const HDWalletProvider = require("@truffle/hdwallet-provider");

    require('dotenv').config()  // store environment variables from '.env' to process.env

    module.exports = {
      compilers: {
        solc: {
          version: "^0.8.0"
        }
      },
      networks: {
        palm_testnet: {
          provider: () => new HDWalletProvider({
            providerOrUrl: `https://palm-testnet.infura.io/v3/${process.env.INFURA_PROJECT_ID}`,
            privateKeys: [
              process.env.PRIVATE_KEY
            ]
          }),
          network_id: 11297108099, // chain ID
          gas: 3000000, // gas limit
          gasPrice: 10000000000 // gas price in gwei
        },
        palm_mainnet: {
          provider: () => new HDWalletProvider({
            providerOrUrl: `https://palm-mainnet.infura.io/v3/${process.env.INFURA_PROJECT_ID}`,
            privateKeys: [
              process.env.PRIVATE_KEY
            ]
          }),
          network_id: 11297108109,
          gas: 3000000,
          gasPrice: 10000000000
        }
      }
    }
    ```

8. Deploy to the target Palm network environment:

    === "Palm Testnet"

        ```bash
        npx truffle migrate --network palm_testnet
        ```

    === "Palm Mainnet"

        ```bash
        npx truffle migrate --network palm_mainnet
        ```

    === "Example logs"

        ```bash
        Compiling your contracts...
        ===========================
        > Everything is up to date, there is nothing to compile.

        Starting migrations...
        ======================
        > Network name:    'palm_testnet'
        > Network id:      11297108099
        > Block gas limit: 18800000 (0x11edd80)

        1_initial_migration.js
        ======================

          Deploying 'Migrations'
          ----------------------
          ...
          0xa5f964329db84e4afed621a520c37c1aa376596912e509722eedba49a2378cc2

          Migrations: 0x588a268a8db4784679c93b99bf6f3ab1c6a4831c

          Saving successful migration to network
          --------------------------------------
          ...
          0x6291aff7d5e14f24e8ee1047f10c84dc2ccd8eb17e8c60a52f2239164a8506c2

          Saving artifacts
          ----------------
          ...
        ```

9. Look up your deployment on Palm explorer

    Go to https://explorer.palm-uat.xyz for testnet deployments and to https://explorer.palm.io for mainnet deployments and paste the address of your contract into the search bar.

!!! question
    Any question? Drop them on our [Discord](https://discord.gg/grcpwNRxVj)
