---
description: >-
  Request Network is creating a transparent, immutable ecosystem of financial
  products.
---

# What is Request Network?

<figure><img src=".gitbook/assets/LinkedIn_Personal_Header.jpg" alt=""><figcaption></figcaption></figure>

Blockchain transactions omit information necessary for accounting, compliance, and other financial services. This is a challenge for businesses that want to use blockchain technology.

Request Protocol stores the information needed by businesses to run their finances on-chain while maintaining compliance and privacy. Proving their financial activity on-chain unlocks a unique opportunity for companies to acquire income-backed services like loans, insurance, or fundraising. Moreover, it paves the way for automatic auditing, triple-entry accounting, and automatic tax reporting.

Users share payment requests securely via a standard, immutable storage medium. It enables a global cooperative financial system where people and organizations fully control their financial data and choices.

## The ecosystem we are building

Below, you can see a map of the builders we imagine that renewed transparent financial system to have, all using Request Protocol as a foundation for their users to have the privacy, security and data ownership they deserve.&#x20;

<div data-full-width="true">

<figure><img src=".gitbook/assets/image (1).png" alt=""><figcaption><p>Request Network Builders Map</p></figcaption></figure>

</div>

* **Payment applications**: wallets and payment modules benefit from the infrastructure provided, including payment requests and invoice creation, which leads to secure, fast, and easy transfers. The builders offer the protocol many transactions and new invoice data. They also provide users with fast, safe, and user-friendly payments that automatically create invoices that can be managed in builders dealing with invoicing applications.
* **Invoicing applications**: invoice, billing, and expenses apps benefit from the infrastructure provided, including payment requests and invoice creation, and the ecosystem that provides payment systems that can easily integrate with their platforms. They can offer extra features to their users, provide integrations with automatic audits and tax reports, and integrate with accounting apps in the protocol. These builders provide the protocol with recurring invoices data, easy connection to risk assessment and financial services, and easy entrance to lenders, insurance, and other risk-associated services. They also provide users with user-friendly applications to create, pay, and manage invoices and related services.
* **Accounting applications** benefit from the infrastructure provided, including payment requests and invoice creation, and the ecosystem that provides payment systems that can easily integrate with their platforms. They can offer extra features to their users, provide integrations with automatic audits and tax reports, and integrate with billing applications. These builders provide the protocol with recurring invoices, financial statement data, more specific data from client finances, and easy connection to risk assessment and financial services. They also provide users with user-friendly applications to create, pay, and manage their finances and related services.
* **Risk assessment, credit scores, and reputation services** benefit from the ecosystem that provides them with detailed data from clients’ finances and easy access to new clients from invoicing and accounting companies that want to use lenders, insurance, and other risk-related services. They provide the protocol with credit analysis, useful for the companies mentioned above, and a clear and transparent rating of clients’ finances to users.
* **Lenders, insurance, and other services associated with risk** benefit from the infrastructure that creates invoices as NFTs so payments can be associated with them. They can integrate with invoicing and accounting applications, provide integrations with automatic audits and tax reports, and offer new financial services data to other financial systems providers. These builders provide the protocol with new users for risk assessment, credit scores and reputation services, and extra services/features to the invoicing and accounting apps.
* **Auditing, tax reporting, and compliance services** benefit from the infrastructure that allows triple-entry accounting, which enables them to do automatic and real-time accounting, tax reporting, and compliance. They can integrate with billing and accounting applications and provide new, ongoing data to other financial systems providers and risk assessment. These builders provide users with cheap, automatic, real-time audits, tax reports, financial statements, etc.

Surely, many other companies fit in our protocol we just don't know it yet. But if you have any suggestions we are here to listen to them.

## Typical Lifecycle of a Request

<div data-full-width="true">

<figure><img src=".gitbook/assets/Lifecycle of a Request.png" alt=""><figcaption><p>Lifecycle of a Request</p></figcaption></figure>

</div>

* Create a request
  * The payer or payee signs the request which contains the payee, payer, currency, amount, payment details, and arbitrary content data.
  * The request can be optionally encrypted such that only the payee, payer, and approved 3rd parties can view the request contents.
  * The request is persisted in IPFS.&#x20;
  * The IPFS Content-addressable ID (CID) is stored in a smart contract on Gnosis chain

{% hint style="info" %}
Requests are _created_ by storing their CIDs on Gnosis, but this doesn't mean _payment_ must occur on Gnosis. _Payment_ can occur on any of the supported chains including 20+ EVM-compatible chains or NEAR.
{% endhint %}

{% hint style="info" %}
Once a request is created:&#x20;

* The payee can optionally cancel the request or increase/decrease the expected amount.
* The payer can optionally accept the request, indicating that they intend to pay it.
* Both payee and payer can add 3rd party stakeholders if the request is encrypted.
{% endhint %}

* Pay a request
  * The payer derives a `paymentReference` from the request contents.
  * The payer calls a function on the payment network smart contract, passing in the token address, to address, amount, and paymentReference.
  * An event is emitted containing the token address, to address, amount, and paymentReference.

{% hint style="info" %}
Most requests are "reference-based" meaning that a `paymentReference` derived from the request contents is logged on-chain via a smart contract that emits an event. Nothing gets written back to IPFS when paying a "reference-based" request.&#x20;

The exception is when paying a "declarative" request, in which case, data _is_ written back to IPFS. This includes when the payer declares that the payment was sent and the payee declares that the payment was received.
{% endhint %}

* Detect payment
  * The event is indexed by the payments subgraph
  * An app can retrieve the request contents from IPFS and calculate the balance based on events from the payments subgraph.

{% hint style="info" %}
The request balance is calculated by adding up all the on-chain payment events with the same `paymentReference`. Partial payments are possible.
{% endhint %}

All of these steps are facilitated by the Request Network JavaScript SDK such that the developer needs only make a few function calls. See the Quickstart to learn more.
