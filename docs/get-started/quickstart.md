# Quickstart

This page will introduce the four primary operations provided by Request Network’s browser-side JavaScript library, `request-client.js`.

{% hint style="info" %}
You will learn:

* How to retrieve a user’s requests
* How to create a request
* How to pay a request
* How to detect a payment
{% endhint %}

## Typical Lifecycle of a Request

Note: This is for a "Reference-based" Request, not "Address-based" or "Declarative" requests.

* Create a request
  * App specifies the payee, payer, currency, and amount (and some other stuff).
  * Store request contents to IPFS.
  * Store IPFS Content-addressable ID (CID) on Gnosis chain.
* Pay a request
  * Derive paymentReference from the request contents.
  * Call the payment network smart contract, passing in token address, to address, amount, and paymentReference.
  * Event is emitted containing token address, to address, amount, and paymentReference.
* Detect payment
  * Event is indexed by a Subgraph.
  * App queries request content from IPFS and payment events from Subgraph.

<div data-full-width="true">

<figure><img src="../.gitbook/assets/Lifecycle of a Request.png" alt=""><figcaption><p>Lifecycle of a Request</p></figcaption></figure>

</div>

## Retrieve a user's requests

First, construct a `RequestNetwork` object and connect it to the Goerli Request Node Gateway:

<pre class="language-typescript"><code class="lang-typescript">import { RequestNetwork, Types } from "@requestnetwork/request-client.js";
<strong>const requestClient = new RequestNetwork({
</strong>  nodeConnectionConfig: { 
    baseURL: "https://goerli.gateway.request.network/",
  }
})
</code></pre>

Then, call `fromIdentity()` to get an array of `Request` objects. This function retrieves the `Request`s stored in IPFS and queries on-chain events to determine the balances paid so far.

```typescript
const identity = "0x7eB023BFbAeE228de6DC5B92D0BeEB1eDb1Fd567";
const requests = await requestClient.fromIdentity({
  type: Types.Identity.TYPE.ETHEREUM_ADDRESS,
  value: identity
})
```

Finally, call `getData()` on each `Request` to get the request contents.

```typescript
const requestDatas = requests.map((request) => request.getData())
```

Altogether it looks like this:

{% embed url="https://codesandbox.io/p/sandbox/retrieve-a-users-requests-mqrjqy?embed=1" %}
[https://codesandbox.io/p/sandbox/retrieve-a-users-requests-mqrjqy?embed=1](https://codesandbox.io/p/sandbox/retrieve-a-users-requests-mqrjqy?embed=1)
{% endembed %}

## Create a request

To create an unencrypted ERC-20 request, first construct a `RequestNetwork` object:

* Connect it to the Goerli Request Node Gateway
* Pass in a `Web3SignatureProvider` instance connected to the web3 provider

{% tabs %}
{% tab title="ethers Provider" %}
```typescript
import { RequestNetwork } from "@requestnetwork/request-client.js";
import { Web3SignatureProvider } from "@requestnetwork/web3-signature";
import { BrowserProvider, JsonRpcProvider, parseUnits} from "ethers";

let provider;
if (RPC_URL === undefined) {
  provider = new ethers.BrowserProvider(window.ethereum) // Metamask
} else {
  provider = new ethers.JsonRpcProvider(RPC_URL) // Alchemy, Infura, etc.
}

const web3SignatureProvider = Web3SignatureProvider(provider);
const requestClient = new RequestNetwork({
  nodeConnectionConfig: { 
    baseURL: "https://goerli.gateway.request.network/",
  },
  web3SignatureProvider,
});
```
{% endtab %}

{% tab title="wagmi/viem WalletClient" %}
```typescript
import { RequestNetwork } from "@requestnetwork/request-client.js";
import { Web3SignatureProvider } from "@requestnetwork/web3-signature";
import { useWalletClient } from "wagmi";

const { data: walletClient } = useWalletClient();
const web3SignatureProvider = Web3SignatureProvider(walletClient);
const requestClient = new RequestNetwork({
  nodeConnectionConfig: { 
    baseURL: "https://goerli.gateway.request.network/",
  },
  web3SignatureProvider,
});
```
{% endtab %}
{% endtabs %}

Prepare the Request creation parameters:

```typescript
import { Types, Utils } from "@requestnetwork/request-client.js";

const payeeIdentity = '0x7eB023BFbAeE228de6DC5B92D0BeEB1eDb1Fd567';
const payerIdentity = '0x519145B771a6e450461af89980e5C17Ff6Fd8A92';
const paymentRecipient = payeeIdentity;
const feeRecipient = '0x0000000000000000000000000000000000000000';

const requestCreateParameters: Types.ICreateRequestParameters = {
  requestInfo: {
    
    // The currency in which the request is denominated
    currency: {
      type: Types.RequestLogic.CURRENCY.ERC20,
      value: '0xBA62BCfcAaFc6622853cca2BE6Ac7d845BC0f2Dc',
      network: 'goerli',
    },
    
    // The expected amount in parsed units, respecting `decimals`
    expectedAmount: 1234000000000000000000,
    
    // The payee identity. Not necessarily the same as the payment recipient.
    payee: {
      type: Types.Identity.TYPE.ETHEREUM_ADDRESS,
      value: payeeIdentity,
    },
    
    // The payer identity. If omitted, any identity can pay the request.
    payer = {
      type: Types.Identity.TYPE.ETHEREUM_ADDRESS,
      value: payerIdentity,
    };
    // The request creation timestamp.
    timestamp: Utils.getCurrentTimestampInSecond(),
  },
  
  // The paymentNetwork is the method of payment and related details.
  paymentNetwork: {
    id: Types.Extension.PAYMENT_NETWORK_ID.ERC20_FEE_PROXY_CONTRACT,
    parameters: {
      paymentNetworkName: 'goerli',
      paymentAddress: payeeIdentity,
      feeAddress: feeRecipient,  
      feeAmount: '0',
    },
  },
  
  // The contentData can contain anything.
  // Consider using rnf_invoice format from @requestnetwork/data-format
  contentData: {
    reason: '🍕',
    dueDate: '2023.06.16',
  },
  
  // The identity that signs the request, either payee or payer identity.
  signer: {
    type: Types.Identity.TYPE.ETHEREUM_ADDRESS,
    value: payeeIdentity,
  },
};
```

Then, call `createRequest()` to create the request and `waitForConfirmation()` to wait until the request is persisted in IPFS and the CID hash is stored on-chain.

```typescript
const request = await requestClient.createRequest(requestCreateParameters);
const confirmedRequestData = await request.waitForConfirmation();
```

Altogether it looks like this:

{% embed url="https://codesandbox.io/p/sandbox/create-a-request-shffng?embed=1" %}
[https://codesandbox.io/p/sandbox/create-a-request-shffng?embed=1](https://codesandbox.io/p/sandbox/create-a-request-shffng?embed=1)
{% endembed %}



Other operation quickstarts coming soon! In the meantime check out the following guides:

<table data-card-size="large" data-column-title-hidden data-view="cards"><thead><tr><th></th><th></th><th data-hidden></th><th data-hidden data-card-target data-type="content-ref"></th><th data-hidden data-card-cover data-type="files"></th></tr></thead><tbody><tr><td></td><td>Retrieve a request</td><td></td><td><a href="../learn-request-network/guides/retrieve-a-request.md">retrieve-a-request.md</a></td><td></td></tr><tr><td></td><td>Create a request</td><td></td><td><a href="../learn-request-network/guides/creating-an-erc20-request.md">creating-an-erc20-request.md</a></td><td></td></tr><tr><td></td><td>Pay a request</td><td></td><td><a href="../learn-request-network/guides/pay-a-request.md">pay-a-request.md</a></td><td></td></tr><tr><td></td><td>Detect a payment</td><td></td><td><a href="../learn-request-network/guides/detect-a-payment.md">detect-a-payment.md</a></td><td></td></tr></tbody></table>
