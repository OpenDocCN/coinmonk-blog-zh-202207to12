# 摄氏破产外卖

> 原文：<https://medium.com/coinmonks/celsius-bankruptcy-take-away-4f3ab53a8695?source=collection_archive---------19----------------------->

![](img/bfb7877f33ddb77afb1616368b396fbb.png)

在申请破产保护后的过去一个月里，Celsius 一直处于聚光灯下。大多数媒体或播客只涉及表面。然而，我们想更深入。在我们最新的文章中，我们努力揭开最大的加密贷款平台之一的神秘面纱，更好地描述 Celsius 实际上是如何运作的，最重要的是它是如何失败的。

Celsius 的破产是继 Terra/Luna 崩溃后的过去几个月中密码世界最重大的崩溃之一。该公司在去年 12 月获得由 WestCap 和 CDPQ(加拿大最大的养老基金之一)牵头的 6 亿美元 B 轮融资后，估值为 30 亿美元。早在 2021 年 10 月，Celsius 报告称其总资产超过 250 亿美元。快进到今天，Celsius 的总资产缩减到 43 亿美元，总负债超过其资产 12 亿美元，即其股权价值为负 12 亿美元。Celsius 现已正式破产。它并不孤单。Voyager Digital 和 Babel Finance 是另外两家最近申请破产的著名借贷平台。是什么导致了 Celsius 在如此短的时间内失败？我们在这里可以学到什么？有很多关于哪里出了问题的猜测，但没有一个提供了一个好的和准确的画面。感谢 Celsius 的第 11 章申请，我们现在可以更好地了解整个故事。

在这次深度探讨中，我们将探究 Celsius 的商业模式，报道一些相关的故事，并列出一些我们可以从其失败中吸取的重要教训。我们还计划在下一篇文章中写航海家，巴别塔，BlockFi 和 3AC。

为了帮助读者导航，这一深度探讨组织如下:

*   **Celsius 商业模式——简要描述和流程图**
*   **该商业模式运作的 3 个标准及详细解释**
*   【Celsius 配置资产的 5 种方式——1)贷款，2) stETH，3) Anchor，4)比特币采矿或其他长期项目，5) CeFi 交易策略和 DeFi yield farming
*   **摄氏资产负债表重建及净亏损对账**
*   **过度冒险和糟糕的风险管理**
*   **结论和经验教训**

**从高层面来说，这里有一些关键的要点:** 1。流动性管理必须是重中之重。历史上，当商业模式依赖于短期借款(存款)和长期投资(贷款、采矿等)时，我们已经看到了许多失败。它需要在一定的安全范围内运作，并有应对危机的应急措施。

2.资产和负债不匹配增加了裸风险敞口。Celsius 无法承保足够的贷款来抵消存款。市场波动会造成巨大的市值损失。

3.集中的风险。丽都的 stETH 占总控股 ETH 的 1/3，Celsius/3AC/Alameda 拥有总 stETH 的 1/3 以上。每个人都会去出口看鲸鱼。

4.交易对手风险！在 Celsius 存在的短暂历史中，它因交易对手未能履行其义务而损失了超过 5 亿美元:对贷款人的索赔 4.4 亿美元，对 StakeHound/Fireblock 的索赔约 1 亿美元。

5.杠杆是一把双刃剑。根据 Celsius 在 2022 年 3 月披露的资产负债表，Celsius 当时的股本仅为 2000 万美元，这意味着杠杆率为 1000 倍。典型的银行会有 10 倍的杠杆率。你猜 08 年雷曼兄弟的杠杆率超过 30 倍时发生了什么？

要了解更多细节，请阅读下面的深入探讨。

[](/algotune/why-did-celsius-network-fail-a-deep-dive-into-celsius-chapter-11-filing-959ca897168a) [## 摄氏网络为什么会失败？—深入了解 Celsius 的第 11 章申请

### 我们研究了 Celsius 的商业模式，涵盖了一些相关的故事，并列出一些重要的经验教训…

medium.com](/algotune/why-did-celsius-network-fail-a-deep-dive-into-celsius-chapter-11-filing-959ca897168a) 

> 交易新手？试试[加密交易机器人](/coinmonks/crypto-trading-bot-c2ffce8acb2a)或者[复制交易](/coinmonks/top-10-crypto-copy-trading-platforms-for-beginners-d0c37c7d698c)