# Quickstart - Node.js

This page will introduce the primary operations provided by Request Network’s SDK while using the `EthereumPrivateKeySignatureProvider` to sign requests with a private key that is managed outside of a wallet.

This approach works well for Node.js environments _without_ access to a Web3 wallet.

{% hint style="info" %}
You will learn:

* How to create a request
* How to update a request (coming soon...)
* How to pay a request
* How to detect a payment
* How to retrieve a user’s requests
{% endhint %}

## Repository

All of the following examples can be found in this repository [https://github.com/RequestNetwork/quickstart-node.js](https://github.com/RequestNetwork/quickstart-node.js)

## Create a request

To create an unencrypted ERC-20 request, first construct an `EthereumPrivateKeySignatureProvider` with a private key.

```javascript
const {
  EthereumPrivateKeySignatureProvider,
} = require("@requestnetwork/epk-signature");
const { Types } = require("@requestnetwork/request-client.js");

const epkSignatureProvider = new EthereumPrivateKeySignatureProvider({
  method: Types.Signature.METHOD.ECDSA,
  privateKey: process.env.PAYEE_PRIVATE_KEY, // Must include 0x prefix
});
```

Then, first construct a `RequestNetwork`, passing in the:

* Request Node URL. In this example, we use the Sepolia Request Node Gateway.
* `EthereumPrivateKeySignatureProvider` constructed in the previous step.

```javascript
const { RequestNetwork } = require("@requestnetwork/request-client.js")

const requestClient = new RequestNetwork({
  nodeConnectionConfig: { 
    baseURL: "https://sepolia.gateway.request.network/",
  },
  signatureProvider: epkSignatureProvider,
});
```

Prepare the Request creation parameters:

```javascript
const { Types, Utils } = require("@requestnetwork/request-client.js");

const payeeIdentity = '0x7eB023BFbAeE228de6DC5B92D0BeEB1eDb1Fd567';
const payerIdentity = '0x519145B771a6e450461af89980e5C17Ff6Fd8A92';
const paymentRecipient = payeeIdentity;
const feeRecipient = '0x0000000000000000000000000000000000000000';

const requestCreateParameters = {
  requestInfo: {
    
    // The currency in which the request is denominated
    currency: {
      type: Types.RequestLogic.CURRENCY.ERC20,
      value: '0x370DE27fdb7D1Ff1e1BaA7D11c5820a324Cf623C',
      network: 'sepolia',
    },
    
    // The expected amount as a string, in parsed units, respecting `decimals`
    // Consider using `parseUnits()` from ethers or viem
    expectedAmount: '1000000000000000000',
    
    // The payee identity. Not necessarily the same as the payment recipient.
    payee: {
      type: Types.Identity.TYPE.ETHEREUM_ADDRESS,
      value: payeeIdentity,
    },
    
    // The payer identity. If omitted, any identity can pay the request.
    payer: {
      type: Types.Identity.TYPE.ETHEREUM_ADDRESS,
      value: payerIdentity,
    },
    
    // The request creation timestamp.
    timestamp: Utils.getCurrentTimestampInSecond(),
  },
  
  // The paymentNetwork is the method of payment and related details.
  paymentNetwork: {
    id: Types.Extension.PAYMENT_NETWORK_ID.ERC20_FEE_PROXY_CONTRACT,
    parameters: {
      paymentNetworkName: 'sepolia',
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

```javascript
const request = await requestClient.createRequest(requestCreateParameters);
const confirmedRequestData = await request.waitForConfirmation();
```

Altogether it looks like this:

{% @github-files/github-code-block url="https://github.com/RequestNetwork/quickstart-node-js/blob/main/src/createRequest.js" %}

## Pay a request / Detect a payment

First, construct a `RequestNetwork` object and connect it to a Request Node. In this example, we use the Sepolia Request Node Gateway:

<pre class="language-javascript" data-full-width="false"><code class="lang-javascript">const { RequestNetwork, Types } = require("@requestnetwork/request-client.js");
<strong>
</strong><strong>const requestClient = new RequestNetwork({
</strong>  nodeConnectionConfig: { 
    baseURL: "https://sepolia.gateway.request.network/",
  }
});
</code></pre>

Then, retrieve the request and get the request data. Take note of the current request balance, to be used later for payment detection.

```javascript
const request = await requestClient.fromRequestId(
  '019830e9ec0439e53ec41fc627fd1d0293ec4bc61c2a647673ec5aaaa0e6338855',
);
const requestData = request.getData();
```

Then, construct an `ethers` v5 `Provider` and `Wallet` using a private key. These allow you to read and write to the chain, respectively.&#x20;

{% hint style="warning" %}
Unfortunately, the Request Network SDK does not yet support ethers v6.
{% endhint %}

{% tabs %}
{% tab title="ethers v5" %}
```javascript
const { providers, Wallet } = require("ethers");

const provider = new providers.JsonRpcProvider(
  process.env.JSON_RPC_PROVIDER_URL,
);
const payerWallet = new Wallet(
  process.env.PAYER_PRIVATE_KEY, // Must include 0x prefix
  provider,
);
```
{% endtab %}

{% tab title="viem" %}
Coming soon. Probably involves `publicClientToProvider()` and `walletClientToSigner()`.
{% endtab %}
{% endtabs %}

Then, check that the payer has sufficient funds using `hasSufficientFunds()`

<pre class="language-javascript"><code class="lang-javascript">const { hasSufficientFunds } = require("@requestnetwork/payment-processor);

const _hasSufficientFunds = await hasSufficientFunds(
  requestData,
  payerAddress,
  {
    provider: provider,
<strong>  },
</strong><strong>);
</strong></code></pre>

Then, in the case of an ERC-20 request, check that the payer has granted sufficient approval using `hasErc20Approval()`. If not, submit an approval transaction using `approveErc20`. Wait for an appropriate number of block confirmations. On Sepolia or Ethereum, 2 block confirmations should suffice. Other chains may require more.

```javascript
const { 
  approveErc20,
  hasErc20Approval,
} = require("@requestnetwork/payment-processor);

const _hasErc20Approval = await hasErc20Approval(
  requestData,
  payerAddress,
  provider
);
if (!_hasErc20Approval) {
  const approvalTx = await approveErc20(requestData, signer);
  await approvalTx.wait(2);
}
```

Finally, pay the request using `payRequest()`

<pre class="language-javascript"><code class="lang-javascript"><strong>const { payRequest } = require("@requestnetwork/payment-processor");
</strong><strong>
</strong><strong>const paymentTx = await payRequest(requestData, signer);
</strong><strong>await paymentTx.wait(2);
</strong></code></pre>

Detect that the payment was successful by polling the request and waiting until the request balance is greater than or equal to the expected amount.

```javascript
const request = await requestClient.fromRequestId(requestData.requestId);
let requestData = request.getData();

while (requestData.balance?.balance < requestData.expectedAmount) {
  requestData = await request.refresh();
  await new Promise((resolve) => setTimeout(resolve, 1000));
}
```

Altogether it looks like this:

{% @github-files/github-code-block url="https://github.com/RequestNetwork/quickstart-node-js/blob/main/src/payRequest.js" %}

## Retrieve a user's requests

First, construct a `RequestNetwork` object and connect it to a Request Node. In this example, we use the Sepolia Request Node Gateway:

<pre class="language-javascript"><code class="lang-javascript">const { RequestNetwork, Types } = require("@requestnetwork/request-client.js");
<strong>const requestClient = new RequestNetwork({
</strong>  nodeConnectionConfig: { 
    baseURL: "https://sepolia.gateway.request.network/",
  },
});
</code></pre>

Then, call `fromIdentity()` to get an array of `Request` objects or `fromRequestId()` to get a single `Request` object. This function retrieves the `Request`s stored in IPFS and queries on-chain events to determine the balances paid so far. Finally, call `getData()` on each `Request` to get the request contents.

{% tabs %}
{% tab title="fromIdentity()" %}
{% code fullWidth="true" %}
```javascript
const identityAddress = "0x519145B771a6e450461af89980e5C17Ff6Fd8A92";
const requests = await requestClient.fromIdentity({
  type: Types.Identity.TYPE.ETHEREUM_ADDRESS,
  value: identityAddress,
});
const requestDatas = requests.map((request) => request.getData());
```
{% endcode %}
{% endtab %}

{% tab title="fromRequestId()" %}
<pre class="language-javascript"><code class="lang-javascript"><strong>const request = await requestClient.fromRequestId(
</strong><strong>  '019830e9ec0439e53ec41fc627fd1d0293ec4bc61c2a647673ec5aaaa0e6338855',
</strong>);
const requestData = request.getData();
</code></pre>
{% endtab %}
{% endtabs %}

Altogether it looks like this:

{% @github-files/github-code-block url="https://github.com/RequestNetwork/quickstart-node-js/blob/main/src/retrieveRequest.js" %}
