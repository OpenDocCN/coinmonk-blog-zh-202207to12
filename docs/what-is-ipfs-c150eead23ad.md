# 什么是 IPFS？

> 原文：<https://medium.com/coinmonks/what-is-ipfs-c150eead23ad?source=collection_archive---------16----------------------->

***IPFS(inter planetary File System)是一种开源的超媒体通信协议，对等节点通过该协议在单个分布式文件系统中存储和分发数据。它是如何工作的？让我们看看！***

![](img/4a6b748c5e0ef16e367389c66d9d0d20.png)

**美国初创公司协议实验室的创始人兼首席执行官胡安·贝内**创造了这个解决方案。他称之为“分布式的、永久的网络”，暗示 IPFS 创建的网站永远不会被任何人关闭。

![](img/b3d72f857e29821bcf9bd48d2aa2179e.png)

> 在某些方面，IPFS 类似于网络，但是 IPFS 可以被看作是一个单独的 BitTorrent 群，在一个 Git 仓库中交换对象。换句话说，IPFS 提供了一个高吞吐量的内容寻址块存储模型，带有内容寻址超链接，”—他在项目白皮书中说。

# IPFS 是如何工作的？

当信息被上传到 IP 地址以访问对象、文件或用户数据时，系统参照其唯一的加密散列标识符而不是服务器(内容标识符，CID)来形成。

当再次下载文件时，CID 保持不变，而更新的版本被赋予新的散列标识符。星际命名系统(IPNS)名称服务——类似于传统互联网上的 DNS 用于允许有权访问该文件早期版本的用户拥有该文件及其后续版本。

在该系统中，大小大于 256 KB 的文件被分成多个部分，被散列化，并被组织成 IPLD 对象(星际链接数据)，这些对象由两个部分组成:数据本身和通过 Merkle 树的有向非循环图(Merkle DAG)相互连接的文件部分的链接。

然后，负责系统通信的软件 IPFS 守护程序会临时缓存数据，或者根据用户的判断，将数据永久“附加”(固定)到自身，并根据请求将其分发到其他节点。将来，这样的节点可以作为内容提供者或内容接收者。

在请求系统的分布式哈希表(DHT)中的内容后，将寻找最接近用户的节点，这些节点具有所需数据的副本，并且它们是提供部分文件的节点。

IPFS 超链接示例:[https://ipfs . io/ipfs/qmr TSA 1 ufhsx 3 z 7 tanrwuvm 8 ajb 2 eqwkvyzu 3 BF jg 9 qrtz/home . html](https://ipfs.io/ipfs/QmRTSA1UFHSx3z7taNRwUVM8AjB2EQwKvyZu3BfJg9QRtZ/home.html)

# **如何使用 IPFS？**

IPFS 协议及其实现仍处于早期开发阶段，可能包含错误或隐藏的漏洞。然而，人们认为 IPFS 将有助于存储重要数据和创建静态网站。在实践中，它的使用增加了数据传输速率和网络带宽，降低了由于分发造成的节点负载，允许您绕过审查，避免 DDoS 攻击和“死”链接的出现。该系统没有单点故障，并且节点不需要相互信任。此外，IPFS 内容理论上可以无限期存储。

Geocities 托管服务是 2015 年第一个支持 IPFW 的网络资源。在 IPS 的基础上，开发了去中心化的视频平台 DTube、在线交易平台 OpenBazaar 等解决方案。

加密货币交易中添加的 IPFS 链接使您能够保存大量数据，这些数据不受区块链变化的影响，而不会降低其速度。例如，在今年春天朱利安·阿桑奇(Julian Assange)被捕后，作为支持的表示，一名比特币现金开发者在 IPFS 的维基解密网站(Wikileaks.cash)上发布了一份完整的维基解密文件档案——约 30 GB，并作为 BCH 区块链的一个链接。

自 **2014** 以来，Protocol Labs 开发团队一直致力于开发一个基于 IPFS 的 Filecoin 文件分散托管系统。该项目正准备推出测试和主网络，但胡安·贝内(Juan Benet)在 **2018** 发现，他的公司的成就激励了 TRON 的创造者:来自 Filecoin 和 IPFS 文档的几页被包括在这个中国项目的白皮书中，尽管形式略有修改。TRON 还宣布打算在 2019 年**春天推出自己的基于 BitTorrent 的 IPFS 版本——BTFS。**

> 如果你对 IPFS 话题有任何补充，欢迎评论！
> 在跟踪更新方面，订阅我们的 [Medium feed。](https://medium.com/sunflowercorporation)
> 
> 敬请期待！

> 交易新手？尝试[加密交易机器人](/coinmonks/crypto-trading-bot-c2ffce8acb2a)或[复制交易](/coinmonks/top-10-crypto-copy-trading-platforms-for-beginners-d0c37c7d698c)