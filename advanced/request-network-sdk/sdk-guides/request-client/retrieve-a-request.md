# Compute a Request ID without creating the request

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
