# CSC101-基于时间的智能合同

> 原文：<https://medium.com/coinmonks/csc101-time-based-smart-contract-79ebde37d811?source=collection_archive---------29----------------------->

有时你可能需要在特定的时间运行一个功能或事件。这里我们利用可靠性引入基于时间的智能合同。在本教程中，我们将研究基于时间的事件及其通过 solidity 的实现。这是基于时间的智能合约(如授权合约)中使用的方法之一。

![](img/78db353f85d00a7149d8bc2b6bc3c9de.png)

## 与时间一起工作

在 solidity 中，我们使用整数来存储 Unix 时间格式的时间。看看下面的智能合同。

```
// SPDX-License-Identifier: GPL-2.0
pragma solidity ^0.8.10;

contract Time{

    uint public Time;

    constructor(uint _Time){
        Time = _Time;
    }

    function getCurrentTime()public view returns(uint){
        return block.timestamp;
    }

}
```

*   我们使用类型单元(无符号整数)来存储 Unix 时间戳。
*   在 solidity 中， **block.timestamp** 给出当前时间。我们可以通过调用 **getCurrentTime()来验证这一点。**
*   我们还可以使用 Unix 时间格式执行不同的操作，如**=**、 **≤** 、 **≥** 。

坚固性提供了一些处理时间的原生单位。我们可以使用以下单位:`seconds`、`minutes`、`hours`、`days`、`weeks`、`years`。这些单位将转换成特定时间长度**中的秒数的`uint`。**比如`1 minutes`是 60，1 小时是 3600 (60 秒 x 60 分钟)等等。

```
uint a = 3 hours; //  10800 or 3*60*60
uint b = 5 minutes // 300 or 5*60
uint d = 2 weeks // 1209600 or 2*7*24*60*60
```

这些单元对于在智能合约中处理日期和时间非常有用，因为它们允许我们使用像`+`和`-`这样的数学运算符。

## 检查时间是否已过

下面的代码显示了如何检查自合同部署以来是否已经过了 10 分钟:

```
pragma solidity >=0.8.10;

contract MyContract {
  uint deployDate;
  constructor(){
    deployDate = now;
  }
  // Will return `true` if 10 minutes have passed since `the contract was deployed
  function tenMinutesHavePassed() public view returns (bool) {
    return (now >= (deployDate + 10 minutes));
  }
}
```

## 添加和休息日

我们可以使用任何时间单位的数学运算符`+`和`-`来计算具体日期:

```
uint date = 1638352800; // 2012-12-01 10:00:00
uint later = date + 1 days;
uint nextYear = date + 1 years;
uint before = date - 3 days;
uint prevWeek = date - 1 weeks;
```

## 相对时间

为了演示如何利用时间来影响交易处理，我将开发一个智能合同来帮助我在指定的时间段内省钱。那段时间，我(或者其他任何人！)可以在合同里存乙醚，但是合同不让我提取任何乙醚，直到等待期结束。

坚固性有内置的“时间单位”。例如，我可以指定`3 days`或`5 years`。储蓄合同将以天数指定的等待期为参数。因为 EVM 自然地以秒为单位存储时间，所以契约必须用一天中的秒数来衡量等待时间，这是由 Solidity 中的字面`1 days`给出的。

```
pragma solidity ^0.8.10;

contract Time{
    address owner;
    uint256 deadline;
    modifier onlyOwner() {
        require(msg.sender == owner);
        _;
    }
    function deposit(uint256 amount) public payable {
        require(msg.value == amount);
    }
    function Savings(uint256 numberOfDays) public payable {
        owner = msg.sender;
        deadline = now + (numberOfDays * 1 days);
    }
    function withdraw() public onlyOwner {
        require(now >= deadline);
        msg.sender.transfer(address(this).balance);
    }
}
```

上面的代码演示了以下新概念:

*   `(numberOfDays * 1 days)`以秒为单位计算`numberOfDays`天的时间。
*   `now + (numberOfDays * 1 days)`计算代表未来`numberOfDays`天的 EVM 时间。
*   `now >= deadline`测试当前时间是否大于或等于`deadline`。(即，截止日期是否已过？)

储蓄合同由不允许提款的天数参数化。在此期间，任何`withdraw`交易都会失败。这段时间过后，取款将会成功。

任何时候都允许存款。还要注意，构造函数有`payable`修饰符，它允许在部署期间将 ether 附加(和转移)到契约上。

*   区块链中的每个块都包括一个时间戳，该时间戳被指定为自 Unix 纪元以来的秒数。时间值是整数。
*   Solidity smart contracts 可以访问当前块的时间戳作为`now`或`block.timestamp`。
*   Solidity 提供了方便的时间单位，如`days`和`years`，这有助于计算时间跨度。

> 交易新手？尝试[加密交易机器人](/coinmonks/crypto-trading-bot-c2ffce8acb2a)或[复制交易](/coinmonks/top-10-crypto-copy-trading-platforms-for-beginners-d0c37c7d698c)
> 
> 加入 Coinmonks [电报频道](https://t.me/coincodecap)和 [Youtube 频道](https://www.youtube.com/c/coinmonks/videos)获取每日[加密新闻](http://coincodecap.com/)

## 另外，阅读

*   [复制交易](/coinmonks/top-10-crypto-copy-trading-platforms-for-beginners-d0c37c7d698c) | [加密税务软件](/coinmonks/crypto-tax-software-ed4b4810e338)
*   [网格交易](https://coincodecap.com/grid-trading) | [加密硬件钱包](/coinmonks/the-best-cryptocurrency-hardware-wallets-of-2020-e28b1c124069)
*   [密码电报信号](/coinmonks/top-3-telegram-channels-for-crypto-traders-in-2021-8385f4411ff4) | [密码交易机器人](/coinmonks/crypto-trading-bot-c2ffce8acb2a)
*   [最佳加密交易所](/coinmonks/crypto-exchange-dd2f9d6f3769) | [印度最佳加密交易所](/coinmonks/bitcoin-exchange-in-india-7f1fe79715c9)
*   [开发者最佳加密 API](/coinmonks/best-crypto-apis-for-developers-5efe3a597a9f)
*   最佳[密码借贷平台](/coinmonks/top-5-crypto-lending-platforms-in-2020-that-you-need-to-know-a1b675cec3fa)
*   [免费加密信号](/coinmonks/free-crypto-signals-48b25e61a8da) | [加密交易机器人](/coinmonks/crypto-trading-bot-c2ffce8acb2a)
*   杠杆代币的终极指南