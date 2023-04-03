# 理解细胞神经网络

> 原文：<https://medium.com/coinmonks/understanding-cellular-neural-networks-5517aff52a65?source=collection_archive---------45----------------------->

![](img/0cad70b299fb49358ff2e0f98d52d9a5.png)

Photo by [Nastya Dulhiier](https://unsplash.com/@dulhiier?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/network?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

1.  **用于生成三维几何图形的遗传细胞神经网络(**[**【arXiv】**](https://arxiv.org/pdf/1603.08551)**)**

**作者:** [雨果·马泰](https://arxiv.org/search/?searchtype=author&query=Martay%2C+H)

**摘要:**有许多方法可以程序化地生成有趣的三维形状，本文介绍了一种将细胞神经网络与网格生长算法相结合的方法。目的是从遗传密码中创建一个形状，这样粗略的搜索就可以找到有趣的形状。相同的神经网络被放置在网格的每个顶点，该网格可以与相邻顶点上的神经网络通信。神经网络的输出决定了网格如何生长，允许模仿生物有机体发展的一些复杂性，紧急产生有趣的形状。由于神经网络的参数可以自由变异，这种方法适用于遗传算法。

**2。使用光流绘制细胞神经网络中钙信号的时空动力学(**[**arXiv**](https://arxiv.org/pdf/0912.0265)**)**

**作者:** [马里乌斯·布巴斯](https://arxiv.org/search/?searchtype=author&query=Buibas%2C+M)，[戴安娜·于](https://arxiv.org/search/?searchtype=author&query=Yu%2C+D)，[郑秀晶·尼扎](https://arxiv.org/search/?searchtype=author&query=Nizar%2C+K)，[加布里埃尔·席尔瓦](https://arxiv.org/search/?searchtype=author&query=Silva%2C+G+A)

**摘要:**应用光流梯度算法对培养物中自发形成的神经元和胶质细胞网络进行荧光光学显微成像，以单像素分辨率绘制功能性钙信号。光流估计所记录的数字图像序列(即电影)中连续帧之间的图像中物体的运动方向和速度。通过该算法计算的矢量场输出能够跟踪钙信号模式的时空动态。我们首先简要回顾光流算法的数学，然后描述如何求解位移向量以及如何测量它们的可靠性。然后，我们将计算的流量向量与人工估计的向量进行比较，以确定从代表性星形胶质细胞培养物中记录的钙信号的进展。最后，我们将该算法应用于原代星形胶质细胞和海马神经元的制备以及 rMC-1 Muller 神经胶质细胞系，以说明该算法捕获不同类型的时空钙活动的能力。我们讨论了可靠测量的成像要求、参数选择和阈值选择，并对矢量数据的使用提出了展望

**3。求解 NP 难优化问题的细胞神经网络(**[**arXiv**](https://arxiv.org/pdf/0802.1150)**)**

**作者:** [玛丽亚·埃尔塞-拉瓦兹](https://arxiv.org/search/?searchtype=author&query=Ercsey-Ravasz%2C+M)，[塔玛斯·罗斯卡](https://arxiv.org/search/?searchtype=author&query=Roska%2C+T)，[佐尔坦·内达](https://arxiv.org/search/?searchtype=author&query=N%C3%A9da%2C+Z)

**摘要:**目前，细胞神经网络(CNN)已在模拟计算机上并行实现，并呈现出快速发展的趋势。物理学家必须意识到，这样的计算机适合于以优雅的方式解决实际上重要的问题，而这些问题在经典的数字体系结构上是极其缓慢的。在这里，CNN 被用来解决网格上的 NP 难优化问题。证明了所有细胞的参数可以分别控制的细胞神经网络是二维 Ising 型(Edwards-Anderson)自旋玻璃系统的模拟对应。利用 CNN 计算机的特性，可以为这类问题建立一种快速优化方法。估计在基于 CNN 的计算机上解决这种 NP-hard 优化问题所需的模拟时间，并将其与使用模拟退火算法的普通数字计算机上所需的时间进行比较，结果是惊人的:CNN 计算机将比已经具有 10*10 晶格大小的数字计算机更快。现在实现的硬件尺寸是 176*144。此外，使 CNN 芯片适应这些问题似乎没有技术困难，所需的本地控制有望在不久的将来完全开发出来。

**4.p-adic 细胞神经网络(**[**arXiv**](https://arxiv.org/pdf/2107.07980)**)**

**作者:** [B. A .詹布拉诺-卢娜](https://arxiv.org/search/?searchtype=author&query=Zambrano-Luna%2C+B+A)， [W. A .苏尼加-加林多](https://arxiv.org/search/?searchtype=author&query=Z%C3%BA%C3%B1iga-Galindo%2C+W+A)

**摘要:**本文介绍 p-adic 细胞神经网络，它是 Chua 和 Yang 提出的经典细胞神经网络的数学推广。新的网络有无限多的单元，这些单元以有根的树的形式分层组织，并且它们还有无限多的隐藏层。直观地说，p-adic CNNs 是大型分层离散 CNN 的极限。更准确地说，新网络可以很好地用分层离散 CNN 来近似。从数学上讲，每个新网络都由一个依赖于几个 p-adic 空间变量和时间的积分微分方程来建模。我们研究了与这些积分微分方程相关的柯西问题，并提供了求解它们的数值方法。

> 交易新手？试试[加密交易机器人](/coinmonks/crypto-trading-bot-c2ffce8acb2a)或者[复制交易](/coinmonks/top-10-crypto-copy-trading-platforms-for-beginners-d0c37c7d698c)