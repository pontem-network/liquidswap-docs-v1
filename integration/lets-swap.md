# Let's Swap

**Always practice solid risk management rules when experimenting with swaps: use the correct slippage values and trusted currency pairs, double-check the numbers before confirming, and compare external price sources.**

In this guide, we'll focus on using the Router to perform a swap from Aptos coins to test USDT. To begin, let's create a new module and then import the Router into this module:

{% embed url="https://gist.github.com/borispovod/1fbdef36000268a18c975e157118ae23" %}

When you compile the code provided above, you may encounter a warning. This is expected and not a cause for concern. Next, we'll create a function to engage with a test pool for APT/USDT with a BinStep of 5. This function will facilitate the purchase of APT using the USDT available in your account.

&#x20;Let's start by calculating the expected amount of APT we can receive through this swap:

{% embed url="https://gist.github.com/borispovod/1173c1ac604886c306766ef6446bc4ee" %}
Get amount we will get after swap
{% endembed %}

Now, let's proceed to withdraw the specified amount and perform the swap:

{% embed url="https://gist.github.com/borispovod/c8f3110e625057d670eb2c26158253f7" %}
Perfom swap
{% endembed %}

Everything proceeded successfully; through the `buy_apt` function, we've swapped test USDT for APT. Importantly, we utilized the leftover value to precisely calculate the amount needed for the swap. However, it's crucial to note that the code provided here should not be used in a production environment, as it's vulnerable to being front-run. This example is for demonstration purposes only, and you must configure slippage settings appropriately before sending a transaction for production use.&#x20;

A more suitable approach for production environments would include:

{% embed url="https://gist.github.com/borispovod/af0dfb79fc616729d7cf232d3c67597c" %}
Example with offchain slippage
{% endembed %}

All done. Similarly, you are encouraged to explore and experiment with other router functions detailed in the [Router](../smart-contracts/#router) documentation. This exploration can provide further insights and capabilities within your projects.

_An important note regarding decentralized AMM securities and user transactions:_

* _Always use safety checks._
* _Never swap tokens without a slippage value provided offline._
* _Use external price feeds or oracles._
