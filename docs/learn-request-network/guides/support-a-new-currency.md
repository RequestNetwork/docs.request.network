# Support a new currency

Request Network allows you to support any currency. Head out to the [currency](https://github.com/RequestNetwork/requestNetwork/tree/master/packages/currency) package to see how we identify and manage currencies in the `CurrencyManager`. You don't need to add new currencies in this repository.

Instead, when instanciating the `CurrencyManager`, you can feed it with a list of supported currencies for your dapp:

```
const list: CurrencyInput[] = [
    { type: RequestLogicTypes.CURRENCY.ETH, decimals: 18, network: 'anything', symbol: 'ANY' },
];
const currencyManager = new CurrencyManager(list);
```

{% hint style="info" %}
You can check the details of a currency with the following REST API: [https://api.request.network/currency/MKR-mainnet](https://api.request.network/currency/MKR-mainnet)
{% endhint %}

To implement new types of currencies (aside fiat, BTC, ETH, ERC20), [head towards payment networks](how-payment-networks-work.md#other-currencies).
