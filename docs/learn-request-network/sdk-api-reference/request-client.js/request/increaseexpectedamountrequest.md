# increaseExpectedAmountRequest()

## Description

Increase the expected amount

## Parameters

<table data-full-width="true"><thead><tr><th>Name</th><th>Type</th><th>Required?<select><option value="cc244d24a18446b6acfe6740883eb7b8" label="required" color="blue"></option><option value="c086eb7ae26b445f82d1acb9f55c9a6d" label="recommended" color="blue"></option><option value="109f12ad18c8471b813348912808365e" label="optional" color="blue"></option></select></th><th>Description</th></tr></thead><tbody><tr><td>deltaAmount</td><td>number | string</td><td><span data-option="cc244d24a18446b6acfe6740883eb7b8">required</span></td><td>The amount by which to increase the expected amount</td></tr><tr><td>signerIdentity</td><td><a href="../iidentity.md">IIdentity</a></td><td><span data-option="cc244d24a18446b6acfe6740883eb7b8">required</span></td><td>The value returned by <code>getRequestFromId()</code></td></tr><tr><td>refundInformation</td><td>any</td><td><span data-option="109f12ad18c8471b813348912808365e">optional</span></td><td>Depends on the payment network</td></tr></tbody></table>

## Returns

Promise<[IRequestDataWithEvents](../irequestdatawithevents.md)>
