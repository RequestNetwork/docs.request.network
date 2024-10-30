# Single Request Proxy

{% hint style="danger" %}
The Single Request Proxy is currently still in Beta and only deployed on [Sepolia](https://sepolia.etherscan.io/address/0x435E81E12136414e2c09cAFe05E902E23bD46030#code)
{% endhint %}

## Overview

The Single Request Proxy is a smart contract solution that enables integration with Request Network's payment system without modifying existing smart contracts.

<figure><img src="../../.gitbook/assets/Single Request Proxy@2x (1).png" alt=""><figcaption><p>Single Request Proxy Payment Flow</p></figcaption></figure>

## Key Benefits:

* **Universal Compatibility:** Works with any system that can make standard crypto transfers.
* **No Code Changes**: Integrate with Request Network without modifying existing smart contracts.
* **Exchange Friendly:** Enable payments from centralized exchanges.

## How it works:

1. **Request**: Create a request in the Request Network protocol
2. **Deploy:** Deploy a unique Single Request Proxy for your request
3. **Pay:** The Payer sends funds to the Single Request Proxy
4. **Complete:** The Single Request Proxy forwards the payment to the Payee and emits an event to enable payment detection.

## Integration Guide

### Create a request

{% hint style="info" %}
For a complete guide on request creation, see [#create-a-request](../../get-started/quickstart-browser.md#create-a-request "mention")
{% endhint %}

```typescript
const request = await requestClient.createRequest(requestCreateParameters);

const requestData = request.getData() 

// In case of in-memory request
 const requestData = request.inMemoryInfo.requestData
```

### Deploy Single Request Proxy

To deploy a Single Request Proxy, call `deploySingleRequestProxy()`  which takes in the following arguments:

* **`requestData`:** the data of the created request
* **`signer`:** An Ethers v5 Signer to sign the deployment transaction

The `deploySingleRequestProxy()` function automatically deploys the correct type of Single Request Proxy based on the Request data passed into the function; either an `EthereumSingleRequestProxy` or `ERC20SingleRequestProxy`

It returns

* **Single Request Proxy Address**

```typescript
import { deploySingleRequestProxy } from "@requestnetwork/payment-processor"

 const proxyAddress = await deploySingleRequestProxy(
      requestData,
      signer
    );

console.log(`Single Request Proxy Deployed At: ${proxyAddress}`)
// Single Request Proxy Deployed At : 0x1234567890123456789012345678901234567890
```

### Pay through a Single Request Proxy

#### Pay through a Single Request Proxy using the RN SDK

To pay a request through a Single Request Proxy using the Request Network SDK, call `payRequestWithSingleRequestProxy()` which takes in the following arguments:

* **`singleRequestProxyAddress`:** the address of the SRP deployed in the previous step.
* **`signer`:** A wallet signer who is making the transfer of funds.
* **`amount`:** Amount of funds that need to be transferred**.**

```typescript
import { payWithSingleRequestProxy } from "@requestnetwork/payment-processor"
import { utils } from "ethers"
const paymentAmount = utils.parseUnits("1" , 18)
await payRequestWithSingleRequestProxy(proxyAddress , signer, paymentAmount)
```

#### Pay through a Single Request Proxy using a Direct Transfer

Once we have the **Single Request Proxy** address, we can pay by directly transferring the money to the address itself. The Single Request Proxy will automatically process the payment.\
\
For ERC20 payments, the process of paying with a **Single Request Proxy** happens in two steps:

* Transferring the tokens to the **Single Request Proxy**
* Make a zero-value transaction to the **Single Request Proxy** (i.e. Send 0 ETH to the contract)

## Design Features:

* **Single Use:** Each **Single Request Proxy** deployment processes payments for a specific request.
* **Immutable Parameters:** Payment details cannot be modified after deployment.
* **Fund Recovery:** Built-in mechanisms to send stuck funds to the payment receiver.



























&#x20;



