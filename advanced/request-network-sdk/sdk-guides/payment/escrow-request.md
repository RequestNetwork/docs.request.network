# Escrow Payment

## Escrow

The Request Network Escrow isn't a separate payment network. Rather, it builds on top of the `ERC20_FEE_PROXY_CONTRACT` payment network.

## Typical Workflow

1. Using the [`request-client.js`](../../sdk-api-reference/request-client.js/) package, the `payer` creates a request with the `ERC20_FEE_PROXY_CONTRACT` payment network.
2. Using the [`payment-processor`](../../sdk-api-reference/payment-processor/) package, `payer`:
   1. Approves the escrow contract using `approveErc20ForEscrow()`
   2. Pays the escrow contract using `payEscrow()`
   3. Waits until the work is complete
   4. Pays the payee from the Escrow contract using `payRequestFromEscrow()`

These steps are shown by our unit tests:

[https://github.com/RequestNetwork/requestNetwork/blob/master/packages/payment-processor/test/payment/erc20-escrow-payment.test.ts#L200-L339](https://github.com/RequestNetwork/requestNetwork/blob/master/packages/payment-processor/test/payment/erc20-escrow-payment.test.ts#L200-L339)
