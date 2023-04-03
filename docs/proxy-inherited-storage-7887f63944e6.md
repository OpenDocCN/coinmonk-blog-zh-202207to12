# 代理—继承的存储

> 原文：<https://medium.com/coinmonks/proxy-inherited-storage-7887f63944e6?source=collection_archive---------2----------------------->

正如我在[上一篇文章](/@eszymi/variable-immutability-proxy-112b861a9cb4)中提到的，不恰当地使用 *delegatecall* 函数可能会导致安全漏洞。其中一个问题是对变量的偶然重写，因为*delegatecall*没有使用变量的名称，而是它们在存储中的位置。

让我们看看这些合同。`addressOfLogic`变量包含当前逻辑实现的地址。只有`owner`能够改变这个变量。逻辑契约给了我们两个功能:设置值`var1` i `var2`。但是当我们代理使用的时候，结果会和我们想象的不一样。如果我们调用参数为 5 的函数 *setVar1* ，那么代理的第二个存储元素的值将等于 5，所以在我们的例子中它将是`var2`。所以如果我们不小心，我们可能会不小心改变了不正确的变量。

使用逻辑中实现的第二个函数，我们可以注意到代理存储和逻辑存储之间的差异会产生多大的后果。乍一看，和前面的函数差不多。但是这个函数改变了第三个存储槽中变量的值。在逻辑中是`var2`，但在代理中是`owner`。这是我们代理安全性的一个很大的缺口。有人可以使用 *setVar2* 作为参数设置自己的地址，这样他就成为了代理的所有者，结果，他就可以设置`addressOfLogic`并实现自己的逻辑。

# 如何调整我们的储物空间？

我们现在知道逻辑和代理的存储必须相同，但是当我们升级逻辑时，我们应该如何处理存储呢？我们的新逻辑必须从以前的逻辑中继承存储，所以新版本不能改变存储的结构，但是可以在下一个未被占用的存储槽中添加新的状态变量，所以在现有变量的末尾。让我们看看例子。如果我们有逻辑 1

```
contract LogicV1 {
    uint256 public var1;
    uint256 public var2;
}
```

我们不能像那样将其升级到 LogicV2 合同:

```
contract LogicV2 {
    uint256 public var1; 
}
```

移除`var2`(理论上是允许的，但是我们必须记住`var2`的值仍然在存储器的第二个槽中。因此，如果我们用新变量创建 LogicV3(升级此契约 LogicV2 的契约)，new 的值将在开始时等于`var2`的值

```
contract LogicV2 {
    uint256 public var2;
    uint256 public var1; 
}
```

改变 var1 和`var2`的位置

```
contract LogicV2 {
    uint256 public var1;
    address public var2;
}
```

改变`var2`的类型

```
contract LogicV2 {
    uint256 public var1;
    uint256 public var3;
    uint256 public var2;
}
```

在现有变量的末尾添加一个新变量。

新实现中新变量的正确添加如下所示

```
contract LogicV2 {
    uint256 public var1;
    uint256 public var2;
    uint256 public var3;
}
```

如果我们想改变变量的名字，我们可以这样做。但是允许的只是它的名字，没有类型。所以这也是一份合适的新合同

```
contract LogicV2 {
    uint256 public slot0;
    uint256 public slot1;
    uint256 public var3;
}
```

# 从其他契约继承的逻辑契约的升级

让我们假设我们的逻辑契约看起来像

```
contract Logic is A, B{
    uint256 public var1;
    uint256 public var2;
}
```

合同 A 和 B 看起来像

```
contract A {
    uint256 public A0;
}contract B {
    uint256 public B0;
}
```

那么我们的存储逻辑是(存储[0]是存储的第一个槽，存储[1]是第二个槽，依此类推)

```
storage[0] -> A0
storage[1] -> B0
storage[2] -> var1
storage[3] -> var2
```

很容易预测，如果有人在 A 或 B 中增加了一个新变量，逻辑的存储顺序会有所不同。同样，如果我们写

```
contract Logic is B, A{
    uint256 public var1;
    uint256 public var2;
}
```

未来的存储将会有所不同。

如果我们不确定在母公司的合同中不需要更多的变量，我们可以使用存储间隙来保留存储。为此，我们像这样声明固定数组

```
contract A {
    uint256 public A0;
    uint256[100] public __gap;
}
```

如果将来我们需要添加一个新的变量，我们只需要写

```
contract A {
    uint256 public A0;
    uint256 public A1;
    uint256[99] public __gap;
}
```

__gap 这个名字来自于 OpenZeppelin 创建的一个约定。当我们谈论 OpenZeppelin 时，我使用了他们的[伟大文章](https://docs.openzeppelin.com/upgrades-plugins/1.x/writing-upgradeable)中的信息。

## 另一种可能性

另一种选择是创建包含所有状态变量的契约。逻辑和代理都将继承它。谢谢，我们确信它们的存储顺序完全相同。

# 摘要

我们熟悉了继承存储代理模式的概念。我们来看看这样做的利弊。

## 优势

*   这给了我们更新合同的可能性
*   这只是简单的方法

## 不足之处

*   新版本需要继承可能包含许多他们不使用的状态变量的存储。因此，随着时间的推移，这种方法的实施成本可能会很高。
*   该逻辑的所有实现变得与特定的代理契约紧密耦合，并且不能被声明不同状态变量的其他代理契约使用。

我希望这篇文章对你有用。如果你有任何想法，我如何能使我的帖子更好，请告诉我。我随时准备学习。你可以在 [LinkedIn](https://pl.linkedin.com/in/szymon-skrzy%C5%84ski-881462214) 和 [Telegram](https://t.me/eszymi) 上和我联系。

如果你想和我谈论这个话题或者我写的其他话题，请随意。我乐于交谈。

快乐学习！

> 交易新手？尝试[加密交易机器人](/coinmonks/crypto-trading-bot-c2ffce8acb2a)或[复制交易](/coinmonks/top-10-crypto-copy-trading-platforms-for-beginners-d0c37c7d698c)