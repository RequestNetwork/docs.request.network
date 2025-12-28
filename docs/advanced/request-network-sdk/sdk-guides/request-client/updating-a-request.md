# Updating a Request

After a request is created, it can be updated by the authorized parties. Each update requires a signature and is persisted to the Request Network.

## Summary of Actions

| Action | Description | Authorized Role |
| :--- | :--- | :--- |
| **accept** | Accept a request, indicating that it will be paid | Payer |
| **cancel** | Cancel a request | Payee or Payer |
| **reduceExpectedAmount** | Reduce the expected amount | Payee |
| **increaseExpectedAmount** | Increase the expected amount | Payer |

## Examples

### Initialize the Request Client

First, retrieve the request you want to update. You must provide a `signatureProvider` to sign the update transactions.

```javascript
const { RequestNetwork, Types } = require("@requestnetwork/request-client.js");

const requestClient = new RequestNetwork({
  nodeConnectionConfig: { baseURL: "https://sepolia.gateway.request.network/" },
  signatureProvider: epkSignatureProvider, // Required for updates
});

const request = await requestClient.fromRequestId('YOUR_REQUEST_ID');
```

### Accept a Request (Payer)

The payer can accept a request to signal their intention to pay.

```javascript
const updatedRequestData = await request.accept({
  type: Types.Identity.TYPE.ETHEREUM_ADDRESS,
  value: payerAddress,
});

// Wait for the update to be persisted
await request.waitForConfirmation();
```

### Cancel a Request (Payee or Payer)

Either the payee or the payer can cancel a request.

```javascript
const updatedRequestData = await request.cancel({
  type: Types.Identity.TYPE.ETHEREUM_ADDRESS,
  value: signerAddress,
});

await request.waitForConfirmation();
```

### Increase Expected Amount (Payer)

The payer can increase the expected amount (e.g., adding a tip or adjusting for additional services).

```javascript
const updatedRequestData = await request.increaseExpectedAmountRequest(
  '100000000000000000', // Amount to add in base units (e.g., 0.1 ETH)
  {
    type: Types.Identity.TYPE.ETHEREUM_ADDRESS,
    value: payerAddress,
  }
);

await request.waitForConfirmation();
```

### Reduce Expected Amount (Payee)

The payee can reduce the expected amount (e.g., applying a discount).

```javascript
const updatedRequestData = await request.reduceExpectedAmountRequest(
  '100000000000000000', // Amount to subtract in base units
  {
    type: Types.Identity.TYPE.ETHEREUM_ADDRESS,
    value: payeeAddress,
  }
);

await request.waitForConfirmation();
```
