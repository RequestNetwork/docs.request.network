---
description: >-
  Pay requests using stablecoins from any supported network, without manual
  bridging or token swaps.
---

# Crosschain Payments

{% hint style="info" %}
**Talk to an expert**\
Discover how Request Network API can enhance your app's features - [book a call](https://calendly.com/mariana-rn/request-network-demo-docs) with us.
{% endhint %}

{% hint style="warning" %}
**Warning:** Crosschain payments are currently only available in our staging environment at [https://api.stage.request.network](https://api.stage.request.network) and are not ready for production use. Use with caution.
{% endhint %}

Crosschain payments allow users to pay a request using a stablecoin from a different blockchain network than the one specified on the request. For example, a payer can pay an request for USDC on Base using USDT from their Polygon wallet.

## Benefits

* **Flexibility:** Payers can pay with their preferred currency.
* **Cost-Effective:** Automated routing balances cost and speed.
* **Time-Saving:** Payers don't need to swap or bridge tokens manually.
* **Simplified UX:** Payment settlement requires only 1 or 2 signatures from the Payer.

## Supported Networks

Crosschain payments are supported on the following blockchain networks:

* Polygon
* Base
* Optimism
* Arbitrum
* Ethereum

## Supported Stablecoins

{% hint style="warning" %}
**Warning:**  Crosschain payments work only with mainnet funds (real money). Test networks are not supported.
{% endhint %}

The following stablecoins are supported for crosschain payments on both the sending and receiving networks.

* USDC
* USDT
* DAI

## How it works

### 1. Request creation

To enable crosschain payments, the request must be created with the following parameters:

* `paymentCurrency`  included in the [#supported-stablecoins](crosschain-payments.md#supported-stablecoins "mention") and [#supported-networks](crosschain-payments.md#supported-networks "mention").&#x20;
* `amount` greater than 2 - _executing_ crosschain payments under 2 stablecoins is not allowed, even though _creating_ requests has no restrictions on `amount` .

&#x20;even though _creating_ requests for under 2 stablecoins is a

For more details about creating requests, please see the [#v1-request](create-and-pay-requests.md#v1-request "mention") endpoint.

### 2. Payment route fetching

To display a list of possible routes for a given request and payer address, use the [#v1-request-paymentreference-routes](crosschain-payments.md#v1-request-paymentreference-routes "mention") endpoint. It returns all of the possible routes based on the payer's token balances.&#x20;

#### Route Ranking

The API automatically ranks available payment routes based on the following factors:

* Transaction fees
* Processing speed

Routes that offer a _balanced_ combination of lower fees and faster processing times are ranked higher in the results.

#### Samechain routes

The API may return samechain routes if the payer address has supported currencies on the same chain as the `paymentCurrency` .

* Example: `paymentCurrency` is USDC on Polygon, and the payer has USDT on Polygon
* Gassless transactions - the transaction fees are added on top of the request amount
* No native token (ETH, POL, etc..) needed for gas

{% openapi src="https://api.stage.request.network/open-api/openapi.json" path="/v1/request/{paymentReference}/routes" method="get" %}
[https://api.stage.request.network/open-api/openapi.json](https://api.stage.request.network/open-api/openapi.json)
{% endopenapi %}

### 3. Getting payment calldata

Once the route is selected, the payer needs to fetch the unsigned payment calldata or intents.\
\
If the selected route is a crosschain payment, the [#v1-request-paymentreference-pay](crosschain-payments.md#v1-request-paymentreference-pay "mention") endpoint returns an unsigned payment intent. It will also return an unsigned approval permit or unsigned approval calldata, depending on whether the `paymentCurrency` supports [EIP-2612 Permit](https://eips.ethereum.org/EIPS/eip-2612). For crosschain payments, this endpoint is NOT approval aware - it will return an approval permit or approval calldata even if approval has already been granted.

If the selected route is a direct payment, the [#v1-request-paymentreference-pay](crosschain-payments.md#v1-request-paymentreference-pay "mention") returns an unsigned payment calldata. It may also return an approval calldata. For direct payments, this endpoint IS approval aware - it will omit the approval calldata if sufficient approval has already been granted.

{% openapi src="https://api.stage.request.network/open-api/openapi.json" path="/v1/request/{paymentReference}/pay" method="get" %}
[https://api.stage.request.network/open-api/openapi.json](https://api.stage.request.network/open-api/openapi.json)
{% endopenapi %}

### 4. Signing the payment intent

The intents and calldata returned by the [#v1-request-paymentreference-pay](crosschain-payments.md#v1-request-paymentreference-pay "mention") endpoint in the previous step must be signed by the payer's wallet to authorize the crosschain payment. The process for signing the approval varies depending on whether the `paymentCurrency` supports [EIP-2612 Permit](https://eips.ethereum.org/EIPS/eip-2612), indicated by the `metadata` response parameter.

```json
    "metadata": {
        "supportsEIP2612": true
    }
```

If the token does not support EIP-2612 Permit, the payer must sign and submit a standard ERC20 approval transaction.

{% tabs %}
{% tab title="Ethers V5" %}
```typescript
import { ethers } from "ethers";

const ethersProvider = new ethers.providers.Web3Provider(
  // Connected wallet provider
  walletProvider as ethers.providers.ExternalProvider,
);
const signer = await ethersProvider.getSigner();

// Response from the `GET /request/{paymentReference}/pay` endpoint
const response = ...

const paymentIntent = JSON.parse(paymentData.paymentIntent);
const supportsEIP2612 = paymentData.metadata.supportsEIP2612;
let approvalSignature = undefined;
let approval = undefined;

if (supportsEIP2612) {
  approval = JSON.parse(paymentData.approvalPermitPayload);

  approvalSignature = await signer._signTypedData(
    approval.domain,
    approval.types,
    approval.values,
  );
} else {
  const tx = await signer.sendTransaction(paymentData.approvalCalldata);
  await tx.wait();
}

const paymentIntentSignature = await signer._signTypedData(
  paymentIntent.domain,
  paymentIntent.types,
  paymentIntent.values,
);

const signedData = {
  signedPaymentIntent: {
    signature: paymentIntentSignature,
    nonce: paymentIntent.values.nonce.toString(),
    deadline: paymentIntent.values.deadline.toString(),
  },
  signedApprovalPermit: approvalSignature
    ? {
      signature: approvalSignature,
      nonce: approval.values.nonce.toString(),
      deadline: approval?.values?.deadline
        ? approval.values.deadline.toString()
        : approval.values.expiry.toString(),
    }
    : undefined,
};
```
{% endtab %}
{% endtabs %}

### 5. Sending the signed data

Finally, the signed payment intent (and possibly the signed approval permit) are sent back to execute the crosschain payment via the [#v1-request-paymentintentid-send](crosschain-payments.md#v1-request-paymentintentid-send "mention") endpoint. It will handle all the necessary steps to complete the payment. A `payment.complete` event will be sent to the platform's webhooks when the payment is completed.

{% openapi src="https://api.stage.request.network/open-api/openapi.json" path="/v1/request/{paymentIntentId}/send" method="post" %}
[https://api.stage.request.network/open-api/openapi.json](https://api.stage.request.network/open-api/openapi.json)
{% endopenapi %}

## Custom fee configuration

It will be possible in the future to add a custom fee to the payment, this is currently under development.
