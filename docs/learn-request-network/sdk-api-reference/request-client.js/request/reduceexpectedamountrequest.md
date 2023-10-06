# reduceExpectedAmountRequest()

## Description

Reduce the expected amount

## Parameters

<table data-full-width="true"><thead><tr><th>Name</th><th>Type</th><th data-type="select">Required?</th><th>Description</th></tr></thead><tbody><tr><td>deltaAmount</td><td>number | string</td><td></td><td>The amount by which to reduce the expected amount</td></tr><tr><td>signerIdentity</td><td><a href="reduceexpectedamountrequest.md#iidentity">IIdentity</a></td><td></td><td>The value returned by <code>getRequestFromId()</code></td></tr><tr><td>paymentInformation</td><td>any</td><td></td><td>Depends on the payment network</td></tr></tbody></table>

## Returns

Promise<[IRequestDataWithEvents](../irequestdatawithevents.md)>

### IIdentity

<table data-full-width="true"><thead><tr><th>Name</th><th>Type</th><th data-type="checkbox">Required</th><th>Description</th></tr></thead><tbody><tr><td>type</td><td><a href="reduceexpectedamountrequest.md#types.identity.type">Types.Identity.TYPE</a></td><td>true</td><td>Identity type</td></tr><tr><td>value</td><td>string</td><td>true</td><td>Identity address</td></tr></tbody></table>

### Types.Identity.TYPE

<table data-full-width="true"><thead><tr><th>Name</th><th>Value</th><th>Description</th></tr></thead><tbody><tr><td>ETHEREUM_ADDRESS</td><td>'ethereumAddress'</td><td>Externally owned account (EOA)</td></tr><tr><td>ETHEREUM_SMART_CONTRACT</td><td>'ethereumSmartContract'</td><td>Smart Contract. Not supported.</td></tr></tbody></table>

hello
