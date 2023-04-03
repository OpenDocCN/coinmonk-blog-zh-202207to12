# CSC101-访问控制

> 原文：<https://medium.com/coinmonks/csc101-access-control-217b15aea132?source=collection_archive---------49----------------------->

访问控制意味着“谁被允许做某事”，这在智能合约的世界中非常重要。你的合同的访问控制可能决定谁可以铸造代币，对提议投票，冻结转让，以及许多其他事情。

![](img/78db353f85d00a7149d8bc2b6bc3c9de.png)

## 所有权和`Ownable`

最常见和最基本的访问控制形式是**所有权**的概念:有一个帐户是契约的`owner`，可以对它执行管理任务。这种方法对于只有一个管理用户的合同来说是完全合理的。

OpenZeppelin 为在合同中实现所有权提供了`[Ownable](https://docs.openzeppelin.com/contracts/2.x/api/ownership#Ownable)`。

```
pragma solidity ^0.8.10;
```

```
import "@openzeppelin/contracts/ownership/Ownable.sol";contract MyContract is Ownable {
    function normalThing() public {
        // anyone can call this normalThing()
    } function specialThing() public onlyOwner {
        // only the owner can call specialThing()!
    }
}
```

默认情况下，`Ownable`合同的`[owner](https://docs.openzeppelin.com/contracts/2.x/api/ownership#Ownable-owner--)`是部署它的账户，这通常正是您想要的。

Ownable 还允许您:

*   `[transferOwnership](https://docs.openzeppelin.com/contracts/2.x/api/ownership#Ownable-transferOwnership-address-)`从业主账户转入新账户，以及
*   对于所有者来说，在集中管理的初始阶段结束之后，放弃这种管理特权是一种常见的模式。

**注意:**一个合同也可以是另一个合同的所有者。

## 基于角色的访问控制

虽然所有权**的简单性**对于简单的系统或快速原型开发很有用，但是通常需要不同级别的授权。帐户可以禁止用户进入系统，但不能创建新令牌。**基于角色的访问控制(RBAC)** 在这方面提供了灵活性。

本质上，我们将定义多个**角色**，每个角色被允许执行不同的动作集合。而不是到处都用`onlyOwner`——例如，你会在一些地方用`onlyAdminRole`，而在另一些地方用`onlyModeratorRole`。另外，您将能够定义如何为帐户分配角色、转移角色等规则。

大多数软件开发都使用基于角色的访问控制系统:一些用户是普通用户，一些可能是主管或经理，还有一些通常拥有管理特权。

## 使用`Roles`

**OpenZeppelin** 为实现基于角色的访问控制提供了`[Roles](https://docs.openzeppelin.com/contracts/2.x/api/access#Roles)`。它的用法很简单:对于您想要定义的每个角色，您将存储一个类型为`Role`的变量，该变量将保存具有该角色的帐户列表。

这里有一个在`[ERC20](https://docs.openzeppelin.com/contracts/2.x/tokens#ERC20)` [令牌](https://docs.openzeppelin.com/contracts/2.x/tokens#ERC20)中使用`Roles`的简单例子:我们将定义两个角色`minters`和`burners`，它们将能够分别铸造新令牌，并烧掉它们。

```
pragma solidity ^0.8.10;
```

```
import "@openzeppelin/contracts/access/Roles.sol";
import "@openzeppelin/contracts/token/ERC20/ERC20.sol";
import "@openzeppelin/contracts/token/ERC20/ERC20Detailed.sol";contract MyToken is ERC20, ERC20Detailed {
    using Roles for Roles.Role; Roles.Role private _minters;
    Roles.Role private _burners; constructor(address[] memory minters, address[] memory burners)
        ERC20Detailed("MyToken", "MTKN", 18)
        public
    {
        for (uint256 i = 0; i < minters.length; ++i) {
            _minters.add(minters[i]);
        } for (uint256 i = 0; i < burners.length; ++i) {
            _burners.add(burners[i]);
        }
    } function mint(address to, uint256 amount) public {
        // Only minters can mint
        require(_minters.has(msg.sender), "DOES_NOT_HAVE_MINTER_ROLE"); _mint(to, amount);
    } function burn(address from, uint256 amount) public {
        // Only burners can burn
        require(_burners.has(msg.sender), "DOES_NOT_HAVE_BURNER_ROLE"); _burn(from, amount);
    }
}
```

这么干净！通过以这种方式划分关注点，可以实现比简单的**所有权**访问控制方法更细粒度的权限级别。请注意，如果需要，一个帐户可以有多个角色。

OpenZeppelin 广泛使用`Roles`和预定义的契约，这些契约为每个特定的角色编码规则。例如:`[ERC20Mintable](https://docs.openzeppelin.com/contracts/2.x/api/token/ERC20#ERC20Mintable)`使用`[MinterRole](https://docs.openzeppelin.com/contracts/2.x/api/access#MinterRole)`来确定谁可以铸造代币，而`[WhitelistCrowdsale](https://docs.openzeppelin.com/contracts/2.x/api/crowdsale#WhitelistCrowdsale)`使用`[WhitelistAdminRole](https://docs.openzeppelin.com/contracts/2.x/api/access#WhitelistAdminRole)`和`[WhitelistedRole](https://docs.openzeppelin.com/contracts/2.x/api/access#WhitelistedRole)`来创建一组可以购买代币的账户。

这种灵活性允许有趣的设置:例如，一个`[MintedCrowdsale](https://docs.openzeppelin.com/contracts/2.x/api/crowdsale#MintedCrowdsale)`期望被给予一个`ERC20Mintable`的`MinterRole`以便工作，但是令牌契约也可以扩展`[ERC20Pausable](https://docs.openzeppelin.com/contracts/2.x/api/token/ERC20#ERC20Pausable)`并将`[PauserRole](https://docs.openzeppelin.com/contracts/2.x/api/access#PauserRole)`分配给一个 DAO，作为在契约代码中发现漏洞时的应急机制。限制系统的每个组件能够做什么被称为最小特权原则，这是一个很好的安全实践。

## OpenZeppelin 中的用法

你会注意到没有一个 OpenZeppelin 合同使用`Ownable`。`Roles`是一个首选的解决方案，因为它为库的用户提供了足够的灵活性来调整所提供的合同以满足他们的需求。

然而，在某些情况下，合同之间存在直接关系。例如，`[RefundableCrowdsale](https://docs.openzeppelin.com/contracts/2.x/api/crowdsale#RefundableCrowdsale)`在建筑上部署了一个`[RefundEscrow](https://docs.openzeppelin.com/contracts/2.x/api/payment#RefundEscrow)`，以持有其资金。对于这些情况，我们将使用`[Secondary](https://docs.openzeppelin.com/contracts/2.x/api/ownership#Secondary)`创建一个次级契约，允许一个*主*契约来管理它。你也可以认为这些是辅助合同。

> 交易新手？尝试[加密交易机器人](/coinmonks/crypto-trading-bot-c2ffce8acb2a)或[复制交易](/coinmonks/top-10-crypto-copy-trading-platforms-for-beginners-d0c37c7d698c)
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
*   [杠杆代币](/coinmonks/leveraged-token-3f5257808b22)终极指南