# Protocol Overview

Liquidswap V1 introduces a novel approach to decentralized exchange (DEX) mechanisms, moving away from the traditional Automated Market Maker (AMM) model governed by a constant product formula (e.g., X\*Y=K). Instead, it employs a liquidity book model, offering enhanced flexibility for liquidity providers and traders. This design allows users to specify exact prices for buying or selling assets, akin to placing limit orders, thereby enabling precise control over their liquidity positions.

This model also mitigates concerns about price slippage for traders. Since liquidity is supplied at specific prices without reliance on a constant formula for swaps, assets can be exchanged at the stated price, ensuring predictability and stability. For instance, stablecoins can be swapped at a one-to-one ratio, barring transaction fees, with each bin maintaining a fixed price.

A 'bin' functions similarly to a mini liquidity pool within the broader global pool, designated for a specific price point of a pair of assets (X and Y). Each bin possesses a unique ID, enabling liquidity providers to target their contributions to specific price levels. The 'bin step' is a critical concept, representing the minimum price increment between adjacent bins, typically measured in basis points. This metric facilitates the precise calculation of price movements within the pool.

The arrangement within the pool is such that bins on the left side contain liquidity primarily for asset Y, whereas bins on the right are reserved for asset X. The 'active bin' signifies the bin that matches the current market price of the asset pair, serving as the starting point for swaps. As trades occur and the price shifts, the active bin may change, with the bin corresponding to the new price becoming the focal point for subsequent transactions.

<figure><img src=".gitbook/assets/Blank diagram (23).png" alt=""><figcaption></figcaption></figure>

Liquidswap's architecture is influenced by innovative models seen in the DeFi space, notably similar to approaches like those introduced in Trader Joe V2. As we refine our [documentation](https://docs.traderjoexyz.com/concepts/concentrated-liquidity), we recommend exploring available resources from leading platforms for a broader perspective on the strategies and mechanics that inspire our system's functionality.&#x20;

## Bins

In Liquidswap, each bin serves a dual function: it not only holds liquidity but also defines the price at which this liquidity is available. The organization of bins is directional—bins allocated for coin X extend towards the right, while bins designated for coin Y liquidity extend in the opposite direction. Central to this system is the 'active bin', which at any given moment contains liquidity for both X and Y coins, reflecting the pair's current market price.

The movement of the active bin is dynamic; it shifts to correspond with the bin that accurately reflects the post-swap market price, ensuring that trades are executed at prevailing rates.

Bins are uniquely identified by a numeric ID (represented as a `u32`), facilitating easy navigation and operations within the pool. The bin ID system is designed around a central pivot—the 'zero bin id' is set at 8388608, representing an equilibrium price ratio of 1:1 between X and Y coins. This central value is strategically chosen halfway through the maximum range, which caps at 16777215, equivalent to $$224224$$. This arrangement allows for a wide spectrum of price representations within the pool.

To translate a bin ID into a price, you can apply the following formula:

```
(1 + BinStep / 10000) ** (BinID - 8388608)
```

Learn more in [Trader Joe v2 doc](https://docs.traderjoexyz.com/concepts/bin-math).

## Fees & Treasury

In Liquidswap V1, liquidity providers earn fees not from the entire pool but specifically from the liquidity they've contributed to bins actively used in swaps. This targeted rewards system ensures that providers who directly facilitate trading activity are compensated. Furthermore, the fee structure in V1 is dynamic, varying with the volatility of the asset pair. This means liquidity providers can potentially earn more from pairs with higher volatility compared to those with stable valuations.

Additionally, Liquidswap V1 incorporates a protocol fee. This fee is collected separately and directed to the treasury, where it is governed by the DAO. This mechanism not only supports the operational sustainability of the platform but also empowers the community through DAO oversight

Learn more about [fees](smart-contracts/fees.md).&#x20;

## LB Positions

In Liquidswap V1, liquidity providers no longer receive traditional LP tokens. Instead, they are awarded LB position tokens—Aptos Token v1—which signify their proportional share in the liquidity provided to a specific bin or pool.&#x20;

Each LB position token includes properties such as the bin ID and the amount of liquidity contributed, rendering these tokens fungible. This fungibility enables liquidity providers to seamlessly transfer their entire position or a portion thereof to another account, engage with other DeFi protocols, or consolidate positions. Such flexibility enhances the utility and versatility of liquidity contributions within the ecosystem.

Learn more about[ LB tokens.](smart-contracts/lb-tokens.md)
