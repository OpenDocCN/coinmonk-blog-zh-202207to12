# Solana 智能合约的安全性:SPL 联合代币账户的两个警告

> 原文：<https://medium.com/coinmonks/security-of-solana-smart-contracts-two-caveats-of-the-spl-associated-token-account-65f7d6a8facb?source=collection_archive---------10----------------------->

SPL 关联令牌程序在索拉纳智能合约中频繁使用。我们在[的前一篇文章](/coinmonks/solana-programs-part-2-understanding-spl-associated-token-account-25082b9b5471)中回顾了它的技术细节。在本文中，我们重点关注 Sec3 核心团队了解到的使用关联令牌帐户的两个重要注意事项。

1.  *小心将任何关联的代币账户设置为代币账户的所有者*
2.  *小心验证任何由所有者和铸币者输入的相关令牌账户*

## 1.不要设置…