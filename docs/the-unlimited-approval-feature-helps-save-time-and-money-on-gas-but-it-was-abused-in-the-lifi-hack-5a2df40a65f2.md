# 无限制的批准功能有助于节省汽油的时间和金钱，但它在 LiFi 黑客中被滥用了

> 原文：<https://medium.com/coinmonks/the-unlimited-approval-feature-helps-save-time-and-money-on-gas-but-it-was-abused-in-the-lifi-hack-5a2df40a65f2?source=collection_archive---------59----------------------->

通过 LI，用户可以方便地在各种主要连锁店之间进行各种资产转移和交换。FI，一种高级桥接和分散交换(DEX)聚合器。对于 DeFi 开发者来说，将所有不同的桥集成到跨链去中心化应用程序(d apps)中是一个巨大的挑战。菲，你不需要理解那些桥的所有错综复杂。

2022 年 3 月 20 日，一名黑客利用交换功能，利用李。菲的智能合同。

李。FI 攻击是一个跨链桥漏洞，其根本原因是由于未经检查的外部调用。事实上，整个攻击相当简单，没有部署恶意的智能合约。同样缺席现场的还有闪贷。

袭击者瞄准了李。菲的智能合同。该契约具有交换功能，允许在桥接之前进行交换。攻击者跳过交换，直接在 LI 的上下文中调用令牌契约。菲的智能合同。

攻击者可以设法击中 29 个钱包来偷钱。基于用户给予的具有无限批准的令牌契约，攻击者可以彻底搜查各种令牌。

攻击者只是调用了 CBridgeFacet 契约的 swapAndStartBridgeTokensViaCBridge(0x 01 c0a 31 a)函数。因此，它将多个代币从不同的消费者转移到 EOA。

智能路由发生在链外，可以为用户确定最佳路线。前端网站确定最佳路线，并将参数提供给 swapAndStartBridgeTokensViaCBridge 函数。

由于该函数不检查参数，包括白名单和互换滑动，LIFI 项目不允许区块链上的任何帐户调用该函数。

包含风险的部分是 CBridgeFacet 合同。通过这份合同，用户认可代币，将他们的钱置于风险之中。

用户调用了函数 swapAnStartBridgeTokensViaCBridge 并批准了令牌。不同令牌的 transferFrom 函数也会被调用，导致 59.6 万美元的损失。

以下令牌是被盗走的:贾维斯奖励令牌()，多边形(MATIC)，火箭池()，灵知()，系绳()，元宇宙指数()，奥迪斯(Audius)，AAVE (AAVE)，戴(戴)。

任何对合同给予无限赞同的人都变得脆弱。为了消除漏洞，该团队立即禁用了公司智能合同中的交换方法。

分散化金融领域的这一黑客行为表明，给予智能合约无限批准会增加用户资金的风险。

这种利用再次说明了安全是最重要的。他们没有尽到个人责任，没有通过推迟完成审计来提供最大程度的安全。通过无限批准，用户可以无限制地交换硬币，而不必批准更多的交易。

> 交易新手？试试[加密交易机器人](/coinmonks/crypto-trading-bot-c2ffce8acb2a)或者[复制交易](/coinmonks/top-10-crypto-copy-trading-platforms-for-beginners-d0c37c7d698c)