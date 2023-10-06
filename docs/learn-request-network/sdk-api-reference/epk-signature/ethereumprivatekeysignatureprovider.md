# EthereumPrivateKeySignatureProvider

## Description

Sign using a private key outside of a wallet

## Usage

```typescript
import { EthereumPrivateKeySignatureProvider } from "@requestnetwork/epk-signature";
```

## Constructor Paramters

<table data-full-width="true"><thead><tr><th>Name</th><th>Type</th><th data-type="checkbox">Required?</th><th>Description</th></tr></thead><tbody><tr><td>signatureParameter</td><td><a href="broken-reference">ISignatureParameters</a></td><td>true</td><td>Signing method and private key</td></tr></tbody></table>

## Types and Interfaces

### ISignatureParameters

<table data-full-width="true"><thead><tr><th>Name</th><th>Type</th><th data-type="checkbox">Required?</th><th>Description</th></tr></thead><tbody><tr><td>method</td><td><a href="broken-reference">Types.Signature.METHOD</a></td><td>true</td><td>Signing method</td></tr><tr><td>privateKey</td><td>string</td><td>true</td><td>Private key</td></tr></tbody></table>

### Types.Signature.METHOD

<table data-full-width="true"><thead><tr><th>Name</th><th>Value</th><th></th></tr></thead><tbody><tr><td>ECDSA</td><td>'ecdsa'</td><td>"Vanilla" ECDSA</td></tr><tr><td>ECDSA_ETHEREUM</td><td>'ecdsa-ethereum'</td><td>Ethereum ECDSA with the prefix "\x19Ethereum Signed Message:\n"</td></tr></tbody></table>
