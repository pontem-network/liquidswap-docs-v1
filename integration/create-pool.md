# Add Liquidity

Given that pool creation on Liquidswap requires specific permissions, this tutorial will focus on adding liquidity as the accessible action for users. We will be working with APT and test USDT, utilizing a BinStep of X5:

{% embed url="https://gist.github.com/borispovod/b33938f8ae517822edfe2618334c3500" %}

In the example above, we first withdraw a fixed amount of coins from the account and prepare the necessary information regarding bin IDs and liquidity. Next, we specify the slippage for the active bin along with the active bin itself as transaction parameters.&#x20;

Following this, we proceed to add liquidity. Finally, any leftovers and LB tokens are deposited back into the account.
