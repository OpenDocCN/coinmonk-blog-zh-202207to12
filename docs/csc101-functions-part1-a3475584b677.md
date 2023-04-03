# CSC 101-功能第 1 部分

> 原文：<https://medium.com/coinmonks/csc101-functions-part1-a3475584b677?source=collection_archive---------30----------------------->

函数是执行任务的代码块。它可以被多次调用和重用。你可以把信息传递给一个函数，它也可以把信息发送回来。许多编程语言都有内置函数，您可以在它们的库中访问这些函数，但是您也可以创建自己的函数。

![](img/78db353f85d00a7149d8bc2b6bc3c9de.png)

## 功能

功能是一个代码单元，通常由其在更大的代码结构中的角色来定义。具体来说，函数包含一个处理各种输入(其中许多是变量)的代码单元，并根据输入产生具体的结果，包括变量值的变化或实际操作。

函数可以读取或写入契约的状态。

函数按需代码，以执行区块链合同中的代码。

该功能包括两个部分

*   声明函数
*   调用函数

使用 **Function** 关键字可以在 solidity 中定义一个函数。

**格式**:

```
function functionname() <visibility-specifier> <modifier> returns (type) {
//state
}
```

*   `function`是 solidity 中的一个关键字
*   `function-name`:solidity 中的函数名和有效标识符
*   `visibility`:函数可见性调用方如何看到函数
*   `modifier`:声明改变合同数据行为的函数
*   `returns`:返回给调用者的是什么类型的数据

**示例:**

```
function Sum() public view returns(uint){
      uint a = 1; // local variable
      uint b = 2;
      uint c = a + b;
      return c;
   }
```

## 函数可见性说明符

函数的可见性指定调用方如何从内部或外部看到函数。

以下是有效的可见性:

*   `public` : -公共函数在`internal`和`external`契约中可见。-可以在协定内部或协定外部调用-默认为 public -它对 public 可见，因此需要考虑安全性。
*   `private` : -私有函数在同一个契约中可见-可以在一个契约中调用，但不能从外部契约中调用
*   `internal`:——类似于私有函数，对契约层次结构可见——可以在契约和从它继承的契约中调用。
*   `external`:
*   外部函数对公共函数是可见的。
*   可以叫外约`only`。

## 状态可变性

*   **视图**:用`view`声明的函数只能读取状态，不能修改状态。
*   **纯**:用`pure`声明的函数既不能读取也不能修改状态。
*   **应付款**:用`payable`声明的函数可以接受发送给合同的以太，如果没有指定，函数会自动拒绝所有发送给它的以太。

## 功能参数

一个函数可以接受多个用逗号分隔的参数。这些参数可以在函数内部捕获，并且可以对这些参数进行任何操作

## 返回语句

一个实性函数可以有一个可选的**返回**语句。如果你想从一个函数返回一个值，这是必须的。

```
function Sum() public view returns(uint){
      uint a = 100; // local variable
      uint b = (a(a+1))/2;
      return b;
   }
```

> 交易新手？尝试[加密交易机器人](/coinmonks/crypto-trading-bot-c2ffce8acb2a)或[复制交易](/coinmonks/top-10-crypto-copy-trading-platforms-for-beginners-d0c37c7d698c)