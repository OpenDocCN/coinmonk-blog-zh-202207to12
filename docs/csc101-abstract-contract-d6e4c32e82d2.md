# CSC 101-抽象合同

> 原文：<https://medium.com/coinmonks/csc101-abstract-contract-d6e4c32e82d2?source=collection_archive---------29----------------------->

抽象契约是一种具有抽象设计的契约，实现契约提供了抽象函数的实现。在前面的教程中，我们提到了抽象契约，现在让我们更深入一些。

![](img/78db353f85d00a7149d8bc2b6bc3c9de.png)

## 抽象合同

抽象契约是至少有一个函数没有实现的契约，或者是在没有为所有的基契约构造函数提供参数的情况下。

实体中的契约类似于面向对象语言中的类

抽象合同是用**抽象**关键字创建的合同。

**语法:**

```
abstract contract Contract_Name{
    function Function_name() public virtual returns (bytes32);
}
```

**举例**:

```
contract Calculator {
   function getResult() public virtual returns(uint);
}
contract Cal is Calculator {
   function getResult() public view returns(uint) {
      uint X = 1;
      uint Y = 2;
      uint result = a + b;
      return result;
   }
}
```

**注意**:一旦创建了抽象契约，你需要使用 is 关键字来扩展它

抽象契约是一种不能自行部署的契约。抽象契约必须由另一个契约继承。

当您想要将模式灌输到您正在构建的 dapp 中时，抽象契约尤其有用。它们让你能够实现契约的大部分内容，但是你仍然可以在其中包含抽象函数，以便定义和自文档化你的 dapp 的框架。其他契约可以从抽象契约继承。

抽象契约有点类似于接口，但它们之间存在一些差异。抽象契约可以定义函数签名，也可以实现它的一些功能。

## 抽象契约 VS 接口

接口类似于抽象契约，因为它必须被另一个像抽象契约一样的契约继承。接口函数的可见性必须标记为**外部**。它不能有构造函数，也不能声明状态变量。

*   接口不能有构造函数，而抽象协定可以实现构造函数。
*   接口不能定义状态变量，但是抽象契约可以。
*   继承契约必须实现接口中定义的所有函数，而在抽象契约中，继承契约必须实现抽象契约的至少一个函数。
*   抽象协定可以从另一个协定或抽象协定继承，而接口不能从协定或另一个接口继承。

## 什么时候应该使用抽象契约？

当您想要将模式灌输到您正在构建的 dapp 中时，抽象契约尤其有用

## 我们什么时候应该使用界面？

当您构建大型复杂的 dapps 时，可扩展性是关键。当涉及到在全面实现之前设计更大规模的 dapps 时，接口是最有用的。

> 交易新手？尝试[加密交易机器人](/coinmonks/crypto-trading-bot-c2ffce8acb2a)或[复制交易](/coinmonks/top-10-crypto-copy-trading-platforms-for-beginners-d0c37c7d698c)