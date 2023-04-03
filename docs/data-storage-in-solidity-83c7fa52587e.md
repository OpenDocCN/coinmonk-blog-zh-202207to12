# 数据存储的可靠性

> 原文：<https://medium.com/coinmonks/data-storage-in-solidity-83c7fa52587e?source=collection_archive---------30----------------------->

`storage`和`memory`是 Solidity 中存储数据的两种方式。

`memory`就像 RAM 存储，它的数据是易失的，它在每次调用结束时被销毁，在每次函数调用开始时被创建，类似于当计算机关机时 RAM 被擦除，当计算机启动和运行时数据被添加。

`storage`就像硬盘一样，它的数据是不可变的，数据一旦设置，将在将来的函数调用中持续存在，类似于硬盘，当计算机关机时，其数据不会被擦除/销毁。

**一般来说，除了函数的自变量和显式定义的`memory`变量外，大多数数据类型都是** `**storage**`。

*   **状态变量**和**结构**的局部变量，数组默认总是存储在`storage`中。

```
uint totalCount;address owner;address[] participants;string messageData;struct person {
   string name;
   string age;
}
person[] personDatabase;mapping(uint => person) register;
```

`totalCount`、`owner`、`participants`、`messageData`、`personDatabase`、`register`都是`storage`类型。

*   **函数参数**在`memory`中。

```
string memory messageData;*function* Action1(uint minimumBet, uint maxAmountOfBets, string memory userDescription) {...}
```

`minimumBet`、`maxAmountOfBets`、`userDescription`都是记忆类型

*   每当使用关键字`memory`、**创建数组**的新**实例时，在每次调用**时都会创建该变量的新副本，因此，当数组在每次调用时被重置回原点时，更改新实例的数组值不会反映出来。

> 交易新手？尝试[加密交易机器人](/coinmonks/crypto-trading-bot-c2ffce8acb2a)或[复制交易](/coinmonks/top-10-crypto-copy-trading-platforms-for-beginners-d0c37c7d698c)