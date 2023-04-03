# 赌徒谜题

> 原文：<https://medium.com/coinmonks/ciphershastra-gambler-puzzle-fef84a519796?source=collection_archive---------32----------------------->

![](img/972f1067349a9a370df8758d216aed65.png)

Ciphershastra

![](img/5e1ed1510feafb24494e348e10f61637.png)

你可以在这里找到挑战:[https://ciphershastra.com/Gambler.html](https://ciphershastra.com/Gambler.html)

赌徒真的很难对付。从表面上看，这是一个简单的合同，允许你购买一些筹码，然后在赌博游戏中使用这些筹码。

# 事实

关于该合同的一些有趣的事实是:

*   Ciphershastra 网站上仅显示主合同代码。其余的代码可以在 Etherscan 中找到，因为合同已经过验证。
*   它使用 0.7 编译器版本。从 0.7 版到 0.8 版有许多变化，但最突出的变化之一是引入了内置的安全数学运算。然而，契约似乎在使用 SafeMath 库，所以它仍然可以防止可能的上溢/下溢。
*   在用这些筹码赌博之前，用户最多可以购买 20 个筹码
*   在赌博之前，用户必须通过支付所请求的筹码来获得验证。特别是，用户必须为每个芯片支付 576460752303423488 魏。这绝对是一个奇怪的数字。如果我们把它转换成十六进制，看看它是什么样子呢？我们得到 0x0800000000000000。这是一个 64 位的数字，如果乘以 32 就会溢出
*   只有当呼叫者是另一个合同时，`buyChips`中的`_safeMint`才会通过`onERC721Received`回叫呼叫者。这个回调来自于`ERC721Receiver`接口，它用于确保调用方契约能够接收和处理 ERC721 令牌
*   `doubleOrNothing`函数调用`libGambler`库来计算掷骰子的结果。如果我们在 Etherscan 中查找这个库的源代码，我们可以看到它使用一些数据源，如块时间戳、coinbase 和难度，来计算掷骰子。通过使用另一个合同并在那里运行相同的计算来得出要使用的赌注，可以很容易地预测所有这些数据
*   如果骰子滚动预测正确，那么将以 NFTs 形式获得奖励的帐户将以非常特殊的方式进行计算，如果我们仔细观察，我们可以看到`acount`参数中的一个错别字，它可能会与`account`存储变量混淆。

# 盲点

在查看了所有事实之后，首先想到的是可能存在可重入攻击。这可以通过调用`_safeMint`的`onERC721Received`回调来实现。这肯定会让我们买更多的芯片。我们可以打电话给`buyChips(16)`，然后在`onERC721Received`回调时，我们可以再次打电话给`buyChips(16)`，从而总共购买 32 个筹码。但是，这仍然不起作用，因为`getVerified`函数会计算出要支付的总金额为 576460752303423488 * 32，由于使用了`SafeMath`，这将溢出并还原交易

一遍又一遍地检查 Etherscan 中的代码并没有对可能的攻击提供任何线索，直到我被暗示 Etherscan 可能在骗我。撒谎？怎么会？

# 真理的来源

当您在 Etherscan 中验证一个合同时，您会发送与合同编译相关的所有文件。但是，在使用库时，您也可以链接一个已经部署的库，以便在协定中使用。这个独立的库必须至少有一个外部/公共函数，这样它们才能被链接。如果一个库有所有的内部函数，它将被嵌入到使用它们的契约中。

这个链接库可以在 Etherscan 代码选项卡底部的`Libraries Used`部分看到。有什么条件？Etherscan 的 code 选项卡中显示的源代码是合同编译时可用的代码。然而，为库实际部署的代码可能会有所不同。通过将`Gambler`契约代码页中的`libGambler`源代码与契约中链接的`libGambler`库的验证源代码(可在地址`0x8674141e5af6B97D0be342C1DD157D0B8bb7ef57`获得)进行比较，我们可以看到。

我们可以看到，代码是不同的，骰子滚动的计算方式在结尾有一个额外的可疑参数。

好，这是一个可能会打乱我们赌注计算的差异。但是我们引起溢出的惊人想法呢？

如果我们在地址`0x8a5ba2bca9801BEac0A679f1D7e72a339b2Ca8c2`查看链接的`SafeMath`库，我们还会看到代码是不同的，事实上，`mul`函数实际上没有防止溢出。头奖！

# 解决方案合同

现在，我们唯一需要做的就是在我们的解决方案契约中实现来自实际链接`libGambler`的掷骰子，并利用可重入攻击来解决挑战。我们的攻击函数看起来像这样:

```
 address account = 0x0000000000000000000000000000000000000000;
 uint256 bet = uint256(keccak256(abi.encodePacked(block.timestamp, block.coinbase,
 account, “sus”))) % block.difficulty;

 gambler.buyChips(16);
 gambler.getVerified{ value: 0 }();
 gambler.doubleOrNothing(account, bet);
```

零地址在这里用于`account`强制`doubleOrNothing`中的`to`变量取`msg.sender`的值，这是我们的解决方案契约。然后我们需要准备回调函数:

`buyAgain`变量在合约构造时设置为`true`，然后一旦回调被调用一次就设置为`false`，所以我们只买 2 次 16 个筹码。

```
function onERC721Received(
    address operator,
    address from,
    uint256 tokenId,
    bytes calldata data
) external returns (bytes4) {
    if (buyAgain) {
        buyAgain = false;
        gambler.buyChips(16);
    }
    return this.onERC721Received.selector;
}
```

# 准备攻击！

最后，我们可以 pwn 赌徒契约。我们部署我们的解决方案契约，然后调用`pwn`函数，该函数将执行以下步骤:

*   买 16 个筹码
*   接受回调，在合约阻止我们购买更多筹码之前，再购买 16 个筹码
*   通过支付 0 魏获得验证，因为计算将溢出，但不会触发恢复
*   双倍或零，计算出的赌注将与赌徒契约中的骰子滚动相匹配
*   利润！

赌徒合同为我们铸造了额外的 32 个筹码，并标志着我们的合同是一个真正的赌徒！

# Github 回购

你可以在我的 Github repo:[https://github.com/robercano/ciphershastra](https://github.com/robercano/ciphershastra)中找到这个解决方案以及其他的密码破解挑战

# 关于我

我叫罗伯特·卡诺，你可以在 https://thesolidchain.com 找到我

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