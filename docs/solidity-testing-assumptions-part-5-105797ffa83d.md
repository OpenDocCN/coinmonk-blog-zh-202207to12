# 可靠性测试假设第 5 部分

> 原文：<https://medium.com/coinmonks/solidity-testing-assumptions-part-5-105797ffa83d?source=collection_archive---------28----------------------->

欢迎来到我测试自己和他人的汽油/储物节能假设的系列。请随意在评论中提出你的假设，:D

假设:使用接口比使用低级调用函数更便宜。

![](img/78e75db550b22c80f34f4423d670c2e5.png)![](img/b635ad0d8735ffefe9dcf4b1d4285556.png)

呜哇！我是正确的 xD

好吧，下一个假设！

假设—在存储中设置 uint8 比设置 uint256 成本更低

![](img/699389e6ee708e61a6a5f906623f0ead.png)![](img/16458a98cba7f9eaf39d0dffdcf0aa5d.png)

我错了，我认为 Test2 更便宜，因为 uint256 适合 32 字节的内存插槽&这可能与 Yul (Solidity 的汇编编译器)一起工作更流畅。

如果你觉得这很有趣，为什么不看看这个呢！
[https://medium.com/p/28a8bb064e86](/p/28a8bb064e86)

坚实发展研究小组—[https://discord.gg/KzbcGmrnfN](https://discord.gg/KzbcGmrnfN)

-多边形联盟—[https://www.polygonalliance.com/](https://www.polygonalliance.com/)

——多边形联盟不和—[https://discord.gg/kJKPCGQu66](https://discord.gg/kJKPCGQu66)

你喜欢这篇文章吗？想请我喝杯咖啡吗？
Polygon/Eth/Bsc—0x4a 581 E0 EAF 6b 71d 05905 e8e 6014 DC 0277 a1 b 10 ad

> *交易新手？试试* [*加密交易机器人*](/coinmonks/crypto-trading-bot-c2ffce8acb2a) *或* [*复制交易*](/coinmonks/top-10-crypto-copy-trading-platforms-for-beginners-d0c37c7d698c) *上* [*最好的加密交易*](/coinmonks/crypto-exchange-dd2f9d6f3769)

> 加入 Coinmonks [电报频道](https://t.me/coincodecap)和 [Youtube 频道](https://www.youtube.com/c/coinmonks/videos)获取每日[加密新闻](http://coincodecap.com/)

# 另外，阅读

*   [免费加密信号](/coinmonks/free-crypto-signals-48b25e61a8da) | [加密交易机器人](/coinmonks/crypto-trading-bot-c2ffce8acb2a)
*   [杠杆代币](/coinmonks/leveraged-token-3f5257808b22)终极指南
*   [16 款最佳折叠电动自行车](/coinmonks/top-17-folding-electric-bikes-5e296f0918cb)
*   [28 款最佳电动自行车点评](/coinmonks/the-28-best-electric-bikes-review-and-buying-guide-in-2023-7bb3146cb403)
*   前三名[币安期货交易机器人](/coinmonks/top-3-binance-futures-trading-bots-e6031f84b3f9)