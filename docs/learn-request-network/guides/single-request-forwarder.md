# Single Request Forwarder

## Overview

The Single Request Forwarder is a smart contract solution that enables integration with Request Network's payment system without modifying existing smart contracts.

<figure><img src="../../.gitbook/assets/image (4).png" alt=""><figcaption><p>Single Request Forwarder Payment Flow</p></figcaption></figure>

{% hint style="info" %}
The Single Request Forwarder Factory contact addresses can be found : [smart-contract-addresses.md](../../get-started/smart-contract-addresses.md "mention")

The contract name is `SingleRequestProxyFactory`
{% endhint %}

## Key Benefits:

* **Universal Compatibility:** Works with any system that can make standard crypto transfers.
* **No Code Changes**: Integrate with Request Network without modifying existing smart contracts.
* **Exchange Friendly:** Enable payments from centralized exchanges.

## How it works:

1. **Request**: Create a request in the Request Network protocol
2. **Deploy:** Deploy a unique Single Request Forwarder for your request
3. **Pay:** The Payer sends funds to the Single Request Forwarder
4. **Complete:** The Single Request Forwarder forwards the payment to the Payee and emits an event to enable payment detection.

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

### Deploy Single Request Forwarder

To deploy a Single Request Forwarder, call `deploySingleRequestForwarder()`  which takes in the following arguments:

* **`requestData`:** the data of the created request
* **`signer`:** An Ethers v5 Signer to sign the deployment transaction

The `deploySingleRequestForwarder()` function automatically deploys the correct type of Single Request **Forwarder** based on the Request data passed into the function; either an Ethereum Single Request Forwarder or ERC20 Single Request Forwarder

It returns

* **Single Request Forwarder Address**

```typescript
import { deploySingleRequestForwarder } from "@requestnetwork/payment-processor"

 const forwarderAddress = await deploySingleRequestForwarder(
      requestData,
      signer
    );

console.log(`Single Request Forwarder Deployed At: ${forwarderAddress}`)
// Single Request Forwarder Deployed At : 0x1234567890123456789012345678901234567890
```

### Pay through a Single Request **Forwarder**

#### Pay through a Single Request **Forwarder** using the RN SDK

To pay a request through a Single Request **Forwarder** using the Request Network SDK, call `payRequestWithSingleRequestForwarder()` which takes in the following arguments:

* **`singleRequestForwarderAddress`:** the address of the SRP deployed in the previous step.
* **`signer`:** A wallet signer who is making the transfer of funds.
* **`amount`:** Amount of funds that need to be transferred**.**

```typescript
import { payRequestWithSingleRequestForwarder } from "@requestnetwork/payment-processor"
import { utils } from "ethers"
const paymentAmount = utils.parseUnits("1" , 18)
await payRequestWithSingleRequestForwarder(forwarderAddress , signer, paymentAmount)
```

#### Pay through a Single Request **Forwarder** using a Direct Transfer

Once we have the **Single Request Forwarder** address, we can pay by directly transferring the money to the address itself. The Single Request Forwarder will automatically process the payment.\
\
For ERC20 payments, the process of paying with a **Single Request Forwarder** happens in two steps:

* Transferring the tokens to the **Single Request Forwarder**
* Make a zero-value transaction to the **Single Request Forwarder** (i.e. Send 0 ETH to the contract)

## Design Features:

* **Single Use:** Each **Single Request Forwarder** deployment processes payments for a specific request.
* **Immutable Parameters:** Payment details cannot be modified after deployment.
* **Fund Recovery:** Built-in mechanisms to send stuck funds to the payment receiver.
