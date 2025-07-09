# Currencies

{% hint style="info" %}
**Talk to an expert**

Discover how Request Network API can enhance your app's features - [book a call](https://calendly.com/mariana-rn/request-network-demo-docs) with us.
{% endhint %}

## Overview

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
