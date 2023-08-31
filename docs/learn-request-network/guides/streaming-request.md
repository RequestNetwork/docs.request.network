# Streaming Request

Feature exists. Docs coming soon...

## Streaming payment request

To create a streaming payment request, you have to follow the same steps as for [Broken link](broken-reference "mention") but you will need to add a new kind of extension, identified with ERC777\_STREAM.

For example, one stream will probably be used to pay many requests in a row for a subscription.

The first request will be created with the usual parameters:

```typescript
extensions: {
   [ExtensionTypes.PAYMENT_NETWORK_ID.ERC777_STREAM]: {
      events: [], id: ExtensionTypes.PAYMENT_NETWORK_ID.ERC777_STREAM,
      type: ExtensionTypes.TYPE.PAYMENT_NETWORK,
      values: {
        paymentAddress,
        salt,
        feeAddress,
        feeAmount,
        refundAddress
      },
      version: '0.1.0',
    },
},
```

Subsequent requests are created with the following:

```
extensions: {
  [ExtensionTypes.PAYMENT_NETWORK_ID.ERC777_STREAM]: {
      events: [], id: ExtensionTypes.PAYMENT_NETWORK_ID.ERC777_STREAM,
      type: ExtensionTypes.TYPE.PAYMENT_NETWORK,
      values: {
        // For indexing purpose, originalRequestId is the first request created for the stream
        originalRequestId,
        // For balance computation, the request before this one to be paid by the stream
        previousRequestId,
        // For balance and indexing, rank in the series. 1 when originalRequestId = previousRequestId
        recurrenceNumber
      },
      version: '0.1.0',
    },
},
```

Tests are available [here](https://github.com/RequestNetwork/requestNetwork/tree/master/packages/payment-detection/test/erc777).
