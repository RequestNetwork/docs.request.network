# fromIdentity()

## Description

Retrieve an array of requests from an Identity

## Parameters

<table data-full-width="true"><thead><tr><th>Name</th><th>Type</th><th data-type="checkbox">Required?</th><th>Description</th></tr></thead><tbody><tr><td>identity</td><td><a href="fromidentity.md#iidentity">IIdentity</a></td><td>true</td><td>Identity type and value</td></tr><tr><td>updatedBetween</td><td><a href="fromidentity.md#types.itimestampboundaries">ITimestampBoundaries</a></td><td>false</td><td>Start time and end time</td></tr><tr><td><a href="fromidentity.md#options">options</a></td><td>Object</td><td>false</td><td>Options</td></tr></tbody></table>

## Returns

Promise<[Request](../request/)\[]>

## Types and Interfaces

### IIdentity

<table data-full-width="true"><thead><tr><th>Name</th><th>Type</th><th data-type="checkbox">Required?</th><th>Description</th></tr></thead><tbody><tr><td>type</td><td><a href="broken-reference">Types.Identity.TYPE</a></td><td>true</td><td>type of the identification</td></tr><tr><td>value</td><td>string</td><td>true</td><td>the identification</td></tr></tbody></table>

### Types.Identity.TYPE

<table data-full-width="true"><thead><tr><th>Name</th><th>Value</th><th>Description</th></tr></thead><tbody><tr><td>ETHEREUM_ADDRESS</td><td>'ethereumAddress'</td><td>Externally owned account (EOA)</td></tr><tr><td>ETHEREUM_SMART_CONTRACT</td><td>'ethereumSmartContract'</td><td>Smart Contract. Not supported.</td></tr></tbody></table>

### Types.ITimestampBoundaries

<table data-full-width="true"><thead><tr><th>Name</th><th>Type</th><th data-type="checkbox">Required?</th><th>Description</th></tr></thead><tbody><tr><td>from</td><td>number (Unix timestamp)</td><td>false</td><td>Start time</td></tr><tr><td>to</td><td>number (Unix timestamp)</td><td>false</td><td>End time</td></tr></tbody></table>

### options

<table data-full-width="true"><thead><tr><th>Name</th><th>Type</th><th data-type="checkbox">Required?</th><th>Description</th></tr></thead><tbody><tr><td>disablePaymentDetection</td><td>boolean</td><td>false</td><td>Disable payment detection</td></tr><tr><td>disableEvents</td><td>boolean</td><td>false</td><td>Disable events</td></tr></tbody></table>
