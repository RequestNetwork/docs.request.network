# Smart Contracts Overview

<table data-view="cards"><thead><tr><th></th><th></th><th></th><th data-hidden data-card-target data-type="content-ref"></th></tr></thead><tbody><tr><td></td><td><span data-gb-custom-inline data-tag="emoji" data-code="1f4eb">📫</span> Smart Contract Addresses</td><td></td><td><a href="../../general/supported-chains/smart-contract-addresses.md">smart-contract-addresses.md</a></td></tr><tr><td></td><td><span data-gb-custom-inline data-tag="emoji" data-code="2139">ℹ️</span> Smart Contract Source</td><td></td><td><a href="https://github.com/RequestNetwork/requestNetwork/tree/master/packages/smart-contracts/src/contracts">https://github.com/RequestNetwork/requestNetwork/tree/master/packages/smart-contracts/src/contracts</a></td></tr></tbody></table>

{% hint style="warning" %}
This page is missing the RequestToken, DAIbasedREQBurner, lockForREQBurn, ChainlinkConversionPath contracts
{% endhint %}

## Contracts Overview

Request Network smart contracts are available [here](https://github.com/RequestNetwork/requestNetwork/tree/master/packages/smart-contracts/src/contracts).

## Contracts type

There are three types of contracts

* **Storage** - These store [Content Identifiers (CIDs)](https://docs.ipfs.tech/concepts/content-addressing/) for Requests stored in IPFS.
* **Payments** - These process various payment types, also known as [Payment Networks](how-payment-networks-work.md), and are deployed across many [Supported Chains](../../general/supported-chains/).
* **REQ Token and Burn Mechanism** - These lock, bridge, and burn REQ tokens each time a Request is stored.

### Storage

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

This contract makes multiple payments with references, in one transaction, without conversion.

#### [ERC20SwapToPay](https://github.com/RequestNetwork/requestNetwork/blob/master/packages/smart-contracts/src/contracts/ERC20SwapToPay.sol)

This contract swaps ERC20 tokens before paying a request such that the payer sends currency A, but payee receives currency B.

{% hint style="info" %}
**Swap-to-Pay is different from Conversion.** For details see [#difference-between-conversion-swap-to-pay-and-swap-to-conversion](how-payment-networks-work.md#difference-between-conversion-swap-to-pay-and-swap-to-conversion "mention")
{% endhint %}

#### [ERC20ConversionProxy](https://github.com/RequestNetwork/requestNetwork/blob/master/packages/smart-contracts/src/contracts/Erc20ConversionProxy.sol)

This contract uses a chainlink price feed to pay a request denominated in one currency (usually a fiat currency like USD) but paid in an on-chain currency. This variant supports ERC20 payments.

#### [ETHConversionProxy](https://github.com/RequestNetwork/requestNetwork/blob/master/packages/smart-contracts/src/contracts/EthConversionProxy.sol)

This contract uses a chainlink price feed to pay a request denominated in one currency (usually a fiat currency like USD) but paid in an on-chain currency. This variant supports native currency payments.

#### [ERC20SwapToConversion](https://github.com/RequestNetwork/requestNetwork/blob/master/packages/smart-contracts/src/contracts/ERC20SwapToConversion.sol)

This contract combines "conversion" and "swap-to-pay". It executes an ERC20 swap before paying a request denominated in one currency (usually a fiat currency like USD) but paid in an on-chain currency. This variant supports ERC20 payments.

#### [ERC20TransferableReceivable](https://github.com/RequestNetwork/requestNetwork/blob/master/packages/smart-contracts/src/contracts/ERC20TransferableReceivable.sol)

This contract allows minting requests as NFTs thus allowing them to be transferred. The owner of the request NFT receives the payment.

#### [**ERC20SingleRequestProxy**](https://github.com/RequestNetwork/requestNetwork/blob/master/packages/smart-contracts/src/contracts/ERC20SingleRequestProxy.sol)

A contract that allows payment through the [#erc20feeproxy](contracts.md#erc20feeproxy "mention") without having to make a function call.

#### [**EthereumSingleRequestProxy**](https://github.com/RequestNetwork/requestNetwork/blob/master/packages/smart-contracts/src/contracts/EthereumSingleRequestProxy.sol)

A contract that allows payment through [#ethereumfeeproxy](contracts.md#ethereumfeeproxy "mention") without having to make a function call.

#### [**SingleRequestProxyFactory**](https://github.com/RequestNetwork/requestNetwork/blob/master/packages/smart-contracts/src/contracts/SingleRequestProxyFactory.sol)

A factory smart contract responsible for deploying [#erc20singlerequestproxy](contracts.md#erc20singlerequestproxy "mention") and [#ethereumsinglerequestproxy](contracts.md#ethereumsinglerequestproxy "mention") contracts.
