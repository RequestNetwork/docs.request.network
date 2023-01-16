# Advanced payment methods

## Escrow

One can pay a request in 2 steps to show his contractor that funds are available without making them available. In the first step, the`PayEscrow` method from the [payment-processor](https://github.com/RequestNetwork/requestNetwork/blob/master/packages/payment-processor/src/payment/erc20-escrow-payment.ts) will lock funds until the payer approves the work done. The payer will then use the method `payRequestFromEscrow` to unlock funds.

The escrow contract is deployed on all the chains supported by the Request Network Protocol and is available [here](https://github.com/RequestNetwork/requestNetwork/blob/master/packages/smart-contracts/src/lib/artifacts/ERC20EscrowToPay/index.ts). For now, it is only compatible with ERC20FeeProxy requests.

## Streaming payment request

To create a streaming payment request, you have to follow the same steps as for [creating-an-erc20-request.md](creating-an-erc20-request.md "mention") but you will need to add a new kind of extension, identified with ERC777\_STREAM.

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

Tests are available [here](https://github.com/RequestNetwork/requestNetwork/tree/796d3f465ea78f75e068645adca38d0836568fe6/packages/payment-detection/test/erc777).
