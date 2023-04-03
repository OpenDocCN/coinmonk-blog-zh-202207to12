# 可靠性中的随机数

> 原文：<https://medium.com/coinmonks/keccak-adding-pseudorandom-numbers-to-your-smart-contract-6908f9d9e3ef?source=collection_archive---------12----------------------->

![](img/04d9cb4b5ffff146aae274df38e31fb7.png)

Photo by [Mick Haupt](https://unsplash.com/@rocinante_11?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

**警告:**这是**而不是**一种向你的智能合约添加随机数的安全方式，如果你计划将它部署到任何类型的 mainnet-你应该看看像 [Chainlink 的 VRF](https://chain.link/vrf) 这样的东西。使用我们将在文章中讨论的方法仅仅是为了学习，将这些代码部署到 mainnet 将会使您的契约暴露在许多攻击媒介之下。这是给你的警告，不听就别想告我，到头来丢了你的刀库。

# “为什么”:

所以在过去的几天/周末，你一直在做一些简单的 web3 游戏或彩票，你就要完成了。你已经编写了你的测试，花了几个小时阅读日志错误，防弹你的合同(在部署*至少* 300 个合同给 Goerli 之后)，并且完成了 UI。唯一的问题:你的游戏**超级无聊**。没有情趣，没有变化。当你的角色攻击老板时，命中目标。当 boss 玩家攻击你的时候，他们就打。游戏来回进行。不是很有说服力，是吗？

我们想给这种游戏体验增加一些趣味，因为每个人都知道真正的战斗充满了机会。如果在攻击/防御功能上有所变化的话，这个游戏将会更吸引最终用户。

建立一个具体的目标:假设当任何角色攻击时，如果随机数小于 5，他们可能会错过。如果更高，我们可以说他们击中了，并从对手那里扣除必要的生命值。因此，简而言之，我们需要添加一个函数来返回数字 1-10，这样我们就可以适当地路由游戏。

在这篇文章中，我们将涵盖一个快速和肮脏的方式来实现这一点。

# 介绍凯克卡:

[Keccak](https://keccak.team/) (读作“ketchak”)最好被描述为一系列“海绵函数”。海绵函数是一种算法，在这种算法中，固定长度的输入被接受并带有任意数量的填充值，然后被转换并映射到固定长度的输出。

我知道这听起来有点吓人，但是让我们来分析一下。我提到了三个主要部分；输入、一些填充值和输出。

## 一个接一个地走:

*   输入:这个非常简单。从凯克的角度来看，这是你在改变什么。
*   填充:*这些是应该总是唯一的数据点*。如前所述，keccak 将固定输入映射到固定输出。如果你要传递一个固定的变量，比如说，unsigned int (uint) '7 '，输出总是相同的。注意到这一点，您可以选择使用类似“now”的变量，该变量引用提出下一个块的节点中的全局系统时间戳。增加一些这样的变量会使伪造随机性或预测结果变得更加困难，但无论如何，这仍然不应该被混淆为安全。
*   输出:输入的结果+添加到 keccak 函数中的任何填充变量。我们将使用该输出的子集来生成我们的“随机”数字 1-10。

当它被分解并从离散数学演讲中提取出来时，它并不是那么糟糕(顺便说一下，这里有一个更多的链接，如果那是[你的事情](https://keccak.team/sponge_duplex.html))。

# 代码:

代码本身非常简单，感谢 GeeksForGeeks 向我介绍了这个概念。

```
pragma solidity ^0.6.6;contract YourContract {//One of our state variables to add padding to Keccak
uint nonce = 0;//the meat and potatoes, what will yield our 'random' numbers
function randMod(uint _modulus) internal returns(uint) { //increment to ensure no repeated inputs
    nonce++; //cast the value returned from the modulus of the 
    return uint(keccak256(abi.encodePacked(
          //the current timestamp, mentioned previously
          now,
          //a reference to whomever sent the transaction
          msg.sender,
          //the nonce that we incremented in the lines above
          nonce)
       )) % _modulus;
    }
}
```

通过在代码中添加 nonce 状态变量和 randMod 函数，您就可以开始比赛了。只要调用 randMod，并传入 _modulus 参数，就可以根据契约中的返回值添加逻辑门！

为了解决前面的问题陈述，我们将制作一个标准的 if-else 逻辑门，并检查随机数是否大于 5，如果是，攻击角色将成功着陆，否则，他们将失败。轻松点。

# 关闭:

如果你还没有一份正在处理的智能合同，但想自己尝试一下，你可以启动一个[混音](https://remix.ethereum.org/)的快速会话来探索，或者你可以去 [buildspace](http://buildspace.so) ！

如果你不知道什么是 buildspace，它是一个结构非常好的免费资源，可以用来学习 web3 开发的基础知识。我不会从吸引人们访问网站中获得任何经济利益，所以你可以放心，我的推荐是真诚的。Buildspace 是目前我所知道的最好的 web3 学习资源，所以我想让尽可能多的人知道它。

然而，*我确实*偶尔会对课程进行编辑/改进，这是我添加的一个额外挑战的例子！如果你想开始构建一个[以太坊回合制 NFT 游戏](https://buildspace.so/p/create-turn-based-nft-game/lessons/what-we-building)并使用 Keccak 添加伪随机机会，现在*现场直播的课程计划*包含你需要的一切！

你到底在等什么？

非常感谢您的阅读，我希望您喜欢这篇关于 Keccak 的介绍，并且理解为什么它只应用于学习目的。也许在以后的文章中，我会解释如何真正地使用*可验证的*随机数。如果你对此感兴趣，请在评论中告诉我！

下次见。

> 交易新手？试试[密码交易机器人](/coinmonks/crypto-trading-bot-c2ffce8acb2a)或[复制交易](/coinmonks/top-10-crypto-copy-trading-platforms-for-beginners-d0c37c7d698c)