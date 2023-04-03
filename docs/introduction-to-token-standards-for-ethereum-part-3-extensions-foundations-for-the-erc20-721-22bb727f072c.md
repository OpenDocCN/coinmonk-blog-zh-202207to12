# 以太坊令牌标准介绍:第 3 部分:ERC20/721 的扩展和基础

> 原文：<https://medium.com/coinmonks/introduction-to-token-standards-for-ethereum-part-3-extensions-foundations-for-the-erc20-721-22bb727f072c?source=collection_archive---------10----------------------->

![](img/36d40d7b439957238b1d12f63dff5b6d.png)

Photo by [Kanchanara](https://unsplash.com/@kanchanara?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

这是令牌标准 4 部分系列的第 3 部分。在这里，我将介绍作为基础的令牌标准，并扩展 ERC 20 和 ERC 721 标准。

要了解更多关于可替换和不可替换令牌的标准，请参考我在本系列中的上一篇文章。

## **标准接口检测的令牌标准:ERC 165**

![](img/46cb1237900c0fd8156d79779b82bf31.png)

Photo by [Axel Ruffini](https://unsplash.com/es/@4xel?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

ERC 165 实际上是一种方法的标准，而不是令牌。ERC721 依赖于该标准。智能合约与不同的令牌交互。存在基于不同 ERC 标准的不同类型的令牌，并且令牌实现那些特定的标准功能。开发人员需要知道令牌实现了哪些接口，以便使用令牌，并且应该发布这些信息。

这是其他 ERC 的重要基础，原因如下—

*   开发人员需要知道智能合约实现了哪些接口
*   ERC 165 定义了一种方法来发布和检测智能合约实现的接口
*   它标准化了接口的标识

[**规格为 ERC 165**](https://eips.ethereum.org/EIPS/eip-165)

符合 ERC-165 的合同应实现以下接口

> 接口 ERC165 {
> 
> 函数支持 sInterface(bytes4 interfaceID)外部视图返回(bool)；
> 
> }

该接口的接口标识符为 ***0x01ffc9a7。*** 实现契约将有一个***supports interface***函数，该函数返回:

*   当 interfaceID 为 0x01ffc9a7 时为真(EIP165 接口)
*   当 interfaceID 为 0xffffffff 时为 false
*   对于此协定实现的任何其他接口为真
*   对于任何其他接口 ID 为 false

此函数必须*返回一个布尔值，并且最多使用 30，000 gas。*

# **ERC 20 的令牌标准扩展:ERC 223/ERC 621/ERC 777/ERC 827**

![](img/b7da5960f153d09401d265896d20515c.png)

Photo by [Nathan Dumlao](https://unsplash.com/@nate_dumlao?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

ERC 20 是一个流行的和广泛使用的标准。然而，它有一些限制—

*   如果 ERC 20 令牌被发送到不处理令牌的智能合约，则它们会丢失/烧毁
*   令牌供应是固定的，因为存在单个发布事件，并且被限制为单个不可变值
*   当调用智能合约时，在第一次交易之后，ERC 20 标准要求另一次交易来验证是否满足标准。智能协定仅在此之后调用。这增加了交易的数量，实际上造成了摩擦。

以下标准是旨在解决 ERC 20 的这些缺点的扩展

[**ERC 223**](https://github.com/Dexaran/ERC223-token-standard) **:** 防止令牌意外烧毁。提供接受/拒绝发送令牌的能力

[**ERC 621**](https://docs.ethhub.io/built-on-ethereum/erc-token-standards/erc621/)**:**提供 2 个附加功能:*增加供应* & *减少供应*。只能由合同所有者或受信任的用户执行。

[ERC**777**](https://eips.ethereum.org/EIPS/eip-777)

*   提出了一组函数来识别令牌的接收，并在第一次交易后立即启动智能合约。
*   在降低交易开销的同时，它还允许用户拒绝来自黑名单地址的传入令牌。它还支持操作员的“白名单”

[**ERC 827**](https://github.com/ethereum/eips/issues/827)

*   为获得消费许可的第三方启用代币转账。
*   它还支持令牌重用，所有各方都同意第三方使用动态金额的特定标准。

# **伪自检注册契约令牌标准:ERC 1820**

该标准定义了一个通用注册表智能协定，其中任何地址(协定或常规帐户)都可以注册它支持的接口以及负责其实现的智能协定。

**ERC 1820 的特点**

*   定义一个注册表，智能合约和普通帐户可以在其中发布它们实现的功能，可以直接发布，也可以通过代理合约发布
*   可以查询 Registry 来询问特定地址是否实现了给定的接口，以及哪个智能协定处理其实现。
*   ERC-1820 修复了由 Solidity 0.5 更新引入的 ERC-165 逻辑中的不兼容性。
*   ERC-165，其具有不能被常规账户使用的限制
*   ERC -1820 在功能上等同于 ERC-820，必须替代使用
*   这个标准比 ERC-672 简单得多，而且是完全分散的。

[**ERC 1820 的规格**](https://eips.ethereum.org/EIPS/eip-1820)

*   符合 ERC-1820 的合同应实现以下接口

*接口 ERC 1820 实现者接口{*

*函数 canImplementInterfaceForAddress(bytes 32 interface hash，addr)外部视图返回(bytes 32)；*

*}*

*   官方注册实现 *ERC1820Registry* 可以部署在任何链上，并在所有链上共享相同的地址。
*   该合同也作为 ERC-165 的缓存，以减少气体消耗。

# **多令牌的令牌标准&批处理可替代/不可替代令牌:ERC1155**

ERC-20 和 ERC-721 等令牌标准要求为每种令牌类型或集合部署单独的合同。这在以太坊区块链上放置了大量冗余的字节码，并通过将每个令牌合约分离到其自己的许可地址的性质来限制某些功能

ERC 1155 解决了 ERC 20 和 ERC 721 的所有主要问题。它是最先进的令牌标准之一。它适用于多令牌类型，并允许在单个契约中描述它们。

**ERC 1155 的特色**

*   ERC-20 要求为每种令牌类型部署单独的合同。
*   ERC-721 标准的令牌 ID 是单个不可替换的索引，并且这些不可替换的组被部署为具有用于整个集合的设置的单个契约。
*   ERC-1155 多令牌标准允许每个令牌 ID 代表一个新的可配置令牌类型，它可以有自己的元数据、供应和其他属性。
*   借助 ERC 1155，用户可以在单个合同中创建多个令牌，并可用于可替换和不可替换的令牌使用情形

[**规格 ERC 1155**](https://eips.ethereum.org/EIPS/eip-1155)

ERC1155 接口。

*   安全转移自
*   safebattchtransferfrom
*   平衡 Of
*   平衡批次
*   setApprovalForAll
*   isApprovedForAll

事件

*   转移单个
*   转移批次
*   批准全部
*   上呼吸道感染

智能合约必须实现 ERC1155TokenReceiver 接口中的所有函数来接受转账。必须遵守标准规定的安全转移规则

功能:

*   onerc 1155 已接收
*   onerc 1155 批次已接收

在本系列的下一篇文章中，我将介绍以太坊的身份和安全标准。

> 交易新手？尝试[加密交易机器人](/coinmonks/crypto-trading-bot-c2ffce8acb2a)或[复制交易](/coinmonks/top-10-crypto-copy-trading-platforms-for-beginners-d0c37c7d698c)