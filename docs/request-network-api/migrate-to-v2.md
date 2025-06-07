---
description: >-
  A comprehensive guide to help you transition from V1 to V2 of the Request
  Network API
---

# Migrate to V2

{% hint style="warning" %}
**V1 API Deprecation Notice**

V1 of the Request Network API is deprecated and in security-fixes-only mode. This means:

* **Deprecated**: We strongly recommend upgrading to V2
* **Security-fixes-only**: V1 will only receive critical security patches, no new features, enhancements, or non-security bug fixes

Please migrate to V2 as soon as possible to ensure continued support and access to the latest features.
{% endhint %}

The Request Network API V2 introduces significant improvements while maintaining backward compatibility. This guide provides a comprehensive overview of the breaking changes between V1 and V2, along with a step-by-step migration guide.

{% hint style="info" %}
**Important**: V2 is designed to coexist with V1. You can migrate incrementally and don't need to migrate all endpoints at once.
{% endhint %}

## Breaking Changes from V1 to V2

### Core Architectural Changes

**Path Parameter Changes**

* **V1**: Uses `paymentReference` as the path parameter
* **V2**: Uses `requestId` as the path parameter

**Response Schema Standardization**

* **V1**: Returns `requestID` (uppercase D) in create responses
* **V2**: Returns `requestId` (lowercase d) in create responses

**Enhanced Validation**

* **V2**: Stricter type checking and validation schemas
* **V1**: More permissive validation

## API Interface Differences

### Endpoint Structure Changes

#### Request Endpoints

| Feature            | V1 Endpoint                                 | V2 Endpoint                          |
| ------------------ | ------------------------------------------- | ------------------------------------ |
| Create Request     | `POST /v1/request`                          | `POST /v2/request`                   |
| Get Request        | `GET /v1/request/{paymentReference}`        | `GET /v2/request/{requestId}`        |
| Get Request Status | `GET /v1/request/{paymentReference}/status` | `GET /v2/request/{requestId}/status` |

#### Payment Endpoints

| Feature              | V1 Endpoint                                 | V2 Endpoint                          |
| -------------------- | ------------------------------------------- | ------------------------------------ |
| Get Payment Calldata | `GET /v1/request/{paymentReference}/pay`    | `GET /v2/request/{requestId}/pay`    |
| Get Payment Routes   | `GET /v1/request/{paymentReference}/routes` | `GET /v2/request/{requestId}/routes` |

### Request Schema Differences

#### Create Request Schema

**V1 Schema:**

```json
{
  "amount": "string",
  "payee": "string",
  "invoiceCurrency": "string",
  "paymentCurrency": "string"
}
```

**V2 Schema:**

```json
{
  "amount": "string",
  "payee": "string",
  "invoiceCurrency": "string",
  "paymentCurrency": "string"
  // V2 accepts additional optional fields for extended functionality
}
```

#### Create Response Schema

**V1 Response:**

```json
{
  "requestID": "string",  // Note: uppercase D
  "paymentReference": "string"
}
```

**V2 Response:**

```json
{
  "requestId": "string",  // Note: lowercase d
  "paymentReference": "string"
}
```

### Payment Query Parameter Differences

#### Pay Request Query Parameters

**V1 Query Parameters:**

```typescript
interface PayRequestQueryV1 {
  payerAddress?: string;
  routeId?: string;
}
```

**V2 Query Parameters:**

```typescript
interface PayRequestQueryV2 {
  payerAddress?: string;
  routeId?: string;
  // Enhanced validation with stricter type checking
}
```

## Step-by-Step Migration Guide

### Phase 1: Assessment and Planning

1. **Audit Your Current Integration**
   * List all V1 endpoints you're currently using
   * Identify which features you need (basic payments vs advanced features)
2. **Choose Migration Strategy**
   * **Incremental**: Migrate endpoints one by one (recommended)
   * **Full Migration**: Switch all endpoints at once
   * **Parallel**: Run V1 and V2 side by side

### Phase 2: Update Path Parameters

#### Before (V1):

```typescript
// Get request status
const response = await fetch(`/v1/request/${paymentReference}/status`);

// Get payment calldata
const payData = await fetch(`/v1/request/${paymentReference}/pay?payerAddress=${address}`);
```

#### After (V2):

```typescript
// Get request status
const response = await fetch(`/v2/request/${requestId}/status`);

// Get payment calldata
const payData = await fetch(`/v2/request/${requestId}/pay?payerAddress=${address}`);
```

### Phase 3: Update Response Handling

#### Before (V1):

```typescript
const createResponse = await fetch('/v1/request', {
  method: 'POST',
  body: JSON.stringify(requestData)
});

const { requestID, paymentReference } = await createResponse.json();
// Note: requestID with uppercase D
```

#### After (V2):

```typescript
const createResponse = await fetch('/v2/request', {
  method: 'POST',
  body: JSON.stringify(requestData)
});

const { requestId, paymentReference } = await createResponse.json();
// Note: requestId with lowercase d
```

### Phase 4: Update Error Handling

V2 has enhanced error responses with more specific error codes:

```typescript
try {
  const response = await fetch('/v2/request', {
    method: 'POST',
    body: JSON.stringify(requestData)
  });

  if (!response.ok) {
    const error = await response.json();

    // V2 provides more detailed error information
    console.error('Error:', error.message);
    console.error('Status Code:', error.statusCode);
    console.error('Error Code:', error.error);
  }
} catch (error) {
  console.error('Request failed:', error);
}
```

### Phase 5: Testing and Validation

1. **Test Core Functionality**
   * Create requests using V2 endpoints
   * Verify payment flows work correctly
   * Check response formats match expectations
2. **Enhanced Validation Testing**
   * Test stricter type checking
   * Verify improved error responses
3. **Performance Testing**
   * Compare response times between V1 and V2
   * Test with realistic data volumes

## Support and Resources

* **Migration Support**: [Book a call](https://calendly.com/mariana-rn/request-network-demo-docs) with our team for migration assistance
* **GitHub Examples**: Check the `easy-invoice` repository for V2 implementation examples

## Backward Compatibility

V1 endpoints will continue to work during the migration period. However, we recommend migrating to V2 to access improvements and future features:

* Enhanced security and validation
* Better error handling and debugging
* Improved webhook events
* Access to new features as they are released

V2 is the foundation for all future Request Network API features and improvements.

