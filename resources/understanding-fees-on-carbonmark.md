# Understanding fees on Carbonmark

It is **completely free** to purchase or retire carbon from a seller listing with USDC. You can also create and manage your own listings at zero additional cost.

However, for other types of purchases, you may encounter **network fees** or **payment processing fees** from our partner technology providers. This document is intended to help you understand how these fees work, and where the money goes.

## Summary - by payment method

<table><thead><tr><th>Payment Method</th><th>Network fee</th><th width="220">Payment processing fee</th><th>Gas fee</th></tr></thead><tbody><tr><td>USDC</td><td>0-26%*</td><td>0%</td><td>$0.02 - $0.50**</td></tr><tr><td>Credit card</td><td>0-26%*</td><td>20%</td><td>$0.02 - $0.50**</td></tr></tbody></table>

As shown in the table above, when you pay with a credit card, our payment processing partners (Stripe & Offsetra) charge a fee on top of the listed tonne price. This fee is used to facilitate the currency conversions necessary to acquire the carbon asset on Polygon's blockchain network.

_\*Network fees depend on purchase type (see below)_

_\*\*Gas fees increase during periods of high network congestion, but are typically only a few cents per transaction. When paying with USDC, you pay the gas in MATIC from your wallet. When paying with a credit card, the gas cost is estimated and added to the total._

{% hint style="info" %}
**Need MATIC for gas fees or USDC?** See this guide on [How to get USDC or MATIC](broken-reference).
{% endhint %}

## Summary - by purchase type

<table><thead><tr><th>Purchase type</th><th width="170.33333333333331">Total network fee</th><th>Description</th></tr></thead><tbody><tr><td>Buy from seller listing</td><td>0%</td><td>No network fees</td></tr><tr><td>Retire from seller listing</td><td>0%</td><td>No network fees</td></tr><tr><td>Buy from BCT pool</td><td>25.6%</td><td>+25% BCT pool fee for Toucan Protocol<br>+0.6% asset swap fee for SushiSwap</td></tr><tr><td>Retire from BCT pool</td><td>26.6%</td><td>+25% BCT pool fee for Toucan Protocol<br>+0.6% Asset swap fee for SushiSwap<br>+1% Retirement service fee for KlimaDAO</td></tr><tr><td>Buy from NCT pool</td><td>10.3%</td><td>+10% NCT pool fee for Toucan Protocol<br>+0.3% Asset swap fee for SushiSwap</td></tr><tr><td>Retire from NCT pool</td><td>11.3%</td><td>+10% NCT pool fee for Toucan Protocol<br>+0.3% Asset swap fee for SushiSwap<br>+1% Retirement service fee for KlimaDAO</td></tr><tr><td>Buy from UBO pool</td><td>2.8%</td><td>+2.5% UBO pool fee for C3<br>+0.3% Asset swap fee for SushiSwap</td></tr><tr><td>Retire from UBO pool</td><td>3.8%</td><td>+2.5% UBO pool fee for C3<br>+0.3% Asset swap fee for SushiSwap<br>+1% Retirement service fee for KlimaDAO</td></tr><tr><td>Buy from NBO pool</td><td>2.8%</td><td>+2.5% NBO pool fee for C3<br>+0.3% Asset swap fee for SushiSwap</td></tr><tr><td>Retire from NBO pool</td><td>3.8%</td><td>+2.5% NBO pool fee for C3<br>+0.3% Asset swap fee for SushiSwap<br>+1% Retirement service fee for KlimaDAO</td></tr></tbody></table>

## Seller listing prices vs pool prices

Carbonmark compares prices across different sources in the Digital Carbon Market. Currently, we compare prices from two sources: _marketplace sellers_, and _carbon pools_.

**Marketplace seller listings** are the prices set by sellers who have listed a specific asset for sale directly on our platform. There are zero network fees incurred for purchases from marketplace listings.

**Carbon pools** are similar to 'index funds' for carbon assets. These pools contain a variety of carbon projects and vintages, but each pool has a single price-per-tonne that changes based on supply and demand. In addition to the trading price, purchasing or retiring an asset from a pool may incur a network fee (described in the section above). These pools are not operated by Carbonmark—- currently, we have integrated with the carbon pools operated by C3 and Toucan. You can explore these pools and the carbon assets that they contain at [carbon.klimadao.finance](https://carbon.klimadao.finance/).
