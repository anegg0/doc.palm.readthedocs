---
Description: Connect to the Palm Development network.
---

# Connect to the Palm development network

Use [Infura](https://infura.io/) to connect to the Palm development network.

1. Create an Infura account and set up an Ethereum project.
   Although the Palm development network is a separate network from Ethereum, we’ll use your Infura account and your
   Ethereum project ID to authenticate you on the network.

   !!! note

       You can follow [this step-by-step guide](https://blog.infura.io/getting-started-with-infura-28e41844cc89/) for
       creating an account and your first Ethereum project.


1. Fill out [this form](https://docs.google.com/forms/d/e/1FAIpQLSetkTsotYiiGdMjNkJEUgUyRlWliIQ7O8YGHbrzJyfnCYnBfA/viewform)
   and enter your Infura account information (and the address of a smart contract you control).
   From here you’ll need to wait to make sure your access is approved.
   Please let your Palm contact know that you've done this, and we'll ensure that access is approved.
   Include an Ethereum address when you fill out the form to have some dev net $PALM airdropped into that account.

1. Locate your Ethereum project ID on Infura.
   To do this, you'll need to go the project you created in Step 1, and navigate to the Settings page.
   You'll see a box that looks like the following (of course, we've crossed out sensitive info on our account, but you get
   the gist!):

1. Create your access URL.
   You can do this by the following template: `https://palm-devnet.infura.io/v3/<YOUR PROJECT ID>`

   You can confirm that the access works using the following command (replacing `<YOUR-PROJECT-ID>` with the project ID):

   ```bash
   curl https://<network>.infura.io/v3/<YOUR-PROJECT-ID> -X POST -H "Content-Type: application/json" -d '{"jsonrpc":"2.0","method":"eth_accounts","params":[],"id":1}'
   ```

1. That's it! You've got access!
   Since this process isn’t automated, you'll need to check in to make sure the steps above were completed successfully.
   Please feel free to reach out to anyone at ConsenSys for help.
