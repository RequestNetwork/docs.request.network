# fromTopic()

## Description

Retrieve an array of requests from a topic

## Parameters

<table data-full-width="true"><thead><tr><th>Name</th><th>Type</th><th data-type="checkbox">Required?</th><th>Description</th></tr></thead><tbody><tr><td>topic</td><td>string</td><td>true</td><td>Topic string</td></tr><tr><td>updatedBetween</td><td><a href="fromtopic.md#types.itimestampboundaries">iTimestampBoundaries</a></td><td>false</td><td>Start time and end time</td></tr><tr><td><a href="fromtopic.md#options">options</a></td><td>Object</td><td>false</td><td>Options</td></tr></tbody></table>

## Returns

Promise<[Request](../request/)\[]>

## Types and Interfaces

### Types.ITimestampBoundaries

<table data-full-width="true"><thead><tr><th>Name</th><th>Type</th><th data-type="checkbox">Required?</th><th>Description</th></tr></thead><tbody><tr><td>from</td><td>number (Unix timestamp)</td><td>false</td><td>Start time</td></tr><tr><td>to</td><td>number (Unix timestamp)</td><td>false</td><td>End time</td></tr></tbody></table>

### options

<table data-full-width="true"><thead><tr><th width="244">Name</th><th width="261">Type</th><th width="112" data-type="checkbox">Required?</th><th>Description</th></tr></thead><tbody><tr><td>disablePaymentDetection</td><td>boolean</td><td>false</td><td>Disables payment detection</td></tr><tr><td>disableEvents</td><td>boolean</td><td>false</td><td>Disabled events</td></tr></tbody></table>
