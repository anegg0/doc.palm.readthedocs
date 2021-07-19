---
Description: How to deploy a smart contract to the Palm network.
---

# Deploy using Truffle

This guide walks you through using [Truffle migrations](https://www.trufflesuite.com/docs/truffle/getting-started/running-migrations)
to deploy a dapp to the Palm network.

## Prerequisites

- [Access to PALM](../Get-Started/Tokens.md) in order to pay the deployment transaction fees
- [Access to a Palm Infura endpoint](../Get-Started/Connect/Overview.md)
- [MetaMask](https://metamask.io/) wallet set up
- [Truffle](https://www.trufflesuite.com/docs/truffle/getting-started/installation) installed
- [Node.js](https://nodejs.org/en/download/) installed

## Steps

1. Initialize Truffle on your project folder:

    ```bash
    truffle init
    ```

2. Install Truffle's [`HDWalletProvider`](https://github.com/trufflesuite/truffle/tree/develop/packages/hdwallet-provider):

    ```bash
    npm install @truffle/hdwallet-provider
    ```

4. Set environment variables with [`dotenv`](https://www.npmjs.com/package/dotenv).

    Install `dotenv`:

    ```bash
    npm install --save dotenv
    ```

    Create a `.env` file in your project's root folder, and set environment variables in the file as follows:

    ```text
    // if deploying to Palm Testnet
    TESTNET_PRIVATE_KEY = // (string) private key of the account you intend to use on Palm Testnet (this can be accessed on MetaMask)
    TESTNET_PROJECT_ID = // (string) project ID corresponding to your Palm Testnet Infura project

    // if deploying to Palm Mainnet
    MAINNET_PRIVATE_KEY = // (string) private key of the account you intend to use on Palm Mainnet (this can be accessed on MetaMask)
    MAINNET_PROJECT_ID = // (string) project ID corresponding to your Palm Mainnet Infura project
    ```

    !!! warning "Security warning"

        **Keep your private keys secret.**

        Private keys must be kept secret and not committed to any code respositories.
        Improper handling of private keys can lead to loss of funds and identity fraud.

        For example, see [MyCrypto's Protecting Yourself and Your Funds guide](https://support.mycrypto.com/staying-safe/protecting-yourself-and-your-funds).

3. Edit `truffle-config.js` with the following text, filling in the variables `mnemonic` and `project_id`.

    ```js
    const HDWalletProvider = require("@truffle/hdwallet-provider");

    require('dotenv').config()  // store environment variables from '.env' to process.env

    module.exports = {
        networks: {
            palm_testnet: {
                provider: () => new HDWalletProvider({
                    providerOrUrl: "https://palm-testnet.infura.io/v3/" + process.env.TESTNET_PROJECT_ID,
                    privateKeys: [
                        process.env.TESTNET_PRIVATE_KEY
                    ]
                }),
                network_id: 11297108099, // chain ID
                gas: 3000000, // gas limit
                gasPrice: 10000000000 // gas price in gwei
            },
            palm_mainnet: {
                provider: () => new HDWalletProvider({
                    providerOrUrl: "https://palm-mainnet.infura.io/v3/" + process.env.MAINNET_PROJECT_ID,
                    privateKeys: [
                        process.env.MAINNET_PRIVATE_KEY
                    ]
                }),
                network_id: 11297108109,
                gas: 3000000,
                gasPrice: 10000000000
            }
        }
    }
    ```

4. Deploy to the target Palm network environment:

    === "Palm Testnet"

        ```bash
        truffle migrate --network palm_testnet
        ```

    === "Palm Mainnet"

        ```bash
        truffle migrate --network palm_mainnet
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
