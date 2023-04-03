# CSC 上的可租赁 NFT

> 原文：<https://medium.com/coinmonks/rentable-nft-on-csc-4d921424f404?source=collection_archive---------28----------------------->

NFT 不仅仅是图像所有权的加密保证。大多数人认为 NFT 只是一个图像！但是图像只是 NFT 的一部分，我们称之为**元数据。**在本教程中，我们将讨论可租赁的 NFT。

![](img/2cc4372bca4b650cc3e0edf1b9b70510.png)

## **NFT**

**不可替代令牌** ( **NFT** )是唯一的数字标识符，不可复制、替代或细分，记录在区块链中，用于证明真实性和所有权。NFT 的所有权记录在区块链，所有者可以转让，允许非森林交易。任何人都可以创建 NFT，创建 NFT 只需要很少或不需要编码技能。NFT 通常包含对数字文件的引用，如照片、视频和音频。因为 NFT 是唯一可识别的资产，所以不同于加密货币，加密货币是可替代的。

NFTs 的支持者声称，NFTs 提供了一个公共的真实性证书或所有权证明，但 NFT 传达的法律权利可能是不确定的。由区块链定义的 NFT 的所有权没有固有的法律含义，并且不一定授予其相关数字文件的版权、知识产权或其他法律权利。NFT 不限制共享或复制其关联的数字文件，也不阻止创建引用相同文件的 NFT。

例如，在游戏领域，NFT 公用事业公司提供了一种管理游戏内资产所有权的新方法，或者在社交空间，它们有助于保证对独家社区的安全访问。

另一个用例可能是与“可出租智能物理资产”(es)相关的 NFT。储物柜、度假屋或汽车)，其中用户可以发送命令(es。“锁门”、“解锁门”)后证明他是 NFT 的现任主人。

谈到公用事业 NFT，在某些情况下，所有者和使用者并不总是相同是有道理的。NFT 的所有者可以在一定时间内将其出租给用户。在此期间，用户暂时获得 NFT 赋予的特权，但不能转让其所有权。

## ERC721

ERC-721 是一个免费的开放标准，它描述了如何在基于以太坊的区块链上构建不可替换的或唯一的令牌。虽然大多数令牌都是可替换的(每个令牌都是相同的)，但 ERC-721 令牌都是唯一的。

ERC-721 定义了智能合约必须实现的最小接口，以允许管理、拥有和交易唯一令牌。它没有强制要求令牌元数据的标准，也没有限制添加补充功能。

好消息是，出租 NFT 的 EIP 是存在的( [EIP 4907](https://github.com/ethereum/EIPs/blob/master/EIPS/eip-4907.md) )声明:

> “这个标准是 EIP-721 的延伸。它提出了一个可以授予地址的附加角色(用户)，以及该角色自动撤销的时间(过期)。用户角色代表“使用”NFT 的权限，但不代表转移它或设置用户的能力

该标准被设计为完全兼容 ERC-721，并且基本上引入了一个接口来实现( [IERC4907.sol](https://github.com/ethereum/EIPs/blob/master/assets/eip-4907/contracts/IERC4907.sol) )以下方法:

```
function setUser(uint256 tokenId, address user, uint64 expires)
```

如果呼叫者是 NFT 或批准地址的所有者，此方法将设置新用户和 NFT 的过期时间。

```
function userOf(uint256 tokenId) external view returns(address);
```

这将返回 NFT 的当前用户的地址，其中零地址表示没有用户或者租期已过。

```
function userExpires(uint256 tokenId) external view returns(uint256);
```

最后一个方法返回 NFT 用户的到期时间，其中零值表示没有用户。

查看参考实现( [ERC4907.sol](https://github.com/ethereum/EIPs/blob/master/assets/eip-4907/contracts/ERC4907.sol) )，我们可以注意到合同是如何扩展 ERC721 标准的，并使用 NFT 和其最终用户之间的映射以及到期时间。

```
contract ERC4907 is ERC721, IERC4907 {
  struct UserInfo
  {
    address user; // address of user role
    uint64 expires; // unix timestamp, user expires
  }
  mapping (uint256 => UserInfo) internal _users;
  …
```

我问自己的第一个问题是:“如果 EIP 的名称是“出租 NFT”，为什么 IERC4907 或参考实现缺少“出租”方法？

好的，有 setUser 方法，但是使用它，只有 NFT 的所有者(或批准的地址)可以设置用户。在这种情况下，该过程听起来更像是一个贷款过程，其中所有者将与公用事业 NFT 相关的特权借给用户，此外，还要为此支付汽油费。

在这里，所有者可以选择:

1.  为了将他的 NFTs 设置为可出租的，
2.  设定在一定时期内租赁其 NFTs 所需的乙醚量。

如果 NFT 被列为可租赁，用户就有能力租赁它并为此向所有者付费。

想要租赁 NFT 的用户必须向合同发送一个事务，指定他想要租赁的 NFT 和租赁时间间隔。此外，交易必须具有所选时间间隔的正确醚量。

让我们看看 solidity 代码的相关部分:

```
contract RentableNFT is ERC4907, Ownable
```

当然，RentableNFT 契约扩展了引用实现 ***ERC4907*** 和 OpenZeppelin Ownable 契约(好吧，我同意你的观点，Ownable 扩展并不是绝对必要的，但是在这个契约中，我希望只能允许契约的所有者进行铸造)。

```
uint256 public baseAmount = 1000000000000000; //0.001 Cet
struct RentableItem {
   bool rentable;
   uint256 amountPerMinute;
}
mapping(uint256 => RentableItem) public rentables;
```

*   ***baseAmount*** 是租赁一分钟的默认支付金额，
*   ***RentableItem*** 是存储某个 NFT 的可租性和费用信息的结构，
*   ***rentables*** 是 NFTs 和 RentableItem 结构之间的映射。

```
function mint() public onlyOwner {
    currentTokenId.increment();
    uint256 newItemId = currentTokenId.current();
    _safeMint(owner(), newItemId);
    rentables[newItemId] = RentableItem({
        rentable: false,
        amountPerMinute: baseAmount
    });
}
```

默认情况下，在**的*薄荷*的**功能中，新创建了 NFT:

*   是由合同的所有者铸造的，
*   被设置为不可出租，
*   其费用设置为 ***基数*** 。

```
function setRentFee(uint256 _tokenId, uint256 _amountPerMinute) public {
   require(_isApprovedOrOwner(_msgSender(), _tokenId), "Caller is not token owner nor approved");
   rentables[_tokenId].amountPerMinute = _amountPerMinute;
}
```

这是 NFT***amountPerMinute***值的设置方法。此方法只能由 NFT 的所有者(或批准的地址)调用。

```
function setRentable(uint256 _tokenId, bool _rentable) public {
   require(_isApprovedOrOwner(_msgSender(), _tokenId), "Caller is not token owner nor approved");
   rentables[_tokenId].rentable = _rentable;
}
```

此方法设置 NFT 的可出租布尔值。此方法只能由 NFT 的所有者(或批准的地址)调用。

```
function rent(uint256 _tokenId, uint64 _expires) public payable virtual {
   uint256 dueAmount = rentables[_tokenId].amountPerMinute * _expires;
   require(msg.value == dueAmount, "Uncorrect amount");
   require(userOf(_tokenId) == address(0), "Already rented");
   require(rentables[_tokenId].rentable, "Renting disabled for the NFT");
   payable(ownerOf(_tokenId)).transfer(dueAmount);
   UserInfo storage info = _users[_tokenId];
   info.user = msg.sender;
   info.expires = block.timestamp + (_expires * 60);
   emit UpdateUser(_tokenId, msg.sender, _expires);
}
```

这里核心的 ***租*** 法，它:

*   计算租用 NFT 的应付金额，
*   验证发送的金额是否正确，
*   验证 NFT 尚未被其他人租用，
*   验证 NFT 是由其所有者出租的，
*   将到期金额转给 NFT 所有者，
*   更新有关用户和过期时间的信息。

提议的合同可在 github 的[链接](https://github.com/donpabblo/rentable-nft/blob/main/contracts/RentableNFT.sol)上获得。

> 交易新手？尝试[加密交易机器人](/coinmonks/crypto-trading-bot-c2ffce8acb2a)或[复制交易](/coinmonks/top-10-crypto-copy-trading-platforms-for-beginners-d0c37c7d698c)