# accept()

## Description

Accept a request

## Parameters

<table data-full-width="true"><thead><tr><th>Name</th><th>Type</th><th data-type="select">Required?</th><th>Description</th></tr></thead><tbody><tr><td>signerIdentity</td><td><a href="../iidentity.md">IIdentity</a></td><td></td><td>The value returned by <code>getRequestFromId()</code></td></tr><tr><td>refundInformation</td><td>any</td><td></td><td>Depends on the payment network</td></tr></tbody></table>

## Returns

Promise<[IRequestDataWithEvents](../irequestdatawithevents.md)>
