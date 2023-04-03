# 不可预测的随机性 NFT 铸币与链环 VRF

> 原文：<https://medium.com/coinmonks/unpredictable-randomness-to-nft-minting-with-chainlink-vrf-v2-f6ecd43052cc?source=collection_archive---------9----------------------->

![](img/15f1f4c654c892df9e0e71fc7848e3d0.png)

Photo by [Guillermo Velarde](https://unsplash.com/@guille_velard?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

***目录***

1.  **简介**
2.  **实现**
    *-从 Chainlink 中获取随机性
    -找到一个随机令牌 Id
    -编写 Mint 函数*
3.  **编写测试**
    *-模拟链环 VRF
    -测试随机性请求&接收周期
    -分段测试*

# 介绍

像以太坊和比特币这样的区块链是透明的、确定的。这意味着生成防篡改不可预测的随机性是一项颇具挑战性的任务。然而，在许多区块链应用程序中，它仍然是一个至关重要的方面，例如确定 Dao 中的治理角色、赠品、即玩即赚游戏、生成随机 NFT 特征、公平分配资产等等。

实现这一点的一个简单方法是使用一个全局可用的变量如`block.difficulty`或`block.timestamp`作为熵的来源来产生随机性。但是这种产生随机性的方式，如果你正在建造一些严肃的东西，从来都不是一个好主意。为什么？因为这些都可以被矿工操纵[。](https://blog.positive.com/predicting-random-numbers-in-ethereum-smart-contracts-e5358c6b8620)

因此，解决方案将是使用 oracle 网络，它可以在链外生成随机性，但在链上提供加密证明。而[链环 VRF(可验证随机函数)](https://blog.chain.link/chainlink-vrf-on-chain-verifiable-randomness/)正是这么做的。因此，我想带您看一下 NFT 铸币合同中 VRF 的一个简单实现，以便在铸币商之间公平地分配稀有的 NFT。大多数 NFT 艺术品项目通过离线计算生成 NFT 特征，并将 NFT 图像存储在像 IPFS 这样的分布式存储中。然而，这带来了铸币者狙击稀有 NFT 的风险，从而阻碍了代币的公平分配。因此，为了解决这个问题，我们可以编写一个智能合同，使铸币商能够使用 VRF 链获得一个随机的令牌 ID。

> 交易新手？在[最佳加密交易](/coinmonks/crypto-exchange-dd2f9d6f3769)上尝试[加密交易机器人](/coinmonks/crypto-trading-bot-c2ffce8acb2a)或[复制交易](/coinmonks/top-10-crypto-copy-trading-platforms-for-beginners-d0c37c7d698c)

我会用安全帽和打字稿写铸造合同和测试。此外，使用 chainlink 模拟测试智能契约。[这里有一个链接](https://github.com/Ak-prog-50/VRF_minting_contract)指向包含合同、测试和部署脚本的 GitHub 存储库。

# 履行

## 从 chainlink 获取随机性

创建随机令牌 Id 首先需要来自 chainlink 的随机值。那么，让我们快速看一下如何做到这一点。有两个契约对获得随机性很重要。第一个是`[VRFCoordinatorV2](https://github.com/smartcontractkit/chainlink/blob/develop/contracts/src/v0.8/VRFCoordinatorV2.sol)`。顾名思义，这个契约负责协调整个请求和接收随机性的过程。点击可阅读更多关于请求和接收数据周期[的信息。而另一个是](https://docs.chain.link/docs/vrf/v2/subscription/#request-and-receive-data)`[VRFConsumerBaseV2.sol](https://github.com/smartcontractkit/chainlink/blob/develop/contracts/src/v0.8/VRFConsumerBaseV2.sol)`。要从 chainlink 请求一个随机值，你的契约(消耗随机性的契约)必须继承这个契约并实现`[fulfillRandomWords](https://github.com/smartcontractkit/chainlink/blob/develop/contracts/src/v0.8/VRFConsumerBaseV2.sol#L122)`函数。VRFConsumerBaseV2 的构造函数将 vrfCoordinator 地址作为参数。你可以在这里找到支持的网络和 VRFCoordinator 地址[。](https://docs.chain.link/docs/vrf/v2/subscription/supported-networks/#configurations)

这里，我实现了 VRFConsumerBaseV2 中定义的`fulfillRandomWords`函数。这是 VRF 回调函数，当随机性请求得到满足时调用。这只能由消费者合同中预定义的`VRFCoordinatorV2`合同调用。如这里的[所示](https://github.com/smartcontractkit/chainlink/blob/acdbe197438553b2c5effdf6b4f5ab6d8a0f09e8/contracts/src/v0.8/VRFConsumerBaseV2.sol#L129)，如果任何其他契约试图调用它，它会返回`OnlyCoordinatorCanFulfill`错误。

要发送随机性请求，消费者契约应该使用下面的参数调用`VRFCoordinatorV2`中的`[reqeustRandomWords](https://github.com/smartcontractkit/chainlink/blob/acdbe197438553b2c5effdf6b4f5ab6d8a0f09e8/contracts/src/v0.8/VRFCoordinatorV2.sol#L370)`函数。

*   **keyHash** :用于表示随机性请求的气价限制。在这里找到可用的密钥散列。
*   **subscriptionId** :您需要[创建一个订阅，并用一些 testnet 链接令牌为其提供资金](https://docs.chain.link/docs/vrf/v2/subscription/examples/get-a-random-number/#create-and-fund-a-subscription)，以获得一个订阅 Id。
*   **request confirmations**:VRF 服务将等待响应的阻塞确认的数量。最大和最小确认可在找到[。](https://docs.chain.link/docs/vrf/v2/subscription/supported-networks/#configurations)
*   **callbackGasLimit**:vrf coordinator 契约在调用 vrfConsumerBaseV2 契约中的回调函数(`rawFulfillRandomWords`)时，会使用这个量的气体。(*见*T1)

```
[*VRFCoordinatorV2.sol#L567*](https://github.com/smartcontractkit/chainlink/blob/acdbe197438553b2c5effdf6b4f5ab6d8a0f09e8/contracts/src/v0.8/VRFCoordinatorV2.sol#L567)**bytes memory resp =     abi.encodeWithSelector(v.rawFulfillRandomWords.*selector*, requestId, randomWords)**[VRFCoordinatorV2.sol#L575](https://github.com/smartcontractkit/chainlink/blob/acdbe197438553b2c5effdf6b4f5ab6d8a0f09e8/contracts/src/v0.8/VRFCoordinatorV2.sol#L575)
**bool success = callWithExactGas(rc.callbackGasLimit, rc.sender,resp);**
```

*   **numWords:** 要请求的随机数的个数。最大值可以在[这里](https://docs.chain.link/docs/vrf/v2/subscription/supported-networks/#configurations)找到。

您还可以指示基金链接令牌来请求随机性，而不是管理订阅。你可以在这里阅读更多相关信息[。](https://docs.chain.link/docs/vrf/v2/introduction/#two-methods-to-request-randomness)

## 查找随机令牌 Id

我们从 chainlink 得到的随机数是一个 32 字节的整数。此特定合约的最大令牌供应量为 3333。起始令牌索引为 0。因此，要获得一个随机令牌 id，我们可以将 32 字节的随机整数除以 3333，然后得到余数。`uint16 randomId = randomInt % 3333`

因此，为了避免随机令牌 id 之间的冲突，我们需要检查该 id 是否已经被铸造。

上面的函数将获得一个随机的令牌 id，如果 id 已经生成，它将寻找下一个可用的令牌 Id。您可以看到，我添加了一个 require 语句来检查随机性请求是否已经完成。这里看一下代码[。](https://github.com/Ak-prog-50/VRF_minting_contract/blob/main/contracts/VRFMinting.sol)

## 编写 Mint 函数

我在这里使用了 OpenZeppelin 的 ERC721 可枚举扩展。`[_safeMint](https://docs.openzeppelin.com/contracts/2.x/api/token/erc721#ERC721-_safeMint-address-uint256-)` [函数](https://docs.openzeppelin.com/contracts/2.x/api/token/erc721#ERC721-_safeMint-address-uint256-)有两个参数。NFT 应该转移到的地址和令牌 id。如您所见，现在剩下要做的就是将生成的随机令牌 id 传递给 mint 函数。

# 写作测试

## 模拟链节 VRFCoordinatorV2

正如我之前所说的，所有的随机性请求都由 VRFCoordinator 契约来协调。以太坊主网和测试网上已经部署了协调器合同。但是我想在一个孤立的本地环境中测试整个过程。所以我在 chainlink GitHub repo 中部署了[这个](https://github.com/smartcontractkit/chainlink/blob/develop/contracts/src/v0.8/mocks/VRFCoordinatorV2Mock.sol)模拟契约来模拟协调器。

您可以在这里找到使用 hardhat-deploy 插件[编写的部署脚本。这些脚本还在模拟测试中处理 VRF 订阅的创建和融资。](https://github.com/Ak-prog-50/VRF_minting_contract/tree/main/deploy)

模拟契约使用从`[requestRandomWords](https://github.com/smartcontractkit/chainlink/blob/008d93b87840958d96483147000237563593c04e/contracts/src/v0.8/VRFCoordinatorV2.sol#L370)` 函数返回的 request-id 作为种子来生成一个假随机数。

```
[*VRFCoordinatorV2Mock.sol#L113*](https://github.com/smartcontractkit/chainlink/blob/008d93b87840958d96483147000237563593c04e/contracts/src/v0.8/mocks/VRFCoordinatorV2Mock.sol#L113)**for (uint256 i = 0; i < req.numWords; i++) 
{**
   **_words[i] = uint256(keccak256(abi.encode(_requestId, i)))**;
**}**
```

为了模仿协调器的行为，我们必须在测试脚本中调用`fulfillRandomWords`函数。

## 测试随机性请求和接收周期

在这里，我测试了在随机性请求和响应循环中是否返回了预期的结果。你可以在 Github repo 中找到这个和其他测试合同。

## 阶段测试

在 Goerli 等测试网络中进行测试时，请使用以下测试模块，而不是本地主机。

当在像 Goerli 这样的 testnet 中进行测试时，我们不知道随机性要求何时会得到满足。所以我们不能期望我们的契约像在本地环境中一样立即发出`RandomnessRequestFulfilled`事件。因此，我创建了一个承诺，当事件被发出并且预期的结果被返回时，我将解决这个问题。

> 加入 Coinmonks [电报频道](https://t.me/coincodecap)和 [Youtube 频道](https://www.youtube.com/c/coinmonks/videos)了解加密交易和投资

# 另外，阅读

*   [如何在印度购买比特币？](/coinmonks/buy-bitcoin-in-india-feb50ddfef94) | [WazirX 审核](/coinmonks/wazirx-review-5c811b074f5b)
*   [加密交易机器人](/coinmonks/crypto-trading-bot-c2ffce8acb2a) | [Probit 审查](https://coincodecap.com/probit-review)
*   [CryptoHopper 替代品](/coinmonks/cryptohopper-alternatives-d67287b16d27) | [HitBTC 审查](/coinmonks/hitbtc-review-c5143c5d53c2)
*   [CBET 评论](https://coincodecap.com/cbet-casino-review) | [库科恩 vs 比特币基地](https://coincodecap.com/kucoin-vs-coinbase)
*   [折 App 回顾](https://coincodecap.com/fold-app-review) | [库币交易机器人](/coinmonks/kucoin-trading-bot-automate-your-trades-8cf0ca2138e0)