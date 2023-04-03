# 共识协议入门——区块链路线图

> 原文：<https://medium.com/coinmonks/get-started-with-consensus-protocols-blockchain-roadmap-78fde2afed1d?source=collection_archive---------21----------------------->

![](img/0a85babc35adc69dc48725a062ad942d.png)

Photo by [Vadim Bogulov](https://unsplash.com/@franku84) [](https://unsplash.com/@lazycreekimages) on [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

传统上，计算机系统使用服务器-客户机模型。这是一种集中的方法；服务器响应客户端请求。

[](https://www.geeksforgeeks.org/client-server-model/) [## 客户端-服务器模式—极客论坛

### 客户机-服务器模型是一种分布式应用程序结构，它在提供者之间划分任务或工作负载…

www.geeksforgeeks.org](https://www.geeksforgeeks.org/client-server-model/)  [## 共识(计算机科学)——维基百科

### 分布式计算和多智能体系统中的一个基本问题是在分布式环境中实现整个系统的可靠性

en.wikipedia.org](https://en.wikipedia.org/wiki/Consensus_%28computer_science%29) 

P2P(点对点)模型需要一个完全分散的系统模型。每个节点既是服务器又是客户端，每个节点都是对等节点。这种模式不需要中央基础设施，可以利用现有节点进行自我组织。因此，它是规避法律、禁令和审查的一个很好的模式。标志着一个时代的 P2P 音乐共享应用 Napster 和 BitTorrent 占所有互联网流量的 20%以上。多年来，P2P 系统已经消失，Metallica-Napster 诉讼或各种电影制作公司与 torrent 网站的战争限制了 P2P 模式的范围。

[](https://www.geeksforgeeks.org/what-is-p2ppeer-to-peer-process/) [## 什么是 P2P(点对点进程)？— GeeksforGeeks

### 对等网络是一个简单的计算机网络。它最早出现于 20 世纪 70 年代末。这里每个…

www.geeksforgeeks.org](https://www.geeksforgeeks.org/what-is-p2ppeer-to-peer-process/) [](https://www.bittorrent.com/company/about-us/) [## 关于 BitTorrent |世界领先 P2P 协议的创造者

### 总部位于旧金山的 Rainberry，Inc .是最大的分布式 P2P 通信协议背后的公司

www.bittorrent.com](https://www.bittorrent.com/company/about-us/)  [## Napster:来自各个角度的音乐

### 在路上如果你住的地方还没有我们，我们可能很快就会有了。在社交媒体上关注我们，了解…

www.napster.com](https://www.napster.com/availability) [](https://www.kerrang.com/metallica-vs-napster-the-lawsuit-that-redefined-how-we-listen-to-music) [## 金属乐队对 Napster:重新定义我们如何听的诉讼…

### 世纪之交，Metallica 与文件共享巨头 Napster 展开较量，并取得了胜利。在 21 周年之际…

www.kerrang.com](https://www.kerrang.com/metallica-vs-napster-the-lawsuit-that-redefined-how-we-listen-to-music) 

P2P 系统并不是不切实际的，也不是违背中央模式的。然而，这些研究中的思想和技术被用于传统数据中心系统的开发。与比特币一起，一个实用的拜占庭容错(pBFT)共识协议，即 Nakamoto 共识协议，被开发并作为开源共享。

[](https://github.com/bitcoin/bitcoin/blob/master/src/consensus/consensus.h) [## 比特币/共识. h at 大师比特币/比特币

### 此时您不能执行该操作。您已使用另一个标签页或窗口登录。您已在另一个选项卡中注销，或者…

github.com](https://github.com/bitcoin/bitcoin/blob/master/src/consensus/consensus.h) 

一开始比特币用处不大，所以它的问题并没有浮出水面，但是随着使用的增多，它的问题开始出现。此外，众所周知，Nakamoto 共识协议没有为 P2P 电子现金系统提供足够的扩展性。

随着时间的推移，许多建议和新的发展，如比特币 NG，Tendermint，雪崩已被用来解决硬币的原始问题。

[](https://www.usenix.org/conference/nsdi16/technical-sessions/presentation/eyal) [## 比特币-NG:一个可扩展的区块链协议

### USENIX 致力于开放我们活动中展示的研究成果。论文和会议记录可以免费获得…

www.usenix.org](https://www.usenix.org/conference/nsdi16/technical-sessions/presentation/eyal) [](https://tendermint.com/) [## 嫩薄荷

### 为分布式网络构建最强大的工具。

tendermint.com](https://tendermint.com/) [](https://www.avax.network/) [## Avalanche:速度惊人、成本低廉且环保| Dapps 平台

### 以最少的硬件投入扩展到数百万个验证器，或锁定您的 AVAX，以帮助处理交易和…

www.avax.network](https://www.avax.network/) 

为了更好地理解硬币，有必要更好地理解共识协议。尽管有学术论文和文章，也就是说，与已知的相反，诸如工作证明(PoW)或利益证明(PoS)之类的机制不是共识协议。它们只是防止西比尔发作的措施。

[](https://en.wikipedia.org/wiki/Sybil_attack) [## 西比尔攻击—维基百科

### Sybil 攻击是一种对计算机网络服务的攻击，攻击者破坏服务的声誉…

en.wikipedia.org](https://en.wikipedia.org/wiki/Sybil_attack) 

共识是分布式分类账的基本工作惯例。在计算机科学中，共识是用于在分布式进程或系统中就单个数据值或状态达成一致的协议。共识为不需要相互了解或信任的各方之间的交易的自动执行提供了可靠的软件机制。事实上，它规定节点将同意相同的决定，没有节点可以做出不同的决定。
当一个算法满足协议、有效性和终止性的要求时，它就达到了共识；

协议:任何节点都不能有不同的决定。
有效性:如果它的所有建议值都相同，节点应该返回这个值。所有输出值必须是客户端的输入值。
终止:如果系统已经被信任并同步了足够长的时间，所有的客户端应该返回相同的值。

当评估共识协议的性能时，有两个感兴趣的因素。这些是运行时和消息复杂性。运行时间是使用 Big-O 符号作为某个输入参数的函数给出的。其他因素包括内存使用和邮件大小。

[](https://www.bigocheatsheet.com/) [## 了解你的复杂性！

### 你好。这个网页涵盖了计算机科学中常用算法的空间和时间复杂性。当…

www.bigocheatsheet.com](https://www.bigocheatsheet.com/) 

虽然 pBFT(实用拜占庭容错)被称为经典的一致性协议，在 Big-O 表示法中具有 O(n)复杂度，但脸书 Libra 使用的协议是 HotStuff O(n)，最终雪崩协议是 O(kn log n)到 O(kn)。有些复杂。

[](https://www.geeksforgeeks.org/practical-byzantine-fault-tolerancepbft/) [## 实用拜占庭容错(pBFT) — GeeksforGeeks

### 实用拜占庭容错是一种共识算法，由 Barbara Liskov 和 Miguel 在 90 年代末提出…

www.geeksforgeeks.org](https://www.geeksforgeeks.org/practical-byzantine-fault-tolerancepbft/) [](https://hackernoon.com/hotstuff-the-consensus-protocol-behind-safestake-and-facebooks-librabft) [## hot stuff:safe stake 和脸书 LibraBFT | HackerNoon 背后的共识协议

### 区块链技术爱好者的安全，信任最小化的中间层促进了 ETH 2.0 的去中心化…

hackernoon.com](https://hackernoon.com/hotstuff-the-consensus-protocol-behind-safestake-and-facebooks-librabft) [](https://docs.avax.network/overview/getting-started/avalanche-consensus) [## 雪崩区块链共识|雪崩文档

### 共识的任务是让一组计算机就一项决策达成一致。电脑可以到达一个…

docs.avax.network](https://docs.avax.network/overview/getting-started/avalanche-consensus) 

Nakamoto Consensus 是第一个区块链共识协议，pBFT(实用拜占庭容错)，用于比特币，由中本聪发明，用于以太坊、Monero、Zcash 等多种硬币，以及大部分工作证明系统。算法是这样工作的:

一个领导者是由密码难题选出的。

领导者向网络中的所有其他进程推荐事务块以及 PoW。

当建议使用功率最大的块时，这是可以接受的。通常，块号最高的块是功率最大的块。

如果大多数是由诚实的过程控制，诚实的区块链将增长最快，并领先于竞争链。

最长的链条是诚实的链条。

这些规则存在的原因是你永远不能确定一个块是否正确。假设有这样一个街区。如果最高的块是 200，当看到高度为 201 的 A 块时，它被接受。

[](https://ethereum.org/en/) [## 首页| ethereum.org

### 以太坊是一个全球性的、分散的资金和新型应用平台。在以太坊上，你可以写代码…

ethereum.org](https://ethereum.org/en/) [](https://www.getmonero.org/) [## Monero 项目

### 关于 Monero 加入社区社区已经收集了大量的资源和文档。用户可以找到…

www.getmonero.org](https://www.getmonero.org/) [](https://z.cash/) [## 保护隐私的数字货币| Zcash

### Zcash 是一种具有强大隐私功能的数字货币。高效、安全地交易，费用低廉，同时…

z.cash](https://z.cash/) 

如果块 B 的高度为 204，根据共识规则，先前接受的块 A 必须被反转，并且这个新的块历史必须被接受。

 [## 区块链演示

### 浏览器中的实时区块链演示。

andersbrownworth.com](https://andersbrownworth.com/blockchain/block) 

如果拿中本聪共识和帕克斯共识比较；
在 Paxos 中接受最高出价数，而在 Nakamoto Consensus 中接受最高街区高度。
与 Paxos 不同，Nakamoto 以领先优势一致当选。
在 Paxos 中，领导者的多重选择不会引起任何分叉，但在 Nakamoto 中它变成了一个新的链。

[](https://martinfowler.com/articles/patterns-of-distributed-systems/paxos.html) [## 帕克斯

### 使用两个共识建立阶段来达成安全共识，即使当多个节点共享状态时节点断开连接…

martinfowler.com](https://martinfowler.com/articles/patterns-of-distributed-systems/paxos.html) 

这些差异给中本聪共识带来了一些好处。它可以根据经典协议给出保证，特别是在协议、有效性和终止性方面。

在 pBFT 中，参与者需要事先知道，如果没有达成共识，网络将被终止。当在 Paxos 中选出一个以上的领导人时，共识可能不会持续很久。在中本聪共识中，一个战俘领袖总是被选出，以防止网络变得群龙无首。制度上有奖励制度，防止出现没有领导人选的情况。如果出现一个以上的领导者，网络就会分裂，但共识仍然会达成。

在这个系统中，拥有网络总处理能力 50%以上的攻击者可以连续创建最长的链，从而确认所需的事务并创建任何内容的块。
此外，利用中本聪共识，比特币正在努力解决可扩展性问题。比特币的交易速度和交易量受到区块大小和区块时间框架的限制。

增加块大小(1 MB)会增加吞吐量，但由此产生的较大的块需要更长的时间在网络中传播。新的区块到达较晚意味着矿工在不再具有最高区块高度的旧区块上浪费了他们的处理能力。

减少块间隔(10 分钟)将减少延迟，但将导致频繁分叉，这将导致网络不稳定，比特币在这些参数下每秒处理 1 到 3.5 笔交易。终止该过程也是有问题的。等待 6 个块才知道一个事务已经以健康的方式终止是合理的，但是这将需要 60 分钟。

[](https://en.wikipedia.org/wiki/Bitcoin_scalability_problem#:~:text=The%20Bitcoin%20scalability%20problem%20refers,limited%20in%20size%20and%20frequency) [## 比特币可扩展性问题——维基百科

### 比特币可扩展性问题指的是比特币网络处理大量…

en.wikipedia.org](https://en.wikipedia.org/wiki/Bitcoin_scalability_problem#:~:text=The%20Bitcoin%20scalability%20problem%20refers,limited%20in%20size%20and%20frequency) 

比特币-NG (Next Generation)是为解决比特币的缩放问题而提出的重要提案之一。它的结构发展了中本聪共识。在比特币中，矿工负责他们生产的区块中的两个主要数据；正是 PoW 和 miner 批准的事务允许网络确定最长的链。而 Bitcoin-NG 通过创建单独的块类型，将这两个数据分成两个单独的块类型；这些是关键块和微块。

关键区块包含异能、参考前一区块和采矿奖励，但没有交易。与当前比特币-NG 和常规比特币块的设计一样，平均每 10 分钟就发现一个关键块。此外，与常规比特币块一样，用于查找该块的哈希能力决定了最长的链。在 Bitcoin-NG 中创建密钥块的矿工也将自己的公钥添加到该块中。

[](https://hackingdistributed.com/2015/10/14/bitcoin-ng/) [## 比特币-NG:一个安全、更快、更好的区块链

### 比特币取得了意想不到的巨大成功，市值高达数十亿美元，接近 10 亿美元…

hackingdistributed.com](https://hackingdistributed.com/2015/10/14/bitcoin-ng/) 

挖掘器将使用私钥对它将生成的微块进行签名。与密钥块不同，微块不包含任何 PoW，它们只存储事务。这些微块被非常频繁地创建，在比特币-NG 的初始设计中平均每 10 秒钟就有一个。因此，密钥块的挖掘者本质上负责确认网络上的交易，直到找到新的密钥块。作为测试的结果，比特币-NG 能够达到 100 tps。但是，比特币开发者团队并没有考虑到这一点。
中本聪使我们能够在全球范围内生产硬币。这个非常有启发性的协议被记录为世界历史上一个非常重要的实验。

理论上，正如比特币白皮书中解释的那样；任何节点都可以成为领导者。考虑到区块链分布式网络的公共性质，Nakamoto 认为，领导者的选择将在大量独立节点中以相当平均的概率发生。

在实践中，在网络上执行 PoW 哈希计算的机器已经快速地从 CPU 转移到 GPU，从 GPU 转移到 FPGA，从 FPGA 转移到 ASIC。这实现了网络的集中化。结果是；网络上的权利人不可避免地开始从半导体技术先进的国家出现。尽管如此，比特币，使用中本聪共识，仍然是第一个，仍然是最重要的硬币。

[](https://www.xilinx.com/products/silicon-devices/fpga/what-is-an-fpga.html#:~:text=Field%20Programmable%20Gate%20Arrays%20%28FPGAs,or%20functionality%20requirements%20after%20manufacturing.) [## 什么是 FPGA？现场可编程门阵列

### 由于其可编程特性，FPGAs 是许多不同市场的理想选择。作为行业领导者，赛灵思…

www.xilinx.com](https://www.xilinx.com/products/silicon-devices/fpga/what-is-an-fpga.html#:~:text=Field%20Programmable%20Gate%20Arrays%20%28FPGAs,or%20functionality%20requirements%20after%20manufacturing.) [](https://tr.wikipedia.org/wiki/ASIC) [## ASIC — Vikipedi

### Vikipedi，ozgür ansiklopedi ASIC(专用集成电路；uygulamaya zel tümlik Devre)，genel…

tr.wikipedia.org](https://tr.wikipedia.org/wiki/ASIC) 

下一篇文章再见…

> 交易新手？试试[加密交易机器人](/coinmonks/crypto-trading-bot-c2ffce8acb2a)或者[复制交易](/coinmonks/top-10-crypto-copy-trading-platforms-for-beginners-d0c37c7d698c)