# CSC 101-建造商

> 原文：<https://medium.com/coinmonks/csc-101-constructor-451432c236a5?source=collection_archive---------37----------------------->

构造函数是在契约中使用**构造函数**关键字声明的可选函数。它包含更改和初始化状态变量的代码。

![](img/78db353f85d00a7149d8bc2b6bc3c9de.png)

## 构造器

使用不带任何函数名的**构造函数**关键字来定义构造函数，后跟一个访问修饰符。这是一个可选函数，它初始化契约的状态变量。一旦契约被创建并开始在区块链中执行，t 就会被自动调用。构造函数代码是创建代码的一部分，而不是运行时代码的一部分。

**语法:**

```
constructor() <Access Modifier> {          
}
```

**举例**:

```
pragma solidity ^0.8.10;

contract MyContract{
   constructor() public {}
}
```

如果基础契约有带参数的构造函数，每个派生契约都必须传递它们。

**注意**:一个合同只能有一个建造师。

*   构造函数声明是可选的
*   该协定只包含一个构造函数声明
*   不支持重载构造函数
*   如果未定义构造函数，则使用默认构造函数。

## 继承中的构造函数

有两种方法可以调用父协定的构造函数:

1.  **直接初始化:**在下面的例子中，直接初始化方法用于初始化父类的构造函数。

```
uint data;constructor(uint _data) public {data = _data;}
```

**2。间接初始化:**在下面的例子中，使用*Base(string(ABI . encodepacked(_info，_ info))的间接初始化是*完成*到*初始化基类的构造函数。

可以使用以下方式间接初始化基构造函数:

```
pragma solidity ^0.8.10;

contract Base {
   uint data;
   constructor(uint _data) public {
      data = _data;   
   }
}
contract Derived is Base {
   constructor(uint _info) Base(_info * _info) public {}
}
```

**注意**:不允许直接和间接初始化基础契约构造函数。

构造函数在智能契约中非常有用，参数值可以在运行时定义，也可以限制方法调用。Solidity 不支持构造函数重载，一次只允许一个构造函数。

在下面的例子中，我们展示了构造函数的一个用例:

```
string str;address private owner= 0x.......;constructor(string memory string) public {if(msg.sender == owner){str = string;}}
```

> 交易新手？试试[加密交易机器人](/coinmonks/crypto-trading-bot-c2ffce8acb2a)或者[复制交易](/coinmonks/top-10-crypto-copy-trading-platforms-for-beginners-d0c37c7d698c)