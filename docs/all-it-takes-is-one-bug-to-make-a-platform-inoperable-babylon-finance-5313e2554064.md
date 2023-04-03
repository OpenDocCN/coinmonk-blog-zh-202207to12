# 只要一个错误就能让一个平台无法运行——巴比伦金融

> 原文：<https://medium.com/coinmonks/all-it-takes-is-one-bug-to-make-a-platform-inoperable-babylon-finance-5313e2554064?source=collection_archive---------46----------------------->

随着 2016 年以太坊的 DAO，可重入攻击开始了它的突出旅程，在最初的几个小时内窃取了 360 万 ETH。从那以后，一切都没有改变。可重入性和稳固性一样古老。

在 2021 年的几次再入攻击之后，2022 年已经在 DeFi 市场上看到了多次再入攻击——黑客们明白，从 TVL 每天都在扩展的众多项目中可以获得很多好处。

名单相当长！

让我们来关注一下最新的加密资产管理公司 Babylon Finance 的垮台，该公司由于重入攻击而无法从 Rari 黑客攻击中恢复。Rari/FEI 漏洞让 Babylon Finance Investment Gardens 损失了 340 万美元，这促使客户失去信心，收回了 75%的 TVL。

根据该公司创始人 Ramon Recuero 在 8 月 31 日发表的声明，Babylon Finance 将于 11 月 15 日停止为 DeFi 资产管理协议提供服务。

**重入攻击:检查 FEI 协议事件**

2022 年 4 月，Fei 协议成为重入攻击的受害者。攻击者从易受攻击的合同中提取了大约 8000 万美元的代币。通过使用从 Compound 派生的代码，Fei 协议将自己置于危险之中。CEther 代码中的多个可重入性漏洞在以前的版本中已被修补，但一些易受攻击的函数未被检测到。

契约的内部状态在外部交互发生之前被更新以反映该外部交互。这可以防止可重入性，因为外部交互可能会在从初始调用更新状态之前重入易受攻击的契约。

当智能协定向地址发送值而不遵循检查-效果-交互模式时，它会产生可重入性漏洞。简而言之，可重入攻击发生在两个智能契约相互通信时，其中一个利用另一个的代码(易受攻击的契约)从另一个获取资金。

在易受攻击的智能合约有机会更新余额之前，攻击性智能合约反复调用撤销函数。发起攻击的智能合约可以在转移资金和更新余额之间创建的窗口期间进行另一次调用以提取其现金，并且重复该循环，直到所有资金耗尽。这就是漏洞利用继续运行的方式。

攻击者在 Fei 协议的契约中使用了“exitMarket”和“borrow”函数。如果 exitMarket 功能证明存款不再被用作贷款抵押品，则可以提取存款。借用函数不遵循检查-效果-交互模式，因此容易受到攻击。它使用户能够使用存款资产作为抵押获得贷款。

通过使用智能协定地址来调用 borrow，攻击者就利用了这个漏洞。

借入函数尚未更新其内部状态，以反映当它将借出的金额传送给借入方时，存放的资产目前正被用作抵押品。

fallback 函数执行 exitMarket 函数，并删除用作贷款抵押品的存款，当贷款传输到获取贷款的智能合同时，调用该函数。

由于这个可重入性漏洞，攻击者可以获得贷款，然后提取作为贷款抵押品的存款资产。

利用各种池，攻击者能够从 Fei 协议智能合约中提取价值 8000 万美元的令牌。去年 12 月与 Rari 合并的 Fei Protocol 提出向黑客提供 1000 万美元作为“赏金”，以换取返还剩余资金。

在巅峰时期，Babylon 持有 3000 万美元不同的加密货币，是 Rari 上最大的贷款池之一，拥有 1000 万美元的用户提供的资产。在 Rari 漏洞期间，Babylon 损失了 340 万美元，其用户当时撤回了超过 75%的资产。

**结束语**

尽管这是一个广为人知的老问题，可重入性漏洞仍然出现在今天的新智能契约中。直接的解决方案是查看所有进行外部调用的函数，并根据它们的基本原理决定它们是否安全。当涉及到保护和维护智能契约时，理解可重入攻击是一个很好的起点。

> *加入 Coinmonks* [*电报频道*](https://t.me/coincodecap) *和* [*Youtube 频道*](https://www.youtube.com/c/coinmonks/videos) *了解加密交易和投资*

# 另外，阅读

*   [3 商业评论](/coinmonks/3commas-review-an-excellent-crypto-trading-bot-2020-1313a58bec92) | [Pionex 评论](https://coincodecap.com/pionex-review-exchange-with-crypto-trading-bot) | [Coinrule 评论](/coinmonks/coinrule-review-2021-a-beginner-friendly-crypto-trading-bot-daf0504848ba)
*   [莱杰 vs n rave](/coinmonks/ledger-vs-ngrave-zero-7e40f0c1d694)|[莱杰 nano s vs x](/coinmonks/ledger-nano-s-vs-x-battery-hardware-price-storage-59a6663fe3b0) | [币安评论](/coinmonks/binance-review-ee10d3bf3b6e)
*   [加密交易机器人](/coinmonks/crypto-trading-bot-c2ffce8acb2a) | [Bingbon 审查](https://coincodecap.com/bingbon-review)
*   [Bybit Exchange 评论](/coinmonks/bybit-exchange-review-dbd570019b71) | [Bityard 评论](https://coincodecap.com/bityard-reivew) | [Jet-Bot 评论](https://coincodecap.com/jet-bot-review)
*   [3 commas vs crypto hopper](/coinmonks/3commas-vs-pionex-vs-cryptohopper-best-crypto-bot-6a98d2baa203)|[赚取加密利息](/coinmonks/earn-crypto-interest-b10b810fdda3)