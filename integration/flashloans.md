# Flashloans

Liquidswap V1 enables users to borrow liquidity from pools, utilize it within any third-party DeFi protocol or even another Liquidswap pool, and then return the borrowed liquidity along with the pool's standard fee.&#x20;

This entire process—loaning and returning liquidity—must be completed within a single transaction. This ensures that Liquidswap's liquidity remains intact, as it's not possible to separate the loan and return actions into different transactions.

The Flashloan feature in Liquidswap V1 is inspired by the 'Hot Potato' concept, which is detailed further in a [Medium article](https://medium.com/@borispovod/move-hot-potato-pattern-bbc48a48d93c). At its core, this concept leverages the capabilities of the Move language, allowing the creation of a special kind of structure. This structure is designed with specific abilities, making it impossible to copy, drop, or store; it can only be destructed.

Here's how a Flashloan operates in Liquidswap V1:

{% embed url="https://gist.github.com/borispovod/0bfbc1c019d93277a0a703b6003306fe" %}

