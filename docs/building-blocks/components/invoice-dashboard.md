---
description: A dashboard for viewing and paying invoices in Request Network
layout:
  title:
    visible: true
  description:
    visible: true
  tableOfContents:
    visible: true
  outline:
    visible: true
  pagination:
    visible: true
---

# Invoice Dashboard

<div data-full-width="false">

<figure><img src="../../.gitbook/assets/image (1) (1).png" alt=""><figcaption><p>Screenshot of @requestnetwork/invoice-dashboard 0.3.0</p></figcaption></figure>

</div>

<table data-view="cards" data-full-width="false"><thead><tr><th></th><th></th><th></th><th data-hidden data-card-target data-type="content-ref"></th></tr></thead><tbody><tr><td></td><td><span data-gb-custom-inline data-tag="emoji" data-code="1f579">🕹️</span> <strong>Try it out</strong> </td><td></td><td><a href="https://invoicing.request.network">https://invoicing.request.network</a></td></tr><tr><td></td><td><span data-gb-custom-inline data-tag="emoji" data-code="25b6">▶️</span> <strong>Demo Video</strong></td><td></td><td><a href="../templates/#request-invoicing-demo-video">#request-invoicing-demo-video</a></td></tr><tr><td></td><td><span data-gb-custom-inline data-tag="emoji" data-code="1f3d7">🏗️</span> <strong>Integration Video</strong></td><td></td><td><a href="../templates/#request-invoicing-integration-video">#request-invoicing-integration-video</a></td></tr><tr><td></td><td><span data-gb-custom-inline data-tag="emoji" data-code="1f4e6">📦</span> <strong>View on NPM</strong></td><td></td><td><a href="https://www.npmjs.com/package/@requestnetwork/invoice-dashboard">https://www.npmjs.com/package/@requestnetwork/invoice-dashboard</a></td></tr><tr><td></td><td><span data-gb-custom-inline data-tag="emoji" data-code="2139">ℹ️</span> <strong>View Source</strong></td><td></td><td><a href="https://github.com/RequestNetwork/web-components/tree/main/packages/invoice-dashboard">https://github.com/RequestNetwork/web-components/tree/main/packages/invoice-dashboard</a></td></tr></tbody></table>

The Invoice Dashboard component allows end-users to view and pay an invoice in Request Network. It is built using [Svelte](https://svelte.dev/) but compiled to a [Web Component](https://developer.mozilla.org/en-US/docs/Web/API/Web\_components), making it usable in any web environment, regardless of the framework.

## Features

| Feature                                                                                                                                | Status |
| -------------------------------------------------------------------------------------------------------------------------------------- | ------ |
| ERC20 Request                                                                                                                          | ✅      |
| [rnf\_invoice format](https://github.com/RequestNetwork/requestNetwork/tree/master/packages/data-format/src/format/rnf\_invoice) 0.3.0 | ✅      |
| Configure Logo and Colors                                                                                                              | ✅      |
| Minimal Chains and Currencies                                                                                                          | ✅      |
| Custom currency list                                                                                                                   | ✅      |

## Chains and Currencies

| Chain           | Currencies                                                                                  |
| --------------- | ------------------------------------------------------------------------------------------- |
| Ethereum        | USDC, USDT, DAI, AXS, AUDIO, RAI, SYLO, LDO, UST, MNT, MIR, INJ, OCEAN, ANKR, RLY, REQ, ETH |
| Polygon         | USDC, USDT, DAI, MATIC                                                                      |
| Sepolia         | FAU, ETH, USDT, USDC                                                                        |
| BNB Smart Chain | DAI, BUSD                                                                                   |
| Gnosis          | USDC                                                                                        |
| Avalanche       | USDC, USDT, AVAX                                                                            |
| Optimism        | USDC, USDT, DAI, ETH                                                                        |
| Moonbeam        | USDC (multichain), USDC (wormhole)                                                          |
| Fantom          | FTM                                                                                         |
| Mantle          | MNT                                                                                         |
| zkSync Era      | ETH                                                                                         |
| Base            | ETH                                                                                         |

## Installation

To install the component, use npm:

```bash
npm install @requestnetwork/invoice-dashboard
```

## Usage

{% hint style="warning" %}
The Invoice Dashboard component is only compatible with [Web3 Onboard](https://onboard.blocknative.com/) because it takes a `WalletState` as a prop. Future iterations will allow for other wallet connectors. For status, see [invoicing-template #31](https://github.com/RequestNetwork/web-components/issues/31)
{% endhint %}

### Usage in React and Next.js

Follow the instructions below to add the Invoice Dashboard to a React or Next.js app.&#x20;

#### **invoice-dashboard.tsx**

Configure the invoice dashboard web component by creating a reference to it, setting its properties, and passing the reference as a prop.&#x20;

{% @github-files/github-code-block url="https://github.com/RequestNetwork/invoicing-template/blob/main/pages/index.tsx" fullWidth="false" %}

#### **Supporting files**

* [initializeRN.ts](https://github.com/RequestNetwork/invoicing-template/blob/main/utils/initializeRN.ts) - Initialize the `RequestNetwork` object using an Ethers `Signer` or Viem `WalletClient`.

{% @github-files/github-code-block url="https://github.com/RequestNetwork/invoicing-template/blob/main/utils/initializeRN.ts" %}

* [config.ts](https://github.com/RequestNetwork/invoicing-template/blob/main/utils/config.ts) - Use the config object to pass additional configuration options to the invoice dashboard component. Please replace the `builderId` with your own, arbitrarily chosen ID. This is used to track how many invoices are created by your application.

{% @github-files/github-code-block url="https://github.com/RequestNetwork/invoicing-template/blob/main/utils/config.ts" %}

* [context.tsx](https://github.com/RequestNetwork/invoicing-template/blob/main/utils/context.tsx) - Use a context provider to reinitialize the Request Network instance when the wallet changes.
* [types.d.ts](https://github.com/RequestNetwork/invoicing-template/blob/main/types.d.ts) - Specify types to avoid TypeScript errors.
* [currencies.ts](https://github.com/RequestNetwork/invoicing-template/blob/68677d8823c29c1d00eb93f5285e9aa90540023a/utils/currencies.ts) - A list of custom currencies to extend the default currency list.

{% @github-files/github-code-block url="https://github.com/RequestNetwork/invoicing-template/blob/68677d8823c29c1d00eb93f5285e9aa90540023a/utils/currencies.ts" %}

## Props

| Prop                    | Type                                                                                              | Description                                             |
| ----------------------- | ------------------------------------------------------------------------------------------------- | ------------------------------------------------------- |
| config                  | IConfig                                                                                           | Additional configuration parameters                     |
| config.builderId        | string                                                                                            | Unique builder ID, arbitrarily chosen, used for metrics |
| config.dashboardLink    | string                                                                                            | Path to dashboard page                                  |
| config.logo             | string                                                                                            | Path to logo file                                       |
| config.colors.main      | string                                                                                            | Color used for primary buttons and labels               |
| config.colors.secondary | string                                                                                            | Color used for borders and accents                      |
| requestNetwork          | [RequestNetwork](../../learn-request-network/sdk-api-reference/request-client.js/requestnetwork/) | The RequestNetwork instance                             |
| wallet                  | WalletState                                                                                       | Web3Onboard WalletState                                 |
| currencies              | Currency\[]                                                                                       | A list of custom currencies                             |

