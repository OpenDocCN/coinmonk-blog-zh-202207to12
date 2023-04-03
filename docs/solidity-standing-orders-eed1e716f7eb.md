# 可靠性——长期订单

> 原文：<https://medium.com/coinmonks/solidity-standing-orders-eed1e716f7eb?source=collection_archive---------20----------------------->

![](img/815d8c34cd49b5ae3fbeb6656f42a04c.png)

Photo by [Héctor J. Rivas](https://unsplash.com/@hjrc33?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

作为这篇文章的开篇，我将发现/讨论如何在 solidity 中创建长期订单，这是我在 web3 中从未尝试过的。

让我们说，我们将使用 USDT 作为支付令牌的选择。

我们将需要创建一个具有自定义余额计算功能的新令牌&使用它作为 USDT 的包装版本，让我们称之为…