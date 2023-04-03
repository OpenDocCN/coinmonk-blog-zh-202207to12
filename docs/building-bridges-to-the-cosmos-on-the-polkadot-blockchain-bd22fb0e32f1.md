# 在 Polkadot 区块链上搭建通向宇宙的桥梁

> 原文：<https://medium.com/coinmonks/building-bridges-to-the-cosmos-on-the-polkadot-blockchain-bd22fb0e32f1?source=collection_archive---------9----------------------->

~dwulf

![](img/88f93503102818402931fd59ccd842aa.png)

## **来自 Polkadot 的 IBC(区块链间通信)桥**

IBC 是一种互操作协议，用于在任意状态机之间传递任意数据。在我研究这个问题和建造从波尔卡多特到宇宙的桥梁的过程中，我遇到了这个问题。

SCRT 网—[https://scrt . network/blog/secret-plasm-bridge-cosmos-polkadot](https://scrt.network/blog/secret-plasm-bridge-cosmos-polkadot)

这一直是一个热情的项目，以寻找和工作的机制，允许第二层水平的交易，从其他区块链和绕过干扰和 KYC 的不便，往往造成与 CEX，集中交换。这一直是我在区块链最基础的层面上构建的方式。

我是 Sota | Astar (prev-Plasm)的超级粉丝，他们的技术在 IBC 和 SCRT 之间架起了一座桥梁，对此我一点也不惊讶。

## **开发框架 Cosmos vs Polkadot**

Cosmos 和 Polkadot 的设计使得每个链都有其状态转换功能(STF)，并且都在 Web 汇编器(WASM)和以太坊虚拟机(EVM)中提供对智能合约的支持。Polkadot 提供了超前的 WASM 编译器和解释器(WASMi)来执行，而 Cosmos 只在解释器中执行智能合约。这是区别之一。

Cosmos 链可以使用 Cosmos 软件开发工具包(SDK)来开发，它是用 Go 的计算语言编写的。Cosmos SDK 包含大约 10 个模块(例如，打桩、治理等。)可以包含在链的 STF 中。SDK 建立在 Tendermint 的基础上，tender mint 是建立区块链的 BFT 共识引擎。

副链的主要开发框架是用 Rust 编写的 Substrate。基底带有框架，一套大约 40 个托盘(在 Cosmos 中称为“模块”),用于连锁店的 STF。除了简单地使用托盘，Substrate 还增加了一个抽象层，允许开发人员通过添加定制模块和配置链的参数和初始存储值来组成 FRAME 的托盘。

Polkadot 可以支持用任何语言编写的状态转移函数(STF ),只要它编译成它的元协议 WASM。同样，它仍然可以使用底层客户端(数据库、RPC、网络等。);它只需要在接口上实现原语。

## **底线**

Cosmos network 使用桥-集线器模型来连接具有独立安全保证的链，这意味着链间通信仍然受到接收链对发送链的信任的约束。

Polkadot 设计的首要原则是可伸缩性和互操作性需要共享验证逻辑，这是为了创建一个无信任的环境。

正在发展的区块链应该有他们的安全，必须是合作性的，而不是竞争性的。因为，Polkadot 提供了跨所有链的共享验证逻辑和安全流程，所以它们可以在知道(信任)它们的对话者在相同的安全上下文中执行的情况下进行交互。

## 结论

我认为，阿斯塔和 WASM 的发展可以成为通向“其他”链的桥梁，包括尤其是宇宙。现在我开始了解墨水了！，智能合约的事实标准。

我将致力于在这个生态系统中开发一个完整的系统映像，以避免分心。在使用 AWS 和 Linode 作为云上的日常驱动程序之间，这是一个很难决定的事情。我受过 AWS 的正式培训，但 Linode 似乎非常精简，我不喜欢过于依赖亚马逊的专有系统。

我们离新年越来越近了，我希望已经开始取得进展。熊市即将来临，所以现在是时候快速行动，清除地下街道上的血迹了。

> 交易新手？试试[加密交易机器人](/coinmonks/crypto-trading-bot-c2ffce8acb2a)或者[复制交易](/coinmonks/top-10-crypto-copy-trading-platforms-for-beginners-d0c37c7d698c)
> 
> 多样化的密码持有，了解[币安替代品](https://coincodecap.com/binance-alternatives)
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
*   杠杆代币的终极指南