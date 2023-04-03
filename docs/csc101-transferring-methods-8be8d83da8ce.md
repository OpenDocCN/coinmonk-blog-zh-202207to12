# CSC 101-转移方法

> 原文：<https://medium.com/coinmonks/csc101-transferring-methods-8be8d83da8ce?source=collection_archive---------52----------------------->

有没有尝试过编写一个可以持有 CET 的智能合约？它还可以发送 CET 给另一个人！

在本教程中，我们将讨论发送，转移和调用的可靠性。留在我身边；)

![](img/78db353f85d00a7149d8bc2b6bc3c9de.png)

## 转移方法

Solidity 支持几种方法来发送本地硬币到另一个合同/钱包。

您可以通过以下方式将本地硬币发送到其他合同

*   `transfer` (2300 气，抛出错误)
*   `send` (2300 气体，返回布尔值)
*   `call`(转发所有气体或设定气体，返回 bool)

此外，接收原币的合同必须至少具有以下功能之一

*   `receive() external payable`
*   `fallback() external payable`

如果`msg.data`为空，则调用`receive()`，否则调用`fallback()`。

`call`建议在 2019 年 12 月后使用与重入防护装置结合的方法。

通过以下措施防止再次进入

*   在调用其他合同之前进行所有状态更改
*   使用再入防护修改器

## 转接 vs 发送 vs 通话

**transfer:** 接收智能契约应该定义一个 **fallback** 函数，否则 transfer 调用将抛出一个**错误**。有 **2300 气**的气限，足够完成转移操作。很难编码来防止**重入攻击**。

**发送:**它的工作方式与转移呼叫类似，也有一个气体限制 **2300 气体**。它将状态作为一个**布尔值**返回。

**调用 *:*** 这是向智能合约发送原生币的推荐方式。空参数触发接收地址的**回退**功能。

```
**(bool sent,memory data) = _to.call{value: msg.value}("");**
```

使用 ***调用*** ，也可以触发合同中定义的其他 ***功能*** 并发送固定量的气体来执行该功能。交易状态作为一个**布尔值**发送，返回值在数据变量中发送。

```
**(bool sent, bytes memory data) = _to.call{gas :10000, value: msg.value}("func_signature(uint256 args)");**
```

也有一些方法(视情况而定)转移硬币到其他合同/钱包。

## `address.send(amount)`

第一个也是最重要的特征是**提供 2300 气体极限**用于执行合同接收乙醚的回退功能。顺便说一句，这个数量只够创建一个事件。

```
contract Sender {
  function send(address _receiver) payable {
    _receiver.send(msg.value);
  }
}contract Receiver {
  uint public balance = 0;
  event Receive(uint value);

  function () payable {
    Receive(msg.value);
  }
}
```

`send()`执行不成功，例如气体耗尽错误**返回假**，但**不会抛出**异常。因此，`send()`的每次使用都应该在 require 之内。否则，您将支付天然气费用，以便在区块链提交交易，但所有状态更改都将被撤消。

例如，只需稍微改变一下`Receiver` 合同中的应付款函数:

```
function () payable {
  Receive(msg.value);
  balance += msg.value;
}
```

## `address.transfer(amount)`

让我们考虑一个出现在 solidity 后来版本中的方法。它还有两个值得一提的细节。

首先，该方法具有相同的 **2300 气体极限**。然而，在这些功能的开发过程中，开发人员[讨论了](https://github.com/ethereum/solidity/issues/610#issuecomment-278521662)添加`.gas()`修改器的机会，该修改器重新定义了所提供气体的限制。

第二，与`send()`方法不同，`transfer()` **在执行不成功时抛出异常**。

## address.call.value(金额)( )

最后也是最定制的方法。

如果出现错误，给定的函数**仍然返回 false** ，这就是为什么要记住`require()`的用法。

它与前两个功能的主要区别是有机会通过`.gas(gasLimit)`调节器**设置气体极限**。这是必要的，以防合同接收以太网的可支付功能执行复杂的逻辑，这需要大量的气体。

让我们看看下面的例子:

```
contract Sender {
  function send(address _receiver) payable {
    _receiver.call.value(msg.value).gas(20317)();
  }
}contract Receiver {
  uint public balance = 0;

  function () payable {
    balance += msg.value;
  }
}
```

执行接收器合同中的应付款功能需要花费 2 万多汽油。这就是为什么使用`send()`或`transfer()`会导致气体耗尽错误。

> 加入 Coinmonks [电报频道](https://t.me/coincodecap)和 [Youtube 频道](https://www.youtube.com/c/coinmonks/videos)了解加密交易和投资

# 另外，阅读

*   [最佳期货交易信号](https://coincodecap.com/futures-trading-signals) | [流动性交易所评论](https://coincodecap.com/liquid-exchange-review)
*   [火币的加密交易信号](https://coincodecap.com/huobi-crypto-trading-signals) | [Swapzone 审查](/coinmonks/swapzone-review-crypto-exchange-data-aggregator-e0ad78e55ed7)
*   [最佳密码交易机器人](/coinmonks/crypto-trading-bot-c2ffce8acb2a) | [购买索拉纳](https://coincodecap.com/buy-solana) | [矩阵导出评论](https://coincodecap.com/matrixport-review)
*   [Coldcard 评论](https://coincodecap.com/coldcard-review) | [BOXtradEX 评论](https://coincodecap.com/boxtradex-review)|[uni swap 指南](https://coincodecap.com/uniswap)
*   [比特币基地评论](/coinmonks/coinbase-review-6ef4e0f56064) | [德里比特评论](/coinmonks/deribit-review-options-fees-apis-and-testnet-2ca16c4bbdb2) | [FTX 评论](/coinmonks/ftx-crypto-exchange-review-53664ac1198f)