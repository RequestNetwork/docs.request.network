# Request

## Description

An object used to interact with requests. Returned by [createRequest()](../requestnetwork/createrequest.md) and \_createEncryptedRequest()

## Properties

<table data-full-width="true"><thead><tr><th></th><th></th><th></th></tr></thead><tbody><tr><td>requestId</td><td>string</td><td>The ID of the request. Hash derived from the request contents. Stored alongside the request contents so can be used to look up a request.</td></tr></tbody></table>

## Instance Methods

<table data-full-width="true"><thead><tr><th>Name</th><th>Description</th></tr></thead><tbody><tr><td><a href="waitforconfirmation.md">waitForConfirmation()</a></td><td>Wait for request to be persisted and indexed</td></tr><tr><td><a href="getdata.md">getData()</a></td><td>Unwrap the request contents</td></tr><tr><td><a href="refresh.md">refresh()</a></td><td>Refresh the request data and balance</td></tr><tr><td><a href="cancel.md">cancel()</a></td><td>Cancel a request</td></tr><tr><td><a href="accept.md">accept()</a></td><td>Accept a request</td></tr><tr><td><a href="increaseexpectedamountrequest.md">increaseExpectedAmountRequest()</a></td><td>Increase the expected amount</td></tr><tr><td><a href="reduceexpectedamountrequest.md">reduceExpectedAmountRequest()</a></td><td>Reduce the expected amount</td></tr><tr><td>Others...</td><td>Other features exist. Docs coming soon...</td></tr></tbody></table>
