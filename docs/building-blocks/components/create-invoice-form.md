---
description: A form for creating invoices in Request Network
---

# Create Invoice Form

<figure><img src="../../.gitbook/assets/image (2).png" alt=""><figcaption><p>Screenshot of @requestnetwork/create-invoice-form 0.2.0</p></figcaption></figure>



<table data-view="cards" data-full-width="false"><thead><tr><th></th><th></th><th></th><th data-hidden data-type="content-ref"></th><th data-hidden data-card-target data-type="content-ref"></th></tr></thead><tbody><tr><td></td><td><span data-gb-custom-inline data-tag="emoji" data-code="1f579">🕹️</span> <strong>Try it out</strong></td><td></td><td><a href="https://invoicing.request.network/create-invoice">https://invoicing.request.network/create-invoice</a></td><td></td></tr><tr><td></td><td><span data-gb-custom-inline data-tag="emoji" data-code="25b6">▶️</span> <strong>Demo Video</strong></td><td></td><td><a href="https://www.youtube.com/watch?v=FC6oJR5aKOQ">https://www.youtube.com/watch?v=FC6oJR5aKOQ</a></td><td><a href="../templates.md#request-invoicing-demo-video">#request-invoicing-demo-video</a></td></tr><tr><td></td><td><span data-gb-custom-inline data-tag="emoji" data-code="1f3d7">🏗️</span> <strong>Integration Video</strong></td><td></td><td><a href="https://www.youtube.com/watch?v=CV4uiKM7r7s">https://www.youtube.com/watch?v=CV4uiKM7r7s</a></td><td><a href="../templates.md#request-invoicing-integration-video">#request-invoicing-integration-video</a></td></tr><tr><td></td><td><span data-gb-custom-inline data-tag="emoji" data-code="1f4e6">📦</span> <strong>View on NPM</strong></td><td></td><td><a href="https://www.npmjs.com/package/@requestnetwork/create-invoice-form">https://www.npmjs.com/package/@requestnetwork/create-invoice-form</a></td><td></td></tr><tr><td></td><td><span data-gb-custom-inline data-tag="emoji" data-code="2139">ℹ️</span> <strong>View Source</strong></td><td></td><td><a href="https://github.com/RequestNetwork/web-components/tree/main/packages/create-invoice-form">https://github.com/RequestNetwork/web-components/tree/main/packages/create-invoice-form</a></td><td></td></tr></tbody></table>

The Create Invoice Form allows end-users to create an invoice using the Request Network. It is built using [Svelte](https://svelte.dev/) but compiled to a [Web Component](https://developer.mozilla.org/en-US/docs/Web/API/Web\_components), making it usable in any web environment, regardless of the framework.

## Features

| Feature                                                                                                                                | Status |
| -------------------------------------------------------------------------------------------------------------------------------------- | ------ |
| ERC20 Request                                                                                                                          | ✅      |
| [rnf\_invoice format](https://github.com/RequestNetwork/requestNetwork/tree/master/packages/data-format/src/format/rnf\_invoice) 0.3.0 | ✅      |
| Configure Logo and Colors                                                                                                              | ✅      |
| Minimal Chains and Currencies                                                                                                          | ✅      |

## Chains and Currencies

| Chain    | Currencies             |
| -------- | ---------------------- |
| Ethereum | USDC, USDT, DAI        |
| Polygon  | USDC, USDT, DAI, USDCe |
| Sepolia  | USDC, FAU              |

## Installation

To install the component, use npm:

```bash
npm install @requestnetwork/create-invoice-form
```

## Usage

### Usage in React and Next.js

Follow the instructions below to add the Create Invoice Form to a React or Next.js app. For a video explaining how to integrate, see the [#request-invoicing-integration-video](../templates.md#request-invoicing-integration-video "mention")

#### Disable swcMinify in next.config.mjs

{% hint style="warning" %}
To use the Create Invoice Form in NextJS 14.x, you must set `swcMinify: false` in your `next.config.mjs` file. Without it, the Create Invoice Form won't render.
{% endhint %}

{% @github-files/github-code-block url="https://github.com/RequestNetwork/invoicing-template/blob/main/next.config.mjs" %}

#### **create-invoice.tsx**

Configure the Create Invoice Form web component by creating a reference to it, setting its properties, and passing the reference as a prop.&#x20;

{% hint style="warning" %}
To use the Create Invoice Form in a React or Next.js application, you must _dynamically_ import `@requestnetwork/create-invoice-form` and use the component in your JSX file.

```jsx
import("@requestnetwork/create-invoice-form");
```
{% endhint %}

{% hint style="info" %}
This example uses [Web3 Onboard](https://onboard.blocknative.com/) to connect a wallet but you can use any wallet connection method you prefer.
{% endhint %}

{% @github-files/github-code-block url="https://github.com/RequestNetwork/invoicing-template/blob/main/pages/create-invoice.tsx" %}

#### **Supporting files**

* [initializeRN.ts](https://github.com/RequestNetwork/invoicing-template/blob/main/utils/initializeRN.ts) - Initialize the `RequestNetwork` object using an Ethers `Signer` or Viem `WalletClient`.

{% @github-files/github-code-block url="https://github.com/RequestNetwork/invoicing-template/blob/main/utils/initializeRN.ts" %}

* [config.ts](https://github.com/RequestNetwork/invoicing-template/blob/main/utils/config.ts) - Use the config object to pass additional configuration options to the create invoice form component. Please replace the `builderId` with your own, arbitrarily chosen ID. This is used to track how many invoices are created by your application.

{% @github-files/github-code-block url="https://github.com/RequestNetwork/invoicing-template/blob/main/utils/config.ts" %}

* [context.tsx](https://github.com/RequestNetwork/invoicing-template/blob/main/utils/context.tsx) - Use a context provider to reinitialize the Request Network instance when the wallet changes.
* [types.d.ts](https://github.com/RequestNetwork/invoicing-template/blob/main/types.d.ts) - Specify types to avoid TypeScript errors.

## Props

| Prop                    | Type                                                                                              | Description                                             |
| ----------------------- | ------------------------------------------------------------------------------------------------- | ------------------------------------------------------- |
| config                  | IConfig                                                                                           | Additional configuration parameters                     |
| config.builderId        | string                                                                                            | Unique builder ID, arbitrarily chosen, used for metrics |
| config.dashboardLink    | string                                                                                            | Path to dashboard page                                  |
| config.logo             | string                                                                                            | Path to logo file                                       |
| config.colors.main      | string                                                                                            | Hex color code for primary buttons and labels           |
| config.colors.secondary | string                                                                                            | Hex color code for for borders and accents              |
| requestNetwork          | [RequestNetwork](../../learn-request-network/sdk-api-reference/request-client.js/requestnetwork/) | The RequestNetwork instance                             |
| signer                  | string                                                                                            | The address that signs to create the invoices           |
