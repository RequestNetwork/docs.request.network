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

Crosschain payments allow users to pay a request using a stablecoin from a different blockchain network than the one specified on the request. For example, a payer can pay a request for USDC on Base using USDT from their Optimism wallet.

## Benefits

* **Flexibility:** Payers can pay with their preferred currency.
* **Cost-Effective:** Automated routing balances cost and speed.
* **Time-Saving:** Payers don't need to swap or bridge tokens manually.
* **Simplified UX:** Payment settlement requires only 1 or 2 signatures from the Payer.

## Crosschain Payments Supported Chains and Currencies

For Crosschain (and Samechain) Payments, the Request Network API supports 12 stablecoins: USDC/USDT/DAI on 4 chains (Ethereum, Arbitrum One, Base, OP Mainnet).

### Crosschain Payments Supported Chains

Crosschain payments are supported on the following blockchain networks:

* Ethereum
* Arbitrum One
* Base
* OP Mainnet

### Crosschain Payments Supported Currencies

{% hint style="warning" %}
**Warning:** Crosschain payments work only with mainnet funds (real money). Test networks are not supported.
{% endhint %}

The following stablecoins are supported for crosschain payments on both the sending and receiving networks.

* USDC
* USDT
* DAI

## How it works

### 1. Request creation

To enable crosschain payments, the request must be created with the following parameters:

* `paymentCurrency` included in the [#supported-stablecoins](crosschain-payments.md#supported-stablecoins "mention") and [#supported-networks](crosschain-payments.md#supported-networks "mention").
* `amount` greater than 1 - _executing_ crosschain payments under 1 stablecoins is not allowed, even though _creating_ requests has no restrictions on `amount` .

For more details about creating requests, please see the [#post-v2-request](create-and-pay-requests.md#post-v2-request "mention") endpoint.

### 2. Payment route fetching

To display a list of possible routes for a given request and payer address, use the [#get-v2-request-requestid-routes](crosschain-payments.md#get-v2-request-requestid-routes "mention") endpoint. It returns all of the possible routes based on the payer's token balances.

#### Route Ranking

The API automatically ranks available payment routes based on the following factors:

* Transaction fees
* Processing speed

Routes that offer a _balanced_ combination of lower fees and faster processing times are ranked higher in the results.

#### Fee breakdown

When fetching payment routes, each route displays the total estimated fees in the payment currency. This fee represents the combined costs associated with processing the transactio&#x6E;**,** including:

1.  **Gas Fees:**

    The total fee includes all gas costs incurred by the payment processor wallet for processing the transaction. This covers:

    \- Transferring tokens from the payer's wallet.

    \- Approving the payment execution smart contract.

    \- Executing the crosschain payment transaction.

    **For tokens supporting EIP-2612:**

    \- The payment processor wallet also covers for the onchain permit transaction.

    **For tokens that do not support EIP-2612:**

    \- The payer must perform an onchain approval transaction and pay for the gas fee directly. This fee is **not** included in the total fee shown for the route.
2.  **Service Fees:**

    The total fees also include any service fees charged by the crosschain infrastructure for facilitating transfers or swaps between different blockchains.

#### Samechain routes

The API may return samechain routes if the payer address has supported currencies on the same chain as the `paymentCurrency` .

* Example: `paymentCurrency` is USDC on Base, and the payer has USDT on Base
* Gasless transactions - the transaction fees are added on top of the request amount
* No native token (ETH, etc..) needed for gas

{% openapi-operation spec="request-api" path="/v2/request/{requestId}/routes" method="get" %}
[OpenAPI request-api](https://api.request.network/open-api/openapi.json)
{% endopenapi-operation %}

### 3. Getting payment calldata

Once the route is selected, the payer needs to fetch the unsigned payment calldata or intents.\
\
If the selected route is a crosschain payment, the [#get-v2-request-requestid-pay](crosschain-payments.md#get-v2-request-requestid-pay "mention") endpoint returns an unsigned payment intent. It will also return an unsigned approval permit or unsigned approval calldata, depending on whether the `paymentCurrency` supports [EIP-2612 Permit](https://eips.ethereum.org/EIPS/eip-2612). For crosschain payments, this endpoint is NOT approval aware - it will return an approval permit or approval calldata even if approval has already been granted.

If the selected route is a direct payment, the [#get-v2-request-requestid-pay](crosschain-payments.md#get-v2-request-requestid-pay "mention") returns an unsigned payment calldata. It may also return an approval calldata. For direct payments, this endpoint IS approval aware - it will omit the approval calldata if sufficient approval has already been granted.

{% openapi-operation spec="request-api" path="/v2/request/{requestId}/pay" method="get" %}
[OpenAPI request-api](https://api.request.network/open-api/openapi.json)
{% endopenapi-operation %}

### 4. Signing the payment intent

The intents and calldata returned by the [#get-v2-request-requestid-pay](crosschain-payments.md#get-v2-request-requestid-pay "mention") endpoint in the previous step must be signed by the payer's wallet to authorize the crosschain payment. The process for signing the approval varies depending on whether the `paymentCurrency` supports [EIP-2612 Permit](https://eips.ethereum.org/EIPS/eip-2612), indicated by the `metadata` response parameter.

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

// Response from the `GET /request/{requestId}/pay` endpoint
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

Finally, the signed payment intent (and possibly the signed approval permit) are sent back to execute the crosschain payment via the [#post-v2-request-payment-intents-paymentintentid](crosschain-payments.md#post-v2-request-payment-intents-paymentintentid "mention") endpoint. It will handle all the necessary steps to complete the payment. A `payment.complete` event will be sent to the platform's webhooks when the payment is completed.

{% openapi-operation spec="request-api" path="/v2/request/payment-intents/{paymentIntentId}" method="post" %}
[OpenAPI request-api](https://api.request.network/open-api/openapi.json)
{% endopenapi-operation %}

## Custom fee configuration

It will be possible in the future to add a custom fee to the payment, this is currently under development.
