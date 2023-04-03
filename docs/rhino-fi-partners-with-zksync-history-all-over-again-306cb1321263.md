# rhino.fi 与 zkSync 合作:历史重演

> 原文：<https://medium.com/coinmonks/rhino-fi-partners-with-zksync-history-all-over-again-306cb1321263?source=collection_archive---------35----------------------->

Rhino.fi 将很快与 zk Sync 2.0 整合，ZK Sync 2.0 是第一个在以太坊主网上上线的完全兼容的 EVM ZK 汇总。这就是为什么这对你、我们和 DeFi 很重要。

2020 年，我们与 StarkWare 合作推出了 StarkEx 的可扩展 exchange 实例。这使得 rhino.fi(当时被称为 DeversiFi)成为以太坊上第一个为用户上线的扩展解决方案之一。

从那时起，zk rollup space，以及整个 rollup space，已经发展得面目全非，我们非常自豪能够以多种不同的方式为以太坊社区的发展做出贡献。

现在，我们很高兴地宣布，我们将很快与 zkSync 2.0 集成，这是第一个完全兼容的 EVM zk rollup，将在以太坊主网上上线。

# ZK 卷:结束游戏

你可能会问，为什么 rhino.fi 选择基于 zkSync 2.0 构建？

我们一直认为，零知识，或者说 zk，证明技术是以太坊缩放的终结游戏。

我们选择 StarkEx 作为我们最初的起点，因为对于高速、低成本交易的定制用例来说，这项技术已经接近成熟。

当时，大多数参与 zk 扩展研究的人认为，尽管针对支付或交易等特定用例定制特定应用的 zk 汇总是可行的，但通用计算(如以太坊虚拟机)能够在 zk 汇总中复制还需要很多很多年。

然而，自那以来，创新的步伐一直令人咋舌。

许多团队现在正在证明 ZK roll up*可以*用于扩展一个通用虚拟机，而且他们做得比预期的要早得多。目前 zkSync，Polygon Hermez，StarkNet 都准备在 mainnet Ethereum 上推出。

zkSync 从一开始就支持这个想法，并围绕自己建立了一个奇妙的生态系统，可以在 [zkSync 生态系统页面](https://ecosystem.zksync.io/)上查看。

我们的目标是成为一个平台，将所有的汇总和 EVM 链整合到一个无缝的用户体验中，所以我们知道我们必须从 zkSync 推出的第一天起就加入它。

# 整个 ZK 生态系统

我们的目标是帮助 rhino.fi 用户从一开始就探索 zkSync 生态系统并与之互动，并从在那里启动的项目生态系统中获得最大收益。

首先，我们将部署一个到 zkSync 的桥，帮助我们的用户进入我们已经支持的网络:Ethereum、Polygon、BSC 和 StarkEx。

接下来，我们将推出跨链交换，整合所有交易所，并与我们在 Paraswap 的合作伙伴合作。

最后，我们计划与其他基于 zkSync 的 DeFi 项目直接集成，使他们的应用程序更容易在 rhino.fi 中访问，包括 yield 机会。

# zkSync 2.0 和 zk rollup 技术的优势

zkSync 的一个伟大之处在于，它很好地模拟了 EVM，任何现有的 solidity 智能合约都可以直接部署，并有望像在以太坊或另一个 EVM 链(如 Polygon)上一样工作。

还有几个独特的改进。在 rhino.fi 上，我们特别兴奋地使用的一项功能是“帐户抽象”。

帐户抽象是一个非常强大的特性，意味着帐户可以携带额外的逻辑来改善用户体验。这可以包括帐户恢复(而不是依赖于单个私钥)、多签名钱包、以其他货币支付费用等等。

作为 zk 汇总，与其他 EVM 连锁店相比的一个核心区别是，L1 州需要被观察以确认终结。

区块分三次提交给 L1:

1.  犯罪
2.  证明
3.  执行

包括在执行步骤中的块可以被认为是最终的，并且证明中的 L2 块不能被恢复。

# 关于 zk 和 zk 汇总的更多信息

在 rhino.fi，我们从一开始就是 zk rollup 技术的忠实粉丝。我们认为它有潜力让区块链的交易更快、更高效，最重要的是，更不可信。

在这里，我们有一个关于 zk rollup 技术(特别是 [zk 校对](https://rhino.fi/blog/what-is-zero-knowledge-proofing-and-why-do-we-use-it/))的完整解释。我们的“ [zk 汇总 vs 乐观汇总](https://rhino.fi/blog/what-are-rollups/(opens%20in%20a%20new%20tab))”指南解释了汇总的两个主要分支在证明机制上的不同。

我们也强烈建议你查看以太坊自己的[ZK 汇总项目](https://ethereum.org/en/developers/docs/scaling/zk-rollups/)指南，它非常详细地概述了这项技术是如何工作的。

> 交易新手？尝试[加密交易机器人](/coinmonks/crypto-trading-bot-c2ffce8acb2a)或[复制交易](/coinmonks/top-10-crypto-copy-trading-platforms-for-beginners-d0c37c7d698c)