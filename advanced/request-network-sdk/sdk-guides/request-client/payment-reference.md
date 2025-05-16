# Payment Reference

In the Reference-based Payment Networks, Payments are linked to Requests via a `paymentReference` which is derived from the `requestId` and payment recipient address.

This `paymentReference` consists of the last 8 bytes of a salted hash of the `requestId` and payment recipient address, concatenated :&#x20;

```typescript
last8Bytes(hash(lowercase(requestId + salt + address)))
```

* `requestId` is the id of the request
* `salt` is a random number with at least 8 bytes of randomness. It must be unique to each request
* `address` is the payment address for payments, the refund address for refunds
* `lowercase()` transforms all characters to lowercase
* `hash()` is a keccak256 hash function
* `last8Bytes()` take the last 8 bytes

Use the [paymentreferencecalculator.md](../../sdk-api-reference/request-client.js/paymentreferencecalculator.md "mention") to calculate the payment reference.
