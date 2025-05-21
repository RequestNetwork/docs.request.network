# SDK and Request Node Overview

## Request Network SDK

The **Request Network SDK** is the set of software packages for interacting with the Request Network. The [packages](../request-network-sdk/get-started/installation.md#external-packages) can be installed via npm and allow developers to:

* Create new requests
* Update requests
* Pay requests
* Retrieve requests
* Detecting payments

## Request Node

The **Request Node** is a software bundle that provides a gateway to the various layers of the Request Network Protocol:

* [IPFS](https://developers.cloudflare.com/web3/ipfs-gateway/concepts/ipfs/) persists the request content
* Smart contracts persist the unique IPFS [CID](https://developers.cloudflare.com/web3/ipfs-gateway/concepts/ipfs/#content-identifiers) on-chain
* [The Graph](https://thegraph.com/docs/en/about/) indexes smart contract events

This Request Node acts as a relay server which helps reduce friction and costs for the end user. The user signs the request contents, but the node funds the fees required to persist the contents in the various protocol layers.

The Request Network Foundation operates several [Request Nodes](https://docs.request.network/get-started/request-node-gateways) which builders can use to quickly test the protocol. However, for production, we advise you to run your own node which can be installed via either [npm](https://www.npmjs.com/package/@requestnetwork/request-node) or [docker](https://hub.docker.com/r/requestnetwork/request-node).

## Interaction between SDK and Request Node

Shown below is a diagram that depicts how the SDK and Node interact with the protocol.

<div data-full-width="true"><figure><img src="../../.gitbook/assets/image (5) (2).png" alt=""><figcaption><p>Interacting with Request Network via a Request Node</p></figcaption></figure></div>
