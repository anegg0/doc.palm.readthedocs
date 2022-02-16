---
description: how to use the Covalent api
---

# Covalent API

[Covalent](https://www.covalenthq.com/) provides an API that allows you to pull detailed, granular, and historical
blockchain data such as:

- Token balances associated with a given address.
- History of transfers associated with a particular NFT.
- Metadata associated with a given NFT contract.

## Using the API

[The Covalent API documentation for Palm](https://www.covalenthq.com/docs/networks/palm) provides information on available
API endpoints and lets users make requests directly from the browser.

You must [sign up for an API key](https://www.covalenthq.com/platform/#/auth/register/) to use the Covalent API.
The key must be passed as a query parameter with all requests using `key=API_KEY`.

To run queries against the Palm network, update the `chain_id` query parameter to point to the targeted Palm network environment.
For example, to fetch all token balances (including NFTs) of a Palm address, make requests to the `balances_v2` endpoint
with `chain_id` as `11297108109` for Palm Mainnet and `11297108099` for Palm Testnet:

```text
https://api.covalenthq.com/v1/{chain_id}/address/{address}/balances_v2/
```
