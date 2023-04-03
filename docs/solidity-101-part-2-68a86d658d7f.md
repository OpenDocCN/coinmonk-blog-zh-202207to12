# 坚实度-101(第二部分)

> 原文：<https://medium.com/coinmonks/solidity-101-part-2-68a86d658d7f?source=collection_archive---------27----------------------->

![](img/fd07cc58a0cdd829804dd1249d611a85.png)

你可以在我的上一篇文章中读到关于可靠性的基础知识👇

[坚固性-101(第一部分)](https://taniskannpurna.hashnode.dev/solidity-101-part-1)

# 我们开始吧

💜**变量**

*   我们有不同类型的值存储在程序中，比如它可以是整数，字符串，字符，布尔值等。
*   有整数或无符号整数的变量，我们可以在变量数据类型后添加 2 的幂，这将限制它为某个值。
*   坚实度的变量有:
*   **整数**
*   数字可以是正数也可以是负数，但不是小数
*   `**int -> This is to tell solidity that we will store integer in this variable.**`
*   `**int8 -> This will store values from -2^8-1 to 2^8-1 i.e. -255 to 255\. int16 -> This will store values from -2^16-1 to 2^16-1 i.e. -65536 to 65536\. . . .**`
*   `**int256 -> This is the maximum integer value solidity store i.e. -2^256-1 to 2^256-1.**`

👉**记住，只写 int 或者 int256 是一样的。默认情况下，int 被认为是 int256。**

# 无符号整数

*   对于只有正数而没有负数或小数的数字
*   `**uint -> This is to tell solidity that we will store unsigned integer in this variable.**`
*   `**uint8 -> This will store values from 0 to 2^8-1 i.e. 0 to 255.**`
*   `**uint16 -> This will store values from 0 to 2^16-1 i.e. 0 to 65536\. . . .**`
*   `**uint256 -> This is the maximum integer value solidity store i.e. 0 to 2^256-1.**`

👉**记住，只写 uint 或者 uint256 是一样的。默认情况下，int 被认为是 uint256。**

**👉这只限于整数和无符号整数。**

💜**定义变量**

*   我们将实度变量定义为这种模式。

```
**<DATA_TYPE> <ACCESS IDENTIFIER> <VARIABLE NAME>****int8 private fav_num = -8;
uint16 public fav_num = 8;
bool internal is_fav_num = true;
string external word = "Hello";
address private contractAddress = 0x70997970C.......;**
```

💜**功能的特殊访问标识符**

*   这些访问标识符只是告诉 EVM 函数将如何与存储交互。

```
**PURE -> This means that it will not even access the storage.****VIEW-> This means that it will only access the storage but wont change anything.**
```

💜**例子**

```
**address private i_owner;
uint256 public minimumUsd;****function getOwner() public view returns (address) {
        return i_owner;
}****function getAddressToAmountFunded(address funder) public view returns (uint256) {
        return s_addressToAmountFunded[funder];
}**
```

💜摘要

*   这样做是因为我们希望消除不必要的存储使用。
*   因此，如果我们知道值会很小，那么我们可以使用 uint8，uint 16…等等。
*   这样做是为了控制汽油价格。
*   根据不同的任务，EVM 要求支付一定数量的天然气。所以为了控制它，我们使用这些。

👉记得使用纯或查看->特殊访问标识符，因为这些表示他们告诉 EVM，他们有非常有限的功能和气体费用将被视为。

仅此而已。

在下一篇文章中，我们将研究我们在编写智能合同时基本使用的不同存储和数据结构。

> ***你好，我是塔尼斯克·安普尔纳***

```
I post about🚀web3, Blockchain, Ethereum🐦Smart Contract, Solidity🎉JavaScript, ReactJS, NodeJSFollow and like for more such posts. !!✌️!!
```

> 交易新手？试试[密码交易机器人](/coinmonks/crypto-trading-bot-c2ffce8acb2a)或[复制交易](/coinmonks/top-10-crypto-copy-trading-platforms-for-beginners-d0c37c7d698c)