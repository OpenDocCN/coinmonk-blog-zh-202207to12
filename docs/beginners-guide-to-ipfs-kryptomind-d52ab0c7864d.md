# IPFS 入门指南——氪星思维

> 原文：<https://medium.com/coinmonks/beginners-guide-to-ipfs-kryptomind-d52ab0c7864d?source=collection_archive---------33----------------------->

![](img/dd876f53444c5c10c68699c5e11ee8a4.png)

让我们从 IPFS 的基本描述开始:

IPFS 是一个分布式系统，允许您存储和访问文件、网页、应用程序和数据。

这到底是什么意思？假设你正在进行一项关于多叶海龙的研究。你知道它们在南澳大利亚被列为受保护物种，而且没有已知的食肉动物吗？首先，请访问 seadragon 的维基百科页面:

[https://en.wikipedia.org/wiki/Leafyseadragon.](https://en.wikipedia.org/wiki/Leafyseadragon.)

当你在浏览器的地址栏中输入这个 URL 时，你的电脑会从维基百科的一台电脑上请求 Leafy seadragon 页面，这台电脑可能在这个国家的另一端(或者可能在世界的另一端)。

> 交易新手？试试[密码交易机器人](/coinmonks/crypto-trading-bot-c2ffce8acb2a)或者[复制交易](/coinmonks/top-10-crypto-copy-trading-platforms-for-beginners-d0c37c7d698c)

然而，这并不是你满足多叶海龙需求的唯一选择！你可以使用维基百科的 IPFS 镜像。如果您使用 IPFS，您的计算机将请求 Leafy seadragon 网站，如下所示:

```
/ipfs/QmXoypizjW3WknFiJnKLwHCnL72vedxjQkDDP1mXWo6uco/wiki/Leafy seadragon.html
```

IPFS 能够发现美味的多叶海龙信息是基于它的内容，而不是它的位置。多叶海龙信息的 IPFS 化版本由 URL 中间的数字串(QmXo…)表示，你的计算机使用 IPFS 请求世界各地的大量计算机与你共享页面，而不是向维基百科的计算机系统之一请求页面。它可以从任何人那里获取多叶海龙的信息，不仅仅是维基百科。

当你利用 IPFS 时，你不仅仅是从其他地方得到东西；您的计算机也有助于它们的分发。当你的邻居或任何使用 IPFS 的人想要和你一样的维基百科文章时，他们可能会像从你的邻居或任何使用 IPFS 的人那里获得一样从你那里获得。

IPFS 为计算机可能存储的任何类型的文件都启用了这一功能，例如电子邮件、文档，甚至是数据库记录。

有各种各样的 IPFS 发行版和安装方法。然而，本文将使用跨平台的命令行界面程序。命令接口有大量文档记录，内容广泛。不要被冗长的指令吓倒；你只需要几个简单的就可以开始学习 IPFS 了。

您必须先生成对等 ID，然后才能使用它。打开终端并键入以下命令:

稍后将更详细地描述该命令及其结果的意义；现在，只需进入下一步。

让我们看看是否可以张贴这个 PNG 图像。一旦你将它保存为**ipfs-logo.png**，在存储它的目录中启动一个终端并输入以下命令:

```
ipfs add ipfs-logo.png
```

结果将如下所示:

```
added QmbYq2pMi91Xd5Hu6Z1edrvP4BwJXCH9HhRX8Tk99DJWG6 ipfs-logo.png
```

multihash 是一个以 Qm 开头的很长的字符串…它是从文件内容中派生出来的独一无二的标识符。无论文件何时重新发布以及重新发布的频率如何，对于该特定文件来说，它总是相同的。

取回文件同样简单:

```
ipfs get QmbYq2pMi91Xd5Hu6Z1edrvP4BwJXCH9HhRX8Tk99DJWG6 --output out.png
```

**——输出**参数允许您提供下载文件的名称。

您甚至可以同时发布包含文件和嵌套目录的整个目录。为此，将 **-r** (简称 **—递归**)参数添加到 **add** 命令中。将徽标文件放在**徽标**目录中，然后运行:

```
ipfs add -r logo
```

结果将如下所示:

```
added QmbYq2pMi91Xd5Hu6Z1edrvP4BwJXCH9HhRX8Tk99DJWG6 logo/ipfs-logo.png added QmU1muwAeYjHX1kUnYEXPWEhnFxcVGS6wv8tggoHLHkm3f logo
```

目录的标识出现在底部一行。可以使用 **ipfs get** 来访问它，就像访问单个文件一样。

目录中的每个文件可以通过其到父目录的相对路径来标识。然而，每个文件都被赋予了唯一的标识。在没有任何目录上下文的情况下，特定文件可以通过其 multihash 进行访问。在我们的示例中，以下命令应该返回同一个文件，而无需获取整个目录:

```
ipfs get QmU1muwAeYjHX1kUnYEXPWEhnFxcVGS6wv8tggoHLHkm3f/ipfs-logo.png ipfs get QmbYq2pMi91Xd5Hu6Z1edrvP4BwJXCH9HhRX8Tk99DJWG6 --output ipfs-logo2.png
```

您刚刚将一些文件发布到您的本地 IPFS 存储并检索了它们。但是，您的文件还不能被所有人访问。为此，您必须首先启动一个 IPFS 节点:

```
ipfs daemon
```

该节点既充当服务器又充当客户端。它将链接到其他几个节点，并分享关于现有材料的信息。您可以通过输入以下内容来查看哪些节点与您相关:

```
ipfs swarm peers
```

结果将包括如下几行(确切的地址和哈希可能会有所不同):

```
/ip4/99.7.131.248/tcp/4001/ipfs/Qmf3KKqHdL1fUDiguRsTojXBBrqR94yx4EUd8EesXogcSs /ip6/2604:a880:cad:d0::17:2001/tcp/4001/ipfs/QmUR8d2WLbNcAFRMWn3SMdBRDhJugZUezfwLkDYti3Gc3w
```

每行代表一个 IPFS 节点的多地址。它包括 IP 网络位置(地址和端口)和独特的对等标识。节点的地址可能会改变(例如，当您的笔记本电脑从一个地方到另一个地方到咖啡馆时)，但对等 ID 保持不变。

没有一个节点能够存储曾经发布的所有数据。这表明您的节点可能会选择丢弃一些数据。这也意味着你不能完全依赖你的同行:如果没有人对保留你的数据感兴趣，它可能会从网络上消失。

您可以固定数据对象的标识以防止其消失。这确保了如果您的本地节点决定清理一些空间，数据不会被擦除。

因为您上传的文件是自动固定的，所以让我们固定一些您还没有的文件:

```
ipfs pin add /ipfs/QmNhFJjGcMPqpuYfxL62VVB9528NXqDNMFXiqN5bgFYiZ1/its-time-for-the-permanent-web.html
```

结果应该如下所示:

```
pinned Qmcx3KZXdANNsYfSRU1Vu4pchM8mvYXH4N8Zwdpux57YNL recursively
```

此过程还会将数据下载到您的计算机上，以确保数据不会消失。但是，现在恢复它应该轻而易举:

```
ipfs get Qmcx3KZXdANNsYfSRU1Vu4pchM8mvYXH4N8Zwdpux57YNL -o article.html
```

尽管文件大小只有 26 kB，但最后一个操作可能花了一些时间。发生这种情况是因为在保存数据之前必须首先对其进行识别。

所请求的数据块可以存储在世界范围内的任何 IPFS 节点上。因为您的本地节点不太可能保持与所有其他服务器的直接链接，也不太可能记录在其他地方添加的每个块，所以定位正确的节点可能需要一些时间。

关于哪个节点持有哪个块的信息被安排在分布式哈希表中，该哈希表以与数据相同的方式在节点之间传播。当搜索由特定散列指定的数据时，您的节点必须首先发现具有该块的节点。该节点请求它的几个直接对等节点，所以如果其中一个恰好有问题的块，搜索就结束了。如果一个对等点没有保存数据，它会对它的对等点进行相同的查询，直到发现数据块的保存者。

网络的节点被配置成使得该过程具有最小的开销，并且整个网络可以在几分钟内被遍历。然而，在最坏的情况下，搜索可能需要几分钟。对于最近发布的数据来说，这是真的，知道坐在你旁边的同事发布了这些数据可能会令人恼火。幸运的是，如果你已经知道去哪里找，你就可以避免全球搜索。

还记得 **init** 命令打印出来的 **Qm…** hash 吗？毕竟，这是您的节点的对等 ID。当您键入命令时，它会显示更多的信息，所以如果您忘记写下来也不用担心。

产生的 JSON 对象将包括各种字段。目前，最重要的是节点 ID。

如果您知道必须保存所需数据的节点 ID，您可以通过提供与该节点的直接连接来绕过耗时的搜索。为此，请使用以下命令:

```
ipfs swarm connect /ipfs/Qm...
```

替换 **Qm…** 路径的节点 ID

在连接之前，您的 IPFS 节点必须首先寻找一个新的对等点。如果您知道远程节点的整个多地址，也可以跳过这一步。在这种情况下，您可以使用完整的多地址作为输入来运行相同的命令，如下所示:

```
ipfs swarm connect /ip4/<IP address>/tcp/<port number>/ipfs/<peer ID>
```

IPFS 可以在各种网络协议上运行，并且一个节点经常监听许多网络接口。因此，一个节点通常会有许多不同形式的多地址。

每个都包含对等体 ID 和关于如何访问它的信息(例如 IPv5 地址和端口)。

您还可以通过使用节点的对等 id 来检索节点的地址:

```
ipfs dht findpeer Qm...
```

有可能你的同事在你旁边的工作站上传了一个数据块——比如说，一个新的 Fury 版本——但你似乎无法获得它，甚至无法连接到他的 IPFS 节点。这些问题最常见的原因是网络连接，例如阻止同一网络上的计算机相互通信的防火墙。以下步骤将帮助您确定问题的根源。

尝试使用 WWW 网关访问数据。例如，本文开头描述的 IPFS 徽标可以在以下 URL 中看到:

[https://ipfs . io/ipfs/qmbyq 2 PMI 91 xd U6 z 1edr VP 4 bwjxch 9 hhrx 8 tk 99 DJ WG 6](https://ipfs.io/ipfs/QmbYq2pMi91Xd5Hu6Z1edrvP4BwJXCH9HhRX8Tk99DJWG6)

对于新发布的数据，这通常需要几分钟时间。但是，如果请求过期，就会发生以下情况之一:

用于保存数据块的节点目前处于脱机状态，或者发布数据块的节点与整个网络断开连接，这可能是由于网络防火墙的原因。

网关是否成功检索到数据，但是本地 IPFS 节点没有使用 telnet 来查看是否可以访问对等体的地址和端口。如果这种联系能够形成，你就不走运了。数据共享仍然是可行的，但不是在所有情况下，只有通过第三个节点，你和你的同伴可以访问。您有两种选择来解决这个问题:

请咨询您的网络管理员，以允许您的本地网络上的 IPFS 连接。

将您的数据托管在一个 pinning 提供商处可能会将其移出有限的网络。

*原载于 2022 年 7 月 6 日*[*【https://kryptomind.com】*](https://kryptomind.com/beginners-guide-to-ipfs/)*。*