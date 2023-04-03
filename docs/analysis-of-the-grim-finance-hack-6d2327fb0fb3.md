# 严峻的金融黑客分析

> 原文：<https://medium.com/coinmonks/analysis-of-the-grim-finance-hack-6d2327fb0fb3?source=collection_archive---------30----------------------->

当一个程序的操作在中途停止，重新启动，并且两者都完美地运行，没有执行问题时，重入就发生了。

为了获得 Spirit-LP 证书，攻击者使用快速贷款来借入 WFTM 和 BTC 令牌，并为 Spirit Swap 带来流动性。攻击者调用 GrimBoostVault 契约中的 depositFor()，利用证书作为 Grim Finance 的抵押品。

用户指定的令牌可以在用户在 DepositFor()中指定后，通过 safeTransferFrom()传输到 Grim Boost Vault 中。这将根据合同和转移前后保单池收到的资金之间的差额为用户创建担保品。

但是，攻击者会传递令牌合同地址，因为 depositFor()不会验证用户指定的资金的有效性。

当 GrimBoostVault 通过 safeTransferFom 函数调用恶意协定的 transferFrom 函数时，协定再次使用 depositFor 函数。

合同可以多次重入，并将其用作 SPIRIT-LP 证书中的抵押品。通过这样做，保证了 GrimBoostVault 期望在重新进入之前和之后收到的令牌之间存在差异。

然后，depositFor 函数计算差额，并给予攻击者相应的担保品。

攻击者可以获得更多的抵押品，因为他们已经多次签订了 GrimBoostVault 合同。使他们能够从 GrimBoostVault 合同中提取比以前提供的更多的 SPIRIT-LP 流动性证书。

然后，黑客从 WFTM 和 BTC 代币池中移除流动性，并使用 SPIRIT-LP 流动性证书偿还快速贷款。

GrimBoostVault 契约的 depositFor 函数是这次攻击的源头。它缺乏重入保护，并且无法验证用户传递的令牌的真实性。

因此，智能合约可能会重新进入 depositFor()并获得比预期多得多的抵押品。

SlowMist 安全团队建议监控对函数的外部调用，以发现由外部调用带来的威胁(如重入攻击),并验证用户提交的参数，以确保它们符合预期。

这个漏洞应该在智能契约审计期间被发现，因为针对可重入性漏洞的推荐编码实践和指南已经存在多年了。

在恶意令牌合同形成前一小时左右，攻击者使用 Tornado Cash 为以太坊(ETH)和币安智能链(BSC)钱包提供资金。

为了将和戴从 Fantom Mainnet 转移到 ETH Mainnet，攻击者使用了一个桥。

我们的发现表明，许多 DeFi 应用程序漏洞是由于基本的智能合约开发错误造成的。由于区块链的防篡改特性，这些错误有可能严重损害 DeFi 应用。

尽管许多攻击利用了各种漏洞，但研究工作已经成功地识别和阻止了这些攻击，这正是推动区块链快速发展的原因。

> 交易新手？试试[加密交易机器人](/coinmonks/crypto-trading-bot-c2ffce8acb2a)或者[复制交易](/coinmonks/top-10-crypto-copy-trading-platforms-for-beginners-d0c37c7d698c)