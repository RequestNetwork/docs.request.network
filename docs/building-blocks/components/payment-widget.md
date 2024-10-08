---
description: A widget that allows builders to accept crypto payments.
---

# Payment Widget

<figure><img src="../../.gitbook/assets/CleanShot 2024-08-19 at 15.03.46.png" alt="" width="434"><figcaption><p>Screenshot of @requestnetwork/payment-widget@0.1.0</p></figcaption></figure>

<table data-view="cards"><thead><tr><th></th><th></th><th></th><th data-hidden data-card-target data-type="content-ref"></th></tr></thead><tbody><tr><td></td><td><span data-gb-custom-inline data-tag="emoji" data-code="1f579">🕹️</span> <strong>Try it out</strong> </td><td></td><td><a href="https://checkout.request.network">https://checkout.request.network</a></td></tr><tr><td></td><td><span data-gb-custom-inline data-tag="emoji" data-code="25b6">▶️</span> Demo Video</td><td></td><td><a href="../templates.md#payment-widget-demo-video">#payment-widget-demo-video</a></td></tr><tr><td></td><td><span data-gb-custom-inline data-tag="emoji" data-code="1f3d7">🏗️</span> Integration Video</td><td></td><td><a href="../templates.md#payment-widget-integration-video">#payment-widget-integration-video</a></td></tr><tr><td></td><td><span data-gb-custom-inline data-tag="emoji" data-code="1f4e6">📦</span> View on NPM</td><td></td><td><a href="https://www.npmjs.com/package/@requestnetwork/payment-widget">https://www.npmjs.com/package/@requestnetwork/payment-widget</a></td></tr><tr><td></td><td><span data-gb-custom-inline data-tag="emoji" data-code="2139">ℹ️</span> View Source</td><td></td><td><a href="https://github.com/RequestNetwork/web-components/tree/main/packages/payment-widget">https://github.com/RequestNetwork/web-components/tree/main/packages/payment-widget</a></td></tr></tbody></table>

The Payment Widget allows builders to accept crypto payments on their websites with minimal integration. It is built using [Svelte](https://svelte.dev/) but complied into a [Web Component](https://developer.mozilla.org/en-US/docs/Web/API/Web\_components), making it usable in any web environment, regardless of the framework.



## Installation

```bash
npm install @requestnetwork/payment-widget
```

## Usage

### Usage in React and Next.js

```tsx
import PaymentWidget from "@requestnetwork/payment-widget/react";

export default function PaymentPage() {
  return (
    <PaymentWidget
      sellerInfo={{
        logo: "https://example.com/logo.png",
        name: "Example Store",
      }}
      productInfo={{
        name: "Digital Art Collection",
        description: "A curated collection of digital artworks.",
        image: "https://example.com/product-image.jpg",
      }}
      amountInUSD={1.5}
      sellerAddress="0x1234567890123456789012345678901234567890"
      supportedCurrencies={["REQ-mainnet","ETH-sepolia-sepolia","USDC-mainnet"]}
      persistRequest={true}
      onPaymentSuccess={(request) => {
        console.log(request);
      }}
      onError={(error) => {
        console.error(error);
      }}
    />
  );
}
```



## Props

| Prop                    | Type              | Description                                                                                        |
| ----------------------- | ----------------- | -------------------------------------------------------------------------------------------------- |
| amountInUSD             | number            | The total of the purchase in US Dollars                                                            |
| sellerAddress           | string            | Address that would accept the payments                                                             |
| supportedCurrencies     | string\[]         | An array of currency IDS that are supported by the seller                                          |
| sellerInfo.logo         | string            | (Optional) Seller logo                                                                             |
| sellerInfo              | SellerInfo        | (Optional) Information about the seller                                                            |
| sellerInfo.name         | string            | (Optional) Seller name                                                                             |
| productInfo             | ProductInfo       | (Optional) Information about the product                                                           |
| productInfo.name        | string            | (Optional) Name of the product                                                                     |
| productInfo.description | string            | (Optional) Description of the product                                                              |
| productInfo.image       | string            | (Optional) Product image                                                                           |
| persistRequest          | boolean           | (Optional) Defaults to true, when set to false the request data is not persisted to the blockchain |
| showRNBranding          | boolean           | (Optional) Defaults to true, when set to false the "Powered by Request Network" banner is hidden   |
| builderId               | string            | (Optional) An ID added to request to identify request created by builder                           |
| onPaymentSuccess        | (request) => void | (Optional) Event that returns the Request data once the payment is successful.                     |
| onError                 | (error) => void   | (Optional) Event that returns the error when something goes wrong.                                 |

