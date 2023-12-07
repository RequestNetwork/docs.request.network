---
description: Frequently Asked Questions and Common Misconceptions
---

# FAQ

{% hint style="info" %}
If your question is not answered below, please consider posting it to the [Request Network Discussions](https://github.com/orgs/RequestNetwork/discussions) page on Github.
{% endhint %}

## Is Request Network a blockchain, smart contract platform, or L2 scaling solution?

No. Request Network is not a blockchain, smart contract platform, or scaling solution. Rather, it's a protocol for storing payment requests and facilitating on-chain payments. It stores payment requests in [IPFS](https://www.ipfs.com/) with CID hashes stored on [Gnosis Chain](https://www.gnosis.io/). It uses [The Graph](https://thegraph.com/) for on-chain event indexing. It processes payments across a variety of [supported payment chains](https://docs.request.network/get-started/supported-chains).

## Do I need to run a Request Node on \<Chain> to create a request for funds on \<Chain>?

No, you don't need to run a Request Node on \<Chain> to request funds on \<Chain>. Requests are created on Gnosis Chain (or on testnets like Goerli or Sepolia) regardless of where the payment(s) will occur. Payment(s) can occur on any of our [supported payment chains](https://docs.request.network/get-started/supported-chains#payments).

To help builders get started quickly, the Request Network Foundation operates several [Request Node Gateways](https://docs.request.network/get-started/request-node-gateways) that are free for anyone to use. These gateways offer endpoints for creating and retrieving requests.

Requests created on the Gnosis Chain are "real" and should exist forever. Requests created on Goerli or Sepolia are "test" requests and will exist only as long as the test net exists.

## When I use a Request Node, is it possible for the Request Node to modify my request contents before persisting them to IPFS and on-chain?

In theory, yes, a Request Node could change a request's contents before persisting it in IPFS and on-chain. When using a Request Node, the platform and end-user must trust the Request Node operator. This is true even for encrypted requests. That said, the `@requestnetwork/request-node` package and the Request Node Gateways operated by the Request Network Foundation do _NOT_ modify requests before persisting them.

If this trust assumption is not acceptable, then it is possible to persist a request in IPFS and on-chain directly from the SDK without using a Request Node.

## Is the address that signs to create a request the same address that receives the payment?

Not necessarily. The address that signs to create a request is not necessarily the same address that receives the payment. They can be different.

The address that signs to create a request is the `signer` when creating a request via `createRequest()` in the Request Network SDK. It's usually the payee identity but can alternatively be the payer identity.\
The address that receives the payment is defined by the payment network, usually at `paymentNetwork.parameters.paymentAddress`.

This design allows for a single payee identity to have potentially multiple payment addresses.

## Are requests in Request Network stored fully on-chain?

No. Request Network is a hybrid on/off-chain protocol storing the majority of request contents in IPFS. Only the content-addressable ID (CID) is stored on-chain, on Gnosis Chain.

## Can I make a payment \*before\* creating a request?

Yes. Payments are linked to requests via a `paymentReference` which is derived from the `requestId` which is derived from the request contents. Therefore, it is possible to calculate the `paymentReference`, send a payment using the `paymentReference` as input to the smart contract, and then create a request.

Details:

* Sending a payment before creating the request can be done using functions in the `payment-processor` package.
* The higher-level `request-client.js` package doesn't support payments before the request is created.
* This `paymentReference` consists of the last 8 bytes of a salted hash of the `requestId`: `last8Bytes(hash(lowercase(requestId + salt + info)))`:
  * `requestId` is the id of the request
  * `salt` is a random number with at least 8 bytes of randomness. It must be unique to each request
  * `info` is the JSON-stringified string of the `paymentInfo` for a payment, or `refundInfo` for a refund.
  * `lowercase()` transforms all characters to lowercase
  * `hash()` is a keccak256 hash function
  * `last8Bytes()` take the last 8 bytes

## Does Request Network support requests for fiat currency?

Yes and No.

Requests can be _denominated_ in fiat currencies like USD, EUR, etc. ([ISO 4217 currencies](https://en.wikipedia.org/wiki/ISO\_4217)) but our payment smart contracts only support payments in cryptocurrencies. We call these Conversion payments, in which the requested fiat amount is converted to the appropriate cryptocurrency amount using on-chain price feeds at the moment of payment.

It is possible to implement fiat payments using Declarative Requests, where the payer declares that the payment was sent and the payee declares that the payment was received.

## Can I access a user's historical invoices created via Request Finance?

Yes. It is possible to request access to a user's Request Finance invoices using the [`add-stakeholder` web component](https://docs.request.network/learn-request-network/components/add-stakeholder) which is just a thin wrapper around the [Request Finance Add Stakeholders API](https://docs.request.finance/faq#i-am-integrating-the-request-network.-can-i-get-access-to-users-data-on-request-finance). They display a dialog that prompts the end-user to grant access to 1 invoice at a time.

Details:

* Request Finance invoices are encrypted.
* Request Network Foundation cannot grant access to encrypted requests in Request Finance.

## Can I create a request via a smart contract call?

No. It is not currently possible to create a request via a smart contract call. However, [RequestNetwork/public-issues#15](https://github.com/RequestNetwork/public-issues/issues/15) is in our roadmap to make this possible.

The recommended way to create a request is using the Request Network SDK via a frontend so the payee or payer can sign to create the request.

## Does Request Network support cross-chain payments where the payer sends funds on one chain and the payee receives funds on a different chain?

No. Request Network does not currently support cross-chain payments. All of the supported payment types involve the payer sending funds and the payee receiving funds on the same chain.

We would be open to integrating a cross-chain payment system but would need to balance it against the other priorities in our [backlog](https://github.com/orgs/RequestNetwork/projects/3/views/7).

## Does Request Network support private requests?

Yes. See [private-requests-using-encryption.md](protocol-overview/private-requests-using-encryption.md "mention")

## Does Request Network support private payments?

No. Request Network does not currently support private payments. All of the supported payment types use public blockchains on which all transactions are public.

We would be open to integrating a private payment system but would need to balance it against the other priorities in our [backlog](https://github.com/orgs/RequestNetwork/projects/3/views/7).

## Can I use my own payment infrastructure with Request Network requests?

Yes. Request Network is open to integrating 3rd party payment infrastructure as long as the integration is open-source and results in Pull Requests (PRs) into the Request Network SDK that enables all of the Builders in our ecosystem to benefit and use the payment infrastructure.

Request Network payment detection relies on events emitted during the payment transaction that contain a `paymentReference` which ties the payment to the request. The `paymentReference` is an 8-byte hash derived from the request ID.

### Option 1: Update the 3rd party payment smart contract to emit the `paymentReference` event.

This means adding a new payment network. This would require updates to the following components:

* [payment-subgraph](https://github.com/RequestNetwork/payments-subgraph) repo - indexes on-chain payment events
* [requestNetwork](https://github.com/RequestNetwork/requestNetwork) monorepo
  * payment-detection - functions to query the payment-subgraph
  * advanced-logic - payment network definitions
  * payment-processing - functions to call the smart contracts
  * request-client - functions to create and update requests

#### Example

These PRs added the ERC20TransferableReceivable payment network

* [feat: ERC20TransferableReceivable payments-subgraph#48](https://github.com/RequestNetwork/payments-subgraph/pull/48)
* [feat: ERC20TransferableReceivable payment network #1033](https://github.com/RequestNetwork/requestNetwork/pull/1033)

### Option 2: Update the 3rd party payment smart contract to call one of the existing Request Network payment network smart contracts.

In this case, the existing payment detection system should work out of the box without changes. This may, or may not involve adding a new payment network. This would require updates to the following components:

* [requestNetwork](https://github.com/RequestNetwork/requestNetwork) monorepo
  * payment-processing - functions to call the smart contracts
  * request-client - functions to create and update requests

### Option 3: Deploy a One Time Proxy smart contract for each request.

**This option is not yet available and will be implemented in** [**#1283**](https://github.com/RequestNetwork/requestNetwork/issues/1283)
