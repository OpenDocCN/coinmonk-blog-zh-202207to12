# 智能合约中的 Tx.origin 攻击

> 原文：<https://medium.com/coinmonks/tx-origin-attack-in-smart-contract-28a3f13d3445?source=collection_archive---------4----------------------->

## 了解常见的智能合约漏洞。

![](img/a56d978228b3a9a03ea8ec608378b638.png)

Photo by [Mika Baumeister](https://unsplash.com/@mbaumi?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

## 智能合约安全性:

智能合同是一种软件，在满足特定标准时自动运行。因为智能合约是在分散的区块链网络上执行的，所以从设计上来说，它们被认为是安全的。但是，智能合约仍然容易受到某些安全风险的影响，如编码错误、恶意攻击和测试不足。为了降低安全问题的风险，遵循编写和部署智能协定的最佳实践非常重要，例如彻底测试代码、使用安全编码技术以及保持协定代码简单和模块化。定期审核智能合同以识别和解决任何潜在的漏洞也很重要。

## 刀客

涉及智能合约的最大黑客攻击之一发生在 2016 年，当时分散自治组织(DAO)的智能合约代码中的一个漏洞被利用，导致价值约 5000 万美元的加密货币以太被盗。这次黑客攻击是由于 DAO 的智能合同中的一个编码错误造成的，该错误允许攻击者在未经许可的情况下多次从合同中提取资金。这一事件凸显了对智能合约代码进行彻底测试和审计的必要性，以确保其安全且没有漏洞。

# 智能合约中的常见漏洞:

## 重入攻击

当一个协定调用另一个协定，而第二个协定在第一个协定完成执行之前回调到第一个协定时，就会发生可重入攻击。这可能导致意外行为和潜在的资金损失。

为了防止这种情况，Solidity 提供了`revert()`和`require()`功能，如果没有满足指定的条件，这些功能会暂停合同的执行，并恢复所做的任何更改。

下面是一个容易受到可重入攻击的契约示例:

```
contract ReentrancyAttack {
    address public owner;
    uint public balance; constructor() public {
        owner = msg.sender;
        balance = 100;
    } function withdraw() public {
        // vulnerable to reentrancy attack
        if (balance >= 10) {
            balance -= 10;
            msg.sender.transfer(10);
        }
    }
}
```

为了防止这个漏洞，我们可以使用`require()`函数在转账之前检查余额是否足够:

```
contract ReentrancyAttack {
    address public owner;
    uint public balance; constructor() public {
        owner = msg.sender;
        balance = 100;
    } function withdraw() public {
        // prevent reentrancy attack
        require(balance >= 10, "Insufficient balance");
        balance -= 10;
        msg.sender.transfer(10);
    }
}
```

在这个更新的合同中，如果余额不足以转账，`require()`功能将停止合同执行并恢复所做的任何更改。这确保了契约不会被可重入攻击所利用。

## Tx .原点

Solidity 中的 tx.origin 攻击利用了这样一个事实，即 Solidity 契约中的 tx.origin 全局变量是契约外部调用方的地址，而不是实际的契约所有者。这使得攻击者能够通过从另一个地址调用协定来冒充协定所有者，绕过基于协定所有者地址的任何访问控制或权限检查。

下面是一个易受 tx.origin 攻击的代码示例:

```
pragma solidity ^0.8.01;
contract MyContract { address public owner;
constructor() {
    owner = msg.sender;
}

function myFunction() public {
    require(msg.sender == owner, "Unauthorized access");
    // ..........
}
}
```

在本例中，协定检查 myFunction()函数的调用方是否是协定所有者。但是，因为使用 tx.origin 而不是 msg.sender，所以攻击者可以从任何地址调用 myFunction()并冒充契约所有者。这可能会允许攻击者未经授权访问协定的功能和数据。

## 三明治

三明治攻击是 Solidity smart contracts 在使用多个函数修改相同的状态变量时可能出现的漏洞。这可能会导致合同中出现意外和潜在的恶意行为。

下面是一个容易受到三明治攻击的可靠性合同的例子:

```
pragma solidity ^0.8.01;

contract SandwichAttack {
uint public balance;

function deposit(uint amount) public {
    balance += amount;
}

function withdraw(uint amount) public {
    balance -= amount;
}

function transfer(address to, uint amount) public {
    withdraw(amount);
    to.transfer(amount);
    deposit(amount);
}
}
```

在这个契约中，`transfer`函数使用`withdraw`和`deposit`函数将契约余额中的资金转移到另一个地址。然而，如果一个恶意用户在调用`withdraw`和`transfer`之间调用`deposit`函数，他们就可以有效地取消取款，并按照他们存入的金额增加合同余额。这将导致合同余额不正确，并可能导致资金丢失或被盗。

为了防止这个漏洞，Solidity 契约应该使用`require`关键字来确保状态变量不会被多个函数同时修改。例如，上述合同可以修改如下:

```
pragma solidity ^0.8.01;

contract SandwichAttack {
uint public balance;

function deposit(uint amount) public {
    require(balance + amount >= balance, "Overflow detected");
    balance += amount;
}

function withdraw(uint amount) public {
    require(balance >= amount, "Insufficient balance");
    balance -= amount;
}

function transfer(address to, uint amount) public {
    withdraw(amount);
    to.transfer(amount);
    deposit(amount);
}
} 
```

在这个修改的契约中，`deposit`和`withdraw`函数都使用了`require`关键字来确保契约的余额不会被多个函数同时修改。这防止了三明治攻击的发生，并确保了契约状态的完整性。

谢谢你们

在 Twitter 上关注我: [@Param_eth](https://twitter.com/Param_eth)

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