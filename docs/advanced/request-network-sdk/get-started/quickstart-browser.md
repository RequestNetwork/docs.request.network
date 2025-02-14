# Quickstart - Browser

This page will introduce the primary operations provided by Request Network’s SDK while using the `Web3SignatureProvider` to sign requests with a private key stored inside a wallet.

This approach works well for Browser environments _with_ access to a web3 wallet.

{% hint style="info" %}
You will learn:

* How to create a request
* How to update a request (coming soon...)
* How to pay a request
* How to detect a payment
* How to retrieve a user’s requests
{% endhint %}

## Create a request

To create an unencrypted ERC-20 request, first connect to an `ethers` v5 `Provider` and `Signer` or `wagmi` / `viem` `WalletClient.`

{% hint style="warning" %}
Unfortunately, the Request Network SDK does not yet support ethers v6.
{% endhint %}

{% tabs %}
{% tab title="ethers v5" %}
{% code fullWidth="false" %}
```typescript
import { providers } from "ethers";

let provider;
if (process.env.WEB3_PROVIDER_URL === undefined) {
  // Connect to Metamask and other injected wallets
  provider = new providers.Web3Provider(window.ethereum);
} else {
  // Connect to your own Ethereum node or 3rd party node provider
  provider = new providers.JsonRpcProvider(process.env.WEB3_PROVIDER_URL);
}
// getDefaultProvider() won't work because it doesn't include a Signer.
```
{% endcode %}
{% endtab %}

{% tab title="wagmi" %}
```typescript
import { useWalletClient } from "wagmi";

const { data: walletClient } = useWalletClient();
```
{% endtab %}

{% tab title="viem" %}
Very similar to wagmi, but without using hooks. Construct your own `WalletClient` object.
{% endtab %}
{% endtabs %}

Then, construct a `Web3SignatureProvider`, passing in the `ethers` `Provider` or `viem` `WalletClient.`

```typescript
import { Web3SignatureProvider } from "@requestnetwork/web3-signature";

const web3SignatureProvider = new Web3SignatureProvider(provider);
```

Then, construct a `RequestNetwork`, passing in the:

* Request Node URL. In this example, we use the Sepolia Request Node Gateway.
* `Web3SignatureProvider` constructed in the previous step.

```typescript
import { RequestNetwork } from "@requestnetwork/request-client.js"

const requestClient = new RequestNetwork({
  nodeConnectionConfig: { 
    baseURL: "https://sepolia.gateway.request.network/",
  },
  signatureProvider: web3SignatureProvider,
});
```

Then, prepare the Request creation parameters:

```typescript
import { Types, Utils } from "@requestnetwork/request-client.js";

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

Then, call `createRequest()` to prepare a `Request` object.

```typescript
const request = await requestClient.createRequest(requestCreateParameters);
```

Finally, call `request.waitForConfirmation()` to wait until:

* The request contents are persisted in IPFS
* The Content-addressable ID (CID) is stored on-chain
* The resulting on-chain event is indexed by the storage subgraph.

```typescript
const confirmedRequestData = await request.waitForConfirmation();
```

### CodeSandBox: Create a request

{% embed url="https://codesandbox.io/p/sandbox/create-a-request-shffng?file=/app/page.tsx:43,1" fullWidth="true" %}
[https://codesandbox.io/p/sandbox/create-a-request-shffng?file=/app/page.tsx:43,1](https://codesandbox.io/p/sandbox/create-a-request-shffng?file=/app/page.tsx:43,1)
{% endembed %}

## Pay a request

First, construct a `RequestNetwork` object and connect it to a Request Node. In this example, we use the Sepolia Request Node Gateway:

{% hint style="info" %}
Note that paying a request doesn't require a `SignatureProvider` be passed into the `RequestNetwork` object.
{% endhint %}

<pre class="language-typescript" data-full-width="false"><code class="lang-typescript">import { RequestNetwork, Types } from "@requestnetwork/request-client.js";

<strong>const requestClient = new RequestNetwork({
</strong>  nodeConnectionConfig: { 
    baseURL: "https://sepolia.gateway.request.network/",
  }
});
</code></pre>

Then, retrieve the request and get the request data. Take note of the current request balance, to be used later for payment detection.

```typescript
const request = await requestClient.fromRequestId(
  '019830e9ec0439e53ec41fc627fd1d0293ec4bc61c2a647673ec5aaaa0e6338855',
);
const requestData = request.getData();
```

Then, construct an `ethers` v5 `Provider` and `Signer`. These allow you to read and write to the chain, respectively.

{% hint style="warning" %}
Unfortunately, the Request Network SDK does not yet support ethers v6.
{% endhint %}

{% tabs %}
{% tab title="ethers v5" %}
```typescript
import { providers } from "ethers";

let provider;
if (process.env.WEB3_PROVIDER_URL === undefined) {
  // Connect to Metamask and other injected wallets
  provider = new providers.Web3Provider(window.ethereum);
} else {
  // Connect to your own Ethereum node or 3rd party node provider
  provider = new providers.JsonRpcProvider(process.env.WEB3_PROVIDER_URL);
}
// getDefaultProvider() won't work because it doesn't include a Signer.

const signer = await provider.getSigner();
```
{% endtab %}

{% tab title="wagmi" %}
{% code title="page.tsx" %}
```typescript
import { useEthersV5Provider } from './use-ethers-v5-provider';
import { useEthersV5Signer } from './use-ethers-v5-signer';

return Page() {
  const provider = useEthersV5Provider();
  const signer = useEthersV5Signer();
  ...
}
```
{% endcode %}

Ethers.js Adapters copied from [https://wagmi.sh/react/ethers-adapters](https://wagmi.sh/react/ethers-adapters)

{% code title="use-ethers-v5-provider.ts" %}
```typescript
import { useMemo } from "react";
import { providers } from "ethers";
import { type HttpTransport } from "viem";
import { type PublicClient, usePublicClient } from "wagmi";

export function publicClientToProvider(publicClient: PublicClient) {
  const { chain, transport } = publicClient;
  const network = {
    chainId: chain.id,
    name: chain.name,
    ensAddress: chain.contracts?.ensRegistry?.address,
  };
  if (transport.type === "fallback")
    return new providers.FallbackProvider(
      (transport.transports as ReturnType<HttpTransport>[]).map(
        ({ value }) => new providers.JsonRpcProvider(value?.url, network)
      )
    );

  return new providers.JsonRpcProvider(transport.url as string, network);
}

/** Hook to convert a viem Public Client to an ethers.js Provider. */
export function useEthersV5Provider({ chainId }: { chainId?: number } = {}) {
  const publicClient = usePublicClient({ chainId });
  return useMemo(() => publicClientToProvider(publicClient), [publicClient]);
}
```
{% endcode %}

{% code title="use-ethers-v5-signer.ts" %}
```typescript
import { useMemo } from "react";
import { providers } from "ethers";
import { type WalletClient, useWalletClient } from "wagmi";

export function walletClientToSigner(walletClient: WalletClient) {
  const { account, chain, transport } = walletClient;
  const network = {
    chainId: chain.id,
    name: chain.name,
    ensAddress: chain.contracts?.ensRegistry?.address,
  };
  const provider = new providers.Web3Provider(transport, network);
  const signer = provider.getSigner(account.address);
  return signer;
}

/** Hook to convert a viem Wallet Client to an ethers.js Signer. */
export function useEthersV5Signer({ chainId }: { chainId?: number } = {}) {
  const { data: walletClient } = useWalletClient({ chainId });
  return useMemo(
    () => (walletClient ? walletClientToSigner(walletClient) : undefined),
    [walletClient]
  );
}

```
{% endcode %}
{% endtab %}

{% tab title="viem" %}
Very similar to wagmi, but without using hooks. Instead, call `publicClientToProvider()` or `walletClientToSigner()`
{% endtab %}
{% endtabs %}

Then, check that the payer has sufficient funds using `hasSufficientFunds()`

<pre class="language-typescript"><code class="lang-typescript">import { hasSufficientFunds } from "@requestnetwork/payment-processor";

const _hasSufficientFunds = await hasSufficientFunds(
  requestData,
  payerAddress,
  {
    provider: provider,
<strong>  },
</strong><strong>);
</strong></code></pre>

Then, in the case of an ERC-20 request, check that the payer has granted sufficient approval using `hasErc20Approval()`. If not, submit an approval transaction using `approveErc20`. Wait for an appropriate number of block confirmations. On Sepolia or Ethereum, 2 block confirmations should suffice. Other chains may require more.

```typescript
import { approveErc20, hasErc20Approval } from "@requestnetwork/payment-processor";

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

<pre class="language-typescript"><code class="lang-typescript"><strong>import { payRequest } from "@requestnetwork/payment-processor";
</strong>
<strong>const paymentTx = await payRequest(requestData, signer);
</strong><strong>await paymentTx.wait(2);
</strong></code></pre>

You can detect that the payment was successful by polling the request and waiting until the request balance is greater than or equal to the expected amount.

```typescript
const request = await requestClient.fromRequestId(requestData.requestId);
let requestData = request.getData();

while (requestData.balance?.balance < requestData.expectedAmount) {
  requestData = await request.refresh();
  await new Promise((resolve) => setTimeout(resolve, 1000));
}
```

### CodeSandBox: Create and pay a request and detect a payment

{% embed url="https://codesandbox.io/p/sandbox/pay-a-request-dn7kcf?file=/app/page.tsx:71,1" fullWidth="true" %}
[https://codesandbox.io/p/sandbox/pay-a-request-dn7kcf?file=/app/page.tsx:71,1](https://codesandbox.io/p/sandbox/pay-a-request-dn7kcf?file=/app/page.tsx:71,1)
{% endembed %}

### Video: Create and pay a request and detect a payment

{% embed url="https://www.loom.com/share/1839cf3e79784dc4a6e641903b4f10d2?sid=8caf4cdb-04fe-4753-ab99-b97926c36f20" fullWidth="true" %}

## Retrieve a user's requests

First, construct a `RequestNetwork` object and connect it to a Request Node. In this example, we use the Sepolia Request Node Gateway:

<pre class="language-javascript"><code class="lang-javascript">import { RequestNetwork, Types } from "@requestnetwork/request-client.js";
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

### CodeSandBox: Retrieve a user's requests

{% embed url="https://codesandbox.io/p/sandbox/retrieve-a-users-requests-mqrjqy?file=/app/page.tsx:10,1" fullWidth="true" %}
[https://codesandbox.io/p/sandbox/retrieve-a-users-requests-mqrjqy?file=/app/page.tsx:10,1](https://codesandbox.io/p/sandbox/retrieve-a-users-requests-mqrjqy?file=/app/page.tsx:10,1)
{% endembed %}

### Video: Retrieve a user's requests

{% embed url="https://www.loom.com/share/ea76747fd3c14b6b88f916d9edce7bb5?sid=68bfb08e-48c5-4817-95d3-8eb45688978b" fullWidth="true" %}
