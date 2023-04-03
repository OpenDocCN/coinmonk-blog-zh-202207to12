# 10 种简单可靠的省油方法

> 原文：<https://medium.com/coinmonks/10-simple-ways-to-save-gas-with-solidity-f74cd2a59089?source=collection_archive---------2----------------------->

![](img/08d7feff48527a7658976fdf2616a107.png)

下面是一些简单的方法，你可以在合同中节省汽油。完整的列表似乎是无穷无尽的，但都归结为找到最有效的方法，以最少的操作执行一个动作。

## 目录

1.  状态数据与字节码数据
2.  do-while vs for 循环的威力
3.  i++对++i
4.  忽略安全数学—未选中{}
5.  坚持首选数据类型
6.  呼叫数据与内存
7.  使用内存缓存读取变量
8.  有效的变量管理
9.  第一次写入时节省电量
10.  调用结构数据

## 1.状态数据与字节码数据

通过将`_b`存储为常量，它将作为不可变数据存储在契约的字节码中。我们的神奇数字`1000`也是如此。另一方面，`_a`存储在契约的状态中，需要一个`SLOAD`操作来读取它。

## 2.do-while vs for 循环的威力

几乎在每一种编码语言中，循环都是非常标准的，这种熟悉会让我们自满。

标准的`for loop`将在执行语句前检查条件。A `do while`将至少执行一次该语句，然后检查条件，以节省汽油。

> **警告:**只有当你确定代码应该至少执行一次时才使用 do-while，并且记住处理你的条件以防止无限循环和耗尽。

有关 do-while 循环与 while 循环有何不同的信息，请查看本文:

[](https://www.guru99.com/while-vs-do-while.html) [## while 和 do-while 的区别:举例说明

### 循环多次执行语句序列，直到所述条件变为假。一个循环由两个…

www.guru99.com](https://www.guru99.com/while-vs-do-while.html) 

## 3.i++对++i

我知道，我知道，这看起来有点疯狂，但数字不会说谎——但为什么会更便宜呢？

`i++`通过将原始值存储在存储器中来增加`i`的值，增加它，然后将结果值存储在临时存储器中并返回原始值。在返回时，`i`的新值被更新。总之需要 4 次手术。

`++i`递增`i`的值并返回该值，需要 2 次运算。

## 4.忽略安全数学—未选中{}

来自实体文档:

[https://docs . solidi tylang . org/en/v 0 . 8 . 0/control-structures . html # checked-or-unchecked-算术](https://docs.soliditylang.org/en/v0.8.0/control-structures.html#checked-or-unchecked-arithmetic)

> 在 Solidity 0.8.0 之前，算术运算总是会在下溢或溢出的情况下换行，这导致了引入额外检查的库的广泛使用。
> 
> 从 Solidity 0.8.0 开始，默认情况下，所有的算术运算都会发生上溢和下溢，因此不需要使用这些库。
> 
> 为了获得前面的行为，可以使用未检查的块:

这意味着您可以通过使用`unchecked`块来避免对您的`++i`进行不必要的检查，从而节省汽油。

在上面的示例代码中，这样做是安全的，因为我们知道`++i`不会溢出 2 个⁵⁶，当`i == 200`时，我们的两个循环都将停止处理

## 5.坚持首选数据类型

EVM(以太坊虚拟机)由 256 位(32 字节)变量驱动。这意味着在我们上面的循环中，`uint16`变量在被使用前被转换为`uint256`，这种转换耗费了汽油。

如你所见，我们通过使用变量`i`的`uint256`来节省汽油。

## 6.呼叫数据与内存

来自实体文档:

[https://docs.soliditylang.org/en/v0.5.11/types.html?高亮显示=存储器#数据位置](https://docs.soliditylang.org/en/v0.5.11/types.html?highlight=memory#data-location)

> Calldata 仅对外部协定函数的参数有效，并且对于此类参数是必需的。Calldata 是一个不可修改的、非持久的存储函数参数的区域，其行为很像内存。

建议尽可能尝试使用 calldata。它避免了不必要的拷贝，从而降低了天然气价格，并确保数据是不可变的。

值得注意的是，由于 solidity 0.6.9 对`memory`和`calldata`的使用不受函数可见性的限制。

有关`storage`、`memory`和`calldata`之间差异的更多详细信息，请查看本文:

[](/coinmonks/solidity-storage-vs-memory-vs-calldata-8c7e8c38bce) [## 可靠性—存储与内存和呼叫数据

### Solidity 中数据位置之间难以捉摸的比较。

medium.com](/coinmonks/solidity-storage-vs-memory-vs-calldata-8c7e8c38bce) 

## 7.使用内存缓存读取变量

访问冷存储变量(第一次读取)的成本是`2100 gas`和其后的`100 gas`——热存储访问

您可以通过缓存存储变量并读取缓存数据而不是存储数据来节省时间。从存储阵列读取数据时也是如此。

在上面的例子中，我们将`_arr`存储在`SaveGas2 > loop()`的内存中。所有的读取都是从新的`arr`中调用的。

只要存储阵列不是太大，这是一个很好的节省气体的方法。数据必须被复制到内存中，因此大的数组将是低效的。

## 8.有效的变量管理

如果你打算重用一个像`solution`这样的变量，并且你知道它不会继承前一个循环的值，你可以通过在循环之外定义变量来节省时间——比较`SaveGas`和`SaveGas2`中的循环。

此外，如果你知道没有溢出的风险，你可以通过在`unchecked`块中嵌套`solution`计算来节省更多的汽油。

## 9.第一次写入时节省电量

将存储值从零值改变为非零值将花费`20000 gas`，并且所有非零的第 n 次写入将花费`5000 gas`。我们可以通过为`_data`定义一个非零值来为第一个调用该函数的用户节省一些时间。

## 10.调用结构数据

EVM 按顺序存储数据，试图将所有内容打包到 256 位槽(32 字节)中。在上面的例子中，结构数据被安排成`x`和`y`将占用一个 256 位的内存槽，而`z`将占用它自己的槽——非常整洁。

在读取我们的值时，我们可以通过最小化我们必须读取和/或缓存的内存槽的数量来节省能源。

`SaveGas > getUserZ()`将相关的`MyData`结构存储在内存中——复制两个内存插槽，然后读取相关数据。

`SaveGas2 > getUserZ()`正在从包含`z`的单个内存插槽中读取数据。

# 去哪里找我

我通常可以在酷猫不和谐频道
[找到 https://discord.gg/WhBAAHnSz4](https://t.co/PjlbnmaW3a?amp=1)

或者在推特上
https://twitter.com/xtremetom

[](https://twitter.com/coolcatsnft) [## JavaScript 不可用。

### 编辑描述

twitter.com](https://twitter.com/coolcatsnft) 

> 交易新手？试试[加密交易机器人](/coinmonks/crypto-trading-bot-c2ffce8acb2a)或[复制交易](/coinmonks/top-10-crypto-copy-trading-platforms-for-beginners-d0c37c7d698c)