# CSC 101-纯，查看并后退

> 原文：<https://medium.com/coinmonks/csc101-pure-view-and-fall-back-5d90b6a55e26?source=collection_archive---------49----------------------->

过去我们谈论函数。在本教程中，我们想谈谈实性特殊函数。留在我身边；)

![](img/78db353f85d00a7149d8bc2b6bc3c9de.png)

## 纯函数

不读取或修改状态变量的函数称为纯函数。它只能使用在函数中声明的局部变量和传递给函数的参数来计算或返回值。

**例子**:

```
pragma solidity ^0.8.10;

contract MyContract{
   function getResult() public **pure** returns(uint product, uint sum){
      uint X = 2; 
      uint Y = 4;
      product = X * Y;
      sum = X + Y; 
   }
}
```

如果 pure 函数正在执行以下任一操作，编译器会将它们视为读取状态变量，并抛出警告:

1.  读取状态变量
2.  访问余额或地址
3.  调用非纯函数
4.  访问全局变量、消息或块
5.  在内联程序集中使用某些操作码

如果 pure 函数正在执行以下任何操作，编译器会将它们视为修改状态变量，并抛出警告。

1.  修改或重写状态变量
2.  创建新合同
3.  调用非纯函数或视图函数
4.  发射事件
5.  在内联程序集中使用某些操作码
6.  使用`selfdestruct`
7.  使用低级函数调用
8.  随函数调用一起发送以太网

## 查看功能

视图函数确保它们不会修改状态。一个函数可以被声明为**视图**。以下语句如果出现在函数中，将被视为修改状态，在这种情况下编译器将抛出警告。视图函数是只读函数，这确保了状态变量在被调用后不能被修改。

如果修改状态变量、发出事件、创建其他契约、使用自毁方法、通过调用转移硬币、调用非视图或纯函数、使用低级调用等语句出现在视图函数中，则编译器会在这种情况下抛出警告。默认情况下，get 方法是 view 函数。

*   修改状态变量。
*   发射事件。
*   创建其他合同。
*   使用自毁。
*   通过电话发送以太网。
*   调用任何未标记为 view 或 pure 的函数。
*   使用低级调用。
*   使用包含某些操作码的内联程序集。

**举例:**

```
pragma solidity ^0.8.10;

contract MyContract{
   function getResult() public **view** returns(uint product, uint sum){
      uint X = 1; // local variable
      uint Y = 2;
      product = X * Y;
      sum = X + Y; 
   }
}
```

## 后退功能

一个合同最多可以有一个后备功能。它是使用 fallback () external [payable]声明的。此函数不能有参数。它可能不返回任何内容，并且必须具有外部可见性。如果没有一个相反的函数与给定的函数签名相匹配，它将在调用契约时执行。

后退功能具有以下特点:

*   当在协定上调用一个不存在的函数时，就会调用它。
*   要求将其标记为外部。
*   它没有名字。
*   它没有争论
*   它不能返回任何东西。
*   可以为每个合同定义一个。
*   如果未标记为应付，如果合同收到无数据的纯以太网，将引发异常。

> 交易新手？试试[加密交易机器人](/coinmonks/crypto-trading-bot-c2ffce8acb2a)或者[复制交易](/coinmonks/top-10-crypto-copy-trading-platforms-for-beginners-d0c37c7d698c)