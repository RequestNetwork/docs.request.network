# Request Invoicing

An Invoicing Template for creating, viewing, and paying requests in Request Network. Built using the [invoice-dashboard.md](../components/invoice-dashboard.md "mention") and [create-invoice-form.md](../components/create-invoice-form.md "mention") Web Components.

<table data-full-width="false"><thead><tr><th>Framework</th><th>Deployment</th><th>Source</th></tr></thead><tbody><tr><td>Next.js</td><td><a href="https://invoicing.request.network">https://invoicing.request.network</a></td><td><a href="https://github.com/RequestNetwork/invoicing-template">https://github.com/RequestNetwork/invoicing-template</a></td></tr></tbody></table>

### Request Invoicing Demo Video

{% embed url="https://www.youtube.com/watch?v=FC6oJR5aKOQ" fullWidth="false" %}
A video showing the Request Invoicing template where you can create, pay, and list all your requests.
{% endembed %}

## Features

| Feature                                                                                                                                | Status               |
| -------------------------------------------------------------------------------------------------------------------------------------- | -------------------- |
| ERC20 Payments                                                                                                                         | :white\_check\_mark: |
| Native Token Payments                                                                                                                  | :white\_check\_mark: |
| Conversion Payments                                                                                                                    | :construction:       |
| [rnf\_invoice format](https://github.com/RequestNetwork/requestNetwork/tree/master/packages/data-format/src/format/rnf\_invoice) 0.3.0 | :white\_check\_mark: |
| Configure Logo and Colors                                                                                                              | :white\_check\_mark: |
| Inject your own custom currency list                                                                                                   | :white\_check\_mark: |
| Download Invoice as PDF                                                                                                                | :white\_check\_mark: |

## Chains and Currencies

| Chain           | Currencies                                                                                  |
| --------------- | ------------------------------------------------------------------------------------------- |
| Ethereum        | USDC, USDT, DAI, AXS, AUDIO, RAI, SYLO, LDO, UST, MNT, MIR, INJ, OCEAN, ANKR, RLY, REQ, ETH |
| Polygon         | USDC, USDT, DAI, MATIC                                                                      |
| Sepolia         | FAU, ETH, USDT, USDC                                                                        |
| BNB Smart Chain | DAI, BUSD                                                                                   |
| Gnosis          | USDC                                                                                        |
| Avalanche       | USDC, USDT, AVAX                                                                            |
| Optimism        | USDC, USDT, DAI, ETH                                                                        |
| Moonbeam        | USDC (multichain), USDC (wormhole)                                                          |
| Fantom          | FTM                                                                                         |
| zkSync Era      | ETH                                                                                         |
| Base            | ETH                                                                                         |

### Request Invoicing Integration Video

{% embed url="https://www.youtube.com/watch?v=dYlSVr94Ohg" %}
A video showing how to integrate the Request Invoicing web components into a Next.js application.
{% endembed %}
