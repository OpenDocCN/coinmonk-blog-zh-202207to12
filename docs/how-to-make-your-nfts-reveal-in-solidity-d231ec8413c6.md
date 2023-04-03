# 如何让你的 NFTs 在坚实中显露出来

> 原文：<https://medium.com/coinmonks/how-to-make-your-nfts-reveal-in-solidity-d231ec8413c6?source=collection_archive---------1----------------------->

![](img/dbf704fe8e92a69f46ff09e76624b14d.png)

AI art generated from article’s title

随着 NFT 面积每天都在增长，更多新的机制和实验正在进入无价像素的世界。NFT 发布期间最流行的方法之一是显示切换功能。

显示功能代表最初隐藏带有占位符图像的 NFT 图片。在本文中，我们将介绍实现这种行为的过程，并分析为什么您可能会在您的集合中使用它。

# 理论

首先，让我们深入 NFTs 的理论来定义我们的揭示所需的步骤。NFT 遵循不可替换令牌的 [ERC721](https://eips.ethereum.org/EIPS/eip-721) 标准，并且所有的生成集合也遵循`ERC721Metadata`接口。

```
**interface** ERC721Metadata */* is ERC721 */* {
    */// @notice A descriptive name for a collection of NFTs in this contract
*    **function** name() **external** **view** **returns** (**string** _name);

    */// @notice An abbreviated name for NFTs in this contract
*    **function** symbol() **external** **view** **returns** (**string** _symbol);

    */// @notice A distinct Uniform Resource Identifier (URI) for a given asset.
*    */// @dev Throws if `_tokenId` is not a valid NFT. URIs are defined in RFC
*    *///  3986\. The URI may point to a JSON file that conforms to the "ERC721
*    *///  Metadata JSON Schema".
*    **function** tokenURI(**uint256** _tokenId) **external** **view** **returns** (**string**);
}
```

这个接口允许我们为集合中的每个 NFT 指定名称、符号和 tokenURI。我们感兴趣的是负责返回项目元数据值的`tokenURI`函数。

所以我们实现 Reveal 的任务如下:
**1。**在智能合约中创建一个标志变量，指示 NFT 是否隐藏/显示
**2。**添加切换(或设置为真)标志变量值的外部函数，该函数只能由所有者
**3 访问。**覆盖`tokenURI`方法，为隐藏阶段的每个 NFT 返回相同的单个元数据文件，并在显示
**4 后返回适当的 NFT 元数据。**上传隐藏阶段元数据到 IPFS

# 更新智能合同

我不会深入解释常见的 NFT 智能合约，假设你已经有一个；)如果没有，那就看看提供节能铸造的 [ERC721A](https://www.erc721a.org/) 标准，这样你的客户就不会花很多交易费了。我也将在本教程中使用 ERC721A。

让我们将 flag 属性添加到现有的契约类中，并用`false`对其进行初始化:

```
bool public revealed = false;
```

然后让我们添加一个函数来改变我们的`revealed`值，并确保它只能被`owner`调用。为此，我建议从 OpenZeppelin `Ownable`契约中派生出来，为我们增加`onlyOwner`修改器。

```
contract HiddenReveal is ERC721A, Ownable {
  bool public revealed = false; function reveal() external onlyOwner {
      revealed = true;
  }
}
```

如你所见，我没有切换`revealed`值，只是设置了`true`。这样你就不会让你的社区紧张，担心你可能会在揭露后再次隐藏他们的非功能性测试🙃您可以使其切换当前值或设置为传递到函数中的值。

现在是最有趣的部分:基于我们的`revealed`标志覆盖`tokenURI`函数。

```
function tokenURI(uint256 tokenId) public view virtual override returns (string memory) {
    if (!_exists(tokenId)) revert URIQueryForNonexistentToken();

    string memory baseURI = _baseURI();
    string memory metadataPointerId = !revealed ? 'unrevealed' :     _toString(tokenId);
    string memory result = string(abi.encodePacked(baseURI, metadataPointerId, '.json'));

    return bytes(baseURI).length != 0 ? result : '';
}
```

1.  检查带有此类`tokenId`的令牌是否存在，如果不存在，则返回自定义错误
2.  获取`baseURI`值，将其与`tokenId`连接，并获得元数据 URL
3.  检查当前的`revealed`状态，如果是`false`则使用`unrevealed`字符串代替`tokenId`，将其存储到`metadataPointerId`
4.  串联`baseURI`、`metadataPointerId`和`.json`扩展
5.  返回的`result`是被指定的 baseURI

对于每个 NFT，`revealed=false`的最终结果将是这样的:

`https://www.mybaseurilink.com/folderipfshash/unrevealed.json`或`ipfs://folderipfshash/unrevealed.json`

以下是具有隐藏/显示功能的完整智能合约代码:

```
contract HiddenReveal is ERC721A, Ownable {
    string private _baseTokenURI;

    bool public revealed = false;

    function _baseURI() internal view virtual override returns (string memory) {
        return _baseTokenURI;
    }

    function tokenURI(uint256 tokenId) public view virtual override returns (string memory) {
        if (!_exists(tokenId)) revert URIQueryForNonexistentToken();

        string memory baseURI = _baseURI();
        string memory metadataPointerId = !revealed ? 'unrevealed' : _toString(tokenId);
        string memory result = string(abi.encodePacked(baseURI, metadataPointerId, '.json'));

        return bytes(baseURI).length != 0 ? result : '';
    }

    function setBaseURI(string calldata baseURI) external onlyOwner {
        _baseTokenURI = baseURI;
    }

    function reveal() external onlyOwner {
        revealed = true;
    }
}
```

> 请注意，这个智能合同不是生产就绪，因为它没有薄荷功能 lol。但是您可以使用它作为参考，将功能插入到您的解决方案中

# 建立 IPFS

完成智能合同部分后，您必须为未披露的项目元数据设置存储。一般来说，你只需要在一个单独的 IPFS 文件夹中存储一个占位符图像和`unrevealed.json`元数据文件。

然后，通过用文件夹的散列调用契约上的`setBaseURI`,将您的智能契约连接到 IPFS 存储。

这里有一个`unrevealed.json`的例子:

```
{
  "name": "@nfedosenko Hide/Reveal",
  "description": "Tutorial on how to create Reveal functionality on your NFT",
  "image": "ipfs://hAshToYoUrPlAcEhoLderImAge"
}
```

# 结论

总而言之，展示功能是一个伟大的营销工具和一个微小的机制，可以轻松地添加到每一个 NFT 滴。此外，您可以指定多个展示阶段，将它们定义为一个枚举，并为项目的后期制作阶段添加一些娱乐元素！

顺便说一句，这和许多其他的优化和技术都是在我的 NFT 收藏中实现的，具有独特的艺术和实用性🔥所以我欢迎你来看看并加入我们的俱乐部: [p*ssyloversclub.io](https://www.pussyloversclub.io/)

> 交易新手？尝试[加密交易机器人](/coinmonks/crypto-trading-bot-c2ffce8acb2a)或[复制交易](/coinmonks/top-10-crypto-copy-trading-platforms-for-beginners-d0c37c7d698c)