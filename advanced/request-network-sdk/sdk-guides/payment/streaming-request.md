---
description: Pay a series of requests with a stream of ERC777 Super Tokens from Superfluid.
---

# Streaming Payment

The first request of a series is very similar to [payment-network-erc20-fee-proxy](https://github.com/RequestNetwork/requestNetwork/blob/7be84246d3012959739a99db6463062374f6cd91/packages/advanced-logic/specs/payment-network-erc20-fee-proxy-contract-0.1.0.md), it defines the `salt`, `paymentAddress` and `requestId` to compute the `paymentReference` used for the whole series.

Other requests must define a `previousRequestId` and cannot redefine any of the payment properties (`paymentAddress`, `feeAddress` or `salt`).

Multiple requests can be paid with the same stream, typically recurring requests of fixed amounts paid continuously. A group of requests payable with the same stream are called a request series, they must all have the same currency.

For additional details, see the [payment-network-erc777-stream-0.1.0 specification](https://github.com/RequestNetwork/requestNetwork/blob/7be84246d3012959739a99db6463062374f6cd91/packages/advanced-logic/specs/payment-network-erc777-stream-0.1.0.md)

## Create a Streaming Request

To create a streaming request, [#create-a-request](../../get-started/quickstart-node.js.md#create-a-request "mention") like normal, but set the `paymentNetwork` parameter to the `ERC777_STREAM` payment network.

### Create the first request in a series

```typescript
  // The paymentNetwork is the method of payment and related details.
  paymentNetwork: {
    id: Types.Extension.PAYMENT_NETWORK_ID.ERC777_STREAM,
    parameters: {
      expectedFlowRate: expectedFlowRate // number, Expected amount of request currency per second
      expectedStartDate: expectedStartDate // timestamp, Expected start of stream	
      paymentAddress: payeeIdentity,
    },
  },
```

### Create subsequent requests in a series

```
  // The paymentNetwork is the method of payment and related details.
  paymentNetwork: {
    id: Types.Extension.PAYMENT_NETWORK_ID.ERC777_STREAM,
    parameters: {
      originalRequestId: 'abcd', // first reqeust in the series
      previousRequestId: 'abcd', // previous request in the series
      recurrenceNumber: 1, // 1 if previous request is original, 2+ otherwise
    },
  },
```

## Tests

See Github for tests showing usage.

* [https://github.com/RequestNetwork/requestNetwork/blob/master/packages/advanced-logic/test/extensions/payment-network/erc777/stream.test.ts](https://github.com/RequestNetwork/requestNetwork/blob/master/packages/advanced-logic/test/extensions/payment-network/erc777/stream.test.ts)
* [https://github.com/RequestNetwork/requestNetwork/tree/master/packages/payment-detection/test/erc777](https://github.com/RequestNetwork/requestNetwork/tree/master/packages/payment-detection/test/erc777)
