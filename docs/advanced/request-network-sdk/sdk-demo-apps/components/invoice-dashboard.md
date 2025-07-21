---
description: A dashboard for viewing and paying invoices in Request Network
---

# Invoice Dashboard

<div data-full-width="false"><figure><img src="../../../../.gitbook/assets/image (6) (2).png" alt=""><figcaption><p>Screenshot of @requestnetwork/invoice-dashboard 0.3.0</p></figcaption></figure></div>

<table data-view="cards" data-full-width="false"><thead><tr><th></th><th></th><th></th><th data-hidden data-card-target data-type="content-ref"></th></tr></thead><tbody><tr><td></td><td><span data-gb-custom-inline data-tag="emoji" data-code="1f579">🕹️</span> <strong>Try it out</strong></td><td></td><td><a href="https://invoicing.request.network">https://invoicing.request.network</a></td></tr><tr><td></td><td><span data-gb-custom-inline data-tag="emoji" data-code="25b6">▶️</span> <strong>Demo Video</strong></td><td></td><td><a href="../#request-invoicing-demo-video">#request-invoicing-demo-video</a></td></tr><tr><td></td><td><span data-gb-custom-inline data-tag="emoji" data-code="1f3d7">🏗️</span> <strong>Integration Video</strong></td><td></td><td><a href="../#request-invoicing-integration-video">#request-invoicing-integration-video</a></td></tr><tr><td></td><td><span data-gb-custom-inline data-tag="emoji" data-code="1f4e6">📦</span> <strong>View on NPM</strong></td><td></td><td><a href="https://www.npmjs.com/package/@requestnetwork/invoice-dashboard">https://www.npmjs.com/package/@requestnetwork/invoice-dashboard</a></td></tr><tr><td></td><td><span data-gb-custom-inline data-tag="emoji" data-code="2139">ℹ️</span> <strong>View Source</strong></td><td></td><td><a href="https://github.com/RequestNetwork/web-components/tree/main/packages/invoice-dashboard">https://github.com/RequestNetwork/web-components/tree/main/packages/invoice-dashboard</a></td></tr></tbody></table>

The Invoice Dashboard component allows end-users to view and pay an invoice in Request Network. It is built using [Svelte](https://svelte.dev/) but compiled to a [Web Component](https://developer.mozilla.org/en-US/docs/Web/API/Web_components), making it usable in any web environment, regardless of the framework.

## Installation

To install the component, use npm:

```bash
npm install @requestnetwork/invoice-dashboard
```

## Usage

### Usage in React and Next.js

Follow the instructions below to add the Invoice Dashboard to a React or Next.js app.

#### [**invoice-dashboard.tsx**](https://github.com/RequestNetwork/invoicing-template/blob/01e44755b274d17c0718ca03f077d68a9fe8baec/pages/index.tsx)

Configure the invoice dashboard web component by creating a reference to it, setting its properties, and passing the reference as a prop.

{% @github-files/github-code-block url="https://github.com/RequestNetwork/invoicing-template/blob/01e44755b274d17c0718ca03f077d68a9fe8baec/pages/index.tsx" %}

#### [wagmiConfig.ts](https://github.com/RequestNetwork/invoicing-template/blob/01e44755b274d17c0718ca03f077d68a9fe8baec/utils/wagmiConfig.ts)

#### The web component supports any wallet connector module built on top of [wagmi](https://wagmi.sh/). This provides the flexibility to use any wagmi-compatible wallet connector, such as [RainbowKit](https://www.rainbowkit.com/docs/introduction#industry-standards).

{% @github-files/github-code-block url="https://github.com/RequestNetwork/invoicing-template/blob/01e44755b274d17c0718ca03f077d68a9fe8baec/utils/wagmiConfig.ts" %}

#### [initializeRN.ts](https://github.com/RequestNetwork/invoicing-template/blob/01e44755b274d17c0718ca03f077d68a9fe8baec/utils/initializeRN.ts)

Initialize the `RequestNetwork` object using an Ethers `Signer` or Viem `WalletClient`.

{% @github-files/github-code-block url="https://github.com/RequestNetwork/invoicing-template/blob/01e44755b274d17c0718ca03f077d68a9fe8baec/utils/initializeRN.ts" %}

#### [config.ts](https://github.com/RequestNetwork/invoicing-template/blob/main/utils/config.ts)

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
| config.colors.main      | string                                                                      | Color used for primary buttons and labels               |
| config.colors.secondary | string                                                                      | Color used for borders and accents                      |
| requestNetwork          | [RequestNetwork](../../sdk-api-reference/request-client.js/requestnetwork/) | The RequestNetwork instance                             |
| wagmiConfig             | WagmiConfig                                                                 | Wallet connector config                                 |
| currencies              | Currency\[]                                                                 | A list of custom currencies                             |
