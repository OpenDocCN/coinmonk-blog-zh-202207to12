# 共识机制:密码的跳动的心脏

> 原文：<https://medium.com/coinmonks/the-consensus-mechanism-a-cryptos-beating-heart-ab3a340a2dfe?source=collection_archive---------54----------------------->

> 探索共识机制及其如何支撑加密货币网络的安全性。

![](img/8feb3bf7743da4056f0d29c334c25c83.png)

*本文原帖*[*【NOAH.com】*](http://noah.com/)*。NOAH 是一款用于全球支付和赚取比特币和 stablecoins 利息的一体化货币应用程序。报名候补* [*这里*](https://mandrillapp.com/track/click/30895797/noah.com?p=eyJzIjoianNhRFBvdkV6c3BFY2JCTjZtcHcxSjlYN3dVIiwidiI6MSwicCI6IntcInVcIjozMDg5NTc5NyxcInZcIjoxLFwidXJsXCI6XCJodHRwczpcXFwvXFxcL25vYWguY29tXFxcLz9yZWZlcnJhbD00Y3pia2Z2JnJlZlNvdXJjZT1jb3B5XCIsXCJpZFwiOlwiYmM2OTFmYmVhMGVhNGRiOWIyMzc1Y2JlMzI4OGI0ZmJcIixcInVybF9pZHNcIjpbXCI0ZTUwMzQwOTI2NTBkMDBlZWIxM2Q1NzM1NWNjNTg4YTExYTgwOGEzXCJdfSJ9) *。*

每个加密资产的核心是其共识机制。共识机制是加密资产验证交易的规则。如果说区块链是数字时代的新大脑，那么共识机制——网络如何集体同意确认交易——就是它们跳动的心脏。

> *没有共识机制，血液无法持续泵入系统。没有共识机制，系统就会崩溃。*

加密领域发展迅速，就在几年前，加密资产还很少，共识协议就更少了。现在，有十几个。大多数加密资产尚未创建具有共识机制的生态系统，这些机制不会在区块链三元悖论中的至少一个指标上妥协。

区块链三难困境表明，区块链无法同时兼顾可扩展性、安全性和去中心化问题。因此，创新者正在创造新的共识机制，最大限度地在这些领域中给予和索取。

虽然区块链三难困境阻止了加密资产最大化可扩展性、安全性和去中心化，但这只是理论上的，因为[闪电网络](https://noah.com/blog/what-is-the-lightning-network)已经表明，在不牺牲其他指标的情况下提高速度是可能的。

> 交易新手？尝试[加密交易机器人](/coinmonks/crypto-trading-bot-c2ffce8acb2a)或[复制交易](/coinmonks/top-10-crypto-copy-trading-platforms-for-beginners-d0c37c7d698c)

# 共识机制的类型

更多的共识机制正在开发中，我们来看一些比较常见的。

[**【POW】**](https://noah.com/blog/proof-of-work-proof-of-stake)**:**想象一下，给某人一把挂锁，告诉他们通过随机猜测来解决它——相当复杂。但是一旦他们找到了代码，就很容易验证:插上电源，看看它是否工作。网络中的矿工解决极其困难但极其容易验证的密码难题。交易被打包成块的密码难题。一旦一个矿工解决了一个障碍，他们的工作就证明他们投入了必要的精力去寻找解决方案。

**赌注证明(POS)**:POS 验证器不是通过能量消耗来验证交易，而是对其持有的股份进行赌注，并以伪随机方式被选择来确定下一个区块。股份被用作抵押品。欺诈性地操纵交易会导致损失，而公平交易会获得回报。具有最大利害关系的链是有效链。

**委托利益证明(DPOS):** 据报道，DPOS 为基本 POS 系统增加了更多速度和效率。利益相关者用他们的硬币选举大宗商品生产商来验证交易。大多数 DPOS 系统都有 20 到 100 个区块生产商需要验证。选择块生产者减轻了负担，理论上加快了事务时间。此外，DPOS 系统可以选举代表来管理治理决策。

**拜占庭容错(BFT):** 拜占庭容错是在给定节点传播恶意或不正确信息的可能性的情况下，区块链网络能够达成共识以正常运行的程度。BFT 从预选的节点组中伪随机地选出一个领导者来向区块链提供信息。如果 66%的节点同意，则达成共识。如果未达到 66%，则选择新节点。

# 很多选择——但是为什么呢？

有许多共识机制，各有利弊。一些关注可伸缩性，而另一些关注安全性或分散化。随着 crypto 的兴起，一个被广泛接受的理解是，一个单一的协议必须在某些领域做出牺牲，才能在其他领域胜出。这就是为什么许多共识机制被开发出来，试图找到这三个关键部分的完美平衡。

但是我们已经认识到，作为一个行业，效率将在分层架构中蓬勃发展，而不是全部基于一个基础协议。我们知道比特币网络是最安全的共识机制(POW)，如果我们在它的基础上建立额外的层，我们就不必满足于交易。例如， [Lightning Network](https://noah.com/blog/what-is-the-lightning-network) 在比特币协议之上创建了第二层，以在不损害去中心化或安全性的情况下显著缩短交易时间。这样，我们就可以鱼与熊掌兼得了。

[POW](https://noah.com/blog/proof-of-work-proof-of-stake) 屡试不爽。我们不必担心它的坚固性，因为它已经经过了十多年的战斗考验。其他共识机制是技术实验，可能成功，也可能失败。在 POW 这样的可信基础之上构建层使我们能够在不牺牲安全性或分散性的情况下进行创新。

闪电仍然遵循与比特币区块链相同的协议规则，这意味着它同样安全。因为它是在[比特币](https://noah.com/blog/what-is-bitcoin)之上的一层，我们仍然可以利用让比特币变得伟大的所有其他特性。

Jackson Rickun 是一名编剧、文案和密码爱好者。杰克逊的写作集中在加密货币、心理健康、性等方面。

你可以在[推特](http://twitter.com/jacksonrickun)和 [Instagram](https://www.instagram.com/jacksonrickun/) 上关注他。

*免责声明:我是诺亚的活跃文案和内容写手。以上并非投资建议，仅供参考。*