# Contracts

{% hint style="warning" %}
This page is missing the RequestToken, DAIbasedREQBurner, lockForREQBurn, ChainlinkConversionPath, Erc20ConversionProxy, ERC20SwapToConversion, EthConversionProxy, and ERC20TransferableReceivable contracts
{% endhint %}

## Contracts Overview

Request Network smart contracts are available [here](https://github.com/RequestNetwork/requestNetwork/tree/master/packages/smart-contracts/src/contracts).

## Contracts type

There are three types of contracts

* Request Storage
* Payments
* Conversion

### Request Storage

#### [RequestOpenHashSubmitter](https://github.com/RequestNetwork/requestNetwork/blob/master/packages/smart-contracts/src/contracts/RequestOpenHashSubmitter.sol)

Declares data hashes and collects the fees.

After a request has been sent to IPFS, the hash is declared to the whole request network system through the RequestHashStorage.

Anyone can submit hashes.

#### [StorageFeeCollector](https://github.com/RequestNetwork/requestNetwork/blob/master/packages/smart-contracts/src/contracts/StorageFeeCollector.sol)

Manages the fees for the creation of a request.

#### [RequestHashStorage](https://github.com/RequestNetwork/requestNetwork/blob/master/packages/smart-contracts/src/contracts/RequestHashStorage.sol)

This contract is the entry point to retrieve all the hashes of the request network system.

### Payments

#### [ERC20FeeProxy](https://github.com/RequestNetwork/requestNetwork/blob/master/packages/smart-contracts/src/contracts/ERC20FeeProxy.sol)

Performs an ERC20 token transfer with a payment reference and a transfer to a second address for the payment of a fee.

#### [ERC20Proxy](https://github.com/RequestNetwork/requestNetwork/blob/master/packages/smart-contracts/src/contracts/ERC20Proxy.sol)

Performs an ERC20 token transfer with a payment reference and a transfer to a second address.

#### [EthereumFeeProxy](https://github.com/RequestNetwork/requestNetwork/blob/master/packages/smart-contracts/src/contracts/EthereumFeeProxy.sol)

This contract performs an Ethereum transfer with a Fee sent to a third address and stores a reference.

#### [EthereumProxy](https://github.com/RequestNetwork/requestNetwork/blob/master/packages/smart-contracts/src/contracts/EthereumProxy.sol)

This contract performs an Ethereum transfer sent to a third address and stores a reference.

#### [ERC20EscrowToPay](https://github.com/RequestNetwork/requestNetwork/blob/master/packages/smart-contracts/src/contracts/ERC20EscrowToPay.sol)

This contract allows users to lock funds in an escrow and perform payments in ERC20. It contains a refund and emergency feature to unlock funds if needed.

#### [BatchConversionPayments](https://github.com/RequestNetwork/requestNetwork/blob/master/packages/smart-contracts/src/contracts/BatchConversionPayments.sol)

This contract makes multiple conversion payments with a payment references, in one transaction.

#### [BatchNoConversionPayments](https://github.com/RequestNetwork/requestNetwork/blob/master/packages/smart-contracts/src/contracts/BatchNoConversionPayments.sol)

This contract makes multiple payments with payment references, in one transaction.

#### [BatchPayments](https://github.com/RequestNetwork/requestNetwork/blob/master/packages/smart-contracts/src/contracts/BatchPayments.sol)

This contract makes multiple payments with references, in one transaction, without conversion.

#### [ERC20SwapToPay](https://github.com/RequestNetwork/requestNetwork/blob/master/packages/smart-contracts/src/contracts/ERC20SwapToPay.sol)

This contract swaps ERC20 tokens before paying a request, thanks to a payment proxy.
