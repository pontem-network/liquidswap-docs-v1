# Introduction v1

Liquidswap V1 represents the inaugural Concentrated Liquidity Automated Market Maker (AMM) on the Aptos blockchain. It marks a significant evolution from the previous iterations of the Liquidswap protocol, namely versions V0 and V0.5

<figure><img src=".gitbook/assets/Снимок экрана 2024-01-30 в 16.11.22.png" alt=""><figcaption><p>Liquidswap V1 Dapp</p></figcaption></figure>

Built on a liquidity book model, Liquidswap V1 offers heightened efficiency and flexibility compared to its predecessors. This advancement significantly reduces impermanent loss and enables zero slippage. Additionally, it opens up a myriad of strategic possibilities for both traders and liquidity providers.

## Features

Key Features and Advantages of Liquidswap V1 Compared to Versions V0/V0.5:

1. **No Slippage**: Liquidswap V1 employs a unique system where liquidity is divided into 'bins', each representing a specific price range. This structure effectively eliminates slippage.
2. **Bins**: In this system, liquidity is organized into bins. This allows for the existence of the same trading pairs with varying bin steps, which are essentially the price changes between bins. Users have the flexibility to add liquidity at any price and in any configuration.
3. **Fees**: Fees in Liquidswap V1 are dynamic and tailored to the volatility of each pair. Pools with high frequency and significant volatility attract higher fees, aligning costs with market dynamics.
4. **Liquidity Positions**: Instead of traditional LP (Liquidity Provider) tokens, providers in Liquidswap V1 receive fungible tokens that represent their share in a specific bin. This offers a more precise representation of their stake.
5. **Unified Curve Approach**: With V1, there's no longer a need for different curves for stable and uncorrelated pairs. Stable pairs function efficiently with the smallest bin steps, simplifying the overall process.

And more:

1. **Written in Move Language**: Liquidswap V1 is developed using the Move language, renowned for its flexibility and inherent safety features. This choice underscores the protocol's commitment to security and adaptability.
2. **High Speed**: Leveraging the BlockSTM feature of the Aptos consensus, Liquidswap V1 offers exceptionally high transaction speeds. This efficiency is a key advantage in the fast-paced world of cryptocurrency trading.
3. **Security Review**: The source code of Liquidswap V1 has undergone rigorous reviews by several authoritative security teams, ensuring a high level of security and reliability for its users.

## Decentralized Application :tada:

The current version of Liquidswap is actively deployed on both the Aptos mainnet and testnet. DApps utilizing Liquidswap can be accessed through the following links:

* [https://cl.liquidswap.com](https://cl.liquidswap.com) - on the Aptos mainnet.
* [https://testnet.cl.liquidswap.com](https://testnet.cl.liquidswap.com/#/pools) - on the Aptos testnet.

Please test Liquidswap v0 for yourself and share your feedback - it will be greatly appreciated 😊

## Security audits

[Ottersec](https://osec.io/)

{% file src=".gitbook/assets/Liquidswap v1 - Ottersec Audit Report.pdf" %}

[Zellic](https://www.zellic.io/)

{% file src=".gitbook/assets/Liquidswap v1 - Zellic Audit Report.pdf" %}

## Bounty Program

We are currently collaborating with Immunefi to initiate and implement a bounty program, akin to those established for previous versions of our platform.

## Developer Links

For a general idea about Liquidswap v1, continue with the [Protocol Overview](protocol-overview.md).

To get hands-on with the DEX, check out the tutorials:

* [Smart contracts usage & integrations](integration/)

Looking for source code? Visit our [Github](https://github.com/pontem-network):&#x20;

* [Liquidswap v1](https://github.com/pontem-network/liquidswap\_v1)

Need help integrating your service with Liquidswap? Feel free to contact us:

* [Telegram](https://t.me/pontemnetworkchat)
* [Discord](https://discord.gg/44QgPFHYqs)
