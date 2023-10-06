# EthereumPrivateKeyDecryptionProvider

## Description

Decrypt using a private key outside of a wallet

## Usage

```typescript
import { EthereumPrivateKeyDecryptionProvider } from "@requestnetwork/epk-decryption";
```

## Constructor Parameters

<table data-full-width="true"><thead><tr><th>Name</th><th>Type</th><th data-type="checkbox">Required?</th><th>Description</th></tr></thead><tbody><tr><td>decryptionParameters</td><td><a href="ethereumprivatekeydecryptionprovider.md#idecryptionparameters">IDecryptionParameters</a></td><td>true</td><td>Decryption method and private key</td></tr></tbody></table>

## Types and Interfaces

### IDecryptionParameters

<table data-full-width="true"><thead><tr><th>Name</th><th>Type</th><th data-type="checkbox">Required?</th><th>Description</th></tr></thead><tbody><tr><td>method</td><td><a href="ethereumprivatekeydecryptionprovider.md#types.decryption.method">Types.Decryption.METHOD</a></td><td>true</td><td>Decryption method</td></tr><tr><td>key</td><td>string</td><td>true</td><td>Private key</td></tr></tbody></table>

### Types.Decryption.METHOD

<table data-full-width="true"><thead><tr><th>Name</th><th>Value</th><th></th></tr></thead><tbody><tr><td>ECIES</td><td>'ecies'</td><td>Elliptic Curve Integrated Encryption Scheme (ECIES). An asymmetric key cipher.</td></tr><tr><td>AES256_CBC</td><td>'aes256-cbc'</td><td>Advanced Encryption Standard (AES). A symmetric key cipher with keys of length 256 in CBC mode.</td></tr><tr><td>AES256_GCM</td><td>'aes256-gcm'</td><td>Advanced Encryption Standard (AES). A symmetric key cipher with keys of length 256 in GCM mode.</td></tr></tbody></table>
