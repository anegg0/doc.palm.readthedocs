---
description: how to use the Covalent API
---

# Covalent API

[Covalent](https://www.covalenthq.com/) provides an API that allows you to pull historical blockchain data from multiple
blockchains to create applications such as:

- **Multi-chain wallets** presenting balances, transactions, ROI, portfolio values, and token analytics.
- **NFT dashboards** showing price trends, liquidity, and ROI of collectibles.
- **Analytics and transparency dashboards.**

## Using the API

[The Covalent API documentation for Palm](https://www.covalenthq.com/docs/networks/palm) provides information on available
API endpoints and lets users make requests directly from the browser.

You must [sign up for an API key](https://www.covalenthq.com/platform/#/auth/register/) to use the Covalent API.
The key must be passed as a query parameter with all requests using `key=API_KEY`.

The primary query parameter to switch between blockchain networks is `chain_id`.
For example, to fetch all token balances (including NFTs) of a Palm address, requests are made to the `balances_v2` endpoint
with `chain_id` as `11297108109` for Palm Mainnet and `11297108099` for Palm Testnet:

```text
https://api.covalenthq.com/v1/{chain_id}/address/{address}/balances_v2/
```
