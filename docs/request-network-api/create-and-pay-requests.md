# Create and Pay Requests

The Request Network API provides an interface for managing payment requests within your application. It simplifies creating, tracking, and paying requests. This includes generating requests with specific details, securely facilitating payments through blockchain transactions, and providing real-time notifications via webhooks to keep your application synchronized with payment statuses.

**Core Functionality:**

At its core, the Request Network API empowers you to:

* **Create Requests:** Define payment requests with essential information such as payee, payer (optional), amount, and currency details.
* **Facilitate Payments:** Guide users through the payment process, leveraging blockchain transactions for secure and transparent value transfer.
* **Deliver Webhook Notifications:** Receive instant updates on payment status changes, enabling your application to react dynamically to completed transactions.

**Workflow Overview:**

The following diagram illustrates the typical flow for creating and paying requests using the Request Network API:

```mermaid fullWidth="true"
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

{% swagger src="https://api.request.network/open-api/openapi.json" path="/v1/request" method="post" %}
[https://api.request.network/open-api/openapi.json](https://api.request.network/open-api/openapi.json)
{% endswagger %}

{% hint style="warning" %}
The `invoiceCurrency`and `paymentCurrency` strings must each be the `id` of a token listed in the [request-network-token-list.md](../general/request-network-token-list.md "mention")
{% endhint %}

{% swagger src="https://api.request.network/open-api/openapi.json" path="/v1/request/{paymentReference}" method="get" %}
[https://api.request.network/open-api/openapi.json](https://api.request.network/open-api/openapi.json)
{% endswagger %}

{% swagger src="https://api.request.network/open-api/openapi.json" path="/v1/request/{paymentReference}/pay" method="get" %}
[https://api.request.network/open-api/openapi.json](https://api.request.network/open-api/openapi.json)
{% endswagger %}

For detailed information on all available endpoints and their parameters, please refer to the full [Request Network API Reference](https://api.request.network/open-api)
