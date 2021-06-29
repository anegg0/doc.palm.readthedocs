---
Description: Connect to the Palm Development network.
---

# Connect to the Palm development network

Use [Infura](https://infura.io/) to connect to the Palm development network.

1. Create an Infura account and set up an Ethereum project.
   Although the Palm development network is a separate network from Ethereum, your Infura account and Ethereum project
   ID are used to authenticate you on the Palm network.

   !!! note

       You can follow [this step-by-step guide](https://blog.infura.io/getting-started-with-infura-28e41844cc89/) for
       creating an Infura account and your first Ethereum project.


1. Fill out [this Palm Infura registration form](https://docs.google.com/forms/d/e/1FAIpQLSetkTsotYiiGdMjNkJEUgUyRlWliIQ7O8YGHbrzJyfnCYnBfA/viewform),
   entering your Infura account information.
   Include the address of an Ethereum account you control to have some development network PALM airdropped into that account.

   !!! note

       You can use [MetaMask](https://metamask.io/) to create an Ethereum account.
       The address of an Ethereum account is the same on Mainnet and any testnet.

   Let your Palm contact know that you've filled out the form, and they'll ensure that your information is received and
   your access to the Palm development network is approved.

1. Locate your Ethereum project ID on Infura.
   Go to the project you created in Step 1, and click on the "Settings" tab.
   The project ID is located in the "Keys" box on this page.

1. Create your access URL using the following template (replacing `<YOUR-PROJECT-ID>` with the project ID):

   ```https://palm-devnet.infura.io/v3/<YOUR PROJECT ID>```

   You can confirm that the access works using the following command:

   ```bash
   curl https://palm-devnet.infura.io/v3/<YOUR-PROJECT-ID> -X POST -H "Content-Type: application/json" -d '{"jsonrpc":"2.0","method":"eth_accounts","params":[],"id":1}'
   ```
