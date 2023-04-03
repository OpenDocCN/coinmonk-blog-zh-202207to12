# Solana DAO 治理(第 1 部分):理解 SPL 治理工作流

> 原文：<https://medium.com/coinmonks/solana-dao-governance-part-1-understanding-spl-governance-3ccf6d6912bc?source=collection_archive---------2----------------------->

分散自治组织(DAOs)管理是一个复杂的话题。SPL 治理计划([源代码](https://github.com/solana-labs/solana-program-library/tree/master/governance))为管理索拉纳区块链上的 Dao 提供了核心构件。在本系列文章中，我们将详细阐述它的实现细节和技术细节。

治理方案地址:[gover 5 lthms 3 blbqwub 97 yvrmmeogzx 7 xnjdxppcvzw](https://explorer.solana.com/address/GovER5Lthms3bLBqWub97yVrMmEogzX7xNjdXpPPCVZw)

(注:还有芒果治理方案:[gqtpl 6 qrf 5 auuqsclh 8 rg 2 htxpuxfhhaxdpttlhp 1 T2 j](https://explorer.solana.com/address/GqTPL6qRf5aUuqscLh8Rg2HTxPUXfhhAXDptTLhp1t2J)