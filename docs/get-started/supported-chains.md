# Supported Chains

## Payments

The payment proxy smart contracts enable the various payment types.

The most widely deployed payment proxy is the ERC20FeeProxy. The most frequently used payment proxy is the ERC20ConversionProxy.

{% hint style="info" %}
Make sure to scroll horizontally to see all the payment proxy types! :wink:
{% endhint %}

<table data-full-width="true"><thead><tr><th width="191">Chain</th><th data-type="number">Chain ID</th><th data-type="checkbox">ERC20FeeProxy</th><th data-type="checkbox">EthereumFeeProxy</th><th data-type="checkbox">Erc20ConversionProxy</th><th data-type="checkbox">EthConversionProxy</th><th data-type="checkbox">BatchConversionPayments</th><th data-type="checkbox">ERC20SwapToPay</th><th data-type="checkbox">Erc20SwapToConversion</th><th data-type="checkbox">ERC20TransferableReceivable</th><th data-type="checkbox">ERC20EscrowToPay</th><th data-type="checkbox">EthereumProxy</th><th data-type="checkbox">BatchPayments</th><th data-type="checkbox">ERC20Proxy</th></tr></thead><tbody><tr><td>Ethereum Mainnet</td><td>1</td><td>true</td><td>true</td><td>true</td><td>true</td><td>true</td><td>true</td><td>true</td><td>true</td><td>true</td><td>true</td><td>true</td><td>true</td></tr><tr><td>Goerli</td><td>5</td><td>true</td><td>true</td><td>true</td><td>true</td><td>true</td><td>true</td><td>true</td><td>true</td><td>true</td><td>true</td><td>true</td><td>false</td></tr><tr><td>Optimism</td><td>10</td><td>true</td><td>true</td><td>true</td><td>true</td><td>true</td><td>true</td><td>true</td><td>false</td><td>true</td><td>true</td><td>true</td><td>false</td></tr><tr><td>Arbitrum One</td><td>42161</td><td>true</td><td>true</td><td>true</td><td>true</td><td>true</td><td>true</td><td>false</td><td>false</td><td>false</td><td>true</td><td>true</td><td>false</td></tr><tr><td>Gnosis</td><td>100</td><td>true</td><td>true</td><td>true</td><td>false</td><td>true</td><td>true</td><td>true</td><td>false</td><td>false</td><td>true</td><td>true</td><td>false</td></tr><tr><td>Polygon</td><td>137</td><td>true</td><td>true</td><td>true</td><td>true</td><td>true</td><td>true</td><td>true</td><td>true</td><td>true</td><td>true</td><td>true</td><td>false</td></tr><tr><td>Mumbai</td><td>80001</td><td>true</td><td>false</td><td>false</td><td>false</td><td>false</td><td>false</td><td>false</td><td>false</td><td>false</td><td>false</td><td>false</td><td>false</td></tr><tr><td>BSC</td><td>56</td><td>true</td><td>true</td><td>true</td><td>true</td><td>true</td><td>true</td><td>false</td><td>false</td><td>false</td><td>true</td><td>true</td><td>false</td></tr><tr><td>BSC Testnet</td><td>97</td><td>true</td><td>false</td><td>true</td><td>false</td><td>false</td><td>false</td><td>false</td><td>false</td><td>false</td><td>false</td><td>false</td><td>false</td></tr><tr><td>Celo</td><td>42220</td><td>true</td><td>true</td><td>true</td><td>true</td><td>true</td><td>true</td><td>false</td><td>false</td><td>false</td><td>true</td><td>true</td><td>false</td></tr><tr><td>Alfajores</td><td>44787</td><td>true</td><td>false</td><td>false</td><td>false</td><td>false</td><td>false</td><td>false</td><td>false</td><td>false</td><td>false</td><td>false</td><td>false</td></tr><tr><td>Fantom</td><td>250</td><td>true</td><td>true</td><td>true</td><td>true</td><td>true</td><td>true</td><td>false</td><td>false</td><td>false</td><td>true</td><td>true</td><td>false</td></tr><tr><td>Tombchain</td><td>6969</td><td>true</td><td>true</td><td>false</td><td>false</td><td>false</td><td>false</td><td>false</td><td>false</td><td>false</td><td>false</td><td>false</td><td>false</td></tr><tr><td>Avalanche</td><td>43114</td><td>true</td><td>true</td><td>true</td><td>true</td><td>true</td><td>true</td><td>false</td><td>false</td><td>false</td><td>true</td><td>true</td><td>false</td></tr><tr><td>Fuse</td><td>122</td><td>true</td><td>true</td><td>false</td><td>false</td><td>true</td><td>true</td><td>false</td><td>false</td><td>true</td><td>true</td><td>true</td><td>false</td></tr><tr><td>Moonbeam</td><td>1284</td><td>true</td><td>true</td><td>true</td><td>true</td><td>true</td><td>true</td><td>true</td><td>false</td><td>true</td><td>true</td><td>false</td><td>false</td></tr><tr><td>Ronin</td><td>2020</td><td>true</td><td>true</td><td>false</td><td>false</td><td>false</td><td>false</td><td>false</td><td>false</td><td>false</td><td>false</td><td>false</td><td>false</td></tr><tr><td>Mantle</td><td>5000</td><td>true</td><td>true</td><td>false</td><td>false</td><td>false</td><td>false</td><td>false</td><td>false</td><td>false</td><td>false</td><td>false</td><td>true</td></tr><tr><td>Mantle Testnet</td><td>5001</td><td>true</td><td>true</td><td>false</td><td>false</td><td>false</td><td>false</td><td>false</td><td>false</td><td>false</td><td>false</td><td>false</td><td>true</td></tr><tr><td>NEAR</td><td>null</td><td>true</td><td>true</td><td>false</td><td>true</td><td>false</td><td>false</td><td>false</td><td>false</td><td>false</td><td>false</td><td>false</td><td>false</td></tr><tr><td>NEAR Testnet</td><td>null</td><td>true</td><td>false</td><td>false</td><td>false</td><td>false</td><td>false</td><td>false</td><td>false</td><td>false</td><td>false</td><td>false</td><td>false</td></tr></tbody></table>

## Storage

<table data-full-width="true"><thead><tr><th width="194">Chain</th><th width="98" data-type="number">Chain ID</th><th data-type="checkbox">RequestHashStorage</th><th data-type="checkbox">RequestOpenHashSubmitter</th></tr></thead><tbody><tr><td>Gnosis</td><td>100</td><td>true</td><td>true</td></tr><tr><td>Goerli</td><td>5</td><td>true</td><td>true</td></tr><tr><td>Ethereum Mainnet</td><td>1</td><td>true</td><td>true</td></tr></tbody></table>

## Tokenomics

<table data-full-width="true"><thead><tr><th width="195.33333333333337">Chain</th><th width="100">Chain ID</th><th data-type="checkbox">lockForREQBurn</th><th data-type="checkbox">DaiBasedREQBurner</th><th data-type="checkbox">RequestToken</th></tr></thead><tbody><tr><td>Ethereum Mainnet</td><td>1</td><td>false</td><td>true</td><td>true</td></tr><tr><td>Gnosis</td><td>100</td><td>true</td><td>false</td><td>false</td></tr></tbody></table>