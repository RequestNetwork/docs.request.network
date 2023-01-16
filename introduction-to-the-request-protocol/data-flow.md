# Data flow

This page presents the flow of data that occurs when some actions are performed in the protocol.

### Creating and updating requests

The next schemas show the data flow that happens when a user performs an `accept` action on a request.

#### Request Logic

&#x20;_Request Logic flow_

&#x20;_Request Logic flow with extension data_

#### Transaction

&#x20;_Transaction flow without encryption_

&#x20;_Transaction flow with encryption with two stakeholders_

#### Data-access

&#x20;_Data-access flow. In this example, several transactions are batched into the block. This feature is not yet implemented._

#### Storage

&#x20;_A new block is added to the storage_

### Reading requests

The next schemas show the data flow when the user wants to read the content of a request.

In this case, the user calls this function of Request Logic: `getRequestFromId(0xaaa)` that reads the request with the request id: 0xaaa

#### Storage

There is a permanent data flow between Data Access and Storage layers.

For performance purposes, Data Access will periodically synchronize with the current state of Storage. When a new, not synchronized block is detected, the block content will be dispatched into the Data Access cache.

&#x20;_Flow for Data Access synchronization_

#### Data-access

&#x20;_Flow from Data-Access. When a user wants to read a request, Data-Access will read its cache without any communication with the storage layer_

#### Transaction

&#x20;_Flow from Transaction layer. If the request is encrypted, the transactions are decrypted in this layer_

#### Request Logic

&#x20;_Request Logic flow. Request Logic will compute the state of the request based on the list of actions. In this case, the increaseExpectedAmount action has been signed by the payer_

Some actions from the Transaction layer can be invalid; this is the role of Request Logic to filter them to give the consistent state of the request to the user.

For example, only the payer of the request can increase the expected amount of it. If the action `increaseExpectedAmount` is signed by the payee therefore, the action is ignored.

&#x20;_In this example, the increaseExpectedAmount is signed by the payee; therefore, invalid. The expectedAmount of the request keeps its initial value: 5_
