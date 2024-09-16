# PaymentReferenceCalculator

## Description

Compute the payment reference, the last 8 bytes of a salted hash of the request ID.

```typescript
last8Bytes(hash(lowercase(requestId + salt + address)))
```

The payment reference is the parameter that ties the request to events emitted by on-chain payments via Request Network payment smart contracts.

## Usage

```typescript
import { PaymentReferenceCalculator } from "@requestnetwork/request-client.js";
```

## Static method: calculate()

### Parameters

<table data-full-width="true"><thead><tr><th>Name</th><th>Type</th><th data-type="checkbox">Required</th><th>Description</th></tr></thead><tbody><tr><td>requestId</td><td>string</td><td>true</td><td>The ID of the request</td></tr><tr><td>salt</td><td>string</td><td>true</td><td>The salt of the request</td></tr><tr><td>address</td><td>string</td><td>true</td><td>Payment recipient address</td></tr></tbody></table>

### Returns

string

### Implementation

{% @github-files/github-code-block url="https://github.com/RequestNetwork/requestNetwork/blob/master/packages/payment-detection/src/payment-reference-calculator.ts" %}

