# LB Tokens

Source code:  [sources/lb\_token.move](https://github.com/pontem-network/liquidswap\_v1/blob/main/sources/lb\_token.move)

The LB token is a fungible asset token (Aptos token version 1) that represents a liquidity provider's position in a specific pool and bin.&#x20;

When a new pool is created on the protocol, it is assigned a unique collection that belongs to that pool. Each time someone adds liquidity to a specific bin, the token associated with that bin is released in the collection

The collection name can be generated using the following function:

```javascript
/// Create collection name by coin symbols and bin step.
/// Returns string.
public fun create_collection_name(
    pool_id: u128,
    x_coin_symbol: String,
    y_coin_symbol: String,
    bin_step_symbol: String,
): String {
    string_utils::format4(&b"Liquidswap v1 #{} {}-{}-{}", pool_id, x_coin_symbol, y_coin_symbol, bin_step_symbol)
}
```

:warning: **Caution**: When using the collection name to identify that a token belongs to the protocol, exercise caution. This is because there can be coins with the same symbols. The pool id, which serves as a counter of pools created on the protocol, helps determine which pool is in use. Additionally, the creation of the collection should be handled by the minter account.

Each LB token issued under a collection has a property that represents the Bin ID. This Bin ID can be extracted using the 'Bin ID' field as a u64 using BCS (Binary Canonical Serialization). The most crucial aspect is that each token issued has an associated amount, which makes it fungible. This amount precisely represents the share of liquidity provided to the specific bin.&#x20;

Tokens can only be burned by the creator (minter) account, which is represented by the resource account of the protocol. On the mainnet, it's:&#x20;

```
0xa0d8702b7c696d989675cd2f894f44e79361531cff115c0063390922f5463883
```
