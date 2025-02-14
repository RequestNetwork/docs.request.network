# Storage

Storage defines where the data are stored. How to store these data and how to retrieve them.

The currently used package, named `ethereum-storage`, uses IPFS to store the data immutably and uses the Ethereum network to persist the IPFS hash of the data and make them permanently available to everyone.

[https://github.com/RequestNetwork/requestNetwork/tree/master/packages/ethereum-storage](https://github.com/RequestNetwork/requestNetwork/tree/master/packages/ethereum-storage)

The storage of data implementation is:

* Open: Anyone should be able to access the data (though it can be encrypted)
* Decentralized: The database is trustless; we don’t have to refer to a third party to trust the data
* Resilient: The database should always be available, nobody should be able to shutdown it alone

#### IPFS

The interplanetary file system (IPFS) is a decentralized network to store and share files: [https://ipfs.io](https://ipfs.io/)

One of the advantages of IPFS as a storage solution is that it is content addressable. When a file is deleted, if someone reuploads the file, anybody will be able to access it with the same path. For a specific block of data, we will get a specific hash; the hash is persisted on Ethereum to ensure requests immutability.

#### Ethereum

We use Ethereum to store IPFS hashes. The hashes are stored as event logs of a specific smart contract to stay at a minimal cost.

The Ethereum smart contracts are also used to enforce the fee cost of storing a block to Request. The user will store the file's size in addition to the hash. A fee related to this hash will be paid in Ether when storing the hash.

For our solution, we use additional smart contracts for fee verification. Using external smart contracts allows us to implement different fee rules in the future. More information can be found in the ethereum-storage repository.

The RequestHashStorage smart contract address can be found on [GitHub](https://github.com/RequestNetwork/requestNetwork/blob/master/packages/smart-contracts/src/lib/artifacts/RequestHashStorage/index.ts)

```json
 "xdai": {
  "address": "0x2256938E8225a998C498bf86B43c1768EE14b90B"
},
"sepolia": {
  "address": "0xd6c085A4D14e9e171f4aF58F7F48bd81173f167E"
}
```
