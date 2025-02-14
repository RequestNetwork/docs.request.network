---
description: Frequently Asked Questions and Common Misconceptions
---

# FAQ

{% hint style="info" %}
If your question is not answered below, please consider posting it to the [Request Network Discussions](https://github.com/orgs/RequestNetwork/discussions) page on Github.
{% endhint %}

<details>

<summary>Is Request Network a blockchain, smart contract platform, or L2 scaling solution?</summary>

No. Request Network is not a blockchain, smart contract platform, or scaling solution. Rather, it's a protocol for storing payment requests and facilitating on-chain payments. It stores payment requests in [IPFS](https://www.ipfs.com/) with CID hashes stored on [Gnosis Chain](https://www.gnosis.io/). It uses [The Graph](https://thegraph.com/) for on-chain event indexing. It processes payments across a variety of [supported payment chains](https://docs.request.network/get-started/supported-chains).

</details>

<details>

<summary>Do I need a Request Node on &#x3C;Chain> to request funds on &#x3C;Chain>?</summary>

No, you don't need a Request Node on \<Chain> to request funds on \<Chain>. Requests are created on Gnosis Chain (or on a testnet like Sepolia) regardless of where the payment(s) will occur. Payment(s) can occur on any of our [supported payment chains](https://docs.request.network/get-started/supported-chains#payments).

To help builders get started quickly, the Request Network Foundation operates several [Request Node Gateways](https://docs.request.network/get-started/request-node-gateways) that are free for anyone to use. These gateways offer endpoints for creating and retrieving requests.

Requests created on the Gnosis Chain are "real" and should exist forever. Requests created on Sepolia are "test" requests and will exist only as long as the testnet exists.

</details>

<details>

<summary>Can a Request Node modify a request's contents before persisting it to IPFS and on-chain?</summary>

No. A Request Node cannot change a request's contents before persisting it to IPFS and on-chain because doing so would invalidate the signature. This is true for private, encrypted requests as well. The Request Node cannot forge the end-user's signature.

</details>

<details>

<summary>Can I create a request via the Request Network SDK without using a Request Node?</summary>

No. Today, a Request Node is required to interact with the Request Network IPFS Network. That said, it is possible to make the end-user pay the protocol fee when creating a request instead of the Request Node. To do this, inject an `HttpMetaMaskDataAccess` into the frontend `RequestNetwork` instance.

```typescript
const requestNetwork = new RequestNetworkBase({
  dataAccess: new HttpMetaMaskDataAccess({
    ethereumProviderUrl: 'https://eth-mainnet.g.alchemy.com/v2/demo',
  }),
  ...
}
```

</details>

<details>

<summary>Is the address that signs to create a request the same address that receives the payment?</summary>

Not necessarily. The address that signs to create a request is not necessarily the same address that receives the payment. They can be different.

The address that signs to create a request is the `signer` when creating a request via `createRequest()` in the Request Network SDK. It's usually the payee identity but can alternatively be the payer identity.\
The address that receives the payment is defined by the payment network, usually at `paymentNetwork.parameters.paymentAddress`.

This design allows for a single payee identity to have potentially multiple payment addresses.

</details>

<details>

<summary>Are requests in Request Network stored fully on-chain?</summary>

No. Request Network is a hybrid on/off-chain protocol storing the majority of request contents in IPFS. Only the content-addressable ID (CID) is stored on-chain, on Gnosis Chain.

</details>

<details>

<summary>Can I make a payment *before* creating a request?</summary>

Yes - using [in-memory-requests.md](advanced/request-network-sdk/sdk-guides/request-client/in-memory-requests.md "mention"). Payments are linked to requests via a `paymentReference` which is derived from the `requestId` which is derived from the request contents. Therefore, it is possible to calculate the `paymentReference`, send a payment using the `paymentReference` as input to the smart contract, and then create a request.

</details>

<details>

<summary>Does Request Network support requests for fiat currency?</summary>

Yes and No.

Requests can be _denominated_ in fiat currencies like USD, EUR, etc. ([ISO 4217 currencies](https://en.wikipedia.org/wiki/ISO_4217)) but our payment smart contracts only support payments in cryptocurrencies. We call these Conversion payments, in which the requested fiat amount is converted to the appropriate cryptocurrency amount using on-chain price feeds at the moment of payment.

It is possible to implement fiat payments using Declarative Requests, where the payer declares that the payment was sent and the payee declares that the payment was received.

</details>

<details>

<summary>Can I access a user's historical invoices created via Request Finance?</summary>

Yes. It is possible to request access to a user's Request Finance invoices using the [`add-stakeholder` web component](https://docs.request.network/learn-request-network/components/add-stakeholder) which is just a thin wrapper around the [Request Finance Add Stakeholders API](https://docs.request.finance/faq#i-am-integrating-the-request-network.-can-i-get-access-to-users-data-on-request-finance). They display a dialog that prompts the end-user to grant access to 1 invoice at a time.

Details:

* Request Finance invoices are encrypted.
* Request Network Foundation cannot grant access to encrypted requests in Request Finance.

</details>

<details>

<summary>Can I create a request via a smart contract call?</summary>

No. It is not currently possible to create a request via a smart contract call. However, [RequestNetwork/public-issues#15](https://github.com/RequestNetwork/public-issues/issues/15) is in our roadmap to make this possible.

The recommended way to create a request is using the Request Network SDK via a frontend so the payee or payer can sign to create the request.

</details>

<details>

<summary>Does Request Network support cross-chain payments where the payer sends funds on one chain and the payee receives funds on a different chain?</summary>

No. Request Network does not currently support cross-chain payments. All of the supported payment types involve the payer sending funds and the payee receiving funds on the same chain.

We would be open to integrating a cross-chain payment system but would need to balance it against the other priorities in our [backlog](https://github.com/orgs/RequestNetwork/projects/3/views/7).

</details>

<details>

<summary>Does Request Network support private requests?</summary>

Yes. See:

* [private-requests-using-encryption.md](advanced/protocol-overview/private-requests-using-encryption.md "mention")
* [handle-encryption-with-a-web3-wallet.md](advanced/request-network-sdk/sdk-guides/encryption-and-decryption/handle-encryption-with-a-web3-wallet.md "mention")

</details>

<details>

<summary>Does Request Network support private payments?</summary>

Yes. See [hinkal-private-payments.md](advanced/request-network-sdk/sdk-guides/payment/hinkal-private-payments.md "mention")

</details>

<details>

<summary>Can I use Request Network requests even if I have my own payment infrastructure?</summary>

Yes. See [single-request-forwarder.md](advanced/request-network-sdk/sdk-guides/payment/single-request-forwarder.md "mention")

</details>
