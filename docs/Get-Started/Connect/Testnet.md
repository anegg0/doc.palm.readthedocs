---
Description: Connect to Palm Testnet.
---

# Connect to the Palm testnet

To connect to the Palm testnet:

1. Create an [Infura](https://infura.io/) account and set up an Ethereum project.

    !!! note

        You can follow [this step-by-step guide](https://blog.infura.io/getting-started-with-infura-28e41844cc89/) for
        creating an Infura account and your first Ethereum project.

2. Fill out [the Palm Infura registration form](https://docs.google.com/forms/d/e/1FAIpQLSetkTsotYiiGdMjNkJEUgUyRlWliIQ7O8YGHbrzJyfnCYnBfA/viewform),
   entering your Infura account information.
   Include the address of an Ethereum account you control to have some testnet PALM airdropped into that account.

    !!! note

        You can use [MetaMask](https://metamask.io/) to create an Ethereum account.

3. Locate your Ethereum project ID on Infura.
   Go to the project you created in Step 1, and click on the "Settings" tab.
   The project ID is located in the "Keys" box on this page.

4. Create your access URL using the following template (replacing `<YOUR-PROJECT-ID>` with the project ID):

    ```url
    https://palm-testnet.infura.io/v3/<YOUR-PROJECT-ID>
    ```

5. Confirm that the access works using the following command (replacing `<ACCESS-URL>` with the access URL):

    === "curl HTTP request"

        ```bash
        curl <ACCESS-URL> -X POST -H "Content-Type: application/json" -d '{"jsonrpc":"2.0","method":"eth_accounts","params":[],"id":1}'
        ```

    === "JSON result"

        ```json
        {"jsonrpc":"2.0","id":1,"result":[]}
        ```

    You can view the [Infura JSON-RPC API](https://infura.io/docs/ethereum#tag/JSON-RPC-Methods) and
    [how to make requests](https://infura.io/docs/ethereum#section/Make-Requests) in the
    [Infura documentation](https://infura.io/docs/ethereum).

Since this process isn't automated, you may check in with your Palm contact or fill out
[the Palm contact form](https://share.hsforms.com/1_sBreu7XTMWZtH9n1xTP3g2urwb) for help and to make sure you've
completed these steps successfully.
After connecting to and interacting with the Palm testnet, you can view [the Palm testnet block explorer](https://explorer.palm-uat.xyz/).
