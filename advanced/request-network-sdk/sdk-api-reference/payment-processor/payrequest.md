# payRequest()

## Usage

```typescript
import { payRequest } from "@requestnetwork/payment-processor";
```

## Parameters

<table data-full-width="true"><thead><tr><th>Name</th><th>Type</th><th data-type="checkbox">Required?</th><th>Description</th></tr></thead><tbody><tr><td>request</td><td><a href="../request-client.js/irequestdatawithevents.md#irequestdata">IRequestData</a></td><td>true</td><td>The request object</td></tr><tr><td>signerOrProvider</td><td>ethers.providers.Web3Provider | ethers.Signer = getProvider()</td><td>true</td><td>An ethers v5 Provider. See <a data-mention href="../../get-started/quickstart-browser.md#pay-a-request">#pay-a-request</a> for explanation how to wrap a viem WalletClient to look like an ethers v5 Provider.</td></tr><tr><td>amount</td><td>ethers.BigNumberish</td><td>false</td><td>The amount to pay. Defaults to the expected amount of the request.</td></tr><tr><td>overrides</td><td>Omit&#x3C;ethers.providers.TransactionRequest, 'to' | 'data' | 'value'></td><td>false</td><td>Override transaction settings like baseFee and maxPriorityFee</td></tr><tr><td>paymentSettings</td><td><a href="payrequest.md#iconversionpaymentsettings">ConversionPaymentSettings</a></td><td>false</td><td>Settings for conversion payments</td></tr></tbody></table>

## Returns

Promise\<ethers.ContractTransaction>

This is what ethers returns after submitting a transaction.

## Types and Interfaces

### IConversionPaymentSettings

<table data-full-width="true"><thead><tr><th>Name</th><th>Type</th><th data-type="checkbox">Required?</th><th>Description</th></tr></thead><tbody><tr><td>currency</td><td>ICurrency</td><td>true</td><td>The currency in which the payment is made, not the currency in which the request is denominated.</td></tr><tr><td>maxToSpend</td><td>ethers.BigNumberish</td><td>true</td><td>The maximum input currency to spend on conversion payments. Protects the user from rapidly changing exchange rates.</td></tr><tr><td>currencyManager</td><td>ICurrencyManager</td><td>true</td><td>A Currency manager handles a list of currencies and provides utility to retrieve and change format</td></tr></tbody></table>
