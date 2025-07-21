---
description: The Request Network API supports 500+ currencies across 10 EVM chains.
---

# Supported Chains and Currencies

{% hint style="info" %}
**Talk to an expert**

Discover how Request Network API can enhance your app's features - [book a call](https://calendly.com/mariana-rn/request-network-demo-docs) with us.
{% endhint %}

## Request Network API Supported Chains and Currencies

Overall, the Request Network API supports 500+ currencies across 10 EVM chains.

### ERC20, Native, and Conversion Payments Supported Chains

10 EVM Chains:

* Ethereum
* Arbitrum One
* OP Mainnet
* Base
* Polygon
* BSC
* Avalanche
* Fantom
* zkSync Era
* Sepolia

### ERC20 and Native Payments Supported Currencies

For ERC20 and Native Payments, the Request Network API supports 500+ tokens, mostly on Ethereum. See [request-network-token-list.md](../general/request-network-token-list.md "mention") for the full list.

{% hint style="info" %}
The [request-network-token-list.md](../general/request-network-token-list.md "mention") is a _super set_, which contains some currencies on non-supported chains. Cross-reference with the [#request-network-api-supported-chains](supported-chains-and-currencies.md#request-network-api-supported-chains "mention")&#x20;
{% endhint %}

### Conversion Payments Supported Currencies

For Conversion Payments, the Request Network API supports the following _invoice_ currencies:

* USD
* EUR
* CNY
* GBP
* JPY

For Conversion Payments, the Request Network API supports the following _payment_ currencies:

* USDC
* USDT
* DAI
* FAU on Sepolia

### Crosschain Payments Supported Currencies

See [#crosschain-payments-supported-chains-and-currencies](crosschain-payments.md#crosschain-payments-supported-chains-and-currencies "mention")

### Crypto-to-fiat Payments Supported Currencies

See [#crypto-to-fiat-supported-chains-and-currencies](crypto-to-fiat-payments.md#crypto-to-fiat-supported-chains-and-currencies "mention")

## Currencies API Endpoint

The currencies API endpoints provide access to the complete Request Network token list, enabling you to discover and filter available tokens across multiple blockchain networks.&#x20;

## Key Features

* **Payment Request Integration**: Get the exact currency IDs needed for creating payment requests
* **Payment Integration**: Get accurate token information for payment processing
* **Currency Validation**: Verify supported tokens before creating payment requests
* **Multi-Chain Support**: Access tokens across Ethereum, Polygon, Arbitrum, and more
* **Developer-Friendly**: Simple filtering options for easy integration

## Currency Information Structure

Each token in the response includes:

* **id**: Unique Request Network token identifier (e.g., `USDC-mainnet`)
* **name**: Human-readable token name (e.g., `USD Coin`)
* **symbol**: Token symbol (e.g., `USDC`)
* **decimals**: Number of decimal places for the token
* **address**: Contract address on the specified network
* **network**: Blockchain network name
* **type**: Token type (`ERC20`, `ETH`, etc.)
* **hash**: Contract address hash
* **chainId**: Blockchain chain ID

## Endpoints

{% openapi-operation spec="request-api" path="/v2/currencies" method="get" %}
[OpenAPI request-api](https://api.request.network/open-api/openapi.json)
{% endopenapi-operation %}
