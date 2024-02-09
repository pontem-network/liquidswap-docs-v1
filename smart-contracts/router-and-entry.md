# Router & Entry

Source code: [router/](https://github.com/pontem-network/liquidswap\_v1/tree/main/router)

The router module encompasses both router and entry points for the protocol. Initially, it enables the invocation of functions external to smart contracts (utilizing CLI, wallets, libraries, etc.), while concurrently offering functions for securely swapping, minting, and burning liquidity with specified slippage.

Since both the router and entry points function as peripheral contracts and do not necessitate updates to the core system, new versions of the modules can be deployed, or alternative routers with different methodologies can be introduced.

## Router

Source code:  [router/sources/router.move](https://github.com/pontem-network/liquidswap\_v1/blob/main/router/sources/router.move)

It's crucial to note that the router does not encompass all functionalities of the pool, as this is unnecessary. Many getter functions are already available within the pool module itself. Additionally, while entry functions interact directly with account balances, the router continues to accept Aptos Coin objects.&#x20;

The primary role of the router is to facilitate operations involving slippage with the pool.

### Swap

To perform a swap from a specific quantity of X coins to Y coins, utilize the function outlined below:

```javascript
public fun swap_exact_x_for_y<X, Y, BinStep>(
    x_coins_in: Coin<X>,
    y_coins_out_min_val: u64,
): Coin<Y> {
...
```

The parameter `y_coins_out_min_val` specifies the minimum quantity of Y coins you expect to receive in exchange for X coins. If the actual amount received is less than this minimum value, the function will abort the transaction. Upon successful execution, the function returns a `Coin` object representing the amount of Y coins exchanged.

The same logic applies when exchanging Y coins for X coins:

```javascript
public fun swap_exact_y_for_x<X, Y, BinStep>(
    y_coins_in: Coin<Y>,
    x_coins_out_min_val: u64,
): Coin<X> {
...
```

To swap any amount of X for an exact amount of Y coins and receive leftovers, or to do the reverse, use the functions below. These allow precise exchanges with potential surplus returns:

```javascript
public fun swap_x_for_exact_y<X, Y, BinStep>(
    x_coins_in: Coin<X>,
    y_coins_required_out: u64,
): (Coin<Y>, Coin<X>) {
...
}

public fun swap_y_for_exact_x<X, Y, BinStep>(
    y_coins_in: Coin<Y>,
    x_coins_required_out: u64,
): (Coin<X>, Coin<Y>) {
...
}
```

### Add Liquidity

Adding liquidity involves complexity due to the protocol's use of price ranges and bins. When you add liquidity within a specific price range, corresponding to the current active bin, this action can shift the price, potentially activating a different bin.

The function's arguments are structured as follows:

```javascript
/// Adds liquidity to bins in flexible way.
/// Applies active bin id change as shift to liquidity distribution `bin_ids`.
/// This way of adding liqudity should prevent txs failure in case of active bin change
/// during tx pending.
///
/// Ensures that pool `active_bin_id` and minted shares satisfies requirements.
///
/// Returns minted liquidity Tokens, `X` and `Y` coins leftovers.
public fun add_liquidity<X, Y, BinStep>(
    coins_x: Coin<X>,
    coins_y: Coin<Y>,
    bin_ids: vector<u32>,
    active_bin_id_desired: u32,
    active_bin_id_slippage: u32,
    liq_x_coins: vector<u64>,
    liq_y_coins: vector<u64>,
    amount_x_min: u64,
    amount_y_min: u64,
): (vector<Token>, Coin<X>, Coin<Y>) {
...
```

So basically:

* `coins_x`: The amount of X coins you wish to add as liquidity.
* `coins_y`: The amount of Y coins you wish to add as liquidity.
* `bin_ids`: A vector containing the IDs of bins where you intend to allocate your liquidity.
* `active_bin_id_desired`: The ID of the active bin at the time of transaction submission.
* `active_bin_id_slippage`: The maximum number of bins the active bin can shift, accommodating for potential price changes.
* `liq_x_coins`: A vector detailing the exact amount of X coins to be added to each specified bin, corresponding to `bin_ids`.
* `liq_y_coins`: A vector detailing the exact amount of Y coins to be added to each specified bin, also matching `bin_ids`.
* `amount_x_min`: The minimum amount of X coin liquidity you're willing to add to the pool, serving as a form of slippage control.
* `amount_y_min`: The minimum amount of Y coin liquidity you're willing to add to the pool, similarly acting as slippage control.

If the pool's price shifts by an amount less than or equal to `active_bin_id_slippage` in either direction, all specified `bin_ids` will adjust accordingly, mirroring the price movement's bin shift. Should the slippage exceed the specified `active_bin_id_slippage` or the minimum X, Y amounts, the function will revert. Upon successful execution, the function issues LB tokens, which you should either store in a contract or deposit into an account.&#x20;

Given the complexity, we recommend reviewing our [integration](../integration/) examples for clearer understanding.

### Remove Liquidity

To remove or redeem your liquidity, utilize the function outlined below:

```javascript
public fun remove_liquidity<X, Y, BinStep>(
    liq_nfts: vector<Token>,
    amount_x_min: u64,
    amount_y_min: u64,
): (Coin<X>, Coin<Y>) {
...
```

The function for removing or redeeming liquidity requires the following parameters:

* `liq_nfts`: A vector containing your LB tokens.
* `amount_x_min`: The minimum amount of X coins you expect to receive.
* `amount_y_min`: The minimum amount of Y coins you expect to receive.

If the actual amounts received are below these minimums, the function will revert. Upon successful execution, the function returns the redeemed values of X and Y coins.

## Entry

Source code: [router/sources/entry.move](https://github.com/pontem-network/liquidswap\_v1/blob/main/router/sources/entry.move)

This function operates similarly to the router but is designed for direct transaction calls, interacting with user balances.&#x20;

The main difference lies in the input requirements: instead of LB token or `Coin` objects, users simply specify amounts using primitive types. For detailed insights into the function's specifics, refer to the source code available at the provided link.
