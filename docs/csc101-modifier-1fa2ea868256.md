# CSC 101-修改器

> 原文：<https://medium.com/coinmonks/csc101-modifier-1fa2ea868256?source=collection_archive---------58----------------------->

函数修饰符用于修改函数的行为。例如，向函数添加先决条件。

![](img/78db353f85d00a7149d8bc2b6bc3c9de.png)

## 修饰语

修饰符是可以在函数调用之前和/或之后运行的代码。坚固性文件将修改量定义如下:

> *“函数修饰符是编译时源代码的上卷。*
> 
> 它可以用来以声明的方式修改函数的语义。”

修饰符可用于:

*   限制访问
*   验证输入
*   防范重入黑客攻击

假设我们在区块链上部署了一个智能合约，每个人都可以与我们的智能合约进行交互，也可以访问我们的合约和功能。这样，每个人都可以调用所有的功能并签署交易。这是我们使用修饰语的地方。使用修饰符，我们可以在执行函数之前检查条件。

**语法:**

```
modifier Modifier_Name{
    // modifier code goes here...
}
```

**示例**:

```
modifier onlyOwner {
      require(msg.sender == owner);
      _;
   }
```

在上面的代码中，如果交易发送方是合同所有者，将执行该功能。

让我们通过其他例子更深入地了解一下。

```
pragma solidity ^0.8.10;

contract Bank {
uint private _value;
adresse private _owner;

  function Bank(uint amount){

  value = amount;
  owner = msg.sender;
  }

  modifier _ownerFunction {

  require(owner == msg.sender);
  _;
  }
}
```

假设我们有一个虚拟银行，在上面的例子中，只有合同所有者可以执行这个功能。现在我们用 **require** 指定需求。**所有者**和创建者的消息必须相同。只有创建者的地址与发送者的地址进行比较。如果该语句**为真**，则代码被处理。如果该语句为**假**，则什么都不会发生。然后我们设置“_”，这样函数总是先执行。如果不是智能合约创建者的人想要更改某些内容，这将显示一个错误。现在我们只需要给每个函数添加 **ownerFunction** ，我们不希望除了创建者之外的任何人能够改变任何东西。

让我们回到以前的例子:

```
modifier onlyOwner {
      require(msg.sender == owner);
      _;
   }
```

现在我们想知道修改器是如何工作的。

符号`_;`被称为合并通配符。它将功能代码与放置`_;`的修改代码合并。写`_;`符号的地方将决定这个函数是必须在修改代码之前、之间还是之后执行。

## 将参数传递给修饰符

修饰符也可以接受参数。像函数一样，你只需要在修饰符名前面的括号中传递变量 type + name。

**语法**:

你可以写一个带参数或不带参数的修饰语。如果修饰符没有自变量，可以省略括号`()`。

```
modifier MyModifier(uint a) {
    // ...
}modifier MyModifier() {
    // ...
}
```

**示例**:

```
modifier Fee (uint _fee) {
    if (msg.value >= _fee) {
        _;
    }
}
```

> 交易新手？试试[加密交易机器人](/coinmonks/crypto-trading-bot-c2ffce8acb2a)或者[复制交易](/coinmonks/top-10-crypto-copy-trading-platforms-for-beginners-d0c37c7d698c)