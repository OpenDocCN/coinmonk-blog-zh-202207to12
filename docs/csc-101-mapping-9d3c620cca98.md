# CSC 101-映射

> 原文：<https://medium.com/coinmonks/csc-101-mapping-9d3c620cca98?source=collection_archive---------37----------------------->

嘿嘿嘿！
Zeroxlive 来了 **Coinex 智能链**

Solidity 提供了一种数据结构映射，以哈希表格式存储键和值。映射是作为数组和结构的引用类型。

![](img/78db353f85d00a7149d8bc2b6bc3c9de.png)

## 绘图

Solidity 中的映射就像任何其他语言中的哈希表或字典一样。它们用于以键-值对的形式存储数据，键可以是任何内置数据类型，但不允许引用类型，而值可以是任何类型。映射主要用于将唯一的以太坊地址与相关联的值类型相关联。

映射对于关联很有用，比如将一个唯一的以太坊地址与一个特定的余额相关联。

**格式**:

```
mapping(keyType=> valueType) public mappingVariable;
```

*   **_ key type**—可以是任何内置类型加上字节和字符串。不允许引用类型或复杂对象。
*   **_ value type**—可以是任何类型。

## 定义映射

为了定义一个映射，我们和其他变量类型做同样的事情。

**示例**:

```
mapping (address => Users) result;address[] public Users_result;
```

## 向映射添加值

让我们试着给映射添加一些值。要给映射添加值，我们使用 **push。**注意以下例子:

**示例**:

```
struct User { string name; uint ID; uint Gold;}mapping (address => users) result;address[] public users_result;function adding_values() public {var user= result[0x00000000000.....];Users.name = "John";Users.ID= 2;Users.Gold = 1000;Users_result.push(0x0000....) -1;}
```

**注意**:映射只能有**存储类型**，一般用于状态变量。

映射可以标记为公共的。Solidity 自动为它创建 getter。

## 从映射中获取值

为了检索这些值，我们必须创建一个函数来返回添加到映射中的值。

```
function get_result() view public returns (address[]) {return users_result;}
```

## 计数映射

为了得到映射的计数，我们需要得到它的长度。在这种情况下，我们使用**长度**

```
function Count_Users() view public returns (uint) {return users_result.length;}
```

## 限制

*   您不能不将变量定义为映射的 _ValueType。
*   您不能直接迭代映射
*   不可能像在 Java 中那样检索值或键的列表。原因仍然是一样的:所有的变量已经默认初始化。所以内部是不可能的。
*   映射不能作为智能协定中公共和外部函数内的参数传递。
*   映射不能用作函数的返回值

特别感谢**让·科维尔**

> 交易新手？尝试[加密交易机器人](/coinmonks/crypto-trading-bot-c2ffce8acb2a)或[复制交易](/coinmonks/top-10-crypto-copy-trading-platforms-for-beginners-d0c37c7d698c)