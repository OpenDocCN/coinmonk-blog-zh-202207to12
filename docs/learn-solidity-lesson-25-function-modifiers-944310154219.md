# 学习第 25 课固体。功能修饰符。

> 原文：<https://medium.com/coinmonks/learn-solidity-lesson-25-function-modifiers-944310154219?source=collection_archive---------4----------------------->

![](img/5ea7d0aae91752ad4f79bebc6061f1a9.png)

修饰符用于以声明的方式改变函数的行为。它们允许我们在不同的函数中重用代码块，并使它们更具可读性。

我们来看一个经典的例子。在契约中，希望某个函数只由一个特定的地址调用是很常见的，这个地址通常被命名为*所有者*。使用修饰符，我们可以如下声明这样一个函数。

```
function foo() public onlyOwner {...}
```

修饰符`onlyOwner`的使用在函数声明中明确了它只能被所有者调用

为了声明修饰符，我们使用关键字 **mofidier** ，如下例所示。

```
modifier onlyOwner() {
   require(msg.sender == owner);
   _;
}
```

通过上面的声明，函数体`foo`将只在语句`require(msg.sender == owner)`之后执行。更准确地说，函数体将包含在修饰符中标识符`_`的位置。

在函数`foo`中使用修饰符`onlyOwner`相当于如下声明函数。

```
function foo() public {
   require(msg.sender==owner);
   ... body of the function ...
}
```

在同一个函数中可以使用几个修饰符。例如，如果我们声明修饰符`onlyOwner`和`notPaused`，下面的函数是完全合法的。

```
function bar() public onlyOwner notPause {}
```

让我们再看一个使用修饰语的例子。

# 无入口

使用修饰符的另一个例子是防止称为*重入*的攻击。当一个合同调用另一个合同，而另一个合同又以恶意的方式重新进入原始合同时，就会发生这种情况。

有几种方法可以防止这种类型的攻击，使用修饰符是其中之一。让我们看看下面的代码。

```
bool isLocked;modifier noReentrancy() 
   require(isLocked == false);
   isLocked = true;
   _;
   isLocked = false;
}
```

在上面的代码中，函数体将被夹在语句`isLocked = true` 和`isLocked = false`之间。它所做的是在执行过程中锁定函数调用，以避免重入。

您可以在修饰符的任何地方包含标识符`_`。甚至可以在修饰符中多次包含标识符`_`，每个`_`都会被函数体替换。下面的修饰符复制了函数体。

```
modifier duplicate() {
   _;
   _;
}
```

例如，让我们看看下面的函数，它使用了修饰符`duplicate`。

```
function count() public duplicate {
   someCounter++;
}
```

上述函数将增加计数器两次，因为语句`someCounter++`将被复制。

# 修改器中的参数

修饰符可以有参数和访问状态变量。让我们看一个使用带参数的修饰符的例子。

让我们看一个使用带参数的修饰符的例子。

```
uint8 status = 0;modifier inStatus(uint8 _status) {
   require(status==_status);
   _;
}function statusZero() public inStatus(0) {}
function statusOne() public inStatus(1) {}
```

上面的例子使用枚举器会更加易读，但是让我们保持简单。修饰符`inStatus`有一个参数`_status`，它必须等于状态变量`status`。

因此，功能`statusZero`只能在状态等于`0`时调用，而功能`statusOne`只能在状态等于`1`时调用。使用参数可以自定义修改器，如下例所示。

**感谢阅读！**

欢迎对本文提出意见和建议。

欢迎任何投稿。[www.buymeacoffee.com/jpmorais](http://www.buymeacoffee.com/jpmorais)。

> 交易新手？尝试[加密交易机器人](/coinmonks/crypto-trading-bot-c2ffce8acb2a)或[复制交易](/coinmonks/top-10-crypto-copy-trading-platforms-for-beginners-d0c37c7d698c)