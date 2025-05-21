# Manage API Keys and Webhooks programmatically

This page details the endpoints for managing API keys and webhooks programmatically. These endpoints allow you to control access to the Request Network API, and receive real-time notifications about important events.

**Key Features:**

* **API Key Management:** Create, list, toggle, and delete API keys to control access to your platform's resources. API keys are essential for authenticating your application and ensuring secure communication with the Request Network API.
* **Webhook Management:** Configure and manage webhooks to receive real-time notifications about events within the Request Network. This enables your application to react instantly to payment confirmations, request updates, and other critical events.

**Authentication Requirement:**

Most of the endpoints described in this section require an active `session_token` cookie. This cookie is automatically set in your browser after a successful login. Make sure you've logged in before attempting to use these endpoints.

## Authentication

{% openapi src="https://api.request.network/open-api/openapi.json" path="/v1/auth/register" method="post" %}
[https://api.request.network/open-api/openapi.json](https://api.request.network/open-api/openapi.json)
{% endopenapi %}

{% openapi src="https://api.request.network/open-api/openapi.json" path="/v1/auth/login" method="post" %}
[https://api.request.network/open-api/openapi.json](https://api.request.network/open-api/openapi.json)
{% endopenapi %}

{% openapi src="https://api.request.network/open-api/openapi.json" path="/v1/auth/logout" method="post" %}
[https://api.request.network/open-api/openapi.json](https://api.request.network/open-api/openapi.json)
{% endopenapi %}

## Manage API Keys

{% openapi src="https://api.request.network/open-api/openapi.json" path="/v1/key" method="post" %}
[https://api.request.network/open-api/openapi.json](https://api.request.network/open-api/openapi.json)
{% endopenapi %}

{% openapi src="https://api.request.network/open-api/openapi.json" path="/v1/key" method="get" %}
[https://api.request.network/open-api/openapi.json](https://api.request.network/open-api/openapi.json)
{% endopenapi %}

{% openapi src="https://api.request.network/open-api/openapi.json" path="/v1/key/{id}" method="put" %}
[https://api.request.network/open-api/openapi.json](https://api.request.network/open-api/openapi.json)
{% endopenapi %}

{% openapi src="https://api.request.network/open-api/openapi.json" path="/v1/key/{id}" method="delete" %}
[https://api.request.network/open-api/openapi.json](https://api.request.network/open-api/openapi.json)
{% endopenapi %}

## Manage Webhooks

{% openapi src="https://api.request.network/open-api/openapi.json" path="/v1/webhook" method="post" %}
[https://api.request.network/open-api/openapi.json](https://api.request.network/open-api/openapi.json)
{% endopenapi %}

{% openapi src="https://api.request.network/open-api/openapi.json" path="/v1/webhook" method="get" %}
[https://api.request.network/open-api/openapi.json](https://api.request.network/open-api/openapi.json)
{% endopenapi %}

{% openapi src="https://api.request.network/open-api/openapi.json" path="/v1/webhook/{webhookId}" method="put" %}
[https://api.request.network/open-api/openapi.json](https://api.request.network/open-api/openapi.json)
{% endopenapi %}

{% openapi src="https://api.request.network/open-api/openapi.json" path="/v1/webhook/{webhookId}" method="delete" %}
[https://api.request.network/open-api/openapi.json](https://api.request.network/open-api/openapi.json)
{% endopenapi %}

For detailed information on all available endpoints and their parameters, please refer to the full [Request Network API Reference](https://api.request.network/open-api)
