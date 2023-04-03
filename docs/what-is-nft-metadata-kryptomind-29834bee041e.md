# 什么是 NFT 元数据-氪星思维

> 原文：<https://medium.com/coinmonks/what-is-nft-metadata-kryptomind-29834bee041e?source=collection_archive---------28----------------------->

NFT 元数据是 NFT 项目和区块链技术的重要元素。数字资产被跟踪，并使用它们识别其所有者。这篇博客文章将研究 NFT 元数据及其在区块链技术中的应用。

NFT 的元数据描述了数字资产的额外属性和特征。这可以包含项目的创建日期和时间、创建者的姓名和联系方式、资产说明以及可搜索的关键字。保存元数据的区块链分类账使 NFT 所有者能够跟踪和维护他们的资产。

一个 NFT 制造商可以创造出独一无二的东西，由于元数据的原因，很难复制。因此，投资者和收藏家对元数据全面的 NFT 非常感兴趣。

NFT 保存在分散的 IPFS(星际文件系统)中，这是一组使用相同协议进行交互的机器。为了支持大量用户和 NFTs，系统是分布式的和可扩展的。星际文件系统对审查和数据丢失的抵抗力是它的主要优势。这样，如果网络中的一个节点脱机，不会影响其他节点，因为数据分散在几个不同的节点中。

星际文件系统的缺点是比其他存储系统更慢、效率更低。然而，对于许多优先考虑审查阻力和数据保密性的用户来说，这种妥协是值得的。

这区分并增加了 NFT 的价值:因为它们的数据保存在区块链上，它们不能被复制或更改。反映基础数据的令牌是您在购买 NFT 时购买的东西。数据是不可更改的，并且安全地存储在以太坊区块链上。因此，使用 NFTs 来获取和出售数字资产是安全的。

当您离线存储 NFT 时，会将它们委托给第三方服务，例如 Google Drive 或 AWS 等云存储提供商。这项服务会跟踪您的 NFT，并确保它们始终可供您使用。人们应该意识到非功能性食物的链外储存有几个危险。首先，如果提供商停业，您的 NFTs 可能会永久丢失。其次，如果服务遭到黑客攻击，你的 NFTs 可能会被窃取。

您的 NFT 可能会因为服务而变得无法访问，这将禁止您交易或转让它们。因此，在选择之前，考虑让你的 NFTs 离线的优势和劣势是至关重要的。

要创建一个 NFT，您必须首先生成一个 JSON 文件，其中包含描述该令牌代表什么的必要 NFT 信息。

用于编码元数据的 JSON 文件格式将很快在以太网上实现，这使得 NFTs 与智能合约的通信更加简单。由于 ERC 721 以太坊 NFT 标准，开发者可以在以太坊区块链上存储 JSON 信息。

这对于 NFTs 尤其有用，因为 NFTs 经常需要包含额外的信息，比如艺术家的名字、NFT 的描述或者许可细节。得益于 JSON 标准，web3 API 和其他基于 JSON 的系统(如它们)更容易与 NFTs 互操作。此外，它还支持基于元数据的 NFT 查询和过滤。

为了构造 NFT 元数据，JSON 文件中必须有几个关键的数据位。您必须首先给 NFT 一个唯一的标识。它可能是一个 URL 或另一个独特的字符串。必须添加 NFT 的描述、标题和关键字，以及一些其他基础元数据。

还应该指定 NFT 本身的文件类型。这样做将使人们有可能与它互动，并适当地展示它。通过包含这些必要的数据位，您可以为您的 NFTs 生成一个完整的、有价值的 JSON 文件。

> 交易新手？试试[加密交易机器人](/coinmonks/crypto-trading-bot-c2ffce8acb2a)或者[复制交易](/coinmonks/top-10-crypto-copy-trading-platforms-for-beginners-d0c37c7d698c)

下面的 NFT 讨论将采用传统的以太坊 ERC-721 令牌标准。

每个 ERC-721 的描述包括详细描述不可替换令牌的“元数据”串。例如，该信息可以识别某个。JPEG，但是 CryptoPunk.JPEG 和 DeadFellaz.JPEG 有很大的不同。尽管 JPEG 文件大小相似，但它们的值却大不相同。

关于 NFT 元数据，让人们困惑的主要问题是文件存储在什么地方——像 Google Drive 吗？它是亚马逊网络服务上的文件存储区吗？谁监管 NFT 元数据的在线存储？

每个 NFT 指的是基于在线的音频或视频(图像、音频等)。)资产。它发送一个请求到一个特定的地方，返回请求的内容给你看或听。NFT 通常指向在线的 HTTP URL 或 IPFS 散列。

ERC-721 以标准化的 JSON 格式指定元数据，如下所示:ERC-721 以标准化的 JSON (JavaScript 对象表示法)格式指定元数据，该格式通常由托管 NFT 的网站维护。

```
{"title": "Asset Metadata","type": "object","properties": {"name": {"type": "string","description": "Identifies the asset to which this NFT represents",},"description": {"type": "string","description": "Describes the asset to which this NFT represents",},"image": {"type": "string","description": "A URI pointing to a resource with mime type image/* representing the asset to which this NFT represents. Consider making any images at a width between 320 and 1080 pixels and aspect ratio between 1.91:1 and 4:5 inclusive.",}}}
```

由于存储一个 JSON 会非常昂贵并且需要大量资源，所以数据在以太坊契约中作为 URI 保存。然而，URI 字符串将访问者导向一个页面，在那里他们可以获得令牌的 JSON 描述。

在区块链上，令牌的元数据是一个永久的、不可撤销的记录，包含有关其所有权、代表什么以及其交易历史的信息。图像的名称、描述、托管的 URL，偶尔还有其他特定信息，如项目的总供应量、使用的加密类型和唯一的签名，都包含在 JSON 文件中。

通常，这个 JSON 元数据只是用来标识对象，除了绝对最小值之外，不提供任何进一步的信息。

多项计划旨在修复以太坊网络的缺陷和限制，即数据不太容易被其他智能合同搜索或访问。

代币发行者，NFT 合约的合法所有者，提供数据。不管是好是坏，用户都不能更新数据，这是很困难的，原因有几个。

正如我们在变化的互联网生态中观察到的那样，链接可能会断开。由于 NFT 元数据包含一个链接，可以将你引导到另一个可以观看艺术品的位置，如果这个链接断开，你将需要进入一个代价高昂的 404 错误页面。用户无法更改 JSON 数据或链接。

主要问题是，如果数据可以更新，NFT 的内在价值可能会受到威胁。举例来说，如果一个怀有敌意的第三方发现了一个漏洞，用在谷歌上找到的真实猿猴的图像替换了所有无聊的猿猴游艇俱乐部的图像信息，市场肯定会做出激烈的反应。

*原载于 2022 年 7 月 13 日*[*【https://kryptomind.com】*](https://kryptomind.com/what-is-nft-metadata/)*。*

> 加入 Coinmonks [电报频道](https://t.me/coincodecap)和 [Youtube 频道](https://www.youtube.com/c/coinmonks/videos)了解加密交易和投资

# 另外，阅读

*   [有哪些交易信号？](https://coincodecap.com/trading-signal) | [Bitstamp vs 比特币基地](https://coincodecap.com/bitstamp-coinbase) | [买索拉纳](https://coincodecap.com/buy-solana)
*   [加密交易机器人](/coinmonks/crypto-trading-bot-c2ffce8acb2a) | [维护审查](https://coincodecap.com/uphold-review)
*   [如何给 MetaMask 钱包添加 Arbitrum？](https://coincodecap.com/how-to-add-arbitrum-to-metamask-wallet)
*   [KuCoin vs 北海巨妖 vs BitYard](https://coincodecap.com/kucoin-vs-kraken-vs-bityard)
*   [加密交易的最佳 VPN](https://coincodecap.com/best-vpns-for-crypto-trading)
*   [ProfitFarmers 回顾](https://coincodecap.com/profitfarmers-review) | [如何使用 Cornix 交易机器人](https://coincodecap.com/cornix-trading-bot)