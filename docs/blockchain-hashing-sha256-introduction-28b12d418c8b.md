# 区块链哈希(SHA256)简介

> 原文：<https://medium.com/coinmonks/blockchain-hashing-sha256-introduction-28b12d418c8b?source=collection_archive---------56----------------------->

![](img/8f3483b12b51a2e39b422374cd0be569.png)

让我们来看看 SHA256 的运行情况。让我们用[https://andersbrownworth.com/blockchain](https://andersbrownworth.com/blockchain)来演示一下，这个网站提供了一个很好的演示来展示哈希值是如何工作的。当您到达站点时，从顶部导航到 hash 部分，您应该会看到一个类似于大矩形的屏幕。首先，你会注意到我有一个地方可以输入数据和相应的哈希值，这个网站所做的是使用 SHA256 为你输入的任何数据生成一个唯一的哈希值。

试着输入一些东西，观察每次数据改变时哈希值是如何变化的，如果我输入区块链开发者纳米学位程序，它会得到一个哈希值。这个散列是这串文本的唯一值，如果我去掉一些像 developer 这样的文本，散列就变成了完全不同的东西。这是我写的新词的独特之处。如果我把单词 developer 重新输入到我的原始文本中，我又回到了原始散列。

该数据的值将始终是完全相同的字符串。如果世界上的任何人输入相同的字符串，他们也会得到完全相同的散列输出，但是如果数据相似但不完全相同，那么它将是完全不同的散列值。相似的数据不一定意味着哈希值相似。稍微更改数据可能会产生完全不同且不相关的哈希值。

散列的唯一目的是确保它是一个惟一的值，如果您想了解数据是如何关联的，就不要引用它。这是 SHA256 哈希的基本目的，它用于为任何数据值生成数字标识。当我们进入积木、区块链和更详细的概念时，请记住这个基本概念。散列总是服务于这个简单的目的，无论我们自己键入数据，从其他地方获取数据，或者数据是包括许多事务的整个块，都是如此。我希望这个演示有助于哈希变得更容易理解。您可以随意使用这个工具来帮助更好地理解哈希值的工作原理。

![](img/8f211decb9bcb0b64799c6f108dcb8f1.png)

哈希是信息的指纹。它是代表一组数据的字母和数字的独特序列。要获得一个散列值，你需要从一点数据开始，然后通过一个散列函数。哈希函数将一组数据映射到一个唯一的哈希值。它将获取您提供的信息，并创建一个完全唯一的哈希值。这个哈希值现在充当原始数据的唯一标识符。这允许您通过引用给定数据集的哈希值来更容易地识别它。SHA256 是一个特定的哈希函数。SHA 代表安全散列算法，256 代表该算法的输出。

> 交易新手？试试[加密交易机器人](/coinmonks/crypto-trading-bot-c2ffce8acb2a)或者[复制交易](/coinmonks/top-10-crypto-copy-trading-platforms-for-beginners-d0c37c7d698c)

在这种情况下，它告诉我们，您将作为哈希值返回的数字是一个 256 位的数字。有许多不同的哈希函数，但比特币使用 SHA256 为区块链上的每个块创建一个唯一的哈希值。这样做的原因是为链中存在的每个块创建唯一的标识符。这允许我们做一些很酷的事情，比如引用被它们的哈希值阻止。它甚至创造了一个基础，让我们可以把积木连成一条链。要理解这是如何构建这个链条的，我们需要更好地理解布洛克和区块链。这将是我们下一篇课文的主题。

回头见:)

> 加入 Coinmonks [电报频道](https://t.me/coincodecap)和 [Youtube 频道](https://www.youtube.com/c/coinmonks/videos)了解加密交易和投资

# 另外，阅读

*   [CBET 评论](https://coincodecap.com/cbet-casino-review) | [库科恩 vs 比特币基地](https://coincodecap.com/kucoin-vs-coinbase) | [拜比特 vs 比特币基地](https://coincodecap.com/bybit-vs-coinbase)
*   [折叠 App 回顾](https://coincodecap.com/fold-app-review) | [本地比特币回顾](/coinmonks/localbitcoins-review-6cc001c6ed56) | [Bybit vs 币安](https://coincodecap.com/bybit-binance-moonxbt)
*   [加密保证金交易交易所](/coinmonks/crypto-margin-trading-exchanges-428b1f7ad108) | [赚取比特币](/coinmonks/earn-bitcoin-6e8bd3c592d9) | [Mudrex 投资](https://coincodecap.com/mudrex-invest-review-the-best-way-to-invest-in-crypto)
*   [WazirX vs CoinDCX vs bit bns](/coinmonks/wazirx-vs-coindcx-vs-bitbns-149f4f19a2f1)|[block fi vs coin loan vs Nexo](/coinmonks/blockfi-vs-coinloan-vs-nexo-cb624635230d)
*   [比斯勒评论](https://coincodecap.com/bitsler-review)|[WazirX vs coin switch vs coin dcx](https://coincodecap.com/wazirx-vs-coinswitch-vs-coindcx)