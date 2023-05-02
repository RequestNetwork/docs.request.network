# Transaction

The transaction layer is responsible for converting actions into a channel of transactions and vice versa. It also optionally encrypts and decrypts those transactions such that only stakeholders of the request can read them.

[https://github.com/RequestNetwork/requestNetwork/tree/master/packages/transaction-manager](https://github.com/RequestNetwork/requestNetwork/tree/master/packages/transaction-manager)

#### Encryption

Transactions can be stored in the clear, unencrypted, meaning that anyone can read the request. \
\
Transactions can also be encrypted, such that only stakeholders of the request can read them. Any number of stakeholders can be included in the creation of an encrypted request. Request uses an encryption scheme nearly identical to [HTTPS](https://www.freecodecamp.org/news/https-explained-with-carrier-pigeons-7029d2193351/). It uses a symmetric key (usually referred to as the "channel key") to encrypt and decrypt the transaction channel content, and asymmetric keys to encrypt and decrypt copies of the symmetric key.

* A unique channel key that is shared with all the stakeholders
* A set of public and private key pairs, each pair controlled by a single stakeholder

The channel key uses Advanced Encryption Standard (AES), a symmetric encryption technology; this means the same key is used to encrypt and decrypt.

The public and private key pairs use Elliptic Curve Integrated Encryption Scheme (ECIES), an asymmetric encryption technology where the public key encrypts and the private key decrypts.

Every transaction of the same request is encrypted with the same channel key. The encrypted transactions form the channel (hence the name channel key).&#x20;

The channel key is encrypted with each stakeholder's public key. This way, every stakeholder can decrypt the channel key and in turn, decrypt the transactions in the channel.

This design using both symmetric and asymmetric encryption allows the transaction data _once_ and only the channel key needs to be duplicated, once for each stakeholder.

See the details of encrypted request creation in [handling-encryption-with-the-js-library.md](../guides/handling-encryption-with-the-js-library.md "mention")
