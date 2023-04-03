# 什么是实度变量？

> 原文：<https://medium.com/coinmonks/what-are-solidity-variables-eecee946d4ca?source=collection_archive---------20----------------------->

在我们的 YouTube 上观看视频的同时，享受这个流的资源！

YouTube:[https://youtu.be/537AZqx_5Ec](https://youtu.be/537AZqx_5Ec)

不和:【https://discord.gg/J73qhkj7kr】T2

推特:【https://twitter.com/CryptoverseDAO】

linktree:[https://linktr.ee/cryptoversedao](https://linktr.ee/cryptoversedao)

坚固性中的变量类型:

毫无疑问，变量是任何编程语言编写程序的重要要求。因此，你也需要在 Solidity 教程中使用变量来理解它们如何帮助存储不同的信息。重要的是要注意到变量只是用来存储值的保留内存位置。因此，通过创建变量，您可以在内存中保留一定的空间。

坚实度所需要的变量种类繁多，这可能会让人产生疑问，比如“坚实度好学吗？”虽然没有混乱。用户可以存储与不同数据类型相关的信息，如字符、浮点、双浮点、宽字符、布尔、整数等。操作系统根据变量的数据类型确保内存分配和实体选择以存储在保留的内存中。

值类型

任何关于 solidity 区块链编程语言的教程的详细概述都会帮助你找到不同的数据类型。可靠性支持各种强大的内置和用户定义的数据类型。这里是你可以在 solidity 中找到的重要数据类型，

布尔型
有符号和无符号整数
有符号整数从 8 位到 256 位
无符号整数从 8 位到 256 位
有符号和无符号定点数大小不等
有符号定点数，用 fixedMxN 表示，M 表示按类型计的位数，N 表示小数点。m 必须能被 8 整除，范围从 8 到 256。另一方面，N 可以在 0 到 80 之间变化。fixed 的值类似于 fixed128x18。
无符号定点数，用 ufixedMxN 表示，M 和 N 表示同上。ufixed 的值也与 ufixed128x18 相似。

地址

solidity 教程中的‘address’元素指的是 20 字节的值，代表以太坊地址的大小。一个地址基本上可以通过使用“. balance”方法来帮助获得余额。该方法可以帮助通过将余额转移到其他地址。“转移”方法。这里有一个在 Solidity 中使用地址的例子。

地址 x = 0x212

address myAddress = this

if(x . balance< 10 &&myAddress.balance>= 10)x . transfer(10)；

理解固体中的变量

对变量的感知也是学习扎实的关键要求之一。您可以在 Solidity 中找到对三种变量类型的支持，如状态变量、局部变量和全局变量。状态变量的值永久存储在契约存储中。局部变量是那些在函数执行过程中呈现其值的变量。

另一方面，全局变量是存在于全局名称空间中的特殊变量，有助于获得关于区块链的信息。验证作为静态类型语言，Solidity 需要在声明时指定状态或局部变量类型，这一点很重要。所有声明的变量都根据其类型与默认值相关联，没有任何“空”或“未定义”的概念让我们更多地思考一下固体中变量的类型。

态变数

您可以找到状态变量，其中的值永久存储在契约存储中。状态变量的例子在下面的例子中非常明显，

^0.5.0 实用主义；

合同可靠性测试{

uintstoredData//状态变量

构造函数()public {

storedData = 10//使用状态变量

}

}

局部变量

局部变量的值只能在定义它的函数中访问。还需要注意的是，函数参数本质上是相关函数的局部参数，如下例所示。

^0.5.0 实用主义；

合同可靠性测试{

uintstoredData//状态变量

构造函数()public {

storedData = 10

}

函数 getResult()公共视图返回(uint){

uint a = 1；//局部变量

uint b = 2；

uint 结果= a+b；

返回结果；//访问局部变量

}

}
]
全局变量

当涉及到坚固性教程时，全局变量是非常独特的。它们非常适合于获取有关区块链和相关交易属性的信息。这里是一个整体变量的轮廓，以及它们的功能。

block hash(uintblockNumber)returns(bytes 32):表示相关块的哈希，仅适用于 256 个最新的块，而忽略当前块。
block.coinbase(应付地址):显示当前 block miner 的地址。
block . difference(uint):指出一个街区的难度。
block.gaslimit (uint):显示当前块的 gaslimit。
block.number (uint):显示当前的块号。
block.timestamp (uint):显示当前块的时间戳，以 unix 纪元以来经过的秒数表示。
msg.sig (bytes4):可以找出函数标识符或 calldata 的前四个字节。msg.data(字节调用数据):显示完整的调用数据。
now (uint):可以找到当前块时间戳的信息。

变量名规则

你也应该在实体教程中寻找命名变量的理想方法。在 Solidity 编程语言中命名变量时，完全遵守以下规则是很重要的。

在 Solidity 中命名变量的首要条件是不能以数字开头。因此，在 Solidity 中，不能使用 0 到 9 之间的任何数字作为变量名的开头。变量名必须以字母或下划线字符开头。因此，023Freedom 不是有效的变量名，而 Freedom023 或 _023Freedom 是有效的选择。命名变量的可靠性最佳实践也指出了可靠性变量名的大小写敏感性。例如，自由和自由被认为是两个不同的变量。
创建合适变量名的下一个最重要的实践是避免使用保留关键字。保留关键字，如布尔或中断变量名，不适用于实体变量名。

理解变量使用的最后一个方面是变量范围。变量范围在实体教程中很重要，因为它对于局部变量和状态变量是不同的。局部变量只限于定义它们的函数。

另一方面，solidity 教程中的状态变量指的是公共、私有和内部范围。可以在内部或通过消息访问公共状态变量。私有状态变量仅对当前协定的内部访问开放。内部状态变量只允许通过当前协定进行内部访问。

![](img/98bbe5e7694cc81ce92d7b48868068ea.png)

> 交易新手？试试[密码交易机器人](/coinmonks/crypto-trading-bot-c2ffce8acb2a)或者[复制交易](/coinmonks/top-10-crypto-copy-trading-platforms-for-beginners-d0c37c7d698c)