---
description: A high level overview on how our API does payment detection
---

# Payment Detection

## Payment Detection in Request API

### Overview

The Request API uses a [**reference-based payment detection system**](https://docs.request.network/advanced/protocol-overview/how-payment-networks-work#reference-based-payment-networks-recommended) that automatically monitors blockchain transactions to detect when payments are made to your requests. This system works across [multiple blockchains](../general/supported-chains/) and handles various payment scenarios.

### How It Works

#### 1. Payment Reference Generation

When you create a payment request, the API automatically generates a unique[ **payment reference**](https://docs.request.network/advanced/request-network-sdk/sdk-guides/request-client/payment-reference)**,** a 16-character identifier that acts as a fingerprint for your request. This reference is what connects blockchain transactions back to your specific request.

#### 2. Blockchain Monitoring

The API continuously monitors supported blockchains using subgraphs that scan for transactions containing payment references. This happens automatically in the background, no action required from you.

#### 3. Automatic Detection

When someone makes a payment and includes the payment reference in their transaction, our system:

* Detects the transaction within minutes
* Validates the payment details (amount, currency, recipient)
* Updates the request status (partially paid, fully paid, etc.)
* Triggers your configured webhooks

#### 4. Real-time Updates

Once a payment is detected, your request status is immediately updated and you can get the latest information via:

* GET requests to check payment status using the request's id
* Automatically receive updates to your [webhooks](api-portal-manage-api-keys-and-webhooks.md)

### Crosschain Support

All crosschain payments done using the [Request Network API](crosschain-payments.md) use our [ERC 20 Fee proxy contract](https://docs.request.network/advanced/protocol-overview/how-payment-networks-work#erc20-fee-proxy-contract) as the last leg of payment, so payment detection works out of the box.

### Webhook Notifications

For real-time integration, you can configure webhooks to be notified for the following events:

* **Payment Confirmed**: Full payment received
* **Payment Partial**: Partial payment received
* **Payment Failed**: Transaction failed
* **Payment Refunded**: Payment was refunded

This allows your application to react immediately to payment events without constantly polling the API.

### Integration Benefits

* **Zero Configuration**: Payment detection happens automatically
* **Multi-blockchain**: Works across all supported networks
* **Real-time**: Fast detection and status updates
* **Flexible**: Handles various payment scenarios
* **Reliable**: Built on proven blockchain indexing infrastructure

The system is designed to be completely transparent to your application, simply create requests and let the API handle all the complexity of monitoring blockchains for payments.
