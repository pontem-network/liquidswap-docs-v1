# Fees

Liquidswap V1 revolutionizes fee structures by introducing dynamic fees that adjust based on the volatility of each trading pair. This system builds upon the foundations laid by versions V0 and V0.5, where fees are distributed among liquidity providers who have contributed to bins directly involved in swaps. This means that if a swap does not utilize your provided liquidity, you will not earn fees from that transaction. Fees are accumulated within the pool and can be claimed by liquidity providers upon withdrawing their liquidity.

The fee model for each pair in Liquidswap V1 is multifaceted, comprising a base fee, a volatility-dependent dynamic fee, and a cap on the maximum fee to ensure fairness and predictability. The base fee is a mandatory charge applied to all transactions if enabled and is calculated using a 'base factor' established during pool creation, in conjunction with the bin step. This structured approach ensures a balanced and responsive fee system that rewards providers based on market dynamics and their contribution to the liquidity pool:

```
base_fee = base_factor * BinStep
total_fee = base_fee + variable_fee
```

The variable fee in Liquidswap V1 is designed to reflect market volatility, essentially determined by the number of bins crossed during a swap and the time elapsed since the last transaction. This fee is calculated using a 'volatility accumulator', which in turn is influenced by several factors:

1. **Volatility Reference**: A benchmark for assessing market volatility, updated with each swap. It accounts for the amount of market movement, represented by the bins crossed during a swap.
2. **Volatility Control**: This mechanism adjusts the variable fee by considering the frequency of transactions within the pool and the extent of market fluctuations.
   * After each swap, the system tallies the bins crossed to gauge recent market activity.
   * The volatility reference is updated based on the 'volatility accumulator', adjusted downwards by a reduction factor to reflect ongoing market conditions.
   * For pools with low transaction frequency, the volatility reference is reset to zero after a specific 'decay period', acknowledging periods of low market activity.
   * Pools with high transaction frequency maintain their volatility reference, ensuring that the variable fee accurately reflects sustained market activity.
   * In cases of standard transaction frequency ('filter period'), the volatility reference is methodically reduced according to the predefined reduction factor.

This mechanism is designed to adjust fees dynamically, ensuring they align with the prevailing market conditions. It does this by being sensitive to both the frequency of transactions and the extent of price fluctuations within the pool. The 'volatility accumulator' is a key component in this process, calculated by tallying the number of bins traversed during a swap and integrating this with the 'volatility reference'. This calculation ensures that the variable fee remains within the maximum volatility accumulator threshold.

It's important to note that all parameters influencing this fee model are set during pool creation. With future updates aimed at making pool creation permissionless, these settings will be guided by a selection of pre-defined options. However, not all fees within the protocol are subject to this dynamic model:

* **Treasury Fees**: These remain constant and may only be adjusted by the DAO within predefined limits, ensuring a stable contribution to the treasury regardless of market volatility.
* **Flashloan Fees**: Similarly, fees for flashloans are fixed, pre-configured, and do not fluctuate with market conditions. This ensures clarity and predictability for users utilizing flashloan services

## Module

Source code: [sources/libs/fees\_helper.move](https://github.com/pontem-network/liquidswap\_v1/blob/main/sources/libs/fees\_helper.move)

The fee calculation module plays a crucial role in Liquidswap. Upon the creation of each pool, a `StaticFeeParams` struct is established within the fees helper module. This struct encapsulates the pool's fee parameters, including the base fee, dynamic fee, and other relevant variables.

Additionally, the module offers getter functions designed to access these fee parameters, allowing users to retrieve detailed information for any specific pool. For an in-depth understanding of how fee parameters are managed and utilized within the protocol, we recommend consulting the source code.
