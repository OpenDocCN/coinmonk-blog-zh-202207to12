# 了解拓扑数据分析(图论)

> 原文：<https://medium.com/coinmonks/understanding-topological-data-analysis-graph-theory-245c332fa8c6?source=collection_archive---------20----------------------->

![](img/1b64e0af67393097fd0a8b7e2c8391a1.png)

Photo by [John Anvik](https://unsplash.com/@redviking509?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/thread?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

1.  **DB-LSH:具有基于查询的动态桶的位置敏感散列(**[**arXiv**](https://arxiv.org/pdf/2207.07823)**)**

**作者:**[周晓芳](https://arxiv.org/search/?searchtype=author&query=Zhou%2C+X)

**摘要:**在高维近似最近邻(ANN)搜索问题的众多解决方案中，位置敏感哈希(LSH)以其亚线性的查询时间和对查询精度的鲁棒理论保证而闻名。传统的 LSH 方法可以从哈希表中快速生成少量的候选项，但会遇到大的索引大小和哈希边界问题。最近解决这些问题的研究通常会产生额外的开销来识别合格的候选对象或消除误报，从而使查询时间不再是次线性的。为了解决这一难题，本文提出了一种新的 LSH 方案，称为 DB-LSH，它支持对大型高维数据集进行有效的人工神经网络搜索。它使用多维索引组织投影空间，而不是使用固定宽度的哈希桶。我们的方法可以通过避免为不同的桶大小维护许多散列表来显著降低空间成本。在 DB-LSH 的查询阶段，通过基于索引的窗口查询动态构建所需宽度的基于查询的超立方桶，可以高效地生成少量高质量的候选。对于近似比为 c 的 n 维数据集，我们的严格理论分析表明，DB-LSH 实现了更小的查询代价 O(nρ∫dlogn)，其中ρ∫的界为 1/cα，而现有工作中的界为 1/c。在真实世界数据上的大量实验证明了 DB-LSH 在效率和准确性上优于最先进的方法

**2。BCD:使用位置敏感散列算法(**[**arXiv**](https://arxiv.org/pdf/2112.05492)**)**的跨架构二进制比较数据库实验

**作者:** [郝希坦](https://arxiv.org/search/?searchtype=author&query=Tan%2C+H)

**摘要:**给定一个没有源代码的二进制可执行文件，很难通过逆向工程来确定二进制文件中的每个函数是做什么的，如果没有之前的经验和上下文就更难了。在本文中，我们比较了不同哈希函数在检测 LLVM IR 代码的相似提升片段方面的有效性，并介绍了跨架构二进制代码相似性搜索数据库框架的设计和实现，该框架使用 MinHash 作为选择的哈希算法，基于 SimHash、SSDEEP 和 TLSH。其动机是帮助逆向工程师通过与已知函数的数据库进行比较，快速获得未知二进制文件中函数的上下文。这个项目的代码是开源的，可以在 https://github.com/h4sh5/bcddb[找到](https://github.com/h4sh5/bcddb)

**3。面向在线稀疏大数据分析的局部敏感哈希聚合非线性邻域矩阵分解(**[**arXiv**](https://arxiv.org/pdf/2111.11682)**)**

**作者:** [李](https://arxiv.org/search/?searchtype=author&query=Li%2C+Z)，[郝莉](https://arxiv.org/search/?searchtype=author&query=Li%2C+H)，[李垦利](https://arxiv.org/search/?searchtype=author&query=Li%2C+K)，[吴梵](https://arxiv.org/search/?searchtype=author&query=Wu%2C+F)，[陈迪雅](https://arxiv.org/search/?searchtype=author&query=Chen%2C+L)，[李克勤](https://arxiv.org/search/?searchtype=author&query=Li%2C+K)

**摘要:**矩阵分解(MF)可以从高维数据中提取低秩特征，整合数据流形分布的信息，可以考虑非线性的邻域信息。因此，MF 在稀疏大数据的低秩分析方面引起了广泛关注，例如协同过滤(CF)推荐系统、社交网络和服务质量。然而，存在以下两个问题:1)用于构建图相似矩阵(GSM)的巨大计算开销，以及 2)用于中间 GSM 的巨大存储开销。因此，基于 GSM 的 MF，例如，核 MF、图正则化 MF 等。，不能直接应用于云和边缘平台上稀疏大数据的低秩分析。为了解决稀疏大数据分析中的这个棘手问题，我们提出了位置敏感哈希(LSH)聚合 MF (LSH-MF)，它可以解决以下问题:1)LSH-MF 提出的概率投影策略可以避免 GSM 的构建。此外，LSH-MF 可以满足稀疏大数据精确投影的要求。2)为了在 GPU 上运行 LSH-MF 进行细粒度并行化和在线学习，我们还提出了适用于 CUDA 并行化的 CULSH-MF。实验结果表明，CULSH-MF 不仅减少了计算时间和内存开销，而且获得了更高的准确率。与深度学习模型相比，CULSH-MF 不仅可以节省训练时间，而且可以达到相同的准确率。

**4。经由位置敏感散列的次线性最小二乘值迭代(**[**arXiv**](https://arxiv.org/pdf/2105.08285)**)**

**作者:** [安舒马利](https://arxiv.org/search/?searchtype=author&query=Shrivastava%2C+A)、[、](https://arxiv.org/search/?searchtype=author&query=Song%2C+Z)、[徐](https://arxiv.org/search/?searchtype=author&query=Xu%2C+Z)

**摘要:**我们提出了第一个可证明的最小二乘值迭代(LSVI)算法，其运行时间复杂度与动作数量呈次线性关系。我们将值迭代中的值函数估计过程公式化为一个近似的最大内积搜索问题，并提出了一个位置敏感散列(LSH)[因迪克和莫特瓦尼·STOC ' 98，安多尼和拉森斯泰恩·STOC ' 15，安多尼，拉阿霍文，拉森斯泰恩和温加藤·索达' 17]类型的数据结构来解决这个具有次线性时间复杂度的问题。此外，我们在近似最大内积搜索理论和强化学习的后悔分析之间建立了联系。我们证明，通过选择近似因子，我们的次线性 LSVI 算法保持了与原始 LSVI 算法相同的遗憾，同时将运行时间复杂度降低到次线性。据我们所知，这是第一个将 LSH 和强化学习相结合的工作，带来了可证明的改进。我们希望我们结合数据结构和迭代算法的新方法将为进一步研究优化中的成本降低打开大门。

**5。对于流形学习，深度神经网络可以是位置敏感的哈希函数(**[**【arXiv】**](https://arxiv.org/pdf/2103.06875)**)**

**作者:**

**摘要:**众所周知，训练深度神经网络给出了捕捉输入的基本特征的有用表示。然而，这些表述在理论和实践中却鲜为人知。在监督学习的背景下，一个重要的问题是这些表示是否捕捉到用于分类的信息特征，同时过滤掉非信息的噪声特征。我们通过考虑一个生成过程来探索这个问题的形式化，其中每个类都与一个高维流形相关联，并且不同的类定义不同的流形。在该模型下，每个输入使用两个潜在向量产生:(I)一个“流形标识符”γ和；(ii)~沿流形表面移动示例的“变换参数”θ。例如，γ可以代表狗的标准图像，而θ可以代表姿势、背景或光照的变化。我们提供的理论和经验证据表明，神经表征可以被视为类 LSH 函数，它将每个输入映射到一个嵌入，该嵌入仅是信息γ的函数，并且对θ不变，从而有效地恢复流形标识符γ。这种行为的一个重要后果是一次性学会看不见的类。

> 交易新手？试试[密码交易机器人](/coinmonks/crypto-trading-bot-c2ffce8acb2a)或者[复制交易](/coinmonks/top-10-crypto-copy-trading-platforms-for-beginners-d0c37c7d698c)