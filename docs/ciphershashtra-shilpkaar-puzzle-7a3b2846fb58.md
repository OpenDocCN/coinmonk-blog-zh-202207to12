# cipher shashtra-Shilpkaar 难题

> 原文：<https://medium.com/coinmonks/ciphershashtra-shilpkaar-puzzle-7a3b2846fb58?source=collection_archive---------6----------------------->

![](img/7537276fa100d13aeb8733c30a540f21.png)

ChipherShastra Challenge

![](img/16bf1280892851ab93387dbd386f5173.png)

你可以在这里找到挑战:[https://ciphershastra.com/shilpkaar.html](https://ciphershastra.com/shilpkaar.html)

与差异，其中一个最恼人的难题。从维基百科来看， [Shilpkar](https://en.wikipedia.org/wiki/Shilpkar) 是*一个工匠社区，主要与凹版技术和绘画*有关。这确实是一个提示，告诉我们要解决这个难题必须做些什么。特别是，为了解决这个难题，我们需要精心制作一组数据，使我们能够通过挑战契约中的多重复杂检查。

但首先我们需要找到第一把锁的钥匙，它藏在显眼的地方。

# 开启

查看这些函数，我们可以看到,`roulette()`函数首先受到被调用函数在`shilpkaar`映射中的要求的保护。看着合同，似乎实现这一点的唯一可能的方法是首先调用`unlock()`函数并通过所有检查。

然而，解锁功能第一次检查似乎锁定我们调用它！第一次检查需要将状态变量`gate1Unlocked`和`gate2Unlocked`设置为`true`。这两个变量是统一的(意味着它们都是假的)，并且它们没有在任何地方设置。

通常，当遇到这样的难题时，要么有一个数组下溢可以利用，要么有一个更微妙的问题，即在本地声明复杂数据类型，而不指定其数据位置。这是什么意思？回到 Solidity 0.4，你可以声明一个局部复杂变量(比如一个结构)而不用指定它的数据位置。如果您没有指定它，那么默认情况下，它将指向一个存储位置。特别是约定存储器的槽 0。这是可以的，只要你不写它，只是用它来指向正确的位置。但是，如果您在将它指向有效位置之前对其进行写入，它将直接覆盖从槽 0 开始的约定存储的内容。

这正是在`unlock`功能中发生的事情:

```
function unlock(bytes32 _name, bytes32 _password) external { regInfo regRecord; regRecord.name = _name; regRecord.password = _password;
```

`regRecord`被声明但没有指向有效的位置，然后它的成员被设置为`_name`和`_password`。因为`regRecord.name`和`regRecord.password`是`bytes32`变量，实际上是指向契约存储的槽 0 和槽 1(每个槽长 32 字节)。

这使我们能够控制合同存储的前两个插槽。

这部分难题的第二部分是认识到有不止一个变量映射到槽 0，槽 1 也是如此。特别是:

```
// Slot 0address private garbageAddress;  //         160 bitsbool private gate1Unlocked;      //         8 bitsuint16 private gateKey1;         //         16 bitsuint72 public rouletteStartTime; //         72 bits// TOTAL   256 bits = 32 bytes = 1 slot // Slot 1uint64 private gateKey2;         //         64 bitsuint64 private gateKey3;         //         64 bitsbool private gate2Unlocked;      //         8 bitsuint64 private gateKey4;         //         64 bitsuint56 private gateKey5;         //         56 bits// TOTAL   256 bits = 32 bytes = 1 slot
```

有了这些信息，我们可以制作一组数据，让我们通过挑战合同中的多项检查。

# 前两次检查

`gate1Unlocked`和`gate2Unlocked`的值被设置为`true`以通过第一次检查。然后，第二次检查要求我们在`garbageAddress`变量中填入一个满足以下期望的值:

```
require( uint256(garbageAddress) > 2**153 + garbageNonce && uint256(garbageAddress) <= 2**160 - ((garbageDivisor - 1) - garbageNonce), "Problem With garbageAddress" );
```

考虑到条件，我们可以将`garbageAddress`设置为:

```
garbageAddress = address(uint160(2**153 + garbageNonce + 1));
```

并通过两项测试。因为`garbageNonce`是公共的，我们可以读取它并在公式中使用它。

# **素数之谜和万能钥匙**

这部分是最难按照`masterKey`值制作的。下一张支票看起来像这样:

```
require( gateKey3 < (uint256(uint64(-1)) * 49) / 100 && gateKey4 < (uint256(uint64(-1)) * 51) / 100 && probablyPrime(gateKey3) && probablyPrime(gateKey4), "Problem with gateKeys" );
```

基本上就是说`gateKey3`必须小于`uint64`最大值的 49%`gateKey4`必须小于`uint64`最大值的 51%。哦，两者都可能是质数！

因为我需要这个密钥加起来达到某个值才能使`masterKey`有效，所以我精心制作了一个值来加上和减去`gateKey3`和`gateKey4`。这样，通过将`gateKey3`和`gateKey4`相加，额外的值增加和减少相互抵消，但是允许我们将`gateKey3`和`gateKey4`的值带到期望的范围:

```
uint64 gateKeysCompensation = uint64(2**63 - ((uint256(type(uint64).max) * 49) / 100));
```

现在我们可以将`gateKey3`和`gateKey4`值定义为:

```
uint64 gateKey3 = 2**63 - gateKeysCompensation; uint64 gateKey4 = 2**63 + gateKeysCompensation;
```

这使得两个值都在范围内，但它们仍然不是质数。为了找到最接近期望值的质数，我使用了这个[网站](https://www.numberempire.com/primenumbers.php)来计算与给定质数接近的质数(下一个或上一个)。通过传递`gateKey3`和`gatekey4`的值，我们可以选通它们各自之前的质数，并调整公式以达到目标:

```
uint64 gateKey3 = 2**63 - 44 - gateKeysCompensation; uint64 gateKey4 = 2**63 - 92 + gateKeysCompensation;
```

`masterKey`是这样计算的:

```
uint256 masterKey = uint256(gateKey1) + uint256(gateKey2) + uint256(gateKey3) + uint256(gateKey4) + uint256(gateKey5);
```

我们需要`masterKey`满足以下检查:

```
require( masterKey > (2**65 + 2**56) + masterNonce && masterKey < (2**65 + 2**56 + 2**16) - ((masterDivisor - 1) - masterNonce), "Problem with masterkey" );
```

如果我们把组成`masterKey`的所有值加起来，我们可以看到我们少了`masterNonce`和额外的 139。这很好，因为我们有一个可用的自由变量`gateKey1`。我们可以使用这个变量将缺少的值添加到`masterKey`中。它是一个`uint16`，但足以保持所需的偏移:

```
uint16 gateKey1 = 139 + uint16(masterNonce);
```

# **轮盘开始时间**

我们需要设置的最后一个值是`rouletteStartTime`。这将用于`roulette`功能检查的时间:

```
require(block.timestamp >= timeToRoll[msg.sender], "Problem with timeToRoll");
```

最简单的就是设置为 0，这样我们就可以随时调用`roulette`了。

# **最终破解**

现在有了所有这些值，我们可以为`name`和`password`设计值:

```
bytes32 name = bytes32(abi.encodePacked(rouletteStartTime, gateKey1, gate1Unlocked, garbageAddress)); bytes32 password = bytes32(abi.encodePacked(gateKey5, gateKey4, gate2Unlocked, gateKey3, gateKey2));
```

我们可以用这个值调用`unlock`，然后调用`roulette`来赢得挑战

# Github 回购

你可以在我的 Github repo:[https://github.com/robercano/ciphershastra](https://github.com/robercano/ciphershastra)中找到这个解决方案以及其他的密码破解挑战

# 关于我

我叫罗伯特·卡诺，你可以在 https://thesolidchain.com 找到我

> 加入 Coinmonks [电报频道](https://t.me/coincodecap)和 [Youtube 频道](https://www.youtube.com/c/coinmonks/videos)了解加密交易和投资

# 另外，阅读

*   [瓦济里克斯 NFT 评论](https://coincodecap.com/wazirx-nft-review) | [比茨盖普 vs 皮奥克斯](https://coincodecap.com/bitsgap-vs-pionex) | [坦吉姆评论](https://coincodecap.com/tangem-wallet-review)
*   [如何使用 Solidity 在以太坊上创建 DApp？](https://coincodecap.com/create-a-dapp-on-ethereum-using-solidity)
*   [加密交易机器人](/coinmonks/crypto-trading-bot-c2ffce8acb2a) | [OKEx vs 币安](https://coincodecap.com/okex-vs-binance)
*   [币安 vs FTX](https://coincodecap.com/binance-vs-ftx) | [最佳(SOL)索拉纳钱包](https://coincodecap.com/solana-wallets)
*   [如何在 Uniswap 上交换加密？](https://coincodecap.com/swap-crypto-on-uniswap) | [A-Ads 审查](https://coincodecap.com/a-ads-review)