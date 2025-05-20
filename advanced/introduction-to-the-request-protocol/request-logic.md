# Request Logic

This layer is responsible for the business logic of Request. This is where we define the data structure of a request.

This layer has three responsibilities:

* It defines the properties of the requests and the actions performed on them.
* It's responsible for the signature of the actions performed to ensure the request stakeholder identities.
* It manages extensions that can be created to extend the features of the Request Protocol through the Advanced Logic package.

[https://github.com/RequestNetwork/requestNetwork/tree/master/packages/request-logic](https://github.com/RequestNetwork/requestNetwork/tree/master/packages/request-logic)

#### Actions

Actions are the essential elements that compose a request. From this layer's point of view, a request is simply a list of different actions.

<figure><img src="../../.gitbook/assets/2-RequestPresentation.jpg" alt=""><figcaption><p> <em>Example of a request in Request Logic represented by a list of actions</em></p></figcaption></figure>

* The payee creates the request requesting 1 ETH to the payer
* The payer accepts the request
* The payer increases the expected amount of the request by 1 ETH (the expected amount of the request can only be increased by the payer and decreased by the payee)

Given the list of these actions, we can interpret the state of the request. The example above describes a request that has been accepted by the payer where he will have to pay 2 ETH to the payee.

Note that the hash of the action determines the request Id. Therefore, this action doesn't specify the request Id since it doesn't exist yet. The update actions (`accept` and `increaseExpectedAmount`) specify the request Id in their data.

There are two kinds of action:

* Create: This action is not related to an existing request, it will create a new one
* Update: All other actions, it will update the state of an existing request

#### Signature

In addition to providing the structure to form an action composing a request, the request logic layer is also responsible for signing the action.

To abstract the signing process from the layer (and eventually be able to use it in other packages), the signing process is done through external packages named signature providers.

The protocol repository currently contains two signature provider packages:

* [epk-signature](https://github.com/RequestNetwork/requestNetwork/tree/master/packages/epk-signature)
* [web3-signature](https://github.com/RequestNetwork/requestNetwork/tree/master/packages/web3-signature)

Both packages use the Elliptic Curve Digital Signature Algorithm (ECDSA) used in Ethereum. web3-signature will connect to Metamask to ask users to sign requests.  epk-signature uses private keys that are stored in the clear and managed manually.

The `web3-signature` provider should be used to create a fully-decentralized solution where the users manage their own private keys. The `epk-signature` provider is used to manage the private keys on behalf of the users. It's never a good idea to let users handle plain private keys.
