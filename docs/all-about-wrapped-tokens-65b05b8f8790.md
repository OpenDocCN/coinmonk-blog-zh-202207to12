# 关于包装好的代币

> 原文：<https://medium.com/coinmonks/all-about-wrapped-tokens-65b05b8f8790?source=collection_archive---------54----------------------->

你好。在当前形式的区块链中，在不同的网络上使用相同的令牌资产存在一个众所周知的难题。它来自于这样一个事实，即大多数著名的区块链是不同的，他们不能互相交流。最初包装令牌的想法就是为了解决这个问题而产生的。

# 它们是如何工作的？

让我们想象一下这种情况，我们想在宇宙链的主要指标——渗透上使用以太坊链中的 ETH。为了做到这一点，我们必须使用提供该服务的桥梁之一。这里是宇宙和以太坊最常用的地方之一。
整个创意是基于**燃烧**和**铸造**的机制。当您对桥的智能合约执行您的 ETH 的存款交易时，它将在目的地链上持有您的 ETH 和 **mint** 等量的包装 ETH(WETH)。对于 WETH - > ETH，WETH 将被**烧毁**而 ETH **铸造**。两种资产的价格总是相同的。

另一个例子是 WBTC。这是一个包装好的 ERC-20 标准代币，与原有的 BTC 挂钩，存在于以太坊链条上。与 WETH 一样，WBTC 要求一些托管人，一个持有与被打包资产数量完全相同的本地资产的实体。托管人可以是智能合约、DAO 或 multisig wallet。

包装令牌存在于任何区块链生态系统中，如 BSC、Solana 等。

# 他们从哪里来？

包装的令牌通常由存在于商家列表上的实体联盟创建。
-他们向包装令牌契约传递他们想要创建多少包装令牌。
-然后，他们将原生代币的数量传递给保管人。
-保管人与包装好的代币合同互动，即 [WBTC](https://etherscan.io/address/0x2260fac5e5542a773aa44fbcfedf7c193bc2c599) 或[无论](https://etherscan.io/token/0xc02aaa39b223fe8d0a0e5c4f27ead9083c756cc2)合同
-新铸造的代币被放在商家手中

# 他们有什么好处？

*   它们允许为许多违约债券中的本土资产聚集更多流动性。
*   有时在几个链上使用包装好的代币更快更便宜
*   它们允许用分散的资产进行 dex 交易，如 [Uniswap](https://uniswap.org/)
*   可临时用作 DeFi app 中贷款的保证金

# 退税？

*   在高波动时期，包装好的代币倾向于呈现比其原生资产更低的价格
*   铸造过程可能很昂贵
*   在市场危机期间，包装好的代币拖垮了整个市场，因为它们主要用于流动性

# 与 stablecoins 的区别

首先也是最重要的，稳定债券与一些经典的资产类别挂钩，如黄金、美元或欧元，有时由 BTC、瑞士联邦银行或 ATOM 支持，只是为了“安全”，而 BTC=WBTC，瑞士联邦银行=WETH，等等。

> *交易新手？试试* [*密码交易机器人*](/coinmonks/crypto-trading-bot-c2ffce8acb2a) *或* [*复制交易*](/coinmonks/top-10-crypto-copy-trading-platforms-for-beginners-d0c37c7d698c) *上* [*最好的密码交易*](/coinmonks/crypto-exchange-dd2f9d6f3769)

> 加入 Coinmonks [电报频道](https://t.me/coincodecap)和 [Youtube 频道](https://www.youtube.com/c/coinmonks/videos)获取每日[加密新闻](http://coincodecap.com/)

# 另外，阅读

*   [免费加密信号](/coinmonks/free-crypto-signals-48b25e61a8da) | [加密交易机器人](/coinmonks/crypto-trading-bot-c2ffce8acb2a)
*   [杠杆代币](/coinmonks/leveraged-token-3f5257808b22)终极指南
*   [16 款最佳折叠电动自行车](/coinmonks/top-17-folding-electric-bikes-5e296f0918cb)
*   [28 款最佳电动自行车点评](/coinmonks/the-28-best-electric-bikes-review-and-buying-guide-in-2023-7bb3146cb403)
*   前三名[币安期货交易机器人](/coinmonks/top-3-binance-futures-trading-bots-e6031f84b3f9)