# 代理—非结构化存储

> 原文：<https://medium.com/coinmonks/proxy-unstructured-storage-7f41a9ac22f2?source=collection_archive---------10----------------------->

这种存储模式类似于[继承存储](/coinmonks/proxy-inherited-storage-7887f63944e6)模式，但是有一个很大的区别。逻辑契约不需要知道代理的存储布局。

我在[的上一篇文章](/coinmonks/variable-immutability-proxy-112b861a9cb4)中写道，开发人员创建不同类型的存储来避免存储冲突。在继承存储模式中，所有状态变量都被写入存储的第一个槽中。但是这导致了一个很大的风险，这个槽可能被覆盖。

因此，在非结构化存储模式中，所有代理的状态变量(如逻辑契约的地址)都被写入伪随机槽中。这个槽的索引是这样产生的

```
*bytes32 private constant implementationPosition = bytes32(uint256(
keccak256(‘eip1967.proxy.implementation’)) – 1 ));*
```

结果取决于输入，'*EIP 1967 . proxy . implementation*'在我们的例子中，它保存为一个常量，所以没有什么会覆盖它，因为常量值不存储在存储器中。写代理状态变量的槽由 [EIP-1967](http://eips.ethereum.org/EIPS/eip-1967) 定义。

可靠性使用带有 2 个⁵⁶插槽的存储器。所以，我们的逻辑覆盖这一个槽的机会非常非常小。

该模式的其余部分与继承存储相同。因此，新的逻辑契约必须继承旧的逻辑契约的所有状态变量。

# 摘要

## 优势

*   覆盖代理的状态变量的风险是最小的
*   这只是简单的方法

## 不足之处

*   新版本需要继承可能包含许多他们不使用的状态变量的存储。因此，随着时间的推移，这种方法的实施成本可能会很高。
*   该逻辑的所有实现变得与特定的代理契约紧密耦合，并且不能被声明不同状态变量的其他代理契约使用。

我希望这篇文章对你有用。如果你有任何想法，我如何能使我的帖子更好，请告诉我。我随时准备学习。你可以在 [LinkedIn](https://pl.linkedin.com/in/szymon-skrzy%C5%84ski-881462214) 和 [Telegram](https://t.me/eszymi) 上和我连线。

如果你想和我谈论这个话题或者我写的其他话题，请随意。我乐于交谈。

快乐学习！

> 交易新手？尝试[加密交易机器人](/coinmonks/crypto-trading-bot-c2ffce8acb2a)或[复制交易](/coinmonks/top-10-crypto-copy-trading-platforms-for-beginners-d0c37c7d698c)