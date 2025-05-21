# Data flow

This page presents the flow of data that occurs when some actions are performed in the protocol.

### Creating and updating requests

The next schemas show the data flow that happens when a user performs an `accept` action on a request.

#### Request Logic

<figure><img src="../../.gitbook/assets/3-RequestLogicFlow (1).jpg" alt=""><figcaption><p><em>Request Logic flow</em></p></figcaption></figure>

<figure><img src="../../.gitbook/assets/3-AdvancedLogicFlow (1).jpg" alt=""><figcaption><p><em>Request Logic flow with extension data</em></p></figcaption></figure>

#### Transaction

<figure><img src="../../.gitbook/assets/3-TransactionFlow (1).jpg" alt=""><figcaption><p><em>Transaction flow without encryption</em></p></figcaption></figure>

<figure><img src="../../.gitbook/assets/3-TransactionFlowEncrypted (1).jpg" alt=""><figcaption><p><em>Transaction flow with encryption with two stakeholders</em></p></figcaption></figure>

#### Data-access

<figure><img src="../../.gitbook/assets/3-DataAccessFlow (1).jpg" alt=""><figcaption><p><em>Data-access flow. In this example, several transactions are batched into the block. This feature is not yet implemented.</em></p></figcaption></figure>

#### Storage

<figure><img src="../../.gitbook/assets/3-StorageFlow (1).jpg" alt=""><figcaption><p><em>A new block is added to the storage</em></p></figcaption></figure>

### Reading requests

The next schemas show the data flow when the user wants to read the content of a request.

In this case, the user calls this function of Request Logic: `getRequestFromId(0xaaa)` that reads the request with the request id: 0xaaa

#### Storage

There is a permanent data flow between Data Access and Storage layers.

For performance purposes, Data Access will periodically synchronize with the current state of Storage. When a new, not synchronized block is detected, the block content will be dispatched into the Data Access cache.

<figure><img src="../../.gitbook/assets/4-DataAccessAndStorageFlow (1).jpg" alt=""><figcaption><p><em>Flow for Data Access synchronization</em></p></figcaption></figure>

#### Data-access

<figure><img src="../../.gitbook/assets/4-DataAccessFlow (1).jpg" alt=""><figcaption><p><em>Flow from Data-Access. When a user wants to read a request, Data-Access will read its cache without any communication with the storage layer</em></p></figcaption></figure>

#### Transaction

<figure><img src="../../.gitbook/assets/4-TransactionFlow (1).jpg" alt=""><figcaption><p><em>Flow from Transaction layer. If the request is encrypted, the transactions are decrypted in this layer</em></p></figcaption></figure>

#### Request Logic

<figure><img src="../../.gitbook/assets/4-RequestLogicFlow (1).jpg" alt=""><figcaption><p><em>Request Logic flow. Request Logic will compute the state of the request based on the list of actions. In this case, the increaseExpectedAmount action has been signed by the payer</em></p></figcaption></figure>

Some actions from the Transaction layer can be invalid; this is the role of Request Logic to filter them to give the consistent state of the request to the user.

For example, only the payer of the request can increase the expected amount of it. If the action `increaseExpectedAmount` is signed by the payee therefore, the action is ignored.

<figure><img src="../../.gitbook/assets/4-RequestLogicFlowInvalid (1).jpg" alt=""><figcaption><p><em>In this example, the increaseExpectedAmount is signed by the payee; therefore, invalid. The expectedAmount of the request keeps its initial value: 5</em></p></figcaption></figure>
