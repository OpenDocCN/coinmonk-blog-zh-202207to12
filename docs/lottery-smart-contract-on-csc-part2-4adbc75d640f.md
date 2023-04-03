# CSC 上的彩票智能合约-第 2 部分

> 原文：<https://medium.com/coinmonks/lottery-smart-contract-on-csc-part2-4adbc75d640f?source=collection_archive---------33----------------------->

嘿嘿嘿！

ZeroXlive 是为 Coinex 智能链学院而来😃

在本教程中，我们想写彩票智能合同使用 Coinex 智能链上的可靠性。留在我身边；)

![](img/ad5f5886c590e06e022a359764163a8c.png)

**参赛选手**

这里，任何调用该函数并随该调用一起发送费用金额的用户都注册了彩票。我们首先检查发送的金额(msg.value)是否正好等于费用，然后将函数调用方(msg.sender)的地址添加到参赛选手数组中。此函数具有 payable 关键字，因为智能合同在此函数中接收资金。

```
function enter() public payable  {         require(msg.value == fees, "Enter fees amount only as value");         contestants.push(payable(msg.sender));              }
```

**查看奖金**

get balance 就像 getFees 函数一样获取智能合约中的资金量。this 关键字在这里是要注意的，它总是指正在使用的合同。我们把它转换成地址类型并返回余额。

```
function getBalance() public view returns (uint) {
        return address(this).balance;
    }
```

**宣布结果**

这样做的逻辑是在参赛者数组的起始和结束索引中生成一个随机范围，并宣布他为获胜者。然而，有一个问题。solidity 中没有真正的随机数生成器。区块链作为一个状态完整的机器，并不支持真正的随机数。因此，我们必须使用伪随机数生成算法。这个问题可以通过使用**神谕** *来解决。*在该实施中，我们将使用伪随机数生成算法。我们散列块生成时间戳和创建者地址，然后用数组长度对其取模。Solidity 为我们提供了 keecak256 哈希算法，我们用它来进行哈希运算。保持这个函数私有或公开并没有太大的区别，但是因为我们这样做是为了学习，所以它是公开的。随机数生成后，我们将合同中的金额转移到该地址，然后我们重置数组，因为抽奖已经结束，新的抽奖可以再次开始。

完整的代码应该如下所示:

```
//SPDX-License-Identifier: MIT                              pragma solidity ^0.8.0;                                                           contract Lottery{                                 address public creator;                                 address payable[] public contestants;                                 uint fees= 1 ether;                                                                  constructor(){                                     creator = msg.sender;                                 }                                                               function enter() public payable{                                     require(msg.value == fees, "Enter fees amount only as value");                                     contestants.push(payable(msg.sender));                                                                      }                                                               function getRandomNumber() public view returns(uint){                                     return uint(keccak256(abi.encodePacked(creator, block.timestamp)));                                 }                                                               function selectWinner() public onlyOwner{                                     uint winningIndex = getRandomNumber() % contestants.length;                                     contestants[winningIndex].transfer(address(this).balance);                                     contestants=new address payable[](0);                                 }                                 function getBalance() public view returns (uint) {                                     return address(this).balance;                                 }                                 function setFees(uint _fees) public onlyOwner{                                     fees=_fees;                                 }                                 function getFees() public view returns (uint){                                     return fees;                                 }                                 modifier onlyOwner() {                                   require(msg.sender == creator);                                   _;                                 }                             }
```

生:[https://pastebin.pl/view/raw/b0631418](https://pastebin.pl/view/raw/b0631418)

好了，我们已经准备好了彩票智能合约。现在你需要做的就是尝试一下，在 remix 上自己测试一下。IDE 使用起来非常直观，通过体验和测试这些特性，您会学到更多。一些常见的错误是:-

1.  输入彩票时，请确保设置的值正好等于费用，并确保单位设置为以太。默认情况下，它被设置为魏。
2.  确保使用创建者地址调用 onlyOwner 函数。那会给你一个错误。
3.  当输入费用这样的参数时，默认值再次设置为 Wei，您必须添加所需数量的零，记住(

> 交易新手？试试[加密交易机器人](/coinmonks/crypto-trading-bot-c2ffce8acb2a)或者[复制交易](/coinmonks/top-10-crypto-copy-trading-platforms-for-beginners-d0c37c7d698c)
> 
> 加入 Coinmonks [电报频道](https://t.me/coincodecap)和 [Youtube 频道](https://www.youtube.com/c/coinmonks/videos)获取每日[加密新闻](http://coincodecap.com/)

## 另外，阅读

*   [复制交易](/coinmonks/top-10-crypto-copy-trading-platforms-for-beginners-d0c37c7d698c) | [加密税务软件](/coinmonks/crypto-tax-software-ed4b4810e338)
*   [网格交易](https://coincodecap.com/grid-trading) | [加密硬件钱包](/coinmonks/the-best-cryptocurrency-hardware-wallets-of-2020-e28b1c124069)
*   [密码电报信号](/coinmonks/top-3-telegram-channels-for-crypto-traders-in-2021-8385f4411ff4) | [密码交易机器人](/coinmonks/crypto-trading-bot-c2ffce8acb2a)
*   [最佳加密交易所](/coinmonks/crypto-exchange-dd2f9d6f3769) | [印度最佳加密交易所](/coinmonks/bitcoin-exchange-in-india-7f1fe79715c9)
*   开发人员的最佳加密 API
*   最佳[密码借贷平台](/coinmonks/top-5-crypto-lending-platforms-in-2020-that-you-need-to-know-a1b675cec3fa)
*   [免费加密信号](/coinmonks/free-crypto-signals-48b25e61a8da) | [加密交易机器人](/coinmonks/crypto-trading-bot-c2ffce8acb2a)
*   [杠杆代币的终极指南](/coinmonks/leveraged-token-3f5257808b22)