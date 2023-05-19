# Quickstart

This page will introduce the four primary operations provided by Request Network’s browser-side JavaScript library, `request-client.js`.

{% hint style="info" %}
You will learn:

* How to retrieve a user’s requests
* How to create a request
* How to pay a request
* How to detect a payment
{% endhint %}

## Retrieve a user's requests

First, construct a `RequestNetwork` object and connect it to the Goerli Request Node Gateway:

<pre class="language-typescript"><code class="lang-typescript">import { RequestNetwork, Types } from "@requestnetwork/request-client.js";
<strong>const requestClient = new RequestNetwork({
</strong>  nodeConnectionConfig: { baseURL: "https://goerli.gateway.request.network/" }
})
</code></pre>

Then, call `fromIdentity()` to get an array of `Request` objects. This function retrieves the payment requests stored in IPFS and queries on-chain events to determine the balance paid so far.

```typescript
const address = "0x7eB023BFbAeE228de6DC5B92D0BeEB1eDb1Fd567";
const requests = await requestClient.fromIdentity({
    type: Types.Identity.TYPE.ETHEREUM_ADDRESS,
    value: address
})
```

Finally, call `getData()` on each `Request`.

```typescript
const requestDatas = requests.map((request) => request.getData())
```

Altogether it looks like this:

{% embed url="https://codesandbox.io/embed/retrieve-a-users-requests-pwe9o2?codemirror=1&fontsize=12&hidenavigation=1&theme=light&view=split" fullWidth="true" %}
CodeSandBox showing how to retrieve a user's requests and display them in a table.
{% endembed %}

<details>

<summary>CodeSandBox Preview</summary>

The embedded CodeSandBox above is not broken despite the error message. It creates a development environment entirely in the browser and takes a long time to transpile the first time it is viewed. Conservatively, it may take 30 minutes. Here's a preview of what it will look like:

![](../.gitbook/assets/CodeSandBox\_Preview\_Retrieve\_a\_users\_requests.png)

</details>

## Next Steps

Other operation quickstarts coming soon! In the meantime check out the following guides:

<table data-card-size="large" data-column-title-hidden data-view="cards"><thead><tr><th></th><th></th><th data-hidden></th><th data-hidden data-card-target data-type="content-ref"></th><th data-hidden data-card-cover data-type="files"></th></tr></thead><tbody><tr><td></td><td>Create a request</td><td></td><td><a href="../learn-request-network/guides/creating-an-erc20-request.md">creating-an-erc20-request.md</a></td><td></td></tr><tr><td></td><td>Retrieve a request</td><td></td><td><a href="../learn-request-network/guides/retrieve-a-request.md">retrieve-a-request.md</a></td><td></td></tr><tr><td></td><td>Pay a request</td><td></td><td><a href="../learn-request-network/guides/pay-a-request.md">pay-a-request.md</a></td><td></td></tr><tr><td></td><td>Detect a payment</td><td></td><td><a href="../learn-request-network/guides/detect-a-payment.md">detect-a-payment.md</a></td><td></td></tr></tbody></table>
