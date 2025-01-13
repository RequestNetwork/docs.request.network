# Hinkal Private Payments

The Request Network SDK supports Hinkal Private Payments using ERC-20 tokens.  Hinkal is a middleware and suite of smart contracts on EVM-compatible chains that leverage zero-knowledge proofs and shielded addresses to facilitate compliant and private transactions.&#x20;

Each public address has exactly one Hinkal shielded address.

The `@requestnetwork/payment-processor` package provides functions to:

* **Pay a request from a Hinkal shielded address to a public address:** such that the payment sender's public address never appears on-chain.
* **Deposit to a Hinkal shielded address from a public address**: such that the payment recipient's public address never appears on-chain. Callers can choose to deposit to their own shielded address or someone else's shielded address.

{% hint style="info" %}
Paying a request where the payment recipient address is a Hinkal shielded address is not supported because the Request Network payment proxy smart contracts can only send funds to public addresses. Consider using [declarative-request.md](declarative-request.md "mention") instead.
{% endhint %}

## Benefits

* **Privacy**: Obfuscates payer address when paying a request.
* **Compliance**: Ensures transactions adhere to regulatory requirements. See [Hinkal Compliance](https://hinkal-team.gitbook.io/hinkal/hinkal-wallet/compliance) for details

## Installation

To use the Hinkal Private Payments feature, install the necessary package:

```bash
npm install @requestnetwork/payment-processor
npm install @requestnetwork/request-client.js
```

## Usage

### **Pay a request from a Hinkal shielded address**

To pay a request from a Hinkal shielded address to a public address, where only the payment sender's address is obfuscated, use the \``` payErc20FeeProxyRequestFromHinkalShieldedAddress()` `` function. Ensure the payment sender's Hinkal shielded address has a positive balance.

{% hint style="warning" %}
Strongly consider using [encryption-and-decryption](../encryption-and-decryption/ "mention")to keep the request contents private, including the payer and payee identity addresses, when paying requests from a Hinkal shielded address!
{% endhint %}

```typescript
import { payErc20FeeProxyRequestFromHinkalShieldedAddress } from '@requestnetwork/payment-processor';

// Instantiation of `RequestNetwork` and `Signer` omitted for brevity

const request = await requestClient.fromRequestId('insert request id');
const requestData = request.getData();

const paymentResult = await payErc20FeeProxyRequestFromHinkalShieldedAddress(requestData, signer);
```

{% hint style="info" %}
See [quickstart-browser.md](../../get-started/quickstart-browser.md "mention") for how to instantiate a `RequestNetwork` and `Signer`
{% endhint %}

### Deposit to a Hinkal shielded address

To deposit funds to a Hinkal shielded address from a public address, where only the payment recipient's address is obfuscated, use the `sendToHinkalShieldedAddressFromPublic()` function. &#x20;

* Deposit to own Hinkal shielded address: omit the `recipientInfo`argument
* Deposit to someone else's Hinkal shielded address: set `recipientInfo` to the shielded address of the payment recipient.

{% hint style="info" %}
Hinkal shielded addresses must be shared out-of-band. This SDK doesn't offer functions for sharing Hinkal shielded addresses.
{% endhint %}

{% code overflow="wrap" %}
```typescript
import { sendToHinkalShieldedAddressFromPublic } from '@requestnetwork/payment-processor';

// Instantiation of `Signer` omitted for brevity

const recipientShieldedAddress = '142590100039484718476239190022599206250779986428210948946438848754146776167,0x096d6d5d8b2292aa52e57123a58fc4d5f3d66171acd895f22ce1a5b16ac51b9e,0xc025ccc6ef46399da52763a866a3a10d2eade509af27eb8411c5d251eb8cd34d'
await sendToHinkalShieldedAddressFromPublic({
    signerOrProvider: paymentSender,
    tokenAddress: '0x833589fcd6edb6e08f4c7c32d4f71b54bda02913', // USDC on Base
    amount: '1000000', // 1 USDC
    recipientInfo: recipientShieldedAddress, // omit to deposit to own Hinkal shielded address
})
```
{% endcode %}

{% hint style="info" %}
See [quickstart-browser.md](../../get-started/quickstart-browser.md "mention") for how to instantiate a `Signer`
{% endhint %}

## Details

For more details, refer to the [Pull Request #1482](https://github.com/RequestNetwork/requestNetwork/pull/1482) on GitHub.
