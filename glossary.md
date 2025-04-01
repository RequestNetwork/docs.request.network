# Glossary

## Parties of a Request

### Payee Identity

The Payee Identity is the EVM address that uniquely identifies the payee. It can be but is not necessarily the address that will receive the payment. It is authorized to make certain updates to the request after it is created. It is one of the owners of the request data. The Payee Identity is defined by the `payee` field when creating a request.&#x20;

### Payer Identity

The Payer Identity is the EVM address that uniquely identifies the payer. It can be but is not necessarily the address that will send the payment. It is authorized to make certain updates to the request after it is created. It is one of the owners of the request data. The Payer Identity is defined by the `payer` field when creating a request.&#x20;

### Signer Identity

The Signer Identity is the EVM address that provides the signature to create a request. It must be either the Payee Identity or Payer Identity.

### Payment Recipient

The EVM address that receives the payment. It is defined by the `paymentNetwork.parameters.paymentAddress`field when creating a request.

### Payment Sender

The EVM address that sends the payment. Anyone can pay a given request. The payment sender address is NOT stored in the request contents.

### Additional Stakeholder

An EVM address that has been granted view access to an encrypted request.

### Declarative Delegate

An EVM address that has been granted authorization to declare payments sent and payments received on behalf of either the Payee Identity or Payer Identity of a given request.&#x20;

## Request Protocol

### Action

An action is signed data added by a request's stakeholder into the Request Protocol that creates or updates the state of a request. A request can be represented by a list of actions. For example, the creation of a request is an action.

### Balance

When using a payment network, the balance is the current amount paid for a request. The balance is determined by the payment detection method of the payment network used.

A request with no payment network provided doesn't have a balance.

### Confirmed/Pending action

Request relies on other blockchain technologies to ensure data immutability. Most blockchains don't offer transaction instant finality. This means that when performing an action on the request, this action can't directly be confirmed as effective.

As long as the action hasn't persisted and is not confirmed, the action is marked as "pending". The "pending" state helps have a fast response and a good user experience. Until the request is Confirmed, it should not be relied upon.

### Signature Provider

A signature provider is an abstraction of identity management and action signatures. Depending on use cases, it allows you to give your user complete control or handle some parts for them.

### Decryption Provider

A decryption provider is an abstraction of the mechanism that handles the decryption of a request. Depending on use cases, it allows you to give your user complete control or handle some parts for them.

It is not used for clear requests.

### Extension

An extension is a set of actions that extends the feature of a request. A request without extension is a fundamental request for payment with a payee, a currency, and a requested amount. The extension allows for more advanced features.

### Identity

The identity defines a stakeholder of a request that allows signing or encrypting the request actions. The identity is the public data that identifies the stakeholder.

### Request ID

The request ID is the number that uniquely identifies a request. This number is computed from the hash of the request creation action.

### Request Data (aka. Request Contents)

The request data is the current state of a request, the data of the request after having applied all the confirmed actions on it.

### Stakeholder

A request stakeholder is a party involved with the request. Stakeholders are generally the payer and the payee of the request or any other third-party allowed to perform actions on it. For encrypted requests, stakeholders are any party interested in reading the request content.

### Topic

A topic is a string that is used to index a request. This topic is used for request retrieval. Several requests can share the same topic.

Every request has its request id and payee identity as topics (and the payer identity if it is defined). Any custom topic can be appended to a request.

## Payments

### Payment Detection

Payment detection is a method defined by the payment network to determine the current balance of a request.

### Payment Network (aka Payment Extension)

A payment network is a predefined set of rules to agree on the balance of a request. The payment network is defined during the creation of the request.

A payment network is generally related to one currency, but it's not always the case (the Declarative payment network is currency agnostic).

### Payment Reference

In the Reference-based Payment Networks, Payments are linked to Requests via a `paymentReference` which is derived from the `requestId` and payment recipient address. For details see [payment-reference.md](advanced/request-network-sdk/sdk-guides/request-client/payment-reference.md "mention")

### Conversion Payment

A "conversion" request is denominated in one currency but paid in another currency. This is facilitated by on-chain price feeds provided by oracles. The typical use case is to denominate a request in fiat like USD and pay the request in stablecoins like USDC or DAI. For details see [conversion-request.md](advanced/request-network-sdk/sdk-guides/payment/conversion-request.md "mention")

### Swap-to-pay Payment

A "swap-to-pay" payment is where the payment sender sends one currency but the payment recipient receives a different currency. For details see [swap-to-pay-request.md](advanced/request-network-sdk/sdk-guides/payment/swap-to-pay-request.md "mention")

### Swap-to-Conversion Payment

A "swap-to-conversion" payment is where the request is denominated in currency A, the payer sends currency B and the payee receives currency C. For details see [swap-to-conversion-request.md](advanced/request-network-sdk/sdk-guides/payment/swap-to-conversion-request.md "mention")

## Ecosystem

### Request Client

The Request Client is a Javascript library that interacts directly with the Request Protocol. The Request Client connects to a Request Node.

### Request Node

Request Nodes are HTTP servers exposing an API that allows the Request Client to communicate with the Request Protocol. These servers abstract the complexity of IPFS and Ethereum used by the Request Protocol.

### Request Protocol

The Request Protocol is the underlying protocol that powers Request. It defines how requests are stored on a distributed ledger and how to interpret actions performed on them.

## Blockchain, Cryptography

### Confirmation

Confirmation means that the network has verified the blockchain transaction. This happens through a process known as mining in a proof-of-work system (e.g., Bitcoin). Once a transaction is confirmed, it cannot be reversed.

### Ether

Ether is the native token of the Ethereum blockchain, which is used to pay for transaction fees, block proposer rewards, and other services on the network.

### IPFS

The Inter-Planetary File System (IPFS) is a protocol and a peer-to-peer network for storing and sharing data in a distributed file system. IPFS uses content-addressing to uniquely identify each file in a global namespace connecting all computing devices.

The Request Protocol uses IPFS to ensure data accessibility.

### Multi-signature

Multi-signature (multisig) wallets allow multiple parties to require more than one key to authorize a transaction. The needed number of signatures is agreed upon at the creation of the wallet. Multi-signature addresses have a much greater resistance to theft.

### Private Key

A private key is a large number that allows you to sign or decrypt messages. Private keys can be thought of as a password; private keys must never be revealed to anyone but you, as they allow you to spend the funds from your wallet through a cryptographic signature.
