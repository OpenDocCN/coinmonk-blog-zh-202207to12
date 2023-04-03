# 以太坊数据—简化的交易 trie

> 原文：<https://medium.com/coinmonks/ethereum-data-transaction-trie-simplified-795483ff3929?source=collection_archive---------2----------------------->

![](img/baa703e631e424db94aef00488b16299.png)

以太坊节点存储两种类型的数据:块数据和状态数据。状态数据包括世界状态 trie、账户存储 trie、交易收据 trie 和交易 trie。每个块都有一个事务 trie。Transaction trie 将块中的事务组织成树形结构，根哈希保存在块头中(如下所示)。但是为什么需要它呢？

![](img/cd0b1c341491f7c8977e7b0b880b6419.png)

Ethereum Full Node (Inspired from Lucas Saldanha’s [blog](https://www.lucassaldanha.com/author/lucas-saldanha/))

随着以太坊网络中交易和智能合约数量的增长，区块链的规模也在迅速扩大。完整节点需要~ [700GB](https://etherscan.io/chartsync/chaindefault) ，而归档节点需要~ [11TB](https://etherscan.io/chartsync/chainarchive) 。这使得任何小型设备都不可能加入网络。当你想进行交易时，你唯一的选择是运行一个完整的客户端或者依靠一个集中的服务。

最终目标是所有人都能进入区块链。一定有更简单的方式运行以太坊节点，而不需要存储大量数据。light node 就是对此的回应，它可以运行在任何小型设备上，比如智能手机或者树莓 Pi。

轻型客户端是不包含全部区块链数据的节点。相反，它们只下载块头链。它们不能参与数据块验证，但是可以像完整节点一样访问区块链。

![](img/32952c2b99b7976a57461c7b8a9a67ce.png)

Ethereum Light node

假设您想要在启动另一个交易之前检查您的移动应用程序(一个轻量级节点)以查看您最近的交易是否被确认。由于轻型节点只有块头，它们不知道事务是否包含在最新的块中，因此必须请求完整的节点。

![](img/e7764515c40db490224ca72e0debd47b.png)

区块链的基本假设是任何节点都不可信。如果是这样的话，光节点怎么确定全节点没有恶意？它可以要求证据。即使完整节点提供了附加信息，轻型节点又能验证什么呢？因为轻型节点具有块头(假设大多数节点是诚实的)，所以这是轻型节点可以依赖的唯一数据。

让我们看一个简单的例子，看看这是如何工作的。

当创建一个块时，它的所有事务都可以被散列以产生一个事务根散列。然后根哈希保存在块头中。

![](img/f7c9ba9ae3abaffef8550d98bb4d795f.png)

Transaction trie (Simple Tree)

当 light node 想要确认某个块中是否存在某个事务时，它们可以向 full node 请求该块的完整事务列表。然后，光节点可以将它们全部散列在一起，并将它们与报头的根散列进行比较，以确认事务的存在。

![](img/382827af218ed2009a85f2a4eaba7016.png)

Light node transaction verification — Simplified

虽然上面的方法工作得很好，但是每当轻型节点想要确认一个事务时，它必须从完整节点请求整个事务列表来验证散列。鉴于轻型节点是资源有限的小型设备，它可能会很快成为资源密集型设备，尤其是因为数据块可能会有数百个事务。

> 交易新手？试试[加密交易机器人](/coinmonks/crypto-trading-bot-c2ffce8acb2a)或者[复制交易](/coinmonks/top-10-crypto-copy-trading-platforms-for-beginners-d0c37c7d698c)

那我们怎么解决呢？默克尔树。

Merkle tree 通过重复获取其节点对的散列来计算根散列。上述事务列表的 merkle 树如下所示:

![](img/ca70de31d3cba55ae89a9f58576bfd9e.png)

Transaction trie (Merkle tree)

好吧，它是如何促进验证的？如果仔细观察，您会注意到，为了证明根哈希，完整节点不需要返回每个事务哈希，而只需要返回轻型节点验证证明所需的哈希。有什么不清楚的吗？希望下图能更好的解释一下。

![](img/590dafaa393f1921cf59372ed9b5ed79.png)

Light node transaction verification — simplified

Merkle 树实现需要整个节点发送三个散列，而以前的实现需要十个散列。考虑具有 1000 个事务的块。Merkle proof 只需要 10 (log2(n))个散列，而不是请求所有散列，从而允许快速的事务验证。

*我希望这篇文章能让你更好地理解事务 trie 是如何对网络可伸缩性做出贡献的。我将在以后的文章中解构交易收据 trie。如果你想在帖子发布时得到通知，请确保关注。如果您有任何问题或意见，请随时联系我们。*[*Twitter*](https://twitter.com/kirubakumaresh)*|*[*LinkedIn*](https://www.linkedin.com/in/kirubakumaresh/)