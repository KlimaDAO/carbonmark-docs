# Understanding fees on Carbonmark

It is **completely free** to purchase or retire carbon from a seller listing with USDC. You can also create and manage your own listings at zero additional cost.

However, for other types of purchases, you may encounter **network fees** or **payment processing fees** from our partner technology providers. This document is intended to help you understand how these fees work, and where the money goes.

## Summary - by payment method

| Payment Method | Network fee | Payment processing fee | Gas fee         |
|----------------|-------------|------------------------|-----------------|
| USDC           | 0-26%*      | 0%                     | $0.02 - $0.50** |
| Credit card    | 0-26%*      | 20%                    | $0.02 - $0.50** |

As shown in the table above, when you pay with a credit card, our payment processing partners (Stripe & Offsetra) charge a fee on top of the listed tonne price. This fee is used to facilitate the currency conversions necessary to acquire the carbon asset on Polygon's blockchain network.

*\*Network fees depend on purchase type (see below)*

*\*\*Gas fees increase during periods of high network congestion, but are typically only a few cents per transaction. When paying with USDC, you pay the gas in MATIC from your wallet. When paying with a credit card, the gas cost is estimated and added to the total.*

## Summary - by purchase type

| Purchase type | Total network fee | Description |
|---|---|---|
| Buy from seller listing | 0% | No network fees |
| Retire from seller listing | 0% | No network fees |
| Buy from BCT pool | 25.6% | +25% BCT pool fee for Toucan Protocol<br>+0.6% asset swap fee for SushiSwap |
| Retire from BCT pool | 26.6% | +25% BCT pool fee for Toucan Protocol<br>+0.6% Asset swap fee for SushiSwap<br>+1% Retirement service fee for KlimaDAO |
| Buy from NCT pool | 10.3% | +10% NCT pool fee for Toucan Protocol<br>+0.3% Asset swap fee for SushiSwap |
| Retire from NCT pool | 11.3% | +10% NCT pool fee for Toucan Protocol<br>+0.3% Asset swap fee for SushiSwap<br>+1% Retirement service fee for KlimaDAO |
| Buy from UBO pool | 2.8% | +2.5% UBO pool fee for C3<br>+0.3% Asset swap fee for SushiSwap |
| Retire from UBO pool | 3.8% | +2.5% UBO pool fee for C3<br>+0.3% Asset swap fee for SushiSwap<br>+1% Retirement service fee for KlimaDAO |
| Buy from NBO pool | 2.8% | +2.5% NBO pool fee for C3<br>+0.3% Asset swap fee for SushiSwap |
| Retire from NBO pool | 3.8% | +2.5% NBO pool fee for C3<br>+0.3% Asset swap fee for SushiSwap<br>+1% Retirement service fee for KlimaDAO |

## Seller listing prices vs pool prices

Carbonmark compares prices across different sources in the Digital Carbon Market. Currently, we compare prices from two sources: *marketplace sellers*, and *carbon pools*.

**Marketplace seller listings** are the prices set by sellers who have listed a specific asset for sale directly on our platform. There are zero network fees incurred for purchases from marketplace listings.

**Carbon pools** are similar to 'index funds' for carbon assets. These pools contain a variety of carbon projects and vintages, but each pool has a single price-per-tonne that changes based on supply and demand. In addition to the trading price, purchasing or retiring an asset from a pool may incur a network fee (described in the section above). These pools are not operated by Carbonmark—- currently, we have integrated with the carbon pools operated by C3 and Toucan. You can explore these pools and the carbon assets that they contain at [carbon.klimadao.finance](https://carbon.klimadao.finance/).
