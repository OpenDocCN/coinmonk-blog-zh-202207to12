# CSC 学校:CRC 接口#10

> 原文：<https://medium.com/coinmonks/csc-school-crc-interface-10-dbc771fad7b9?source=collection_archive---------50----------------------->

我们现在已经熟悉了 CRC 标准，但是我们如何使用 Solidity 来实现 CRC 标准呢？

在本部分中，我们将了解 CRC 标准的接口。

> 交易新手？试试[密码交易机器人](/coinmonks/crypto-trading-bot-c2ffce8acb2a)或者[在](/coinmonks/top-10-crypto-copy-trading-platforms-for-beginners-d0c37c7d698c)[最好的密码交易所](/coinmonks/crypto-exchange-dd2f9d6f3769)上复制交易

## CRC-20

下面是 CRC20 的基本界面，描述了 CRC20 合同的功能和事件签名，并对每个给定的功能进行了说明:

```
contract CRC20 {
event Transfer(
address indexed from,
address indexed to,
uint256 value
);
event Approval(
address indexed owner,
address indexed spender,
uint256 value
); function totalSupply() public view returns(uint256);
function balanceOf(address who) public view returns(uint256);
function transfer(address to, uint256 value) public returns(bool);
function allowance(address owner, address spender)
public view returns (uint256);
function transferFrom(address from, address to, uint256 value)
public returns (bool);
function approve(address spender, uint256 value)
public returns (bool);
}
```

以下是 CRC-20 智能合同接口的功能和组件。

## 总供应量

功能 **totalSupply** 是公共的，因此所有人都可以访问。它显示当前流通的令牌总数。由于此 **totalSupply** 功能标有视图修改器，因此它不会消耗气体。此外，每当铸造新的令牌时，它都会更新内部令牌值 totalSupply。

```
// its value is increased when new tokens are minted
uint256 totalSupply_;// access the value of totalSupply_
function totalSupply() public view returns (uint256) {
return totalSupply_;
}
```

## 平衡 f

**balanceOf** 是另一个公共视图修改器，它让每个人都可以访问它，并且是无气体的。它获取 CSC 地址，并将令牌返回到分配的地址。

```
// Updated when tokens are minted or transferred
mapping(address => uint256) balances;// Returns tokens held by the address passed as _owner
function balanceOf(address _owner)
public view returns (uint256 balance) {
return balances[_owner];
}
```

## 转移

功能**转移**不同于上述两个功能，因为它需要 fas，因此导致 CSC 智能合同发生重大变化。应相应令牌持有者的请求，它将令牌从一个地址传输到另一个地址。

```
function transfer(address _to, uint256 _value)
public returns (bool) { // Check for blank addresses
require(_to != address(0)); // Check to ensure valid transfer
require(_value <= balances[msg.sender]); // SafeMath.sub will throw if there is not enough balance.
balances[msg.sender] = balances[msg.sender].sub(_value);
balances[_to] = balances[_to].add(_value);
// Event transfer defined in the CRC 20 interface above
Transfer(msg.sender, _to, _value); return true;
}
```

## 津贴、批准和调动自

最后一个功能允许、批准和转移来自支持高级功能，如授权其他地址代表各自持有者使用令牌。

## CRC-721

为了了解 CRC-721 是如何工作的，让我们看看它在这里添加的接口:

```
contract CRC721 {
event Transfer(
address indexed _from,
address indexed _to,
uint256 _tokenId
); event Approval(
address indexed _owner,
address indexed _approved,
uint256 _tokenId
); function balanceOf(address _owner)
public view returns (uint256 _balance); function ownerOf(uint256 _tokenId)public view returns (address _owner); function transfer(address _to, uint256 _tokenId) public; function approve(address _to, uint256 _tokenId) public; function takeOwnership(uint256 _tokenId) public;
}
```

## 平衡 f

在下面的代码片段中，ownedTokens 表示特定地址的令牌 id 的完整列表。鉴于， **balanceOf** 函数返回该地址的令牌数。

```
mapping (address => uint256[]) private ownedTokens;function balanceOf(address _owner) public view returns (uint256) {
return ownedTokens[_owner].length;
}
```

## 所有者

映射令牌所有者获取令牌并输出该 ID 的所有者。但是，因为它的可见性被设置为私有，所以通过使用 **ownerOf** 函数，您可以将该映射的值设置为公共的。它还需要在返回值之前对零地址进行检查。

```
mapping (uint256 => address) private tokenOwner;function ownerOf(uint256 _tokenId) public view returns (address) {
address owner = tokenOwner[_tokenId];
require(owner != address(0));
return owner;
}
```

## 转移

这个**转移**函数接受新所有者的地址作为被转移令牌的 **_to** 参数和 **_tokenId** ，还要注意它只能被令牌的所有者调用。它必须包括检查转移是否通过转移所需的批准检查的逻辑。然后是从当前所有者那里删除令牌的所有权并将其添加到新所有者拥有的令牌列表中的逻辑。

```
modifier onlyOwnerOf(uint256 _tokenId) {
require(ownerOf(_tokenId) == msg.sender);
_;
}function transfer(address _to, uint256 _tokenId)
public onlyOwnerOf(_tokenId) {
// Logic to clear approval for token transfer // Logic to remove token from current token owner // Logic to add Token to new token owner
}
```

## 赞同

**Approve** 是另一个地址声明给定令牌 ID 所有权的另一个函数。它受到修饰符 only OwnerOf 的限制，这解释了只有令牌所有者才能出于明确的原因访问该函数。

```
mapping (uint256 => address) private tokenApprovals;modifier onlyOwnerOf(uint256 _tokenId) {
require(ownerOf(_tokenId) == msg.sender);
_;
}function approvedFor(uint256 _tokenId)
public view returns (address) {
return tokenApprovals[_tokenId];
}function approve(address _to, uint256 _tokenId)
public onlyOwnerOf(_tokenId) {
address owner = ownerOf(_tokenId);
require(_to != owner); if (approvedFor(_tokenId) != 0 || _to != 0) {
tokenApprovals[_tokenId] = _to; // Event initialised in the interface above
Approval(owner, _to, _tokenId);
}
}
```

## 所有权

函数 **takeOwnership** 接受 _tokenId，并对消息发送者应用相同的检查。如果他通过了类似于传递函数的检查逻辑，他必须声明 **following _tokenID** 的所有权。

```
function isApprovedFor(address _owner, uint256 _tokenId)
internal view returns (bool) {
return approvedFor(_tokenId) == _owner;
}function takeOwnership(uint256 _tokenId) public {
require(isApprovedFor(msg.sender, _tokenId)); // Logic to clear approval for token transfer // Logic to remove token from current token owner // Logic to add Token to new token owner
}
```

## ERC-1155

## 批量转移

批量传输与常规 CRC-20 传输非常相似。让我们看看 CRC-20 的转移函数:

```
// CRC-20
function transferFrom(address from, address to, uint256 value) external returns (bool);
// CRC-1155
function safeBatchTransferFrom(
address _from,
address _to,
uint256[] calldata _ids,
uint256[] calldata _values,
bytes calldata _data
) external;
```

CRC-1155 的不同之处在于将令牌值作为数组和 id 数组进行传递。传输结果是这样的:

*   将 200 个 id 为 5 的代币从**_ 从**转移到**_ 到。**
*   将 300 个 id 为 7 的代币从**_ 从**转移到**_ 到。**
*   将 3 个 id 为 15 的代币从**_ 从**转移到**_ 到。**

除了利用 CRC-1155 的函数作为 **transferFrom** ，no transfer 之外，您可以通过将表单地址设置为调用函数的地址来利用它作为常规传输。

## 批量平衡

相应的 CRC-20 **balanceOf** 调用同样具有其支持批处理的伙伴功能。提醒一下，这是 CRC-20 版本:

```
// CRC-20
function balanceOf(address owner) external view returns (uint256);
// CRC-1155
function balanceOfBatch(
address[] calldata _owners,
uint256[] calldata _ids
) external view returns (uint256[] memory);
```

对于 balance 调用来说更简单，我们可以在一个调用中检索多个余额。我们传递所有者数组，后面是令牌 id 数组。
例如，给定 **_ids=[3，6，13]** 和 **_owners=[0xbeef…，0x1337…，0x1111…]，**返回值将为

```
[balanceOf(0xbeef...),balanceOf(0x1337...),balanceOf(0x1111...)]
```

## 批量审批

```
// CRC-1155
function setApprovalForAll(
address _operator,
bool _approved
) external;
function isApprovedForAll(
address _owner,
address _operator
) external view returns (bool);
```

这里的批准与 CRC-20 略有不同。您需要使用 setApprovalForAll 将操作员设置为批准或未批准，而不是批准特定的金额。

## 接收挂钩

```
function onCRC1155BatchReceived(
address _operator,
address _from,
uint256[] calldata _ids,
uint256[] calldata _values,
bytes calldata _data
) external returns(bytes4);
```

CRC-1155 仅支持智能合约的接收挂钩。钩子函数必须返回一个预定义的神奇字节 4 值，如下所示:

```
bytes4(keccak256("onCRC1155BatchReceived(address,address,uint256[],uint256[],bytes)"))
```

一旦接收契约返回这个值，我们就认为契约现在可以接受传输，并且它知道如何管理 CRC-1155 令牌。就这么定了！

> 加入 Coinmonks [电报频道](https://t.me/coincodecap)和 [Youtube 频道](https://www.youtube.com/c/coinmonks/videos)了解加密交易和投资

# 另外，阅读

*   [AscendEx 保证金交易](https://coincodecap.com/ascendex-margin-trading) | [Bitfinex 赌注](https://coincodecap.com/bitfinex-staking) | [bitFlyer 点评](https://coincodecap.com/bitflyer-review)
*   [麻雀交换评论](https://coincodecap.com/sparrow-exchange-review) | [纳什交换评论](https://coincodecap.com/nash-exchange-review)
*   [支持卡审核](https://coincodecap.com/uphold-card-review) | [信任钱包 vs 元掩码](https://coincodecap.com/trust-wallet-vs-metamask)
*   [TraderWagon 回顾](https://coincodecap.com/traderwagon-review) | [北海巨妖 vs 双子 vs 比特亚德](https://coincodecap.com/kraken-vs-gemini-vs-bityard)