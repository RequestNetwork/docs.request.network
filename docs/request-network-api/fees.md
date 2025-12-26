# Fees

### Protocol Fee

Request Network charges a protocol fee on all payments processed through the API.

### Fee Structure

| Fee Type     | Rate                   | Cap                                               |
| ------------ | ---------------------- | ------------------------------------------------- |
| Protocol Fee | 5 basis points (0.05%) | \~$25/€25 for main USD and EUR backed stablecoins |

The protocol fees applies to all the payment types

* ERC20 token payments
* Native currency payments (ETH, POL, etc.)
* Conversion payments
* Batch payments
* Crosschain payments
* Subscription (coming soon)

### Who Pays the Fee?

By default, the payer bears the protocol fee. The fee is added on top of the invoice amount.

#### Example

* Invoice amount: 1000 USDC
* Protocol fee: 1000 x 0.05% = 0.50 USDC
* Total paid by payer: 1000.50 USDC + gas fee
* Amount received by payee: 1000 USDC

For larger payments with stablecoins, the fee is capped:

* Payment of 100,000 USDC -> Fee capped at 25 USDC (not 50 USDC)

### Shifting the Fee to the Payee

If you want the payee to bear the protocol fee instead of the payer, simple reduce the invoice amount by 5bps (0.05%) before creating the request.

#### Example:

* Original intended payment: 1000 USDC
* Reduce by 0.05%: 1000 - (1000 x 0.05%) = 999.50 USDC
* Total paid by the payer: 1000 USDC + gas fee
* Amount received by payee: 999.95 USDC

This way, the payer pays approximately the original amount while the payee effectively absorbs the fee.

### Platform Fees

You can add your own platform fee on top of the protocol fee. Both fees are handled automatically when you configure a platform fee in your request.

To set up platform fees, include the following parameters in your payment request:

* `feePercentage` : Your platform fee percentage (e.g., "2.5").
* `feeAddress` : The wallet address to receive your platform fees.

When both protocol and platform fees are configured, the API automatically batches them into a single transaction for the payer.

#### Fee Information In Responses

Every API response includes the fee details in the metadata:

```json
  {
    "metadata": {
      "protocolFee": {
        "percentage": "0.05",
        "address": "0x..."
      },
      "platformFee": {
        "percentage": "2.5",
        "address": "0x..."
      }
    }
  }
```
