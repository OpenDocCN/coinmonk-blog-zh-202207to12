# 游牧桥——一种快速的群体攻击——缺陷或其他可能吸引黑客的潜在安全漏洞

> 原文：<https://medium.com/coinmonks/the-nomad-bridge-a-fast-mob-attack-flaws-or-other-potential-security-holes-that-could-invite-d254c34589fd?source=collection_archive---------41----------------------->

Nomad 是一种跨链通信标准，有助于在链之间快速、安全地传输令牌和数据，以及在多个区块链之间发送和接收数字令牌。

Nomad Bridge 加密货币项目也受到了威胁，黑客带走了几乎所有价值 2 亿美元的钱包。

在 Nomad 的一个智能合同更新后，用户可以很容易地欺骗交易——从 Nomad bridge 中提取并不真正属于他们的钱。

由于其代码中的一个漏洞，Nomad Bridge 允许大量过剩的 WBTC 代币退出，同时允许非常少的比特币进入。黑客利用了这个漏洞，从 ERC 游牧桥窃取了 20 枚代币，价值 1.9 亿美元。

该桥的目的是通过包装一枚硬币并将其绑定到另一个区块链上的智能合约，来促进几个区块链之间的交易。举个例子，包裹比特币(WBT)是比特币在以太坊区块链上的表征；理论上，一个 WBT 相当于一个 BTC。

“混乱”的游牧袭击与大多数桥梁袭击形成对比，在大多数桥梁袭击中，一个犯罪人对整个骗局负责，这是一场混战，一旦消息传开，机会主义者就会从桥上抢钱。

随着许多其他人的参与，盗窃演变成了聚会抢劫。

在每个受支持的区块链上，Nomad 启动一个名为 Replica 的核心契约，作为所有跨链消息的邮箱。链外代理通过向该契约发布签名的新树根散列来更新树根，并在 Merkle 树中中继和安排跨链消息。

每个需要链上确认的新消息都必须经过 prove()和 process()过程。Merkle 树中的消息使用 prove()过程进行验证，然后该过程声明要验证的消息。

请点击它:

[](https://github.com/nomad-xyz/nomad-monorepo/blob/main/solidity/nomad-core/contracts/Replica.sol) [## nomad-monorepo/Replica.sol 位于主 nomad-xyz/nomad-monorepo

### 此存储库已由所有者存档。它现在是只读的。此时您不能执行该操作。你…

github.com](https://github.com/nomad-xyz/nomad-monorepo/blob/main/solidity/nomad-core/contracts/Replica.sol) 

在逻辑契约的早期版本中，尤其是 process()函数，计算消息散列，对照消息映射检查散列以确保该消息已经被证明，执行可重入性检查，最后更新消息状态。

`require(acceptableRoot(messages[_messageHash]), “!proven”);`

在流程函数内部调用 acceptable root(messages[message hash])，它用于验证乐观超时时间是否已过以及根是否已提交。此实例中的消息[ _messageHash]是 0x000。

当函数 acceptable root(messages[_ message hash])返回 true 时，消息得到验证。(错误在这里)。

为了绕开验证，攻击者可以用任意的“_message”直接调用“process(byte memory _message)”函数。

更彻底的审核流程、渗透测试和部署操作验证可能会发现此问题。

在消息被证实后，攻击者可以将令牌移出桥。

你不需要理解 Merkle Trees，Solidity，或者其他任何东西，因为这个黑客太疯狂了。找到一笔成功的交易，用你自己的地址定位/替换对方的地址，然后重新广播就可以了。

常规升级将零哈希识别为合法根，这导致了在 Nomad 上启用消息欺骗的意外后果。

> 交易新手？试试[加密交易机器人](/coinmonks/crypto-trading-bot-c2ffce8acb2a)或者[复制交易](/coinmonks/top-10-crypto-copy-trading-platforms-for-beginners-d0c37c7d698c)