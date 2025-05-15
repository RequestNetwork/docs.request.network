# Configure the Request Client

## Configure the client

The following command creates a new Request Client instance and configures it to :

* Connect to the Gnosis Request Node Gateway maintained by the Request Network Foundation.
* Use the web3-signature package to create requests using a web3 wallet like Metamask.

```tsx
const web3SignatureProvider = new Web3SignatureProvider(provider);
const requestClient = new RequestNetwork({
  nodeConnectionConfig: { 
    baseURL: 'https://gnosis.gateway.request.network/' 
  },
  signatureProvider: web3SignatureProvider,
});
```

### Mock Storage

To create mock storage requests, where the request is stored in memory on the local machine and cleared as soon as the script is finished running, set the `useMockStorage` argument to `true` when instantiating the `RequestNetwork`object.

```typescript
const requestClient = new RequestNetwork({
  useMockStorage: true,
});
```
