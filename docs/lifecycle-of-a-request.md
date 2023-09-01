# Lifecycle of a Request

The typical lifecycle of a request is as follows:

<div data-full-width="true">

<figure><img src=".gitbook/assets/Lifecycle of a Request.jpg" alt=""><figcaption><p>Typical Lifecycle of a Request</p></figcaption></figure>

</div>

## Create a request

* The payer or payee signs the request which contains the payee, payer, currency, amount, payment details, and arbitrary content data.
* The request can be optionally encrypted such that only the payee, payer, and approved 3rd parties can view the request contents.
* The request is persisted in IPFS.&#x20;
* The IPFS Content-addressable ID (CID) is stored in a smart contract on Gnosis chain

{% hint style="info" %}
Requests are _created_ by storing their CIDs on Gnosis, but this doesn't mean _payment_ must occur on Gnosis. _Payment_ can occur on any of the supported chains including 20+ EVM-compatible chains or NEAR.
{% endhint %}

## Update a request

* The payee can optionally cancel the request or increase/decrease the expected amount.
* The payer can optionally accept the request, indicating that they intend to pay it.
* Both payee and payer can add 3rd party stakeholders if the request is encrypted.

## Pay a request

* The payer derives a paymentReference from the request contents.
* The payer calls a function on the payment network smart contract, passing in the token address, to address, amount, and paymentReference.
* An event is emitted containing the token address, to address, amount, and paymentReference.

{% hint style="info" %}
Most requests are "reference-based" meaning that a paymentReference derived from the request contents is logged on-chain via a smart contract that emits an event. Nothing gets written back to IPFS when paying a "reference-based" request.&#x20;

The exception is when paying a "declarative" request, in which case, data _is_ written back to IPFS. This includes when the payer declares that the payment was sent and the payee declares that the payment was received.
{% endhint %}

## Retrieve a request / Detect a payment

* The event is indexed by the payments subgraph
* An app can retrieve the request contents from IPFS and calculate the balance based on events from the payments subgraph.

{% hint style="info" %}
The request balance is calculated by adding up all the on-chain payment events with the same paymentReference. Partial payments are possible.
{% endhint %}

All of these steps are facilitated by the Request Network JavaScript SDK such that the developer needs only make a few function calls. See the [Quickstart](get-started/quickstart-browser.md) to learn more.
