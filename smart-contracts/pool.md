# Pool

Source Code: [sources/pool.move](https://github.com/pontem-network/liquidswap\_v1/blob/main/sources/pool.move)

The Pool module stands as the cornerstone of Liquidswap V1, encompassing the essential functionalities such as swap, mint, burn logic, and core checks. These operations are efficiently executed with the assistance of helper libraries. Central to this module is the 'Pool resource', a pivotal component of the contracts. It serves as a repository for crucial data including reserves, bin information, fee parameters, and events.&#x20;

This resource is defined with three generics, a design choice that facilitates the determination of the appropriate pool during iterative processes:

<pre class="language-rust"><code class="lang-rust"><strong>/// Pool itself.
</strong><strong>struct Pool&#x3C;phantom X, phantom Y, phantom BinStep> has key {
</strong><strong>    // Current active bin id.
</strong><strong>    active_bin_id: u32,
</strong><strong>    
</strong><strong>    // Current bin step.
</strong><strong>    bin_step: u32,
</strong><strong>    
</strong><strong>    // Bins tree and table.
</strong><strong>    tree: BinsTree,
</strong><strong>    bins: Table&#x3C;u32, Bin>,
</strong><strong>    
</strong><strong>    // Current coin reserves.
</strong><strong>    coins_x: Coin&#x3C;X>,
</strong><strong>    coins_y: Coin&#x3C;Y>,
</strong><strong>    
</strong><strong>    // Gas optimization, just store instead of generating.
</strong><strong>    collection_name: String,
</strong><strong>    
</strong><strong>    // The pool must be locked for swap/mint/burn operations during flashloan.
</strong><strong>    is_locked: bool,
</strong><strong>    
</strong><strong>    // Fees.
</strong><strong>    fee_params: StaticFeeParams,
</strong><strong>    
</strong><strong>    // Events.
</strong><strong>    swap_event_handler: event::EventHandle&#x3C;SwapEvent>,
</strong><strong>    mint_event_handler: event::EventHandle&#x3C;MintEvent>,
</strong><strong>    composition_fees_event_handler: event::EventHandle&#x3C;CompositionFeesEvent>,
</strong><strong>    burn_event_handler: event::EventHandle&#x3C;BurnEvent>,
</strong><strong>    flashloan_event_handler: event::EventHandle&#x3C;FlashloanEvent>,
</strong><strong>    change_fee_params_event_handler: event::EventHandle&#x3C;ChangeFeeParamsEvent>,
</strong><strong>}
</strong></code></pre>

The generics are primarily defined by two coins, X and Y, which establish the trading pair.

For instance, X could be represented as

```
0x1::aptos_coin::AptosCoin
```

&#x20;And Y could be represented as:

```
0xf22bede237a07e121b56d91a491eb7bcdfd1f5907926a9e58338f964a01b17fa::asset::USDC
```

* BinStep: This type is crucial as it determines the step between bins in Liquidswap V1, essentially representing the price difference between adjacent bins
* it is important to note that the coin generics X and Y must be sorted correctly. This sorting ensures proper functioning within the protocol's framework.

Liquidswap V1 operates by creating pools within a designated resource account. Each pool is unique, ensuring there are no duplicates with the same combination of coins and bin size. This principle of uniqueness also applies to LB tokens, where each collection is distinct and non-replicable.

## Bins

Source Code: [bin\_steps/](https://github.com/pontem-network/liquidswap\_v1/tree/main/bin\_steps)

All potential bin steps, ranging from 1 to 100, are defined within the 'BinSteps' module of Liquidswap V1. They can be utilized in the following manner:&#x20;

```
use liquidswap_v1::bin_steps::X1;
use liquidswap_v1::bin_steps::X5;
use liquidswap_v1::bin_steps::X10;
```

Each bin step is indicative of the price difference between bins, measured in basis points. For instance, one basis point, which is equivalent to 0.0001 or 0.01%, corresponds to the X1 bin step. For a more comprehensive understanding, please refer to the [Protocol Overview](../protocol-overview.md) section.

## Functions

:warning: For swap, liquidity mint and burn operations we recommend to use [Router](router-and-entry.md) module as otherwise it could be risky: the pool contract itself doesn't have slippage checks.

The module offers an extensive array of functions and getters that can be highly useful. We recommend exploring the source code to gain a deeper understanding.

These functions enable you to retrieve information about bins, the active bin step, current fee parameters, calculate fees, perform flashloans, and more. Some functions are marked as `#[view]`, indicating that they can be queried off-chain

## Pool Creation

At present, pool creation in Liquidswap V1 is a permissioned process. This decision is driven by the fact that the project is in its early stages, and there is a need for time to comprehensively evaluate and determine the most optimal pre-configurations to offer. Misconfigurations during pool creation can potentially lead to unpredictable consequences, and this precautionary approach helps mitigate such risks.

## Add Liquidity

To contribute liquidity to existing pools, you can utilize the `mint` function:

```javascript
/// Provide liquidity to pair and mint new LB tokens.
/// Would return mitned LB tokens and X, Y leftovers.
public fun mint<X, Y, BinStep>(
    coins_x: Coin<X>,
    coins_y: Coin<Y>,
    bin_ids: vector<u32>,
    liq_x_coins: vector<u64>,
    liq_y_coins: vector<u64>,
): (vector<Token>, Coin<X>, Coin<Y>) acquires Pool, InitConfiguration {
...
```

To add liquidity, you'll need to specify the IDs of the bins where you intend to contribute liquidity, along with the respective amounts for each bin. It's important to note that when adding liquidity, you can only add both X and Y coins to the active bin. Specifically, on the left side of the active bin, you can only use X coins, and on the right side, you can only use Y coins

After you would get the LB tokens which would represent your position in bins of the pools, and leftovers coins if there is something left.

### Remove Liquidity

It's much pretty simple, just use the function `burn` and  provide LB tokens you are willing to burn:

```javascript
/// Burns provided tokens, releasing corresponding `X` and `Y` coins liquidity.
/// Returns released `X` and `Y` coins.
public fun burn<X, Y, BinStep>(
    liq_nfts: vector<Token>,
): (Coin<X>, Coin<Y>) acquires Pool, InitConfiguration {
```

You will get reedemed liquidity and collected fees.

## Swap

You can swap coin X in pair for coin Y or vise versa. To do swap use the following two functions:

```java
/// Swap X to Y.
public fun swap_x_for_y<X, Y, BinStep>(
    coin_in: Coin<X>,
): Coin<Y> acquires Pool {
...

/// Swap Y for X.
public fun swap_y_for_x<X, Y, BinStep>(
    coin_in: Coin<Y>,
): Coin<X> acquires Pool {
```

If the amount to swap is greater than provided liquidity in the pool, the function would abort, so always be sure your swap can be covered.

## Flashloans

The implementation is based on Move's loan concept, where a Move object containing the loan data is issued by `flashloan` function but cannot be stored, copied, cloned or dropped, the only available action being to return the object back to the `pay_flashloan` function, which would verify that payout contains enough fees.

The concept was explained early on by the Pontem team in this [Medium article](https://medium.com/p/bbc48a48d93c).

Take a flashloan:

```javascript
public fun flashloan<X, Y, BinStep>(
    amount_x: u64,
    amount_y: u64,
): (Coin<X>, Coin<Y>, Flashloan<X, Y, BinStep>) acquires Pool {
....
```

Pay flashloan:

<pre class="language-javascript"><code class="lang-javascript">public fun pay_flashloan&#x3C;X, Y, BinStep>(
<strong>  coin_x_loan: Coin&#x3C;X>,
</strong>  coin_y_loan: Coin&#x3C;Y>,
  flashloan: Flashloan&#x3C;X, Y, BinStep>
) acquires Pool {
.....
</code></pre>

The dynamic fees is not applying to the flashloan, the flashloan fee can be changed by DAO account.

Read more in the [integration section](../integration/flashloans.md).

## Getters

Almost all getters are marked as `#[view]` functions which allows to query data of the pools, include query active bin, current bin step as number, fees, get amount you will get after swap, or get amount you need to perfom a swap, pools parameters and many more.

For more details look at source code.

## Events

Each operation, include pool creation, covered by events.

