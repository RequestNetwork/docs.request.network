---
description: An app for creating and paying requests using the Request Network API.
---

# EasyInvoice: API Demo App

<figure><img src="../.gitbook/assets/Screenshot from 2025-02-13 14-48-47.png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/Screenshot from 2025-02-14 01-00-51.png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/Screenshot from 2025-02-14 01-01-00.png" alt=""><figcaption></figcaption></figure>

<table data-card-size="large" data-view="cards" data-full-width="false"><thead><tr><th></th><th></th><th></th><th data-hidden data-card-target data-type="content-ref"></th></tr></thead><tbody><tr><td></td><td><span data-gb-custom-inline data-tag="emoji" data-code="1f579">🕹️</span> <strong>Try it out</strong></td><td></td><td><a href="https://easyinvoice.request.network">https://easyinvoice.request.network</a></td></tr><tr><td></td><td><span data-gb-custom-inline data-tag="emoji" data-code="2139">ℹ️</span> <strong>View Source</strong></td><td></td><td><a href="https://github.com/RequestNetwork/easy-invoice">https://github.com/RequestNetwork/easy-invoice</a></td></tr></tbody></table>

### Overview

Easy Invoice is a web application built with Next.js that allows users to create and manage invoices, accepting payments in cryptocurrencies via the Request Network API. It mimics Web2 apps in its functionalities, providing a user-friendly experience with Google login and real-time updates.

### Key Features

* **Google Login**: Securely log in to your account using Google OAuth.
* **Dashboard**: View key metrics and a table of your invoices, with a prompt to create a new invoice if none exist.
* **Invoice Creation**: A simple form to create invoices with real-time updates.
  * Client name and email fields.
  * Amount and notes fields.
  * Invoice currency and payment currency options, supporting currency conversion via the Request Network API.
* **Payment Section**:
  * Shows the payee address.
  * A stepper indicates the two steps of payment.
  * Wallet integration for connecting to accounts.
  * Payment initiation steps, including getting the transaction and sending the payment.
* **Real-time Updates**: The app receives webhooks from the Request Network API to update the invoice status in real-time.

### Usage

1. **Login:** Log in with your Google account to access the dashboard.
2. **Create Invoice:** Fill out the invoice form with the necessary details, including client information, amount, currency, and payee address.
3. **Pay Invoice:** Connect your wallet and initiate the payment. The app will guide you through the steps, including transaction confirmation.
4. **Monitor Status:** Track the payment status in real-time on the invoice details page and the dashboard.

### Currency Conversion

* If the invoice currency is USD, you must select a payment currency (Sepolia ETH or FAU) for conversion.
* For non-conversion scenarios, the payment currency will be the same as the invoice currency.

### Real-Time Updates

The application uses webhooks from the Request Network API to update invoice statuses automatically, ensuring that the dashboard and invoice details page reflect the latest information.
