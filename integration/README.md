# Integration

Before we begin, please create a new Move project. This initial step will enable you to follow along with the subsequent instructions more effectively.

## Deployments

The code for [Liquidswap v1](https://github.com/pontem-network/liquidswap\_v1) is available in our GitHub repository. You can access it to review, download, or contribute to the project.

### Mainnet

The contracts for our project have been deployed on the Aptos mainnet at the following addresses:

<pre><code><strong># Module addresses (deployer)
</strong>0x54cb0bb2c18564b86e34539b9f89cfe1186e39d89fce54e1cd007b8e61673a85

# Resource account
0xa0d8702b7c696d989675cd2f894f44e79361531cff115c0063390922f5463883

# Emergency resource account
0x9aa8dec24fe7a592de107b4622069c4ed60567900fa3eddade6326d92e9b751c
</code></pre>

### Testnet

If you are working with the Aptos testnet, please use the following addresses:

```
# Module addresses (deployer)
0xc9ccc585c8e1455a5c0ae4e068897a47e7c16cf16f14e0655e3573c2bbc76d48

# Resource account
0x62ff269698a76cf6bccf256d93fc1963926935f74e72d89f5af653ad882568d8

# Emergency resource account
0x370b9a4b15952b33784662e92493b7a78bc93ea2a8d8569028acab49196c2128
```

:warning: **Caution:** ensure you switch to the testnet branch when conducting experiments on the Aptos testnet.

## Dependency

To integrate Liquidswap into your project, begin by adding the Liquidswap dependency to your `Move.toml` file:

{% code lineNumbers="true" %}
```toml
[dependencies.LiquidswapV1]
git = 'https://github.com/pontem-network/liquidswap_v1.git'
rev = 'latest version or testnet'
```
{% endcode %}

Continue the setup by importing the rest of the required dependencies into your project. Ensure each dependency is correctly listed in your 'Move.toml' to facilitate seamless integration:

```toml

[dependencies.BinSteps]
git = 'https://github.com/pontem-network/liquidswap_v1.git'
subdir = 'bin_steps/'
rev = 'latest version or testnet'

[dependencies.Router]
git = 'https://github.com/pontem-network/liquidswap_v1.git'
subdir = 'router/'
rev = 'latest version or testnet'
```

Ensure to replace \`latest version or testnet' with the actual version numbers sourced from the repositories. After updating these details, proceed to compile your project. Ideally, this step should complete without any errors.

