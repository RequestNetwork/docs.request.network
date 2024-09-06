# Payment Reference

In the Reference-based Payment Networks, Payments are linked to requests via a `paymentReference` which is derived from the `requestId` which is derived from the request contents.

The `paymentReference` consists of the last 8 bytes of a salted hash of the `requestId`: `last8Bytes(hash(lowercase(requestId + salt + info)))`:

* `requestId` is the id of the request
* `salt` is a random number with at least 8 bytes of randomness. It must be unique to each request
* `info` is the JSON-stringified string of the `paymentInfo` for a payment, or `refundInfo` for a refund.
* `lowercase()` transforms all characters to lowercase
* `hash()` is a keccak256 hash function
* `last8Bytes()` take the last 8 bytes

Use the [paymentreferencecalculator.md](../sdk-api-reference/request-client.js/paymentreferencecalculator.md "mention") to calculate the payment reference.
