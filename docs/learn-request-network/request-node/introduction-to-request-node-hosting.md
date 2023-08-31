# Request Node Hosting

Now you should be comfortable with features of the Request network.

In this guide, we will explain the Request Node and help you run your own.

### What is the Request Node?

Request Nodes are servers that run the lower layers of the Request Protocol. They connect to the Ethereum and IPFS networks, to store and retrieve request transactions. Protocol users can interact with the Request Node through HTTP using the request-client.js library.

### Why run your own Node?

First, running the Node locally on your machine will allow you to test your code using the Request Client easily.

You may also want to host your Node in a server. Hosting your Node is the most decentralized setup possible. It allows you to:

* Store your data and make sure it is safely backed up
* Be technically independent: own your servers and control how you manage them
* Use custom configuration settings

### How to run your Node?

There are currently three supported ways to run a Request Node:

* Run from [**Docker**](../../setup-request-network/broken-reference/). The easiest way to run the Request Node.
* Run the [**code**](../../setup-request-network/broken-reference/) from the git repository. Especially useful if you are making changes to the protocol layers.
* Use our kubernetes [**helm**](../../setup-request-network/broken-reference/) charts. The best solution if you want to host your Node on a Kubernetes cluster.

On the following pages, you can find detailed steps to run each one.

### Prerequisites

Request uses IPFS and Ethereum to store request transactions. For this reason, the Node needs connections to an Ethereum node and an IPFS node.

#### Ethereum node

You can use any HTTP/S Ethereum node to run your Request Node. For local development, you can use ganache-cli, a local Ethereum RPC client for tests (explained in more detail on the following pages).

An easy way to get going with Ethereum Mainnet or Goerli is to use services like Infura, that will expose an Ethereum node API for you.

#### IPFS node

Request uses a dedicated IPFS network to store our data. You will need an IPFS node configured to connect to our network. You can check this page for more details on our dedicated network.

The good news is it's easy to set up our IPFS node and we will show it to you on our next steps.
