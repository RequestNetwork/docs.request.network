# Add Stakeholder

This template allows builders to quickly include the [Add Stakeholder](https://docs.request.finance/faq#i-am-integrating-the-request-network.-can-i-get-access-to-users-data-on-request-finance) widget from Request Finance in their existing applications.

The Request Finance Add Stakeholder widget allows third parties to request access to encrypted payment requests. Under the hood, Request Finance is calling `request.addStakeholders()` to add the third party as a stakeholder to the encrypted payment request.

The template comes in the form of a [Web Component](https://www.webcomponents.org/introduction#how-do-i-use-a-web-component-) and a native [Svelte](https://svelte.dev/) component, provided by the [@requestnetwork/add-stakeholder](https://www.npmjs.com/package/@requestnetwork/add-stakeholder)  package. The Web Component can be used anywhere including, but not limited to, React, Next.js, Vue, Svelte, or as a browser script.

## Installation

```bash
npm install @requestnetwork/add-stakeholder
```

## Usage

### Svelte

As a native svelte component

```javascript
import { AddStakeholder } from '@requestnetwork/add-stakeholder'
```

```html
<AddStakeholder builderKey="..." webhookUrl=".."/> 
```

As a web component

```javascript
import '@requestnetwork/add-stakeholder'
```

```html
<add-stakeholder builderKey="..." webhookUrl="..."/>
```

### React

```jsx
import '@requestnetwork/add-stakeholder'

export default function App() {
    return (
        <add-stakeholder builderKey="..." webhookUrl="..."/>
    )
}
```

### Browser

```html
<script src="./node_modules/add-stakeholder/dist/web-component.umd.cjs" defer></script>
<!-- or -->
<script src="//unpkg.com/@requestnetwork/add-stakeholder" defer></script>

<add-stakeholder builderKey="..." webhookUrl="..."/>
```
