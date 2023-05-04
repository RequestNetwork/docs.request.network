# Retrieve a request

### Request Data

```typescript
/** Interface request data */
export interface IRequestData extends Omit<RequestLogic.IRequest, 'currency'> {
  currency: string;
  meta: RequestLogic.IReturnMeta | null;
  balance: Payment.IBalanceWithEvents<any> | null;
  contentData: any;
  currencyInfo: RequestLogic.ICurrency;
  pending: RequestLogic.IPendingRequest | null;
}
```

### Retrieve a request by ID

#### fromRequestId Function Signature

```typescript
  /**
   * Create a Request instance from an existing Request's ID
   *
   * @param requestId The ID of the Request
   * @param options options
   * @returns the Request
   */
  public async fromRequestId(
    requestId: RequestLogicTypes.RequestId,
    options?: {
      disablePaymentDetection?: boolean;
      disableEvents?: boolean;
    },
  ): Promise<Request> 
```

#### fromRequestId Usage Example

```tsx
import { RequestNetwork, Types } from '@requestnetwork/request-client.js';

const requestId = '01c9190b6d015b3a0b2bbd0e492b9474b0734ca19a16f2fda8f7adec10d0fa3e7a'
const request = await requestNetwork.fromRequestId(requestId);
const requestData = await request.getData();
```

### Retrieve requests for a given Ethereum address

#### fromIdentity Function Signature

```typescript
  /**
   * Create an array of request instances from an identity
   *
   * @param updatedBetween filter the requests with time boundaries
   * @param options options
   * @returns the Requests
   */
  public async fromIdentity(
    identity: IdentityTypes.IIdentity,
    updatedBetween?: Types.ITimestampBoundaries,
    options?: { disablePaymentDetection?: boolean; disableEvents?: boolean },
  ): Promise<Request[]>
```

#### fromIdentity Usage Example

<pre class="language-tsx"><code class="lang-tsx"><strong>import { RequestNetwork, Types } from '@requestnetwork/request-client.js';
</strong>
<strong>const address = '0x7eB023BFbAeE228de6DC5B92D0BeEB1eDb1Fd567';
</strong><strong>const requests = await requestNetwork.fromIdentity(
</strong>  {
    type: Types.Identity.TYPE.ETHEREUM_ADDRESS,
    value: address,
  },
);
</code></pre>

### Compute a Request ID without creating it

<pre class="language-typescript"><code class="lang-typescript">/** Create request parameters */
export interface ICreateRequestParameters {
  requestInfo: RequestLogic.ICreateParameters | IRequestInfo;
  signer: Identity.IIdentity;
  paymentNetwork?: Payment.PaymentNetworkCreateParameters;
  topics?: any[];
  contentData?: any;
  disablePaymentDetection?: boolean;
  disableEvents?: boolean;
}
<strong>  
</strong><strong>  /**
</strong>   * Gets the ID of a request without creating it.
   *
   * @param requestParameters Parameters to create a request
   * @returns The requestId
   */
  public async computeRequestId(
    parameters: Types.ICreateRequestParameters,
  ): Promise&#x3C;RequestLogicTypes.RequestId>
</code></pre>
