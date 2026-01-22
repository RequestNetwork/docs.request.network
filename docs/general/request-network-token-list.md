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
  "id": "USDC-mainnet",
  "name": "USD Coin",
  "symbol": "USDC",
  "decimals": 6,
  "address": "0xA0b86991c6218b36c1d19D4a2e9Eb0cE3606eB48",
  "network": "mainnet",
  "type": "ERC20",
  "hash": "0xA0b86991c6218b36c1d19D4a2e9Eb0cE3606eB48",
  "chainId": 1
}
```

| Field | Description |
|-------|-------------|
| `id` | Unique identifier, typically `SYMBOL-network` (e.g., `USDC-mainnet`) |
| `name` | Human-readable token name |
| `symbol` | Token symbol |
| `decimals` | Number of decimal places |
| `address` | Token contract address |
| `network` | Network name (e.g., `mainnet`, `matic`, `bsc`) |
| `type` | Currency type (e.g., `ERC20`, `ETH`, `ISO4217`) |
| `hash` | For ERC20 tokens, same as `address`. For native tokens, a calculated hash. |
| `chainId` | Chain ID of the network |

See the [SDK source code](https://github.com/RequestNetwork/requestNetwork/blob/master/packages/types/src/currency-types.ts) for the full list of supported networks and types.

## Adding a New Token

We welcome community contributions! To add a new token to the list:

1. Fork the [request-token-list](https://github.com/RequestNetwork/request-token-list) repository on Github
2. Add your token information to `tokens/token-list.json`
3. Make sure your token meets our requirements (see [CONTRIBUTING.md](https://github.com/RequestNetwork/request-token-list/blob/main/CONTRIBUTING.md))
4. Run tests locally: `npm test`
5. Create a Pull Request
