# 根据智能合同持有和转让 ERC-20

> 原文：<https://medium.com/coinmonks/hold-and-transfer-erc-20-on-a-smart-contract-1a640ca32e0d?source=collection_archive---------11----------------------->

![](img/f1d5e6aa11bc1af1013893cfe5f632b8.png)

[https://www.inventiva.co.in/wp-content/uploads/2022/08/What-is-an-ERC20-token-780x470.jpg](https://www.inventiva.co.in/wp-content/uploads/2022/08/What-is-an-ERC20-token-780x470.jpg)

Solidity 智能合约也可以持有 ERC-20 代币，就像它持有 ETH/MATIC/BNB 等本地硬币一样。持有 ERC-20 的合同有多种目的。它可以用于赌注，锁定和释放代币，银行等。

> 交易新手？在[最佳密码交易所](/coinmonks/crypto-exchange-dd2f9d6f3769)上尝试[密码交易机器人](/coinmonks/crypto-trading-bot-c2ffce8acb2a)或[复制交易](/coinmonks/top-10-crypto-copy-trading-platforms-for-beginners-d0c37c7d698c)

让我们看看如何使我们的合同持有 ERC-20 令牌，并把它们转移到一个地址。

# 如何创建持有令牌的合同？

让我们创建一个实现 ERC20 接口的简单契约。

我们已经创建了一个简单的合同，可以持有任何 ERC-20 令牌，并允许您在需要时随时撤回令牌。

*注意:此代码仅用于演示目的，不应在生产中使用。考虑攻击智能合约*[*C*hapter 1](/coinsbench/attacks-on-smart-contract-chapter-1-9b44e7e44150)*和* [*第二章*](/coinmonks/attacks-on-smart-contract-chapter-2-f26b356d6aee) *为了更好更安全的智能合约开发。*

让我们一起进入 Web3🏊

# 让我们连接起来

在[媒体](/@nufailismath15)、 [LinkedIn](https://www.linkedin.com/in/nufail-i-61377b10b/) 、 [Twitter](https://twitter.com/ismath_nufail) 、 [Instagram](https://www.instagram.com/nufail_ismath/) 上关注我

> 加入 Coinmonks [电报频道](https://t.me/coincodecap)和 [Youtube 频道](https://www.youtube.com/c/coinmonks/videos)了解加密交易和投资

# 另外，阅读

*   [OKEx vs KuCoin](https://coincodecap.com/okex-kucoin) | [摄氏替代品](https://coincodecap.com/celsius-alternatives) | [如何购买 VeChain](https://coincodecap.com/buy-vechain)
*   [ProfitFarmers 回顾](https://coincodecap.com/profitfarmers-review) | [如何使用 Cornix Trading Bot](https://coincodecap.com/cornix-trading-bot)
*   [如何匿名购买比特币](https://coincodecap.com/buy-bitcoin-anonymously) | [比特币现金钱包](https://coincodecap.com/bitcoin-cash-wallets)
*   [瓦济里克斯 NFT 评论](https://coincodecap.com/wazirx-nft-review)|[Bitsgap vs Pionex](https://coincodecap.com/bitsgap-vs-pionex)|[Tangem 评论](https://coincodecap.com/tangem-wallet-review)
*   [如何使用 Solidity 在以太坊上创建 DApp？](https://coincodecap.com/create-a-dapp-on-ethereum-using-solidity)