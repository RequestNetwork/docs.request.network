---
layout:
  title:
    visible: true
  description:
    visible: true
  tableOfContents:
    visible: true
  outline:
    visible: true
  pagination:
    visible: true
---

# Private Requests using Encryption

The Request Network protocol persists request contents on IPFS. When privacy is required, the SDK supports the creation and subsequent reading of encrypted requests.

This is achieved by combining public key (asymmetric) & AES (symmetric) encryption.

## Encryption

<figure><img src="../../.gitbook/assets/encrypting-requests-overview.drawio (1).svg" alt=""><figcaption><p>Request encryption overview</p></figcaption></figure>

The "request contents" are encrypted with a random AES key. This key is then encrypted with the public key of each stakeholder of the request. The stakeholders are the users and platforms who require access to the request contents. The encrypted request contents and the encrypted AES keys are then persisted to IPFS.

## Decryption

<figure><img src="../../.gitbook/assets/decrypting-requests-overview.drawio (1).svg" alt=""><figcaption><p>Request decryption overview</p></figcaption></figure>

The user retrieves the encrypted request content and the encrypted AES keys. They decrypt the AES  key using their private key. Then they decrypt  the request content using the AES key.

## Similarity to HTTPS

The privacy and encryption mechanism used by Request Network has many similarities to HTTPS. If you're not familiar with HTTPS, we recommend reading [HTTPS explained with carrier pigeons](https://www.freecodecamp.org/news/https-explained-with-carrier-pigeons-7029d2193351/)
