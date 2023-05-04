# Installation

Access to Request Network requires a combination of the Request Client library and Request Node service. To help get started quickly, the Request Network Foundation runs several Request Node "Gateways" so builders need only install the Request Client library.&#x20;

### Install the Request Client

```shell
npm install @requestnetwork/request-client.js@0.40.1-next.1892
```

### Install the \`web3-signature\` package

The Request Client library uses modular design and dependency injection to inject custom functionality into any layer of the Request software stack. A common example is the `web3-signature` package that allows signing messages using a web3 wallet like Metamask.

```bash
npm install @requestnetwork/web3-signature@0.4.37-next.1892
```

### Import the request-client and web3-signature packages

The Request Client library is available as a UMD or CommonJS module and can be used in both node applications and web pages.

{% tabs %}
{% tab title="UMD" %}
<pre class="language-tsx"><code class="lang-tsx"><strong>import { RequestNetwork } from '@requestnetwork/request-client.js';
</strong><strong>import { Web3SignatureProvider } from '@requestnetwork/web3-signature';
</strong></code></pre>
{% endtab %}

{% tab title="CommonJS" %}
```tsx
const RequestNetwork = require('@requestnetwork/request-client.js');
const Web3SignatureProvider = require('@requestnetwork/web3-signature');
```
{% endtab %}
{% endtabs %}

### Configure the client

The following command creates a new Request Client instance and configures it to:

* Connect to the Gnosis Request Node Gateway maintained by the Request Network Foundation.
* Use the web3-signature package to sign requests using a web3 wallet like Metamask.

```tsx
const signatureProvider = new Web3SignatureProvider(window.ethereum);
const requestClient = new RequestNetwork({
  nodeConnectionConfig: { baseURL: 'https://xdai.gateway.request.network/' },
  signatureProvider,
});
```

{% hint style="info" %}
Requests are _created_ using Gnosis Chain, but this doesn't mean _payment_ must occur on Gnosis Chain. Payment can occur on any of the supported chains including 20+ EVM-compatible chains or NEAR.
{% endhint %}

#### Alternative Configurations

* To create testnet requests, connect to the Goerli Request Node Gateway: `https://goerli.gateway.request.network`
* To sign requests using an Ethereum private key outside of a wallet, use the `epk-signature` package.
* To create mock storage requests, where the request is stored in memory on the local machine and cleared as soon as the script is finished running, set the `useMockStorage` argument to `true` when instantiating the `RequestNetwork`object.
