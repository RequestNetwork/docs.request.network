# Installation

Access to Request Network requires a combination of the Request Network JavaScript SDK and a Request Node.

## Install request-client.js package

The `request-client.js` package offers functions to create and retrieve requests.

```shell
npm install @requestnetwork/request-client.js@next
```

## Install web3-signature package

The `web3-signature` package that allows signing requests using a web3 wallet like Metamask.

```bash
npm install @requestnetwork/web3-signature@next
```

### Alternative: epk-signature package

The `epk-signature` package allows signing requests using a manually managed private key.

```bash
npm install @requestnetwork/epk-signature@next
```

## Install payment-processor package

The `payment-processor` package offers functions for paying requests and interacting with payment network smart contracts.

```bash
npm install @requestnetwork/payment-processor@next
```

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
