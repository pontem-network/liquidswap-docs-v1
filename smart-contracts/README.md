# Smart Contracts

The contracts are published in the Liquidswap GitHub repository at [https://github.com/pontem-network/liquidswap\_v1](https://github.com/pontem-network/liquidswap\_v1)

Liquidswap V1 employs smart contracts written in the Move language, executed on the Move VM. This setup ensures the core contracts, which encompass the protocol's logic and safety guarantees, are distinctly separated from the peripheral contracts, such as routers or entry points (scripts). While the current implementation of the periphery is quite basic, it's designed to accommodate multiple peripherals in the future. Importantly, these enhancements will not impact the core codebase.

## Branches and versions

The current `main` branch is the development branch and always contains the latest changes. Release branches contain the latest changes created for the specific release.\
\
You can always find the latest release in the [Releases](https://github.com/pontem-network/liquidswap\_v1/releases) section.

## Deployments

**Aptos Mainnet**

```
# Module addresses (deployer)
0x54cb0bb2c18564b86e34539b9f89cfe1186e39d89fce54e1cd007b8e61673a85

# Resource account
0xa0d8702b7c696d989675cd2f894f44e79361531cff115c0063390922f5463883

# Emergency resource account
0x9aa8dec24fe7a592de107b4622069c4ed60567900fa3eddade6326d92e9b751c
```

#### **Aptos Testnet**

:warning:If you wish to experiment with Liquidswap V1 on the testnet, please utilize the `testnet` branch of the Liquidswap V1 repository.

```
# Module addresses (deployer)
0xc9ccc585c8e1455a5c0ae4e068897a47e7c16cf16f14e0655e3573c2bbc76d48

# Resource account
0x62ff269698a76cf6bccf256d93fc1963926935f74e72d89f5af653ad882568d8

# Emergency resource account
0x370b9a4b15952b33784662e92493b7a78bc93ea2a8d8569028acab49196c2128
```

## Coin Generics Sorting

:warning:I**t is crucial to ensure that coins are sorted correctly in Liquidswap V1. Unlike versions V0/V0.5, automatic sorting is not functional in this version, and this applies even at the Router level.**&#x20;

When interacting with the Pool module functions in Liquidswap V1, it's imperative to sort the coins passed as generics. Proper sorting is crucial because it establishes the rules for creating liquidity pools. It's important to note that all functions within the Liquidity Pool module and others require sorted generics to function correctly; failing to do so will result in the transaction being reverted.

The current sorting algorithm takes the types of provided coins, e.g., `address::module::struct_name`, and compares the struct name of both coins. If it's equal, it continues with the module name and, in the end, with the address.

You always can just use the [implementation](https://github.com/pontem-network/liquidswap\_v1/blob/main/sources/pool.move#L1292) in Move language.

Other languages:

* Implementation in Javascript: [Coin Sorting In JS](https://gist.github.com/borispovod/2809728c8959649d42c5cef15b4cedb7).
* Also, it's supported in [Typescript SDK](broken-reference).

