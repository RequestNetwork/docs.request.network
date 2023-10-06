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

<table data-full-width="true"><thead><tr><th>Package</th><th>Description</th></tr></thead><tbody><tr><td><a href="https://www.npmjs.com/package/@requestnetwork/request-client.js"><code>@requestnetwork/request-client.js</code></a></td><td>Create, update, and retrieve requests.</td></tr><tr><td><a href="https://www.npmjs.com/package/@requestnetwork/web3-signature"><code>@requestnetwork/web3-signature</code></a></td><td>Sign requests using web3 wallets like Metamask</td></tr><tr><td><a href="https://www.npmjs.com/package/@requestnetwork/epk-signature"><code>@requestnetwork/epk-signature</code></a></td><td>Sign requests using Ethereum private keys</td></tr><tr><td><a href="https://www.npmjs.com/package/@requestnetwork/data-format"><code>@requestnetwork/data-format</code></a></td><td>Standards for data stored on Request, like invoice format</td></tr><tr><td><a href="https://www.npmjs.com/package/@requestnetwork/epk-decryption"><code>@requestnetwork/epk-decryption</code></a></td><td>Decrypt encrypted requests using Ethereum private keys</td></tr><tr><td><a href="https://www.npmjs.com/package/@requestnetwork/payment-processor"><code>@requestnetwork/payment-processor</code></a></td><td>Pay a request using a web3 wallet</td></tr><tr><td><a href="https://www.npmjs.com/package/@requestnetwork/request-node"><code>@requestnetwork/request-node</code></a></td><td>Web server that allows easy access to the Request system</td></tr><tr><td><a href="https://www.npmjs.com/package/@requestnetwork/currency">@requestnetwork/currency</a></td><td>Tools for managing currency definitions</td></tr><tr><td><a href="https://www.npmjs.com/package/@requestnetwork/utils"><code>@requestnetwork/utils</code></a></td><td>Collection of tools shared between the @requestnetwork packages</td></tr></tbody></table>

For a list of internal packages, see [Internal Packages](request-network-client-introduction.md#internal-packages).

## Import the packages

The Request Client library can be imported as ES6 or CommonJS modules.

{% tabs %}
{% tab title="ES6" %}
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

## Internal Packages

These packages are published publicly but contain functions that are internal to the [External Packages](request-network-client-introduction.md#external-packages). It is less likely that a Builder would need to use these packages.

<table data-full-width="true"><thead><tr><th>Package</th><th>Description</th></tr></thead><tbody><tr><td><a href="https://www.npmjs.com/package/@requestnetwork/advanced-logic"><code>@requestnetwork/advanced-logic</code></a></td><td>Extensions to the protocol</td></tr><tr><td><a href="https://www.npmjs.com/package/@requestnetwork/data-access"><code>@requestnetwork/data-access</code></a></td><td>Indexing and batching of transactions</td></tr><tr><td><a href="https://www.npmjs.com/package/@requestnetwork/ethereum-storage"><code>@requestnetwork/ethereum-storage</code></a></td><td>Storage of Request data on Ethereum and IPFS, with custom indexing</td></tr><tr><td><a href="https://www.npmjs.com/package/@requestnetwork/multi-format"><code>@requestnetwork/multi-format</code></a></td><td>Serialize and deserialize object in the Request Network protocol</td></tr><tr><td><a href="https://www.npmjs.com/package/@requestnetwork/payment-detection"><code>@requestnetwork/payment-detection</code></a></td><td>Payment detection, to compute the balance.</td></tr><tr><td><a href="https://www.npmjs.com/package/@requestnetwork/request-logic"><code>@requestnetwork/request-logic</code></a></td><td>The Request business logic: properties and actions of requests</td></tr><tr><td><a href="https://www.npmjs.com/package/@requestnetwork/smart-contracts"><code>@requestnetwork/smart-contracts</code></a></td><td>Sources and artifacts of the smart contracts</td></tr><tr><td><a href="https://www.npmjs.com/package/@requestnetwork/thegraph-data-access"><code>@requestnetwork/thegraph-data-access</code></a></td><td>Storage of Request data on Ethereum and IPFS, indexed by TheGraph</td></tr><tr><td><a href="https://www.npmjs.com/package/@requestnetwork/transaction-manager"><code>@requestnetwork/transaction-manager</code></a></td><td>Creates transactions to be sent to Data Access, managing encryption</td></tr><tr><td><a href="https://www.npmjs.com/package/@requestnetwork/types"><code>@requestnetwork/types</code></a></td><td>Typescript types shared across @requestnetwork packages</td></tr></tbody></table>
