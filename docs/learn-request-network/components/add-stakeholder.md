# Add Stakeholder

The `add-stakeholder` component allows Builders to quickly integrate the [Request Finance Add Stakeholder](https://docs.request.finance/faq#i-am-integrating-the-request-network.-can-i-get-access-to-users-data-on-request-finance) widget into their applications.

The Request Finance Add Stakeholder widget provides a dialog box for end-users to grant third-party access to one of their encrypted payment requests created via Request Finance. Then, under the hood, Request Finance calls `request.addStakeholders()` to add the third party as a stakeholder to the encrypted payment request.

{% hint style="info" %}
Access is granted on a per-request basis. Once we prove market demand for this feature, we hope that Request Finance will improve its widget to allow granting access to multiple requests at a time.
{% endhint %}

The template comes in the form of a [Web Component](https://www.webcomponents.org/introduction#how-do-i-use-a-web-component-) and a native [Svelte](https://svelte.dev/) component, provided by the [@requestnetwork/add-stakeholder](https://www.npmjs.com/package/@requestnetwork/add-stakeholder) package. The Web Component can be used anywhere including, but not limited to, React, Next.js, Vue, Svelte, or as a browser script.

## Installation

```bash
npm install @requestnetwork/add-stakeholder
```

## Usage

### Web Component in React, Next.js, or Vue

```jsx
import '@requestnetwork/add-stakeholder'

export default function App() {
    return (
        <add-stakeholder builderKey="..." webhookUrl="..."/>
    )
}
```

### Native Svelte component

```javascript
import { AddStakeholder } from '@requestnetwork/add-stakeholder'
```

```html
<AddStakeholder builderKey="..." webhookUrl=".."/> 
```

### Web Component in Svelte

```javascript
import '@requestnetwork/add-stakeholder'
```

```html
<add-stakeholder builderKey="..." webhookUrl="..."/>
```

### Web Component in Browser

```html
<script src="./node_modules/add-stakeholder/dist/web-component.umd.cjs" defer></script>
<!-- or -->
<script src="//unpkg.com/@requestnetwork/add-stakeholder" defer></script>

<add-stakeholder builderKey="..." webhookUrl="..."/>
```
