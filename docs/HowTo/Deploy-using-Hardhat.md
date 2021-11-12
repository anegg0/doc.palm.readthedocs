---
Description: How to deploy a smart contract to the Palm network.
---

# Deploy using Hardhat

This guide walks you through using [Hardhat](https://hardhat.org/)
to deploy a dapp to the Palm network.

## Prerequisites

- [Access to PALM](../Get-Started/Tokens.md) in order to pay the deployment transaction fees
- [Access to a Palm Infura endpoint](../Get-Started/Connect/Overview.md)
- [MetaMask](https://metamask.io/) wallet set up
- [Node.js](https://nodejs.org/en/download/) installed
- [Hardhat](https://hardhat.org/getting-started/#installation) installed

## Steps

1. Initialize your npm project

    ```bash
    npm init
    ```

2. Install Hardhat

    ```bash
    npm install --save-dev hardhat
    ```

3. Create a HardHat Project

    Inside your npm project run:

    ```bash
    npx hardhat
    ```

    Select “create an empty hardhat.config.js”.

4. Add project folders

    In the root directory of your project:

    ```bash
    mkdir contracts $$ mkdir scripts
    ```

5. Write your contract

    We'll use a contract based on the OpenZeppelin library’s [ERC-721 implementation](https://docs.openzeppelin.com/contracts/4.x/erc721).

    First, install the Open Zeppelin library in order to inherit its classes:

    ```bash
    npm install @openzeppelin/contracts
    ```

    Next, add the following smart contract to the "contracts" folder and name it "MyNFT.sol":

    ```js
    //Contract based on [https://docs.openzeppelin.com/contracts/4.x/erc721](https://docs.openzeppelin.com/contracts/4.x/erc721)
    // SPDX-License-Identifier: MIT
    pragma solidity ^0.8.0;

    import "@openzeppelin/contracts/token/ERC721/ERC721.sol";
    import "@openzeppelin/contracts/token/ERC721/extensions/ERC721URIStorage.sol";
    import "@openzeppelin/contracts/utils/Counters.sol";
    import "@openzeppelin/contracts/access/Ownable.sol";

    contract MyNFT is ERC721URIStorage, Ownable {
        using Counters for Counters.Counter;
        Counters.Counter private _tokenIds;

        constructor() ERC721("MyNFT", "NFT") {}

        function awardItem(address player, string memory tokenURI)
            public
            returns (uint256)
        {
            _tokenIds.increment();

            uint256 newItemId = _tokenIds.current();
            _mint(player, newItemId);
            _setTokenURI(newItemId, tokenURI);

            return newItemId;
        }
    }
    ```

6. Install an Ethereum library

    In this example, we will use [Ethers.js](https://docs.ethers.io/)

    ```bash
    npm install --save-dev @nomiclabs/hardhat-ethers ethers@^5.0.0
    ```

7. Set environment variables with [`dotenv`](https://www.npmjs.com/package/dotenv)

    Install `dotenv`:

    ```bash
    npm install --save dotenv
    ```

    Create a `.env` file in your project's root folder, and set environment variables in the file as follows:

    ```text
    // if deploying to Palm Testnet
    TESTNET_PRIVATE_KEY = // (string) private key of the account you intend to use on Palm Testnet
    TESTNET_URL = // (string) https://palm-testnet.infura.io/v3/<YOUR-PROJECT-ID>

    // if deploying to Palm Mainnet
    MAINNET_PRIVATE_KEY = // (string) private key of the account you intend to use on Palm Mainnet
    MAINNNET_URL = // (string) https://palm-mainnet.infura.io/v3/<YOUR-PROJECT-ID>
    ```
    !!! note
    [follow the link to see how your private key can be accessed on MetaMask](https://metamask.zendesk.com/hc/en-us/articles/360015289632-How-to-Export-an-Account-Private-Key)

    !!! critical "Security warning"

        **Keep your private keys secret.**

        Private keys must be kept secret and not committed to any code respository. In the example of this tutorial, the `.env` file should be added to your `.gitignore` file and kept local. 

        For example, see [MyCrypto's Protecting Yourself and Your Funds guide](https://support.mycrypto.com/staying-safe/protecting-yourself-and-your-funds).

8. Edit `hardhat.config.js` with the following text:

    ```js
    /**
    * @type import('hardhat/config').HardhatUserConfig
    */
    require('dotenv').config();
    require("@nomiclabs/hardhat-ethers");
    const { TESTNET_URL, TESTNET_PRIVATE_KEY, MAINNNET_URL, MAINNET_PRIVATE_KEY } = process.env;
    module.exports = {
        solidity: "0.8.0",
        networks: {
            hardhat: {},
            palm_testnet: {
                url: TESTNET_URL,
                accounts: [`0x${TESTNET_PRIVATE_KEY}`]
            },
            palm_mainnet: {
                url: MAINNNET_URL,
                accounts: [`0x${MAINNET_PRIVATE_KEY}`]
            }
        },
    }
    ```

9. Compile your contract

    To make sure everything is working so far, let’s compile our contract:

    ```bash
    npx hardhat compile
    ```

10. Write a deploy script

    Navigate to the scripts/ folder and create a new file called deploy.js, adding the following contents to it:

    ```js
    async function main() {
    const MyNFT = await ethers.getContractFactory("MyNFT")

    // Start deployment, returning a promise that resolves to a contract object
    const myNFT = await MyNFT.deploy()
    console.log("Contract deployed to address:", myNFT.address)
    }

    main()
    .then(() => process.exit(0))
    .catch((error) => {
        console.error(error)
        process.exit(1)
    })
    ```

11. Deploy to the target Palm network environment

    === "Palm Testnet"

        ```bash
        npx hardhat run scripts/deploy.js --network palm_testnet
        ```

    === "Palm Mainnet"

        ```bash
        npx hardhat run scripts/deploy.js --network palm_mainnet
        ```

    === "Example log"

        ```bash
        Contract deployed to address: 0xeC1AFf99d2C331B226A5731B9555Af924932d629
        ```

12. Look up your deployment on Palm explorer

    Go to https://explorer.palm-uat.xyz and paste the address of your contract into the search bar.
    
    
