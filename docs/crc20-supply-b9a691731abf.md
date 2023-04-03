# CRC20 电源

> 原文：<https://medium.com/coinmonks/crc20-supply-b9a691731abf?source=collection_archive---------48----------------------->

在本教程中，我们将学习如何创建自己的自定义供应机制的 CRC20 令牌。为此，我们将展示两种使用 OpenZeppelin 契约的惯用方法，您将能够将它们应用到您的智能契约开发实践中。

![](img/ad5f5886c590e06e022a359764163a8c.png)

由构建在 Coinex 智能链上的令牌实现的标准接口称为 CRC20。CRC20 与以太坊上的 ERC20 相同，Contracts 包含一个广泛使用的实现:名副其实的`[ERC20](https://docs.openzeppelin.com/contracts/2.x/api/token/ERC20)` contract。这个合同，就像标准本身一样，非常简单。事实上，如果您尝试按原样部署一个`ERC20`实例，它将毫无用处…它将没有供应！没有供给的代币有什么用？

ERC20 文件中未定义供应的创建方式。每个令牌都可以自由地试验自己的机制，从最分散的到最集中的，从最天真的到最有研究的，等等。

## 定量供应

假设我们想要一个固定供应量为 1000 的令牌，最初分配给部署合同的帐户。如果您使用过 Contracts v1，您可能已经编写了如下代码:

```
contract CRC20FixedSupply is ERC20 {
    constructor() public {
        totalSupply += 1000;
        balances[msg.sender] += 1000;
    }
}
```

**注意:**不要忘记导入 Oppenzeppelin ERC20.sol

从 Contracts v2 开始，这种模式不仅不被鼓励，而且被禁止。变量`totalSupply`和`balances`现在是`ERC20`的私有实现细节，不能直接写。相反，有一个内部的`[_mint](https://docs.openzeppelin.com/contracts/2.x/api/token/ERC20#ERC20-_mint-address-uint256-)`函数可以做到这一点:

```
contract CRC20FixedSupply is ERC20 {
    constructor() public {
        _mint(msg.sender, 1000);
    }
}
```

像这样封装状态使得扩展契约更加安全。例如，在第一个例子中，我们必须手动保持`totalSupply`与修改后的天平同步，这很容易忘记。事实上，我们忽略了另一件也很容易被遗忘的事情:标准所要求的、一些客户端所依赖的`Transfer`事件。第二个例子没有这个 bug，因为内部的`_mint`函数会处理它。

## 奖励矿工

内部的`[_mint](https://docs.openzeppelin.com/contracts/2.x/api/token/ERC20#ERC20-_mint-address-uint256-)`函数是关键的构建模块，它允许我们编写实现供应机制的 CRC20 扩展。

我们将实施的机制是对生产 CSC 区块的矿工的象征性奖励。在 Solidity 中，我们可以在全局变量`block.coinbase`中访问当前块的矿工地址。每当有人调用我们令牌上的函数`mintMinerReward()`时，我们将向该地址铸造一个令牌奖励。这种机制听起来可能很傻，但是你永远不知道这会导致什么样的动态，这是值得分析和试验的！

```
contract CRC20WithMinerReward is ERC20 {
    function mintMinerReward() public {
        _mint(block.coinbase, 1000);
    }
}
```

正如我们所看到的，`_mint`使得正确地做到这一点变得非常容易。

## 将机制模块化

合同中已经包含一个供应机制:`[ERC20Mintable](https://docs.openzeppelin.com/contracts/2.x/api/token/ERC20#ERC20Mintable)`。这是一种通用机制，其中一组帐户被分配了`minter`角色，授予他们调用`[mint](https://docs.openzeppelin.com/contracts/2.x/api/token/ERC20#ERC20Mintable-mint-address-uint256-)`函数的权限，这是`_mint`的外部版本。

这可用于集中铸造，其中外部拥有的账户(即，具有一对密钥的人)决定制造多少供应和给谁。这种机制有非常合理的用例，比如传统的资产支持的 stablecoins。

但是，具有 minter 角色的帐户不需要为外部所有，也可以是实现无信任机制的智能契约。事实上，我们可以实现与上一节相同的行为。

```
contract MinerRewardMinter {
    ERC20Mintable _token;
```

```
 constructor(ERC20Mintable token) public {
        _token = token;
    } function mintMinerReward() public {
        _token.mint(block.coinbase, 1000);
    }
}
```

这个契约在用一个`ERC20Mintable`实例初始化时，将导致与上一节中实现的行为完全相同的行为。使用`ERC20Mintable`的有趣之处在于，我们可以通过将角色分配给多个合同来轻松地组合多个供应机制，而且我们可以动态地这样做。

## 自动化奖励

除了`_mint`之外，`ERC20`还提供了其他可以使用或扩展的内部函数，如`[_transfer](https://docs.openzeppelin.com/contracts/2.x/api/token/ERC20#ERC20-_transfer-address-address-uint256-)`。该函数实现令牌传输并由`ERC20`使用，因此可用于自动触发功能。这是用`ERC20Mintable`方法做不到的。

除了我们以前的供应机制，我们可以用它来为区块链中包含的每一次代币转移创造一个矿工奖励。

```
contract CRC20WithAutoMinerReward is ERC20 {
    function _mintMinerReward() internal {
        _mint(block.coinbase, 1000);
    }
```

```
 function _transfer(address from, address to, uint256 value) internal {
        _mintMinerReward();
        super._transfer(from, to, value);
    }
}
```

注意我们如何覆盖`_transfer`来首先铸造矿工奖励，然后通过调用`super._transfer`来运行最初的实现。这最后一步对于保留 ERC20 传输的原始语义非常重要。

我们已经看到了两种实现 CRC20 供应机制的方法:内部通过`_mint`，外部通过`ERC20Mintable`。希望这已经帮助你理解了如何使用 **OpenZeppelin** 和它背后的一些设计原则，你可以将它们应用到你自己的智能合同中。

> 交易新手？尝试[加密交易机器人](/coinmonks/crypto-trading-bot-c2ffce8acb2a)或[复制交易](/coinmonks/top-10-crypto-copy-trading-platforms-for-beginners-d0c37c7d698c)
> 
> 加入 Coinmonks [电报频道](https://t.me/coincodecap)和 [Youtube 频道](https://www.youtube.com/c/coinmonks/videos)获取每日[加密新闻](http://coincodecap.com/)

## 另外，阅读

*   [复制交易](/coinmonks/top-10-crypto-copy-trading-platforms-for-beginners-d0c37c7d698c) | [加密税务软件](/coinmonks/crypto-tax-software-ed4b4810e338)
*   [电网交易](https://coincodecap.com/grid-trading) | [加密硬件钱包](/coinmonks/the-best-cryptocurrency-hardware-wallets-of-2020-e28b1c124069)
*   [密码电报信号](/coinmonks/top-3-telegram-channels-for-crypto-traders-in-2021-8385f4411ff4) | [密码交易机器人](/coinmonks/crypto-trading-bot-c2ffce8acb2a)
*   [最佳加密交易所](/coinmonks/crypto-exchange-dd2f9d6f3769) | [印度最佳加密交易所](/coinmonks/bitcoin-exchange-in-india-7f1fe79715c9)
*   [面向开发人员的最佳加密 API](/coinmonks/best-crypto-apis-for-developers-5efe3a597a9f)
*   最佳[密码借贷平台](/coinmonks/top-5-crypto-lending-platforms-in-2020-that-you-need-to-know-a1b675cec3fa)
*   [免费加密信号](/coinmonks/free-crypto-signals-48b25e61a8da) | [加密交易机器人](/coinmonks/crypto-trading-bot-c2ffce8acb2a)
*   杠杆代币的终极指南