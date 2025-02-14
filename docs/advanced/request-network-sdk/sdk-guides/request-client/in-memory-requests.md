# In-Memory Requests

## Overview

In-memory requests allow for creating and managing requests without immediately persisting them to storage. This enables faster payment workflows and deferred persistence.

## Key benefits:

* **Faster payment flow:** In-memory requests are helpful when payment is the priority, such as in e-commerce cases. In this scenario, the request is a receipt rather than an invoice.
* **Deferred Persistence:** With in-memory requests, a request can be created on the front end with a user's signature and passed on to the backend for persistence.

## How it works:

The flow of creating and paying an in-memory request is similar to a regular request with the following key differences:

* Create an in-memory request by passing the argument `skipPeristence: true` when instantiating the `RequestNetwork` instance.
* An in-memory request is _not_ persisted immediately like normal requests.  Instead, it is stored in memory on the device where it was created. It can be persisted at a later time using the `persistTransaction()`function.
* An in-memory request has the `inMemoryInfo`  property.
* Avoid calling `getData()` on an in-memory request because it will fail silently by returning an empty `EventEmitter` object.
* Retrieving an in-memory request with `requestClient.fromRequestId()` will fail because the request has not been persisted yet so it is not possible to read it from the Request Node.

{% stepper %}
{% step %}
### Install necessary dependencies

To create in-memory requests, it is necessary to install the following package:

```bash
npm install @requestnetwork/request-client.js
```

Along with the following package for payments:

```bash
npm install @requestnetwork/payment-processor
```
{% endstep %}

{% step %}
### Create an in-memory request

Create an in-memory request by passing the argument `skipPeristence: true` when instantiating the `RequestNetwork` instance.

<pre class="language-typescript"><code class="lang-typescript">// Request parameters 
const requestParameters = {...}


const web3SignatureProvider = new Web3SignatureProvider(
    ethersProvider!.provider
  );

 const inMemoryRequestNetwork = new RequestNetwork({
    nodeConnectionConfig: {
      baseURL: "https://gnosis.gateway.request.network",
    },
    signatureProvider: web3SignatureProvider,
   <a data-footnote-ref href="#user-content-fn-1"> skipPersistence: true,</a>
  });

 let inMemoryRequest =
    await inMemoryRequestNetwork.createRequest(requestParameters);

</code></pre>
{% endstep %}

{% step %}
### Pay an in-memory request

To pay an in-memory request, pass the `inMemoryInfo.requestData` property to the payment function.

```typescript
import {
  payRequest
} from "@requestnetwork/payment-processor";

const paymentTx = await payRequest(
    inMemoryRequest.inMemoryInfo.requestData,
    signer
  );
  
await paymentTx.wait(confirmationBlocks);

```
{% endstep %}

{% step %}
### Persist in-memory request

In-memory requests need to be persisted using a new `RequestNetwork` client that does not use the `skipPersistence` property.

```typescript
const persistingRequestNetwork = new RequestNetwork({
    nodeConnectionConfig: {
      baseURL: "https://gnosis.gateway.request.network",
    },
  });

await persistingRequestNetwork.persistRequest(inMemoryRequest);
```
{% endstep %}
{% endstepper %}

[^1]: Configure the RequestNetwork instance to produce in-memory requests

