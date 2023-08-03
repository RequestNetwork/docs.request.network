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

{% hint style="info" %}
This section describes a "Reference-based" Request, not "Address-based" or "Declarative" requests.
{% endhint %}

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

First, construct a `RequestNetwork` object and connect it to a Request Node. In this example, we use the Goerli Request Node Gateway:

<pre class="language-typescript"><code class="lang-typescript">import { RequestNetwork, Types } from "@requestnetwork/request-client.js";
<strong>const requestClient = new RequestNetwork({
</strong>  nodeConnectionConfig: { 
    baseURL: "https://goerli.gateway.request.network/",
  }
})
</code></pre>

Then, call `fromIdentity()` to get an array of `Request` objects or `fromRequestId()` to get a single `Request` object. This function retrieves the `Request`s stored in IPFS and queries on-chain events to determine the balances paid so far. Finally, call `getData()` on each `Request` to get the request contents.

{% tabs %}
{% tab title="fromIdentity()" %}
{% code fullWidth="true" %}
```typescript
const identity = "0x7eB023BFbAeE228de6DC5B92D0BeEB1eDb1Fd567";
const requests = await requestClient.fromIdentity({
  type: Types.Identity.TYPE.ETHEREUM_ADDRESS,
  value: identity
})
const requestDatas = requests.map((request) => request.getData())
```
{% endcode %}
{% endtab %}

{% tab title="fromRequestId()" %}
<pre class="language-typescript"><code class="lang-typescript"><strong>const request = await requestClient.fromRequestId(
</strong>    '019830e9ec0439e53ec41fc627fd1d0293ec4bc61c2a647673ec5aaaa0e6338855'
)
const requestData = request.getData()
</code></pre>
{% endtab %}
{% endtabs %}

Altogether it looks like this:

{% embed url="https://codesandbox.io/p/sandbox/retrieve-a-users-requests-mqrjqy?embed=1" %}
[https://codesandbox.io/p/sandbox/retrieve-a-users-requests-mqrjqy?embed=1](https://codesandbox.io/p/sandbox/retrieve-a-users-requests-mqrjqy?embed=1)
{% endembed %}

## Create a request

To create an unencrypted ERC-20 request, first construct a `RequestNetwork` object:

* Connect it to a Request Node. In this example, we use the Goerli Request Node Gateway.
* Pass in a `Web3SignatureProvider` instance connected to an `ethers` v5 `Provider` and `Signer` or `wagmi` / `viem` `WalletClient`.

{% tabs %}
{% tab title="ethers v5" %}
{% code fullWidth="false" %}
```typescript
import { RequestNetwork } from "@requestnetwork/request-client.js";
import { Web3SignatureProvider } from "@requestnetwork/web3-signature";
import { providers } from "ethers";

let provider;
if (RPC_URL === undefined) {
  // Connect to Metamask and other injected wallets
  provider = new providers.Web3Provider(window.ethereum) 
} else {
  // Connect to your own Ethereum node or 3rd party node provider
  provider = new providers.JsonRpcProvider(RPC_URL)
}
// providers.DefaultProvider won't work because it doesn't include a Signer.

const web3SignatureProvider = Web3SignatureProvider(provider);
const requestClient = new RequestNetwork({
  nodeConnectionConfig: { 
    baseURL: "https://goerli.gateway.request.network/",
  },
  web3SignatureProvider,
});
```
{% endcode %}
{% endtab %}

{% tab title="wagmi" %}
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

{% tab title="viem" %}
Very similar to wagmi, but without using hooks. Construct your own `WalletClient` object.
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
    // Consider using `parseUnits()` from ethers or viem
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

## Pay a request

First, construct a `RequestNetwork` object and connect it to a Request Node. In this example, we use the Goerli Request Node Gateway:

<pre class="language-typescript"><code class="lang-typescript">import { RequestNetwork, Types } from "@requestnetwork/request-client.js";
<strong>const requestClient = new RequestNetwork({
</strong>  nodeConnectionConfig: { 
    baseURL: "https://goerli.gateway.request.network/",
  }
})
</code></pre>

Then, retrieve the request and get the request data. Take note of the current request balance, to be used later for payment detection.

```typescript
const request = await requestClient.fromRequestId(
    '019830e9ec0439e53ec41fc627fd1d0293ec4bc61c2a647673ec5aaaa0e6338855'
)
const requestData = request.getData()
const previousBalance = requestData.balance?.balance
```

Then, construct an `ethers` v5 `Provider` and `Signer`. These allow you to read and write to the chain, respectively.

{% tabs %}
{% tab title="ethers v5" %}
```typescript
import { providers } from "ethers";

let provider;
if (RPC_URL === undefined) {
  // Connect to Metamask and other injected wallets
  provider = new providers.Web3Provider(window.ethereum) 
} else {
  // Connect to your own Ethereum node or 3rd party node provider
  provider = new providers.JsonRpcProvider(RPC_URL)
}

const signer = await provider.getSigner()
```
{% endtab %}

{% tab title="wagmi" %}
{% code title="page.tsx" %}
```typescript
import { useEthersV5Provider } from './use-ethers-v5-provider'
import { useEthersV5Signer } from './use-ethers-v5-signer'

return Page() {
  const provider = useEthersV5Provider()
  const signer = useEthersV5Signer()
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

```typescript
import { hasSufficientFunds } from "@requestnetwork/payment-processor";

const hasSufficientFunds = await hasSufficientFunds(requestData, address, {
  provider: provider
})
```

Then, in the case of an ERC-20 request, check that the payer has granted sufficient approval using `hasErc20Approval()`. If not, submit an approval transaction using `approveErc20`. Wait for an appropriate number of block confirmations. On Goerli or Ethereum, 2 block confirmations should suffice. Other chains may require more.

```typescript
import { approveErc20, hasErc20Approval } from "@requestnetwork/payment-processor";

const hasErc20Approval = await hasErc20Approval(requestData, address, provider)
if (!hasErc20Approval) {
  const approvalTx = await approveErc20(requestData, signer);
  const approvalTxReceipt = await approvalTx.wait(2)
}
```

Finally, pay the request using `payRequest()`

<pre class="language-typescript"><code class="lang-typescript"><strong>import { payRequest } from "@requestnetwork/payment-processor";
</strong><strong>
</strong><strong>const paymentTx = await payRequest(requestData, signer);
</strong><strong>const paymentTxReceipt = await paymentTx.wait(2)
</strong></code></pre>

Detect that the payment was successful by polling the request and comparing the current balance to the previous balance.

```typescript
const request = await requestClient.fromRequestId(requestData.requestId)
let requestData = request.getData()
while (requestData.balance?.balance == previousBalance) {
  requestData = request.refresh()
}
```

Other operation quickstarts coming soon! In the meantime check out the following guides:

<table data-card-size="large" data-column-title-hidden data-view="cards"><thead><tr><th></th><th></th><th data-hidden></th><th data-hidden data-card-target data-type="content-ref"></th><th data-hidden data-card-cover data-type="files"></th></tr></thead><tbody><tr><td></td><td>Retrieve a request</td><td></td><td><a href="../learn-request-network/guides/retrieve-a-request.md">retrieve-a-request.md</a></td><td></td></tr><tr><td></td><td>Create a request</td><td></td><td><a href="../learn-request-network/guides/creating-an-erc20-request.md">creating-an-erc20-request.md</a></td><td></td></tr><tr><td></td><td>Pay a request</td><td></td><td><a href="../learn-request-network/guides/pay-a-request.md">pay-a-request.md</a></td><td></td></tr><tr><td></td><td>Detect a payment</td><td></td><td><a href="../learn-request-network/guides/detect-a-payment.md">detect-a-payment.md</a></td><td></td></tr></tbody></table>
