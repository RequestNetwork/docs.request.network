# Advanced Logic

Simplicity is one of the most important characteristics we want to achieve in the Protocol. This is why the actions available in Request Logic are the minimal set of actions needed for any kind of request for payment. In the same way, the basic request state is universally common to any request, every request has a payee (a recipient), a currency (what requested), an expected amount (how much requested) and a basic state (accepted, canceled). To enable more advanced features for the users, we conceived Advanced Logic.

Advanced Logic is a package that allows the user to define extensions that can be added to the request. An extension is an isolated context inside the request that contains his actions and his state. For example, the extension `content-data` allows the user to add metadata to a request (e.g. the additional data needed for an invoice). The Advanced Logic layer is also where the payment networks allowing payment detection are implemented.

Similar to Request Logic, a specific extension can define different actions related to it. There is the Create action of the extension and, eventually different update actions. The extension is initialized at the same time as the request, and any action of the Request Logic can add extension data. There is a specific action, `AddExtensionData`, in Request Logic, only intended to add extension data to the request with no other side-effect.

&#x20;_Example of a request with extension data: the payee creates a request with content data and declarative payment information, the payer accepts the request and declares a sent payment in the same time, and finally, the payee declares the received payment_

The specification for each extension can be found at this link: [https://github.com/RequestNetwork/requestNetwork/tree/master/packages/advanced-logic/specs](https://github.com/RequestNetwork/requestNetwork/tree/master/packages/advanced-logic/specs)
