# Mobile using Expo

## Introduction

This guide demonstrates how to integrate Request Network into a React Native application using Expo. Due to the differences between Node.js and React Native environments, several polyfills and configurations are necessary to make Request Network work properly. Following this guide will set up your Expo project to use Request Network, handle polyfills for missing modules, and ensure smooth integration.\
\
A full Github repository with Request Network support can be found [here](https://github.com/RequestNetwork/rn-expo-support).

## Installation

After creating a new Expo project, install the necessary dependencies:

```bash
npm install @requestnetwork/request-client.js @requestnetwork/types @requestnetwork/payment-processor @requestnetwork/epk-signature buffer eventemitter3 stream-browserify http-browserify https-browserify react-native-get-random-values tweetnacl node-forge ethers@5.5.1
```

## Setup

1- Create a new file named `index.js` in the root of your project

```bash
touch index.js
```

2- Add the following content to `index.js`

{% @github-files/github-code-block url="https://github.com/RequestNetwork/rn-expo-support/blob/5a771c31051ce89c4feee533692028d45e1415ab/index.js" %}

\
3- Create a file named `cryptoPolyfill.js` in the root of your project

```bash
touch cryptoPolyfill.js
```

4- Add the following content to `cryptoPolyfill.js`&#x20;

{% @github-files/github-code-block url="https://github.com/RequestNetwork/rn-expo-support/blob/5a771c31051ce89c4feee533692028d45e1415ab/cryptoPolyfill.js" %}

5- Create / Update `metro.config.js` to use the custom polyfills

{% @github-files/github-code-block url="https://github.com/RequestNetwork/rn-expo-support/blob/5a771c31051ce89c4feee533692028d45e1415ab/metro.config.js" %}

6- Update `package.json` to set the main entry point to `index.js`

{% @github-files/github-code-block url="https://github.com/RequestNetwork/rn-expo-support/blob/5a771c31051ce89c4feee533692028d45e1415ab/package.json" %}

7- Ensure that `app.json` file includes the correct entry point

```json
{
  "expo": {
    "entryPoint": "./index.js",
    ...
  }
}
```

## Polyfills and Configurations

### Crypto Polyfills

We've created a custom crypto polyfill (`cryptoPolyfill.js`) to provide the necessary cryptographic functions. This polyfill uses `tweetnacl` and `node-forge` libraries to implement various cryptographic operations.

**Why `tweetnacl` and `node-forge`?**

1. **`tweetnacl`**: It's a fast, secure, and easy-to-use cryptography library. It provides a pure JavaScript implementation of the NaCl cryptography library, which is particularly useful for generating random bytes, essential for cryptographic operations.
2. **`node-forge`**: It provides a comprehensive set of cryptographic tools and utilities. It implements various cryptographic algorithms and protocols that are not natively available in React Native. It's used in our polyfill for operations like hashing, cipher creation, and PBKDF2 key derivation.

Using these libraries allows us to implement a more complete set of cryptographic functions that closely mimic the Node.js crypto module, which is not available in React Native.

## Important Notes

1. Ensure you're using compatible versions of React Native and Expo.
2. The crypto polyfill may not cover all use cases. Test thoroughly and adjust as needed.
3. Be cautious when handling private keys. Never expose them in your code or version control.
4. The example code uses environment variables for private keys. Ensure you set these up correctly and securely.

