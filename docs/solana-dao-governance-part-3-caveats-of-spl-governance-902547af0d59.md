# 索拉纳道治理(三)——SPL 治理的警示

> 原文：<https://medium.com/coinmonks/solana-dao-governance-part-3-caveats-of-spl-governance-902547af0d59?source=collection_archive---------11----------------------->

在前两篇文章([第一部分](/coinmonks/solana-dao-governance-part-1-understanding-spl-governance-3ccf6d6912bc)和[第二部分](/coinmonks/solana-dao-governance-part-2-understanding-spl-governance-be11b5777a51))中，我们重点介绍了 SPL 治理的基本工作流程及其在芒果道中的使用。本文将关注安全性方面，包括一些重要的注意事项。

以下是总结:

*   警惕**闪贷治理攻击**—攻击者可能获得足够的投票权来创建和通过恶意的治理提案
*   治理计划可以自行升级
*   如果一个程序受 DAO 控制，那么它就不能…