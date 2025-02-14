# IIdentity

## Description

An identity object that is used to uniquely identify a stakeholder in a request. Today, a stakeholder's `IIdentity` is expressed as an Ethereum address, but it is conceivable that future implementations may include alternative identity formats, perhaps W3C DIDs.&#x20;

Examples of `IIdentity` are: payee, payer, signer, creator, and any 3rd party that can view an encrypted request's contents.

{% hint style="warning" %}
The `payee` identity and `payer` identity are NOT necessarily the same as the payment recipient (`paymentAddress` in the [PaymentNetworkCreateParameters](requestnetwork/createrequest.md#paymentnetworkcreateparameters)) or the payment sender (`from` address in the payment-subgraph). Conceptually, the `payee` and `payer` identities are used only for notifications and access control, NOT payment routing.
{% endhint %}

<table data-full-width="true"><thead><tr><th>Name</th><th>Type</th><th data-type="checkbox">Required</th><th>Description</th></tr></thead><tbody><tr><td>type</td><td><a href="iidentity.md#types.identity.type">Types.Identity.TYPE</a></td><td>true</td><td>Identity type</td></tr><tr><td>value</td><td>string</td><td>true</td><td>Identity address</td></tr></tbody></table>

### Types.Identity.TYPE

<table data-full-width="true"><thead><tr><th>Name</th><th>Value</th><th>Description</th></tr></thead><tbody><tr><td>ETHEREUM_ADDRESS</td><td>'ethereumAddress'</td><td>Externally owned account (EOA)</td></tr><tr><td>ETHEREUM_SMART_CONTRACT</td><td>'ethereumSmartContract'</td><td>Smart contract account. Don't use this.<br><br>It is not possible to create or update a request using an <code>IIdentity</code> of this type. This type was added to support creating/updating requests using multisig wallets via EIP-1271 but this feature has not yet been implemented.</td></tr></tbody></table>

