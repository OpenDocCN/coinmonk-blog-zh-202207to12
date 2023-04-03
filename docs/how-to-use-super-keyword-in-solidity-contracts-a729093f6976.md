# 如何在可靠性合同中使用“Super”关键字？

> 原文：<https://medium.com/coinmonks/how-to-use-super-keyword-in-solidity-contracts-a729093f6976?source=collection_archive---------13----------------------->

在 Solidity 中，`super`关键字可以在继承的契约中用来引用它所继承的契约。当您希望从继承协定中的函数内调用基础协定中的函数时，这很有用。

这里有一个例子:

```
pragma solidity ^0.5.0;

// Original contract with the function that we want to call
contract OriginalContract {
    function sayHello() public pure returns (string memory) {
        return "Hello from OriginalContract";
    }
}

// New contract that will override the sayHello function
contract NewContract is OriginalContract {
    // Override the sayHello function
    function sayHello() public pure returns (string memory) {
        // Call the sayHello function in the OriginalContract using the super keyword
        string memory helloMessage = super.sayHello();

        // Return the message from the OriginalContract along with a message from the NewContract
        return helloMessage + " and hello from NewContract";
    }
}
```

在这个例子中，`NewContract`合同中的`sayHello`函数覆盖了`OriginalContract`合同中的`sayHello`函数。然而，`NewContract`契约中的`sayHello`函数也使用`super`关键字调用`OriginalContract`契约中的`sayHello`函数。这使得`NewContract`契约既可以覆盖`sayHello`函数，也可以使用`sayHello`函数的原始实现。

> 交易新手？在[最佳加密交易](/coinmonks/crypto-exchange-dd2f9d6f3769)上尝试[加密交易机器人](/coinmonks/crypto-trading-bot-c2ffce8acb2a)或[副本交易](/coinmonks/top-10-crypto-copy-trading-platforms-for-beginners-d0c37c7d698c)

当在`NewContract`契约的实例上调用`sayHello`函数时，它将返回字符串“Hello from original contract and Hello from new contract”。这演示了如何使用`super`关键字来调用基础契约中的函数，同时仍然允许在继承的契约中覆盖该函数。