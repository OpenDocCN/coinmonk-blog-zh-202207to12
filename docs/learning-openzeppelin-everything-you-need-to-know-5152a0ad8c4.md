# 与 OpenZeppelin 建立智能合同(您需要知道的一切)

> 原文：<https://medium.com/coinmonks/learning-openzeppelin-everything-you-need-to-know-5152a0ad8c4?source=collection_archive---------17----------------------->

## OpenZepplin 库简介

![](img/73d71db6be5421a7cce12adf189112b9.png)

Photo by [Milad Fakurian](https://unsplash.com/@fakurian?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 介绍

**OpenZeppelin** 是**协议**、**模板**、&、**实用程序**的**开源库**，用于**智能合约开发**。这包括令牌标准的实现、灵活的基于角色的许可方案、&可重用组件。

# 装置

OpenZeppelin 通过项目中的本地安装来使用。

```
npm install @openzeppelin/contracts
```

# 代币

OpenZeppelin 库提供了对`ERC20`、`ERC721`、`ERC777`、&、`ERC1155.`的实现

## ERC20

OpenZeppelin 提供了对`ERC20` **可替换令牌**标准的支持。

*下面是一个 ERC20 令牌的实现:*

## ERC777

OpenZeppelin 提供对`ERC777` **可替换令牌**标准的支持。虽然与前面列出的协议相似，但该标准提供了对事务挂钩、委托操作符和其他一些生活质量改进的支持。

*这里是一个 ERC777 令牌的实现:*

## ERC721

OpenZeppelin 提供了对`ERC721` **不可替换令牌**标准的支持。

*这里是一个 ERC721 令牌的实现:*

## ERC1155

OpenZeppelin 提供了对`ERC1155` **可互换不可知令牌**标准的支持。这个标准允许**多个令牌**由一个契约来表示，而不管可替换性如何。

*这里是一个 ERC1155 令牌的实现:*

# 管理

OpenZeppelin 为智能合约提供了**基于角色的管理**。

## 可拥有的

`Ownable`合同可以扩展以提供所有者特定的限制。默认情况下，部署地址被设置为合同所有者。

在适用的情况下,`onlyOwner`修饰符用于强制所有者独占执行。

*下面是可拥有契约的一个实现:*

## 访问控制

可以扩展`AccessControl`契约，以提供更细粒度的基于角色的限制。这意味着可以相互独立地声明和设置各个角色。

`onlyRole(role)`修饰符用于强制角色独占执行。

`grantRole(role, account)`和`revokeRole(role, account)`函数分别用于管理和撤销角色。但是，这些功能只能由关联的管理角色调用。如果最初未由 `_setRoleAdmin(role, adminRole)`设置，则使用由`_setupRole(role, account)`在编译时设置的`DEFAULT_ADMIN_ROLE`。

*下面是访问控制契约的一个实现:*

`getRoleMemberCount(role)`函数用于检索具有特定角色的帐户数量，而`getRoleMember(role, index)`函数用于获取角色成员的地址。

*下面是一个获取指定角色所有成员的循环:*

## TimelockController

`TimelockController`模块充当时间锁控制器。它充当由提议者和执行者管理的代理，延迟维护操作，以便用户在提交之前有机会检查它们。

默认情况下，部署地址获得管理权限。该角色授予分别通过`PROPOSER_ROLE`、`EXECUTOR_ROLE`、&、`ADMIN_ROLE`标签分配提议者、执行者和其他管理员的权利。

# 管理

OpenZeppelin 为智能合同提供了对链上治理的支持。

## 代币

通过扩展`ERC20Votes`，可以为`ERC20`合同提供投票功能。

*这里是一个具有投票功能的 ERC20 令牌:*

投票支持也可用于带有`ERC721Votes`的`ERC721`。

## 管理者

为了配置治理协议，必须创建一个调控器契约。

`Governor`基础契约是治理协议的核心。任何附加功能都是通过扩展模块来实现的。

*以下是常用模块列表:*

*   `GovernorVotes`:挂钩到`IVotes`实例，根据账户的令牌余额确定账户的投票权。
*   `GovernorQuorumFraction`:定义执行提议所需的令牌总供应量的百分比。
*   `GovernorCountingSimple`:定义一个简单的投票方案(`FOR` / `AGAINST` / `ABSTAIN`)
*   `GovernorTimelockControl`:定义一个要使用的`TimelockController`。

*下面是一个简单的调控器契约:*

## 提案生命周期

OpenZeppelin 与第三方治理工具兼容。这意味着可以通过脚本或治理应用程序(如 **Tally** 或 **Defender)来制定和执行提议。**

手动创建提案时，必须调用`propose(targets, values, calldata, description)`功能。

*这是一个使用 Ethers.js 的财政拨款提案的部署:*

如果提议成功，它可以调用`execute(target, values, calldata, descriptionHash)`函数。如果调控器强制执行一个`TimeController`，那么在执行之前必须首先调用`queue(target, values, calldata, descriptionHash)`。

*下面是一个建议队列调用:*

*下面是一个提议执行调用:*

# 公用事业

OpenZeppelin 提供了许多有用的实用程序。

## ECDSA

`ECDSA`(椭圆曲线数字签名算法)实用程序提供了恢复和管理以太坊账户 ECDSA 签名的功能。

`recover(signature)`函数用于恢复数据签名人，并将其地址进行比较以验证签名。

> ⚠️ **警告:**哈希解密可能取决于签名格式

*下面是以太坊签名消息哈希的链上恢复:*

# Base64

`Base64`实用程序允许将`bytes32`类型的 dat 转换成`Base64`字符串表示。这对于为不可替换的令牌构建 URL 安全的令牌 URIs 非常有用。

*下面是编码成 Base64 数据 URI 的 JSON 元数据:*

# 就是这样！

正如我在我的页面上所说的，这些是我个人笔记本上的笔记。如果有任何明显的错误，请随时留下评论，以便我可以修复它们。还有，如果有你想从我这里看到的内容，请告诉我！

资源:[https://docs.openzeppelin.com/](https://docs.openzeppelin.com/)

> 交易新手？尝试[加密交易机器人](/coinmonks/crypto-trading-bot-c2ffce8acb2a)或[复制交易](/coinmonks/top-10-crypto-copy-trading-platforms-for-beginners-d0c37c7d698c)