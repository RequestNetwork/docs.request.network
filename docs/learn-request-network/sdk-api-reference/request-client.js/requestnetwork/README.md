# RequestNetwork

## Description

The Request Network client.

## Usage

```typescript
import { RequestNetwork } from "@requestnetwork/request-client.js";
```

## Constructor Paramters

<table data-full-width="true"><thead><tr><th>Name</th><th>Type</th><th>Required?<select><option value="81549c59cb544fd68de2cf027308c43f" label="required" color="blue"></option><option value="3b8add8e16774066a1622e5f31497c90" label="optional" color="blue"></option><option value="817820d3af4d4cacb66d2dbfa308cebe" label="recommended" color="blue"></option></select></th><th>Description</th></tr></thead><tbody><tr><td>nodeConnectionConfig</td><td><a href="./#axiosrequestconfig-properties">AxiosRequestConfig</a></td><td><span data-option="817820d3af4d4cacb66d2dbfa308cebe">recommended</span></td><td>Axios configurations</td></tr><tr><td>signatureProvider</td><td><a href="./#isignatureprovider-implementations">ISignatureProvider</a></td><td><span data-option="817820d3af4d4cacb66d2dbfa308cebe">recommended</span></td><td>Required to sign and create requests</td></tr><tr><td>decryptionProvider</td><td><a href="./#idecryptionprovider-implementations">IDecryptionProvider</a></td><td><span data-option="3b8add8e16774066a1622e5f31497c90">optional</span></td><td>Required to retrieve encrypted requests</td></tr><tr><td>httpConfig</td><td><a href="./#ihttpdataaccessconfig-properties">IHttpDataAccessConfig</a></td><td><span data-option="3b8add8e16774066a1622e5f31497c90">optional</span></td><td>Options for HTTP transport (timeout, delay, retry, etc.)</td></tr><tr><td>paymentOptions</td><td><a href="./#paymentnetworkoptions-properties">PaymentNetworkOptions</a></td><td><span data-option="3b8add8e16774066a1622e5f31497c90">optional</span></td><td>Payment detection options</td></tr><tr><td>useMockStorage</td><td>boolean</td><td><span data-option="3b8add8e16774066a1622e5f31497c90">optional</span></td><td>Store ephemeral requests in local memory</td></tr><tr><td>currencies</td><td>CurrencyInput[]</td><td><span data-option="3b8add8e16774066a1622e5f31497c90">optional</span></td><td>Custom currency list</td></tr><tr><td>currencyManager</td><td>ICurrencyManager</td><td><span data-option="3b8add8e16774066a1622e5f31497c90">optional</span></td><td>Custom currency manager (will override <code>currencies</code>). A Currency manager handles a list of currencies and provides utility to retrieve and change format</td></tr></tbody></table>

## Types and Interfaces

### AxiosRequestConfig Properties

<table data-full-width="true"><thead><tr><th>Name</th><th>Type</th><th>Required?<select><option value="81549c59cb544fd68de2cf027308c43f" label="required" color="blue"></option><option value="3b8add8e16774066a1622e5f31497c90" label="optional" color="blue"></option><option value="817820d3af4d4cacb66d2dbfa308cebe" label="recommended" color="blue"></option></select></th><th>Description</th></tr></thead><tbody><tr><td>baseUrl</td><td>string</td><td><span data-option="817820d3af4d4cacb66d2dbfa308cebe">recommended</span></td><td>Request Node URL</td></tr><tr><td>Many other properties...</td><td></td><td><span data-option="3b8add8e16774066a1622e5f31497c90">optional</span></td><td></td></tr></tbody></table>

### ISignatureProvider Implementations

<table data-full-width="true"><thead><tr><th>Type</th><th>Description</th></tr></thead><tbody><tr><td><a href="../../web3-signature/web3signatureprovider.md">Web3SignatureProvider</a></td><td>Sign using a private key inside of a wallet</td></tr><tr><td><a href="../../epk-signature/ethereumprivatekeysignatureprovider.md">EthereumPrivateKeySignatureProvider</a></td><td>Sign using a private key outside of a wallet</td></tr></tbody></table>

### IDecryptionProvider Implementations

<table data-full-width="true"><thead><tr><th>Type</th><th>Description</th></tr></thead><tbody><tr><td><a href="../../epk-decryption/ethereumprivatekeydecryptionprovider.md">EthereumPrivateKeyDecryptionProvider</a></td><td>Decrypt using a private key outside of a wallet</td></tr></tbody></table>

### IHttpDataAccessConfig Properties

<table data-full-width="true"><thead><tr><th>Name</th><th>Type</th><th data-type="checkbox">Required?</th><th>Description</th></tr></thead><tbody><tr><td>requestClientVersionHeader</td><td>string</td><td>false</td><td>Name of the header containing the client version</td></tr><tr><td>httpRequestMaxRetry</td><td>number</td><td>false</td><td>Maximum number of retries to attempt when http requests to the Node fail</td></tr><tr><td>httpRequestRetryDelay</td><td>number</td><td>false</td><td>Delay between retry in ms</td></tr><tr><td>httpRequestExponentialBackoffDelay</td><td>number</td><td>false</td><td>Exponential backoff delay in ms when requests to the Node fail</td></tr><tr><td>httpRequestMaxExponentialBackoffDelay</td><td>number</td><td>false</td><td>Maximum exponential backoff delay in ms when requests to the Node fail</td></tr><tr><td>getConfirmationMaxRetry</td><td>number</td><td>false</td><td>Maximum number of retries to get the confirmation of a persistTransaction</td></tr><tr><td>getConfirmationRetryDelay</td><td>number</td><td>false</td><td>Delay between retry in ms to get the confirmation of a persistTransaction</td></tr><tr><td>getConfirmationExponentialBackoffDelay</td><td>number</td><td>false</td><td>Exponential backoff delay in ms to get the confirmation of a persistTransaction</td></tr><tr><td>getConfirmationMaxExponentialBackoffDelay</td><td>number</td><td>false</td><td>Maximum exponential backoff delay in ms to get the confirmation of a persistTransaction</td></tr><tr><td>getConfirmationDeferDelay</td><td>number</td><td>false</td><td>Delay to wait in ms before trying for the first time to get the confirmation of a persistTransaction</td></tr></tbody></table>

### PaymentNetworkOptions Properties

<table data-full-width="true"><thead><tr><th>Name</th><th>Type</th><th data-type="checkbox">Required?</th><th>Description</th></tr></thead><tbody><tr><td>bitcoinDetectionProvider</td><td>IBitcoinDetectionProvider</td><td>false</td><td>Override default bitcoin payment detection</td></tr><tr><td>explorerApiKeys</td><td>Map&#x3C;ChainName, string></td><td>false</td><td>Override explorer API keys</td></tr><tr><td>getSubgraphClient</td><td>function(ChainName)</td><td>false</td><td>Override subgraph payment detection</td></tr><tr><td>getRpcProvider</td><td>function(ChainName)</td><td>false</td><td>Override RPC node provider</td></tr></tbody></table>

## Instance Methods

<table data-full-width="true"><thead><tr><th>Name</th><th>Description</th></tr></thead><tbody><tr><td><a href="createrequest.md">createRequest()</a></td><td>Create an unencrypted request</td></tr><tr><td>_createEncryptedRequest()</td><td>Create an encrypted request. Docs coming soon...</td></tr><tr><td><a href="computerequestid.md">computeRequestId()</a></td><td>Compute a request ID without actually creating a request</td></tr><tr><td><a href="fromrequestid.md">fromRequestId()</a></td><td>Retrieve a request from a requestId</td></tr><tr><td><a href="fromidentity.md">fromIdentity()</a></td><td>Retrieve an array of requests from an Identity</td></tr><tr><td><a href="fromtopic.md">fromTopic()</a></td><td>Retrieve an array of requests from a topic</td></tr></tbody></table>
