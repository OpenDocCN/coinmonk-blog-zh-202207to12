# CSC 上的彩票智能合同-第 1 部分

> 原文：<https://medium.com/coinmonks/lottery-smart-contract-on-csc-part1-138322a5913d?source=collection_archive---------31----------------------->

嘿嘿嘿！

ZeroXlive 是为 Coinex 智能链学院而来😃

在本教程中，我们想写彩票智能合同使用 Coinex 智能链上的可靠性。留在我身边；)

![](img/ad5f5886c590e06e022a359764163a8c.png)

## 智能合同

**智能合同**是一种计算机程序或交易协议，旨在根据合同或协议的条款自动执行、控制或记录与法律相关的事件和行为。智能合同的目标是减少对可信中间人的需求、仲裁成本和欺诈损失，以及减少恶意和意外例外。智能合约通常与加密货币相关联，以太坊推出的智能合约通常被认为是分散金融(DeFi)和 NFT 应用程序的基本构建模块。

自动售货机被认为是最古老的技术，相当于智能合同的实现。Vitalik Buterin 在 2014 年发表的原始以太坊白皮书将比特币协议描述为 Nick Szabo 最初定义的智能合约概念的弱版本，并基于 Solidity 语言提出了更强的版本，即图灵完备。自比特币以来，各种加密货币都支持脚本语言，这些语言允许不受信任方之间签订更高级的智能合同。

本教程的目的是让你熟悉可靠性和智能合同写作的基本概念，并开始你的第一份智能合同。在这篇博客的结尾，你将拥有自己的彩票智能合约。

## 彩票智能合约

我们正在建立一个彩票智能合同，其中有两个主要实体，创作者和参赛者。参赛者支付将存储在智能合同中的报名费，并参加抽奖。创建者可以设置费用，更改费用，并决定何时宣布抽奖结果。当创建者自动结束抽奖时，存储在智能合同中的全部金额将被转移到抽奖的获胜者，该获胜者将从参赛者列表中随机选择。与传统的彩票系统不同，在传统的彩票系统中，创建者持有参赛者的资金，而在基于区块链的彩票系统中，资金保存在智能合同中，只有在选举结束时才能从智能合同中解除。

对于本教程，我们将有一个非常简单的环境，因为我们只对智能契约进行编码和手动测试，而不编写自动化测试。我们将使用一个简单的 web IDE，名为 [Remix](https://ide.coinex.net) 。它是早期开发人员使用最多的工具之一。这是非常友好和简单的初学者。

## 发展

精彩的部分终于来了。每个 solidity smart 合同的第一行都有一个注释，提到了所使用的标准和许可。下一行显示了这个合同使用的 solidity 版本。接下来，我们通过使用关键字 **contract** 后跟合同名称来定义合同。我们所有与此合同相关的代码都将放在这里。

```
//SPDX-License-Identifier: MIT 
pragma solidity ^0.8.10;
contract Lottery{

}
```

**变量和构造函数**

接下来，我们定义将要使用的变量和构造函数。在我们的范围内，我们必须定义的唯一变量是所有者和竞争者的数组。我们还在这里定义了彩票的默认费用，业主可以随时更改这些费用。现在来看构造函数，这段代码只运行一次，也就是部署契约的时候。当我们部署智能合同时，我们希望部署合同的人是彩票的创建者。术语 *msg.sender* 表示支付部署合同费用的地址。

```
address public creator;
address payable[] public contestants;
uint fees= 1 ether;constructor(){
    creator = msg.sender;
}
```

参赛者的地址属于 payable 类型，这意味着智能合约将能够支付这些地址的费用。这是因为任何地址都可以赢得彩票，智能合同应该能够向该地址付款。

**实度修改器**

接下来，我们编写我们将在这个契约中使用的函数。这些函数中的大部分都可以被任何人调用，但是像结束选举或改变费用设置这样的一些函数只能被创建者调用。为了便于编码和不重复要求条件，坚固性有修饰词。它们用于向函数添加先决条件。作为编码实践，我们通常将修饰符放在契约的末尾。

```
modifier onlyOwner() {
      require(msg.sender == creator);
      _;
    }
```

**书写功能**

接下来，我们编写基本函数，如获取费用和设置费用函数。其逻辑非常简单，获取费用函数调用费用变量并返回费用值。由于该函数只查看值，不执行任何事务，因此不收取任何费用，并且我们添加了 view 关键字。对于固定费用，我们改变费用变量的值并更新它。因为只有创造者可以做到这一点，我们在这里添加我们的修改器。

```
function setFees(uint _fees) public onlyOwner  {         fees=_fees;     } function getFees() public view returns (uint)  {         return fees;     }
```

> 交易新手？尝试[加密交易机器人](/coinmonks/crypto-trading-bot-c2ffce8acb2a)或[复制交易](/coinmonks/top-10-crypto-copy-trading-platforms-for-beginners-d0c37c7d698c)
> 
> 加入 Coinmonks [电报频道](https://t.me/coincodecap)和 [Youtube 频道](https://www.youtube.com/c/coinmonks/videos)获取每日[加密新闻](http://coincodecap.com/)

## 另外，阅读

*   [复制交易](/coinmonks/top-10-crypto-copy-trading-platforms-for-beginners-d0c37c7d698c) | [加密税务软件](/coinmonks/crypto-tax-software-ed4b4810e338)
*   [网格交易](https://coincodecap.com/grid-trading) | [加密硬件钱包](/coinmonks/the-best-cryptocurrency-hardware-wallets-of-2020-e28b1c124069)
*   [密码电报信号](/coinmonks/top-3-telegram-channels-for-crypto-traders-in-2021-8385f4411ff4) | [密码交易机器人](/coinmonks/crypto-trading-bot-c2ffce8acb2a)
*   [最佳加密交易所](/coinmonks/crypto-exchange-dd2f9d6f3769) | [印度最佳加密交易所](/coinmonks/bitcoin-exchange-in-india-7f1fe79715c9)
*   [开发人员的最佳加密 API](/coinmonks/best-crypto-apis-for-developers-5efe3a597a9f)
*   最佳[密码借贷平台](/coinmonks/top-5-crypto-lending-platforms-in-2020-that-you-need-to-know-a1b675cec3fa)
*   [免费加密信号](/coinmonks/free-crypto-signals-48b25e61a8da) | [加密交易机器人](/coinmonks/crypto-trading-bot-c2ffce8acb2a)
*   杠杆代币的终极指南