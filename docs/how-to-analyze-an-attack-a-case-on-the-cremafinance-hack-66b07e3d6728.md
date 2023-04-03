# 如何分析一次攻击？一个关于 CremaFinance 黑客的案子

> 原文：<https://medium.com/coinmonks/how-to-analyze-an-attack-a-case-on-the-cremafinance-hack-66b07e3d6728?source=collection_archive---------11----------------------->

在本系列文章中，我们将对链上协议的一些典型攻击进行深入的事后调查，并分享 Sec3 核心团队为理解这些攻击所使用的技术和工具。

7 月份， [CremaFinance](https://twitter.com/Crema_Finance/status/1543416225622941696) 因智能合约漏洞被黑了 900 万美元。这名黑客最终获得了 170 万美元的赏金，并返还了 800 万美元。

**以下是主要发现(关于黑客):**

1.  关键漏洞在于*中的一个* ***缺少帐号验证***`Claim`…