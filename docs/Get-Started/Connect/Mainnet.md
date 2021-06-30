---
Description: Connect to Palm Mainnet.
---

# Connect to Palm Mainnet

Use [Infura](https://infura.io/) to connect to Palm Mainnet (production network).
Infura is a blockchain development suite that provides access to the Ethereum network over HTTPS and WebSockets.
Although Palm Mainnet is a separate network from Ethereum, your Infura account and project information is used to
authenticate you on Palm Mainnet.

To connect to Palm Mainnet:

1. Create an Infura account and set up an Ethereum project.

    !!! note

        You can follow [this step-by-step guide](https://blog.infura.io/getting-started-with-infura-28e41844cc89/) for
        creating an Infura account and your first Ethereum project.

2. Fill out [the Palm Infura registration form](https://docs.google.com/forms/d/e/1FAIpQLSetkTsotYiiGdMjNkJEUgUyRlWliIQ7O8YGHbrzJyfnCYnBfA/viewform),
   entering your Infura account information.
   If you've already filled out this form when connecting to [the Palm development network](Development.md) or
   [testnet](Testnet.md), you don't need to fill it out again.

3. Fill out [the Palm production airdrop form](https://forms.gle/So2HE8Yfhjyr5fEi6), entering the address of an Ethereum
   account you control to have some Mainnet PALM airdropped into that account.

    !!! note

        You can use [MetaMask](https://metamask.io/) to create an Ethereum account.

4. Locate your Ethereum project ID on Infura.
   Go to the project you created in Step 1, and click on the "Settings" tab.
   The project ID is located in the "Keys" box on this page.

5. Create your access URL using the following template (replacing `<YOUR-PROJECT-ID>` with the project ID):

    ```url
    https://palm-mainnet.infura.io/v3/<YOUR-PROJECT-ID>
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

Since this process isn't automated, you may check in with your Palm contact or fill out
[the Palm contact form](https://share.hsforms.com/1_sBreu7XTMWZtH9n1xTP3g2urwb) for help and to make sure you've
completed these steps successfully.
