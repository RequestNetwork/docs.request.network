# Escrow Request

Feature exists. Docs coming soon...

## Escrow

One can pay a request in 2 steps to show his contractor that funds are available without making them available. In the first step, the`PayEscrow` method from the [payment-processor](https://github.com/RequestNetwork/requestNetwork/blob/master/packages/payment-processor/src/payment/erc20-escrow-payment.ts) will lock funds until the payer approves the work done. The payer will then use the method `payRequestFromEscrow` to unlock funds.

The escrow contract is deployed on all the chains supported by the Request Network Protocol and is available [here](https://github.com/RequestNetwork/requestNetwork/blob/master/packages/smart-contracts/src/lib/artifacts/ERC20EscrowToPay/index.ts). For now, it is only compatible with ERC20FeeProxy requests.
