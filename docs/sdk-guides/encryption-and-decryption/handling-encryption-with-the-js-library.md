# Encrypt with an Ethereum private key

{% hint style="warning" %}
Manipulating private keys must be done with care. Losing them can lead to a loss of data, privacy or non-repudiation safety!
{% endhint %}

{% hint style="info" %}
For an introduction to Encryption and Decryption in Request Network, see [private-requests-using-encryption.md](../../get-started/protocol-overview/private-requests-using-encryption.md "mention")
{% endhint %}

A request can be encrypted to make its details private to selected stakeholders. In this guide, we won't explain how encryption is managed under the hood. We will mention encryption or decryption of requests with payers and payees keys. Although in practice, we will use an intermediate symmetric key. See more details on [github](https://github.com/RequestNetwork/requestNetwork/blob/master/packages/transaction-manager/specs/encryption.md).

The transaction layer manages the encryption, [see more details on the Request Protocol section](../../learn-request-network/introduction-to-the-request-protocol/transaction.md).

To manipulate encrypted requests you need a CipherProvider (recommended) or DecryptionProvider (deprecated). Both of them require direct access to the private key. They're best suited for backends.

* **EthereumPrivateKeyCipherProvider:** Provides both encryption and decryption utilities.
* **EthereumPrivateKeyDecryptionProvider** (deprecated) provides only decryption utilities.&#x20;

### Create an encrypted request

#### EthereumPrivateKeyCipherProvider&#x20;

See on [Github](https://github.com/RequestNetwork/requestNetwork/tree/master/packages/epk-cipher).

```typescript
import { EthereumPrivateKeyCipherProvider } from '@requestnetwork/epk-cipher';

const cipherProvider = new EthereumPrivateKeyCipherProvider({
  # Warning: private keys should never be stored in clear, this is a basic tutorial
  key: '0x4025da5692759add08f98f4b056c41c71916a671cedc7584a80d73adc7fb43c0',
  method: RequestNetwork.Types.Encryption.METHOD.ECIES,
});

const requestNetwork = new RequestNetwork({
  cipherProvider,
  signatureProvider,
  useMockStorage: true,
});
```

Then you can create an encrypted request:

```typescript
const payeeEncryptionPublicKey = {
  key: 'cf4a1d0bbef8bf0e3fa479a9def565af1b22ea6266294061bfb430701b54a83699e3d47bf52e9f0224dcc29a02721810f1f624f1f70ea3cc5f1fb752cfed379d',
  method: RequestNetwork.Types.Encryption.METHOD.ECIES,
};
const payerEncryptionPublicKey = {
  key: '299708c07399c9b28e9870c4e643742f65c94683f35d1b3fc05d0478344ee0cc5a6a5e23f78b5ff8c93a04254232b32350c8672d2873677060d5095184dad422',
  method: RequestNetwork.Types.Encryption.METHOD.ECIES,
};

const invoice = await requestNetwork._createEncryptedRequest(
  {
    requestParameters,
    signer: requestParameters.payee,
    paymentNetwork,
  },
  [payeeEncryptionPublicKey, payerEncryptionPublicKey],
);
```

Note: You must give at least one encryption key you can decrypt with the decryption provider. Otherwise, an error will be triggered after the creation.

#### EthereumPrivateKeyDecryptionProvider

{% hint style="warning" %}
[EthereumPrivateKeyDecryptionProvider](https://github.com/RequestNetwork/requestNetwork/tree/master/packages/epk-decryption) is deprecated in favor of [#ethereumprivatekeycipherprovider](handling-encryption-with-the-js-library.md#ethereumprivatekeycipherprovider "mention")
{% endhint %}

### Get invoice information from its request ID

Let's step back for a second: the requester sent a request that he encrypted with the payer's public key, as well as with his own, to retrieve it later. This is an essential and typical example, but a request can be encrypted with many keys to give access to its status and details.

If the decryption provider knows a private key matching one of the keys used at the creation, it can decrypt it. Like a clear request you can get it from its request id.

```typescript
const invoiceFromRequestID = await requestNetwork.fromRequestId(requestId);

const requestData = invoiceFromRequestID.getData();

console.log(requestData);

/* { 
 requestId,
 currency,
 expectedAmount,
 payee,
 payer,
 timestamp,
 extensions,
 version,
 events,
 state,
 creator,
 meta,
 balance,
 contentData,
} */
```

### Accepting/canceling an invoice information

Like a clear request, you can update it if the decryption provider is instantiated with a matching private key.

```typescript
//Accept
await request.accept(payerIdentity);

//Cancel
await request.cancel(payeeIdentity);

//Increase the expected amount
await request.decreaseExpectedAmountRequest(amount, payeeIdentity);

//Decrease the expected amount
await request.increaseExpectedAmountRequest(amount, payerIdentity);
```

### Enabling/Disabling Decryption

```typescript
// Disable decryption
cipherProvider.enableDecryption(false);
// Check if decryption is enabled
const isEnabled = cipherProvider.isDecryptionEnabled();
// Re-enable decryption
cipherProvider.enableDecryption(true);
```

### Checking Capabilities

```typescript
// Check if encryption is available
const canEncrypt = cipherProvider.isEncryptionAvailable();
// Check if decryption is available
const canDecrypt = cipherProvider.isDecryptionAvailable();
// Check if an identity is registered
const isRegistered = await cipherProvider.isIdentityRegistered({
type: 'ethereum_address',
value: '0x123...'
});// Some code
```

