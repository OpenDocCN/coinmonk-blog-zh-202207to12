# 交易现时

> 原文：<https://medium.com/coinmonks/transaction-nonce-a025321a0214?source=collection_archive---------6----------------------->

![](img/b50ebdf46cc79eaeb622b5a9ec12829c.png)

Photo by [GuerrillaBuzz Crypto PR](https://unsplash.com/@theshubhamdhage?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/blockchain?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

随机数是交易最重要的组成部分之一，对于基于帐户的协议(如以太坊)至关重要。

> *none:一个标量值，等于从此地址发送的交易数量，或者在帐户具有关联代码的情况下，等于此帐户创建的合同数量。*

(以太坊黄皮书)

随机数是源地址的一个属性；它是通过计算源自某个地址的已确认交易的数量来动态计算的。

*Nonce 是一个从零开始的计数器，第一次交易为 0。*

让我们看两个场景来看看 nonce 在事务中的重要性:

1.  这个例子说明了为什么按照创建顺序在事务中计算和包含 nonces 非常重要。假设您需要进行两个事务，一个是 1 ether，另一个是 2 ether。您签署并发送第一个事务(1 以太)，因为它更重要和紧急，然后您签署并发送第二个事务(2 以太)。但是你要记住，你的钱包里只有两个以太网，只有一个交易会被网络接受。你可以认为，既然你先发送了第一笔交易，那笔交易就会被接受，第二笔交易就会被拒绝。但是在以太坊这样的去中心化系统的世界里，节点可能以任何顺序接收事务；不能保证第一个事务会在第二个事务之前被挖掘，这些事务很容易被发送到不同的节点。如果没有 nonce，接受或拒绝哪个交易将是随机的。但是，添加了 nonce 后，您发送的第一个事务将具有 nonce，10(作为示例)，而第二个事务将具有下一个 nonce 值(即 11)。包括在内，如果第二个事务首先到达节点，它将被忽略，直到 nonces 从 0 到 10 的事务被处理，即使它首先被接收。
2.  第二个例子展示了 nonce 如何成为保护事务复制的特性。假设您正在转账 2 以太网以支付产品，因此您签署并发送了交易，然后节点接收该交易以进行验证并将其包含在区块链中，但是如果没有随机数，该交易可以很容易地复制，并且一个人可以“重放”该交易并多次发送它，直到耗尽您钱包中的所有以太网(随机数使每个交易都是唯一的)。这被称为重放攻击。

**结论**

简而言之，nonce 是源自账户的**确认的**交易的数量的最新计数。

*您的钱包负责跟踪它管理的每个地址的随机数。*

这是对什么是随机数及其重要性的一个快速而简短的解释。

在这个主题上还有更多的内容要介绍，尤其是如果您正在构建自己的钱包或其他一些发起交易的应用程序。

谢了。

> 交易新手？试试[加密交易机器人](/coinmonks/crypto-trading-bot-c2ffce8acb2a)或者[复制交易](/coinmonks/top-10-crypto-copy-trading-platforms-for-beginners-d0c37c7d698c)