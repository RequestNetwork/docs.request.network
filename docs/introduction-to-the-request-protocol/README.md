# Request Protocol

Request is an open database for any type of payment request - from business invoices to reimbursements between friends. It aims to support products at any scale from startups to large organizations, from the private to the public sector.

The Request Protocol is the core of Request. It's the bottom layer that defines and handles the data of a request and persists it to a distributed ledger to make Request open, trustless, secure, and resilient.

This section is aimed at helping you understand how the protocol is structured, how it works and meets its requirements. It is particularly useful if you want to propose changes or implement them yourself.

## Overview

The Request Protocol has one fundamental purpose: **to persist, on a distributed ledger, data representing requests and to be able to retrieve these data efficiently**.

To achieve this, the Request Protocol follows the layered architecture pattern. Each layer is responsible for a specific task and a specific level of abstraction. This layered architecture is highly extensible and hopefully easy to understand.

The protocol is composed of four layers:

* Request logic
* Transaction
* Data Access
* Storage

&#x20;_Layers of the Request Protocol, each layer is described in the next section._

This layered architecture allows package reusability and makes the protocol more upgradeable. For example, our current implementation uses Ethereum and IPFS. Still, suppose Arweave turns out to be a better solution for storing data in a decentralized database than IPFS. In that case, we can create a new storage layer that uses Arweave and make the data-access layer using this new package instead.

### Interface vs implementation

The protocol follows a defined interface; each layer has to implement a specific interface. The interfaces for each layer can be found in the Types package of Request Network repository: [https://github.com/RequestNetwork/requestNetwork/tree/master/packages/types](https://github.com/RequestNetwork/requestNetwork/tree/master/packages/types).

The following pages present the first implementation of the protocol used for the released version of Request V2 on mainnet.
