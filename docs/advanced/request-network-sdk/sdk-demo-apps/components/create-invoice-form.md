---
description: A form for creating invoices in Request Network
---

# Create Invoice Form

<figure><img src="../../../../.gitbook/assets/image (7) (1) (1).png" alt=""><figcaption><p>Screenshot of @requestnetwork/create-invoice-form 0.2.0</p></figcaption></figure>

<table data-view="cards" data-full-width="false"><thead><tr><th></th><th></th><th></th><th data-hidden data-type="content-ref"></th><th data-hidden data-card-target data-type="content-ref"></th></tr></thead><tbody><tr><td></td><td><span data-gb-custom-inline data-tag="emoji" data-code="1f579">🕹️</span> <strong>Try it out</strong></td><td></td><td><a href="https://invoicing.request.network/create-invoice">https://invoicing.request.network/create-invoice</a></td><td><a href="https://invoicing.request.network/">https://invoicing.request.network/</a></td></tr><tr><td></td><td><span data-gb-custom-inline data-tag="emoji" data-code="25b6">▶️</span> <strong>Demo Video</strong></td><td></td><td><a href="https://www.youtube.com/watch?v=FC6oJR5aKOQ">https://www.youtube.com/watch?v=FC6oJR5aKOQ</a></td><td><a href="../#request-invoicing-demo-video">#request-invoicing-demo-video</a></td></tr><tr><td></td><td><span data-gb-custom-inline data-tag="emoji" data-code="1f3d7">🏗️</span> <strong>Integration Video</strong></td><td></td><td><a href="https://www.youtube.com/watch?v=CV4uiKM7r7s">https://www.youtube.com/watch?v=CV4uiKM7r7s</a></td><td><a href="../#request-invoicing-integration-video">#request-invoicing-integration-video</a></td></tr><tr><td></td><td><span data-gb-custom-inline data-tag="emoji" data-code="1f4e6">📦</span> <strong>View on NPM</strong></td><td></td><td><a href="https://www.npmjs.com/package/@requestnetwork/create-invoice-form">https://www.npmjs.com/package/@requestnetwork/create-invoice-form</a></td><td><a href="https://www.npmjs.com/package/@requestnetwork/create-invoice-form">https://www.npmjs.com/package/@requestnetwork/create-invoice-form</a></td></tr><tr><td></td><td><span data-gb-custom-inline data-tag="emoji" data-code="2139">ℹ️</span> <strong>View Source</strong></td><td></td><td><a href="https://github.com/RequestNetwork/web-components/tree/main/packages/create-invoice-form">https://github.com/RequestNetwork/web-components/tree/main/packages/create-invoice-form</a></td><td><a href="https://github.com/RequestNetwork/web-components/tree/main/packages/create-invoice-form">https://github.com/RequestNetwork/web-components/tree/main/packages/create-invoice-form</a></td></tr></tbody></table>

The Create Invoice Form allows end-users to create an invoice using the Request Network. It is built using [Svelte](https://svelte.dev/) but compiled to a [Web Component](https://developer.mozilla.org/en-US/docs/Web/API/Web_components), making it usable in any web environment, regardless of the framework.

## Installation

To install the component, use npm:

```bash
npm install @requestnetwork/create-invoice-form
```

## Usage

### Usage in React and Next.js

Follow the instructions below to add the Create Invoice Form to a React or Next.js app. For a video explaining how to integrate, see the [#request-invoicing-integration-video](../#request-invoicing-integration-video "mention")

#### [**create-invoice.tsx**](https://github.com/RequestNetwork/invoicing-template/blob/01e44755b274d17c0718ca03f077d68a9fe8baec/pages/create-invoice.tsx)

Configure the Create Invoice Form web component by creating a reference to it, setting its properties, and passing the reference as a prop.

{% @github-files/github-code-block url="https://github.com/RequestNetwork/invoicing-template/blob/01e44755b274d17c0718ca03f077d68a9fe8baec/pages/create-invoice.tsx" %}

#### [wagmiConfig.ts](https://github.com/RequestNetwork/invoicing-template/blob/01e44755b274d17c0718ca03f077d68a9fe8baec/utils/wagmiConfig.ts)

#### The web component supports any wallet connector module built on top of [wagmi](https://wagmi.sh/). This provides the flexibility to use any wagmi-compatible wallet connector, such as [RainbowKit](https://www.rainbowkit.com/docs/introduction#industry-standards).

{% @github-files/github-code-block url="https://github.com/RequestNetwork/invoicing-template/blob/01e44755b274d17c0718ca03f077d68a9fe8baec/utils/wagmiConfig.ts" %}

#### [initializeRN.ts](https://github.com/RequestNetwork/invoicing-template/blob/01e44755b274d17c0718ca03f077d68a9fe8baec/utils/initializeRN.ts)

Initialize the `RequestNetwork` object using an Ethers `Signer` or Viem `WalletClient`.

{% @github-files/github-code-block url="https://github.com/RequestNetwork/invoicing-template/blob/01e44755b274d17c0718ca03f077d68a9fe8baec/utils/initializeRN.ts" %}

#### [config.ts](https://github.com/RequestNetwork/invoicing-template/blob/01e44755b274d17c0718ca03f077d68a9fe8baec/utils/config.ts)

Use the config object to pass additional configuration options. Please replace the `builderId` with your own, arbitrarily chosen ID. This is used to track how many invoices your application creates.

{% @github-files/github-code-block url="https://github.com/RequestNetwork/invoicing-template/blob/01e44755b274d17c0718ca03f077d68a9fe8baec/utils/config.ts" %}

#### [context.tsx](https://github.com/RequestNetwork/invoicing-template/blob/01e44755b274d17c0718ca03f077d68a9fe8baec/utils/context.tsx)

Use a context provider to reinitialize the Request Network instance when the wallet changes.

{% @github-files/github-code-block url="https://github.com/RequestNetwork/invoicing-template/blob/01e44755b274d17c0718ca03f077d68a9fe8baec/utils/context.tsx" %}

#### [currencies.ts](https://github.com/RequestNetwork/invoicing-template/blob/01e44755b274d17c0718ca03f077d68a9fe8baec/utils/currencies.ts)

A list of custom currencies to extend the default currency list.

{% @github-files/github-code-block url="https://github.com/RequestNetwork/invoicing-template/blob/01e44755b274d17c0718ca03f077d68a9fe8baec/utils/currencies.ts" %}

#### [types.d.ts](https://github.com/RequestNetwork/invoicing-template/blob/01e44755b274d17c0718ca03f077d68a9fe8baec/types.d.ts)

Specify types to avoid TypeScript errors.

{% @github-files/github-code-block url="https://github.com/RequestNetwork/invoicing-template/blob/01e44755b274d17c0718ca03f077d68a9fe8baec/types.d.ts" %}

## Props

| Prop                    | Type                                                                        | Description                                             |
| ----------------------- | --------------------------------------------------------------------------- | ------------------------------------------------------- |
| config                  | IConfig                                                                     | Additional configuration parameters                     |
| config.builderId        | string                                                                      | Unique builder ID, arbitrarily chosen, used for metrics |
| config.dashboardLink    | string                                                                      | Path to dashboard page                                  |
| config.logo             | string                                                                      | Path to logo file                                       |
| config.colors.main      | string                                                                      | Hex color code for primary buttons and labels           |
| config.colors.secondary | string                                                                      | Hex color code for for borders and accents              |
| requestNetwork          | [RequestNetwork](../../sdk-api-reference/request-client.js/requestnetwork/) | The RequestNetwork instance                             |
| wagmiConfig             | WagmiConfig                                                                 | Wallet connector config                                 |
| currencies              | Currency\[]                                                                 | A list of custom currencies                             |

## Next Steps

{% content-ref url="../../get-started/quickstart-browser.md" %}
[quickstart-browser.md](../../get-started/quickstart-browser.md)
{% endcontent-ref %}
