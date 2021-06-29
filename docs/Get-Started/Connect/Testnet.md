---
Description: Connect to Palm Testnet.
---

# Connect to the Palm testnet

Use [Infura](https://infura.io/) to connect to the Palm testnet.
Infura is a blockchain development suite that provides access to the Ethereum network over HTTPS and WebSockets.
Although the Palm testnet is a separate network from Ethereum, your Infura account and project information is used to
authenticate you on the Palm testnet.

To connect to the Palm testnet:

1. Create an Infura account and set up an Ethereum project.

    !!! note

        You can follow [this step-by-step guide](https://blog.infura.io/getting-started-with-infura-28e41844cc89/) for
        creating an Infura account and your first Ethereum project.


2. Fill out [the Palm Infura registration form](https://docs.google.com/forms/d/e/1FAIpQLSetkTsotYiiGdMjNkJEUgUyRlWliIQ7O8YGHbrzJyfnCYnBfA/viewform),
   entering your Infura account information.
   Include the address of an Ethereum account you control to have some testnet PALM airdropped into that account.

    !!! note

        You can use [MetaMask](https://metamask.io/) to create an Ethereum account.
        The address of an Ethereum account is the same on Ethereum Mainnet and any Ethereum testnet.

3. Tell your Palm contact that you've filled out the form, and they'll ensure that your information is received and
   your access to the Palm testnet is approved.

4. Locate your Ethereum project ID on Infura.
   Go to the project you created in Step 1, and click on the "Settings" tab.
   The project ID is located in the "Keys" box on this page.

5. Create your access URL using the following template (replacing `<YOUR-PROJECT-ID>` with the project ID):

    ```url
    https://palm-testnet.infura.io/v3/<YOUR PROJECT ID>
    ```

6. Confirm that the access works using the following command (replacing `<ACCESS-URL>` with the access URL):

    === "curl HTTP request"

        ```bash
        curl <ACCESS-URL> -X POST -H "Content-Type: application/json" -d '{"jsonrpc":"2.0","method":"eth_accounts","params":[],"id":1}'
        ```

    === "JSON result"

        ```json
        {"jsonrpc":"2.0","id":1,"result":[]}
        ```

Since this process isn't automated, you may check in with your Palm or ConsenSys contact for help and to make sure you've
completed these steps successfully.
