# 什么是皮媞亚，它如何验证策略证明

> 原文：<https://medium.com/coinmonks/what-is-the-pythia-and-how-it-validates-proof-of-strategy-10c09cc71787?source=collection_archive---------49----------------------->

![](img/5f2d726237d671e71f1d40abdf853365.png)

之前的一篇文章描述了对[战略家角色](https://chainsatlas.medium.com/why-is-the-strategist-fundamental-to-the-chainsatlas-network-5309b56d6a57)的高度概括。这篇文章将涵盖 Pythians(皮媞亚集体)及其与[策略证明](https://chainsatlas.medium.com/proof-of-strategy-posg-x-proof-of-work-pow-which-is-more-useful-15b765ccb2b9)共识协议的关系。这里总结了全面理解皮媞亚角色所需的网络核心组件。

**雅典娜**:赫尔墨斯(效用令牌)中给予投票权和赌注奖励的治理令牌。

**Hermes:**Athena 的标桩生成的效用令牌，用于支付网络上智能合约的执行。

**客户:**希望在一组偏好和条件下执行软件的用户，以换取一定数量的爱马仕。

**战略家:**为管理单元提供与客户设置相匹配的执行策略，同时最小化处理成本的实体，以换取 Hermes。

**管理单元:**管理 Hermes 和 Athena 平衡、验证最佳策略共识、监督选举和控制虚拟化单元的智能合同。

**虚拟化单元:**处理客户端字节码执行的智能契约。他们被部署在不同的区块链。

**策略证明:**一种新颖、有用的工作证明，其中战略家(相当于矿工的策略证明)通过提供在一组约束条件下执行客户软件的最佳策略来获得 Hermes。

**链配置文件:**以标准化方式定义网络中每个支持的区块链的一组特征。

**Agora DAO:** 一个分散的自治组织，根据其成员的投票来管理 ChainsAtlas 生态系统。每一个雅典娜的支持者，例如皮媞亚，都是道集会的成员并有投票权。

**OLYMPUS treasury:** 一个奖励池，收集客户支付的部分爱马仕，在 28 天周期结束时在 Pythians 和 grant fund 之间分配。每个皮媞亚的可靠性分数决定了它在奥林巴斯金库中的份额。赠款基金的使用由 Agora DAO 决定，所有毕达哥拉斯人都有投票权。

# 什么是皮媞亚？

Pythians 是由战略家操作的甲骨文节点。它们保证管理单元之间的状态同步，对选举进行投票，验证策略，并通过标记 Athena 来铸造 Hermes 实用令牌。战略家必须下注至少 100 个雅典娜令牌来运行皮媞亚节点。

# 策略的验证是如何发生的？

在每次执行策略之前，都会选择一组 Pythians 来验证它。三个标准规定了这些皮提亚人的选择:

*   权重为 40%的股份
*   权重为 20%的随机选择
*   权重为 40%的验证可靠性分数

管理单位通过接受或忽略 Pythians 对所提供策略的评估来设置验证可靠性分数。每个皮媞亚的初始得分是 0。当皮媞亚对战略的评估(积极或消极)与至少 51%的所选皮蒂安对同一战略的评估相同时，管理单位更新皮媞亚分数。

# 皮提亚人投票的结果。

毕达哥拉斯人有权在集市上投票。以及批准或拒绝连锁店的简档更新。因此，皮蒂安人有权选择支持哪家区块链连锁地图公司，以及他们在入选属性列表中的得分，包括但不限于去中心化得分和隐私得分。

为了奖励这样一个重要的角色，奥林巴斯财政部设立了一项特别基金，专门用于奖励皮提亚人在 ChainsAtlas 网络上的交易费分成。每个皮媞亚的可靠性得分和赌注决定了它在每个 28 天周期结束时从皮蒂安基金获得的爱马仕金额。

如果你到现在还在读这篇文章，一定要注册 Athena airdrop，成为网络的第一批战略家之一，帮助 Web3 成为软件应用的新范例！

[点击此处](https://airtable.com/shrTkaeWSOyUTsezc)成为早期链锯的战略家之一，参与雅典娜空投。

如需持续更新，请关注我们:

*   [推特](https://www.twitter.com/chainsatlas?source=about_page-------------------------------------)
*   [不和](https://discord.com/invite/h8Rwh7BBQs?source=about_page-------------------------------------)
*   [社区和发展更新简讯](https://airtable.com/shrx8ZNfAosVhEABK)
*   [白皮书](https://raw.githubusercontent.com/ChainsAtlas/whitepaper/main/whitepaper.pdf?source=about_page-------------------------------------)
*   [网站](https://chainsatlas.com/?source=about_page-------------------------------------)

> 交易新手？试试[加密交易机器人](/coinmonks/crypto-trading-bot-c2ffce8acb2a)或者[复制交易](/coinmonks/top-10-crypto-copy-trading-platforms-for-beginners-d0c37c7d698c)