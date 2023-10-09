# increaseExpectedAmountRequest()

## Description

Increase the expected amount

## Parameters

<table data-full-width="true"><thead><tr><th>Name</th><th>Type</th><th data-type="select">Required?</th><th>Description</th></tr></thead><tbody><tr><td>deltaAmount</td><td>number | string</td><td></td><td>The amount by which to increase the expected amount</td></tr><tr><td>signerIdentity</td><td><a href="../iidentity.md">IIdentity</a></td><td></td><td>The value returned by <code>getRequestFromId()</code></td></tr><tr><td>refundInformation</td><td>any</td><td></td><td>Depends on the payment network</td></tr></tbody></table>

## Returns

Promise<[IRequestDataWithEvents](../irequestdatawithevents.md)>
