# Request Network Token List

The [Request Network Token List](https://requestnetwork.github.io/request-token-list/latest.json) is a curated list of tokens supported by Request Network products. The token list follows a standardized format and includes essential information about each token, such as address, symbol, name, decimals, and chainId.

## Usage

The token list is available at: [https://requestnetwork.github.io/request-token-list/latest.json](https://requestnetwork.github.io/request-token-list/latest.json)

You can fetch the token list directly in your application:

```typescript
const tokenList = await fetch(
  "https://requestnetwork.github.io/request-token-list/latest.json"
).then((res) => res.json());
```

## Token List Structure

Each token in the list contains the following information:

```json
{
  "id": "TKN-mainnet"
  "name": "Token Name",
  "address": "0x...",
  "symbol": "TKN",
  "decimals": 18,
  "chainId": 1,
  "logoURI": "https://..."
}
```

## Adding a New Token

We welcome community contributions! To add a new token to the list:

1. Fork the [request-token-list](https://github.com/RequestNetwork/request-token-list) repository on Github
2. Add your token information to `tokens/token-list.json`
3. Make sure your token meets our requirements (see [CONTRIBUTING.md](https://github.com/RequestNetwork/request-token-list/blob/main/CONTRIBUTING.md))
4. Run tests locally: `npm test`
5. Create a Pull Request
