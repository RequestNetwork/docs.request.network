# Installation

The best way to access Request Network is using the Request Network SDK with a Request Node. In this way, the Request Node operator pays the protocol fee for creating requests and reduces the number of transactions signed by the end user resulting in a better user experience.

The Request Network SDK is split into multiple packages so that Builders can pick and choose the features they need.

{% hint style="warning" %}
For now, we recommend installing the Request Network SDK using the same `@next` version of each package. This is temporary and we are working on fixing our release process.

```bash
npm install @requestnetwork/request-client.js@next
npm install @requestnetwork/web3-signature@next
npm install @requestnetwork/payment-processor@next
```
{% endhint %}

## External Packages

These are the packages that we think would be most commonly used by Builders to build applications.

<table><thead><tr><th width="305">Package</th><th>Description</th></tr></thead><tbody><tr><td><a href="https://github.com/RequestNetwork/requestNetwork/blob/master/packages/request-client.js"><code>@requestnetwork/request-client.js</code></a></td><td>Create, update, and retrieve requests.</td></tr><tr><td><a href="https://github.com/RequestNetwork/requestNetwork/blob/master/packages/data-format"><code>@requestnetwork/data-format</code></a></td><td>Standards for data stored on Request, like invoice format</td></tr><tr><td><a href="https://github.com/RequestNetwork/requestNetwork/blob/master/packages/web3-signature"><code>@requestnetwork/web3-signature</code></a></td><td>Sign requests using web3 wallets like Metamask</td></tr><tr><td><a href="https://github.com/RequestNetwork/requestNetwork/blob/master/packages/epk-signature"><code>@requestnetwork/epk-signature</code></a></td><td>Sign requests using Ethereum private keys</td></tr><tr><td><a href="https://github.com/RequestNetwork/requestNetwork/blob/master/packages/epk-decryption"><code>@requestnetwork/epk-decryption</code></a></td><td>Decrypt encrypted requests using Ethereum private keys</td></tr><tr><td><a href="https://github.com/RequestNetwork/requestNetwork/blob/master/packages/payment-processor"><code>@requestnetwork/payment-processor</code></a></td><td>Pay a request using a web3 wallet</td></tr><tr><td><a href="https://github.com/RequestNetwork/requestNetwork/blob/master/packages/request-node"><code>@requestnetwork/request-node</code></a></td><td>Web server that allows easy access to the Request system</td></tr><tr><td><a href="https://www.npmjs.com/package/@requestnetwork/currency">@requestnetwork/currency</a></td><td>Tools for managing currency definitions</td></tr></tbody></table>

For a list of internal packages, see [Internal Packages](request-network-client-introduction.md#internal-packages).

## Import the packages

The Request Client library is available as a UMD or CommonJS module and can be used in both node applications and web pages.

{% tabs %}
{% tab title="UMD" %}
<pre class="language-tsx"><code class="lang-tsx"><strong>import { RequestNetwork } from '@requestnetwork/request-client.js';
</strong><strong>import { Web3SignatureProvider } from '@requestnetwork/web3-signature';
</strong><strong>import { payRequest } from '@requestnetwork/payment-processor';
</strong></code></pre>
{% endtab %}

{% tab title="CommonJS" %}
```tsx
const { RequestNetwork } = require('@requestnetwork/request-client.js');
const { Web3SignatureProvider } = require('@requestnetwork/web3-signature');
const { payRequest } = require("@requestnetwork/payment-processor");
```
{% endtab %}
{% endtabs %}

## Request Node Gateways

To help builders get started quickly, the Request Network Foundation operates several Request Node "Gateways" that are free for anyone to use. These gateways offer endpoints for creating and retrieving requests. They also subsidize the protocol fee for creating requests.

<table><thead><tr><th width="173">Gateway</th><th width="111.33333333333331">real/test</th><th>URL</th></tr></thead><tbody><tr><td>Gnosis Gateway</td><td>real</td><td><code>https://xdai.gateway.request.network</code></td></tr><tr><td>Goerli Gateway</td><td>test</td><td><code>https://goerli.gateway.request.network</code></td></tr></tbody></table>

## Configure the client

The following command creates a new Request Client instance and configures it to :

* Connect to the Gnosis Request Node Gateway maintained by the Request Network Foundation.
* Use the web3-signature package to sign requests using a web3 wallet like Metamask.

```tsx
const web3SignatureProvider = new Web3SignatureProvider(provider);
const requestClient = new RequestNetwork({
  nodeConnectionConfig: { 
    baseURL: 'https://xdai.gateway.request.network/' 
  },
  signatureProvider: web3SignatureProvider,
});
```

### Alternative: Mock Storage

To create mock storage requests, where the request is stored in memory on the local machine and cleared as soon as the script is finished running, set the `useMockStorage` argument to `true` when instantiating the `RequestNetwork`object.

## Internal Packages

These packages are published publicly but contain functions that are internal to the [External Packages](request-network-client-introduction.md#external-packages). It is less likely that a Builder would need to use these packages.

<table><thead><tr><th width="321">Package</th><th>Description</th></tr></thead><tbody><tr><td><a href="https://github.com/RequestNetwork/requestNetwork/blob/master/packages/advanced-logic"><code>@requestnetwork/advanced-logic</code></a></td><td>Extensions to the protocol</td></tr><tr><td><a href="https://github.com/RequestNetwork/requestNetwork/blob/master/packages/data-access"><code>@requestnetwork/data-access</code></a></td><td>Indexing and batching of transactions</td></tr><tr><td><a href="https://github.com/RequestNetwork/requestNetwork/blob/master/packages/ethereum-storage"><code>@requestnetwork/ethereum-storage</code></a></td><td>Storage of Request data on Ethereum and IPFS, with custom indexing</td></tr><tr><td><a href="https://github.com/RequestNetwork/requestNetwork/blob/master/packages/multi-format"><code>@requestnetwork/multi-format</code></a></td><td>Serialize and deserialize object in the Request Network protocol</td></tr><tr><td><a href="https://github.com/RequestNetwork/requestNetwork/blob/master/packages/payment-detection"><code>@requestnetwork/payment-detection</code></a></td><td>Payment detection, to compute the balance.</td></tr><tr><td><a href="https://github.com/RequestNetwork/requestNetwork/blob/master/packages/request-logic"><code>@requestnetwork/request-logic</code></a></td><td>The Request business logic: properties and actions of requests</td></tr><tr><td><a href="https://github.com/RequestNetwork/requestNetwork/blob/master/packages/smart-contracts"><code>@requestnetwork/smart-contracts</code></a></td><td>Sources and artifacts of the smart contracts</td></tr><tr><td><a href="https://github.com/RequestNetwork/requestNetwork/blob/master/packages/thegraph-data-access"><code>@requestnetwork/thegraph-data-access</code></a></td><td>Storage of Request data on Ethereum and IPFS, indexed by TheGraph</td></tr><tr><td><a href="https://github.com/RequestNetwork/requestNetwork/blob/master/packages/transaction-manager"><code>@requestnetwork/transaction-manager</code></a></td><td>Creates transactions to be sent to Data Access, managing encryption</td></tr><tr><td><a href="https://github.com/RequestNetwork/requestNetwork/blob/master/packages/types"><code>@requestnetwork/types</code></a></td><td>Typescript types shared across @requestnetwork packages</td></tr><tr><td><a href="https://github.com/RequestNetwork/requestNetwork/blob/master/packages/utils"><code>@requestnetwork/utils</code></a></td><td>Collection of tools shared between the @requestnetwork packages</td></tr></tbody></table>
