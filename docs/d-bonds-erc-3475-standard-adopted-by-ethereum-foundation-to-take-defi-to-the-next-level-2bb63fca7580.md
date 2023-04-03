# 以太坊基金会采用了 D/Bond 的 ERC-3475 标准，将 DeFi 推向了新的高度

> 原文：<https://medium.com/coinmonks/d-bonds-erc-3475-standard-adopted-by-ethereum-foundation-to-take-defi-to-the-next-level-2bb63fca7580?source=collection_archive---------0----------------------->

**让 D/Bond** *ing* **开始吧！**

它是密封的。

[D/Bond](http://debond.net) 创建了一个新的标准来引入债券并增加价值，以改善以太坊现有的基础设施和生态系统。它现在被以太坊基金会接受，该基金会是网络和生态系统的主要支持者。

该提案已经过评估和辩论。最终，这份报告被认为是安全、完整的，不仅可以呈现给英孚的专家，也可以呈现给所有 Web 3.0 和去中心化金融(DeFi)的爱好者。

Watch the presentation of our EIP-3475 standard to the Ethereum Foundation

因此，EIP-3475 现已被接受为一种新的应用编程接口(API)标准，使得发行具有多种赎回数据的债券成为可能。([看这里为什么 API 必不可少](/p/4aa467a77e05))。)

所以它现在被称为 ERC-3475。

从本质上说，这意味着“我们准备好了”。这意味着 D/Bond 提供的 [ERC-3475](https://eips.ethereum.org/EIPS/eip-3475) 作为一项技术，使任何人都能够创建自己定制的债券，现在已经过测试并获得批准。该平台继续为区块链的一种新资产类别开拓市场:债券。

**ERC-3475 是 DeFi 现在需要的**

分散债券代表了一项革命性的技术。随着 D/Bond 推出其[抽象存储债券标准](https://eips.ethereum.org/EIPS/eip-3475)，它正在扩展并为 DeFi 行业带来额外的价值，该行业一直在发展并承载着传统金融(TradFi)系统中的几乎所有元素。

分散债券[将成为继互换和赌注之后的下一个资产类别](https://finance.yahoo.com/news/unique-defi-innovation-decentralized-bonds-084245875.html)。

***阅读《每日电讯报》要说的话**关于* *债券和金融的未来:***

[](https://cointelegraph.com/news/bonds-havent-played-a-big-role-in-defi-yet-but-thats-about-to-change) [## 债券还没有在 DeFi 中扮演重要角色——但这种情况即将改变

### 世界各地的主要经济体正处于衰退的边缘——持续的不确定性意味着…

cointelegraph.com](https://cointelegraph.com/news/bonds-havent-played-a-big-role-in-defi-yet-but-thats-about-to-change) 

然而，到目前为止，这些固定利率工具在区块链并不十分有效。发行这些有息证券是 DeFi 的一个方面，没有协议能够胜任。这主要是因为现有的令牌标准——使特定类型的令牌符合 API 模板并相应地在链上运行的代码——不能处理债券。

最基本的令牌标准是 [ERC-20](https://ethereum.org/en/developers/docs/standards/tokens/erc-20/) 。ERC-20 代币目前被绝大多数代币使用，包括法定支持的稳定硬币，但在发行债券方面受到限制。他们代表一个单一的实体。它们要求根据令牌类型部署令牌合约。

另一方面，债券有特定的类型，分为各种类别。例如，有些是在固定利率的基础上(如[奥林巴斯道](https://app.olympusdao.finance/#/bonds/inverse))，有些是作为 TradFi 玩家贷款的担保，如[兴业银行](https://www.sgforge.com/securities-finance-trade-digital-bond-on-public-blockchain/)等。

各种债券的不兼容性，加上可用代币标准的限制，使得发行债券对 DeFi 项目来说很困难。

不同的类别使得任何 DeFi 协议发行由其流动性池(LP)令牌支持的债券的计划几乎不可能实现，这些令牌主要是没有复杂数据结构的 ERC-20 令牌。在系统层面，造成碎片化。

ERC-20 格式也不允许在链上存储奖励和兑换逻辑。这通常会导致更高的煤气费。

为了能够发行债券，DeFi 项目需要一个令牌标准，其数据结构复杂，具有多种赎回功能。

缺乏这种功能导致 [DeFi 项目错过了固定利率功能特性](/indexcoop/why-defi-needs-bond-products-b35f05acaf69):他们无法提供明确定义债券合同条款的固定收益产品，如收益率、还款期限、利率、日期等。

此外，早期版本的自动做市商(AMM)——自动提供流动性的过程——使用单独的智能合约和 ERC-20 LP 令牌来管理一对令牌。这一复杂的过程大大降低了池中的整体流动性。结果是不必要的油费和延误。

![](img/258f73cbed179aebd5411862622dbb40.png)

ERC-3475 升级了 ERC-20

ERC-3475 的情况并非如此。[抽象存储债券标准](https://eips.ethereum.org/EIPS/eip-3475)带有新的内置功能，使用户能够节省燃气费用。与 ERC-20 不同，ERC-3475 使用多层池，允许更大的 LP 创建大量的线对。由于每个债券都存储了所有必要的数据，例如其供应和类型，ERC-3475 消除了每次添加新的 LP 对时发布单独合同的需要，以避免像非永久性损失这样的常见攻击媒介。因此，D/Bond 的新标准有助于简化有限合伙人的管理。

它基本上是 ERC-20 LP 令牌的升级，将它转换为存储链上元数据的多维令牌。

凭借这一点，D/Bond 将人人梦寐以求的资产类别带到了区块链——甚至是全球银行体系薄弱的地区。该平台将让尽可能多的个人和机构在债券交易的二级市场发行债券和衍生品。

由于没有现有的债券投资工具或交易所，D/Bond 将使交易、投资、借贷债券成为可能，而没有大银行等中介机构的麻烦。

> 交易新手？试试[加密交易机器人](/coinmonks/crypto-trading-bot-c2ffce8acb2a)或者[复制交易](/coinmonks/top-10-crypto-copy-trading-platforms-for-beginners-d0c37c7d698c)