---
description: >-
  The Request Network API provides an interface for creating and paying requests
  within your application.
---

# Create and Pay Requests

{% hint style="info" %}
**Talk to an expert**

Discover how Request Network API can enhance your app's features - [book a call](https://calendly.com/mariana-rn/request-network-demo-docs) with us.
{% endhint %}

## **Core Functionality**

At its core, the Request Network API empowers you to:

* **Create Requests:** Define payment requests with information such as payee, payer (optional), amount, currency, and recurrence (optional).
* **Facilitate Payments:** Return transaction calldata, ready to be signed by end-users and sent to the blockchain for secure and transparent value transfer.
* **Deliver Webhook Notifications:** Receive instant updates on payment status changes, enabling your application to react dynamically to completed transactions.

{% openapi src="https://api.request.network/open-api/openapi.json" path="/v1/request" method="post" %}
[https://api.request.network/open-api/openapi.json](https://api.request.network/open-api/openapi.json)
{% endopenapi %}

{% openapi src="https://api.request.network/open-api/openapi.json" path="/v1/request/{paymentReference}" method="get" %}
[https://api.request.network/open-api/openapi.json](https://api.request.network/open-api/openapi.json)
{% endopenapi %}

{% openapi src="https://api.request.network/open-api/openapi.json" path="/v1/request/{paymentReference}/stop-recurrence" method="patch" %}
[https://api.request.network/open-api/openapi.json](https://api.request.network/open-api/openapi.json)
{% endopenapi %}

{% openapi src="https://api.request.network/open-api/openapi.json" path="/v1/request/{paymentReference}/pay" method="get" %}
[https://api.request.network/open-api/openapi.json](https://api.request.network/open-api/openapi.json)
{% endopenapi %}

{% openapi src="https://api.request.network/open-api/openapi.json" path="/v1/pay" method="post" %}
[https://api.request.network/open-api/openapi.json](https://api.request.network/open-api/openapi.json)
{% endopenapi %}

For detailed information on all available endpoints and their parameters, please refer to the full [Request Network API Reference](https://api.request.network/open-api)

## Create and Pay Request Workflow

The following diagram illustrates the typical flow for creating and paying requests using the Request Network API:

```mermaid fullWidth="false"
---
config:
  fontSize: 10
  sequence:
    wrap: true
    actorMargin: 90
    width: 100
    height: 50
---
sequenceDiagram
    actor User
    participant App
    participant Request Network API
    participant Blockchain

    User->>App: Create Request
    App->>Request Network API: POST /request {apiKey, payee, payer?, amount, invoiceCurrency, paymentCurrency}
    Request Network API-->>App: 201 Created {requestId, paymentReference}

    User->>App: Pay Request
    App->>Request Network API: GET /request/{paymentReference}/pay {apiKey}
    Request Network API-->>App: 200 OK {transactions[calldata], metadata{stepsRequired, needsApproval, approvalTransactionIndex}}
    Request Network API-)Request Network API: Start listening {paymentReference}
    
    opt if needs approval 
        App->>User: Prompt for approval signature
        User-->>App: Sign approval transaction
        App->>Blockchain: Submit approval transaction
    end

    App->>User: Prompt for payment signature
    User-->>App: Sign payment transaction
    App->>Blockchain: Submit payment transaction

    Request Network API->>Request Network API: Payment detected, stop listening {paymentReference}
    Request Network API->>App: POST <webhook url> {"payment.confirmed", requestId, paymentReference, request scan link, timestamp}
    App-->>User: Payment Complete
```
