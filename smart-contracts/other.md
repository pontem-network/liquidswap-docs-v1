# Other

## BinSteps

Source code: [bin\_steps/](https://github.com/pontem-network/liquidswap\_v1/tree/main/bin\_steps)

The concept has been previously detailed in the Pools documentation.&#x20;

Within the module, there are functions specifically designed to retrieve both the symbolic and numeric representations of a BinStep. For example, for a BinStep labeled 'X10', the function returns the string 'X10' for its symbolic representation, and numerically, it would return the value 10.

## Math Helpers

Source code: [math\_helpers/](https://github.com/pontem-network/liquidswap\_v1/tree/main/math\_helpers)

The protocol leverages helper functions that implement tree math, optimizing the performance of coin swaps across Bins. At its core, the protocol utilizes fixed-point arithmetic, specifically 128-bit and 64-bit math, to ensure accurate and efficient calculations.

## Config

Source code: [sources/config.move](https://github.com/pontem-network/liquidswap\_v1/blob/main/sources/config.move)

The component houses the current global configuration, primarily encompassing accounts designated for DAO activities, pool creation, and emergency operations.

## Treasury

Source code: [sources/treasury.move](https://github.com/pontem-network/liquidswap\_v1/blob/main/sources/treasury.move)

Keeps the protocol collected fees and allows to withdraw them by DAO account.

## Emergency

Source code: [sources/emergency.move](https://github.com/pontem-network/liquidswap\_v1/blob/main/sources/emergency.move)

The protocol includes a provision for an emergency pause of all swap and mint operations, which can be activated through a transaction initiated by the Emergency account.
