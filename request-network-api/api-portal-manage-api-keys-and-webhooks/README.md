---
description: An app for managing Request Network API keys and webhooks.
---

# API Portal: Manage API Keys and Webhooks

<figure><img src="../../.gitbook/assets/Screenshot from 2025-02-13 16-25-12.png" alt=""><figcaption></figcaption></figure>

<table data-card-size="large" data-view="cards" data-full-width="false"><thead><tr><th></th><th></th><th></th><th data-hidden data-card-target data-type="content-ref"></th></tr></thead><tbody><tr><td></td><td><span data-gb-custom-inline data-tag="emoji" data-code="1f579">🕹️</span> <strong>Try it out</strong></td><td></td><td><a href="https://portal.request.network">https://portal.request.network</a></td></tr></tbody></table>

## Overview

The Request Network API Portal provides app developers with a platform to securely manage their API keys and webhook endpoints.

## Key Features

### API Key Management

* **Create and Manage API Keys:** Users can create new API keys for authentication.
* **Toggle and Delete API Keys:** API keys can be toggled on and off, or deleted if no longer needed, enhancing control over API access.
* **Security Guidelines:** API keys are sensitive and should never be shared publicly. In case of compromise, users are advised to create a new key, update their code, and delete the compromised key.
* **Multiple Keys:** Allows the creation of multiple API keys for different environments or applications.

### Webhook Management

* **Create and Manage Webhooks:** App developers can configure webhook endpoints to receive real-time notifications for payment events.
* **Security Guidelines:** Each webhook request includes a signature in the \`x-request-network-signature\` header to ensure authenticity.
* **Signature Verification:** The signature is a SHA-256 HMAC of the request body, signed using the webhook secret.

Example Verification Code:

```javascript
import express from 'express';
import crypto from 'node:crypto';

const app = express();
const WEBHOOK_SECRET = 'your_webhook_secret';

app.post('/payment', async (req, res) => {
  const signature = req.headers['x-request-network-signature'];
  const expectedSignature = crypto
    .createHmac('sha256', WEBHOOK_SECRET)
    .update(JSON.stringify(req.body))
    .digest('hex');

  if (signature !== expectedSignature) {
    return res.status(401).json({
      success: false,
      message: 'Invalid signature'
    });
  }

  // Business logic here
  return res.status(200).json({ success: true });
});
```

## Usage

### Creating API Keys

* Navigate to the "API Keys" section.
* Click on "Create new key."
* Store the key securely and never share it publicly.

### Configuring Webhooks

* Navigate to the "Webhooks" section.
* Click on "Add webhook."
* Enter the endpoint URL and ensure the endpoint is secure and can handle incoming JSON payloads.

## Security Considerations

* **Keep API keys and webhook secrets secure.** Never expose them in public repositories or client-side code.
* **Verify all webhook signatures** to ensure authenticity and integrity.
* **Use HTTPS** for all endpoints to encrypt communication.
