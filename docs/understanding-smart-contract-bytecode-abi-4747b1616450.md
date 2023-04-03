# 理解智能合约——字节码和 ABI

> 原文：<https://medium.com/coinmonks/understanding-smart-contract-bytecode-abi-4747b1616450?source=collection_archive---------2----------------------->

![](img/632a11ff259e91f5f4ceffe8164e215d.png)

Smart Contract

在上一篇文章中，我们看到了智能合同如何优于纸质合同。在这里阅读👉I [ntro 智能合约](https://taniskannpurna.hashnode.dev/introduction-to-smart-contracts)

# 让我们开始…

我们已经看到智能合同如何优于旧的纸质合同，以及 EVM 如何确保智能合同信守承诺。

现在，EVM 不懂任何高级语言。它能理解低级语言。因此，我们需要将我们的智能合同转换成低级语言，即字节码。

💜**字节码**
——字节码是一种由数字和字母组合而成的低级语言。字节码很难写也很难读。基本上它不是人类可读的。使用不同的框架，比如 solcjs、ethers、hardhat 等等，我们可以将智能合约转换成字节码。这是字节码寻找一个非常小而简单的智能契约的方式。

**608060405234801561001057600080 FD 5b 5061077180610020600396000 F3 Fe 60806040523480156100105760080 FD 5b 5060436106100061005757760000000。
- EVM 是一个堆栈机器，这意味着它使用堆栈来运行任何脚本。它存储函数，推送函数，弹出函数等等。字节码可能看起来乱码，但它们是 EVM 如何与堆栈交互的命令。如果我们将这些字节码转换成操作码，也就是操作码，我们将会看到这些组合是如何或向 EVM 发出什么命令的。
-有很多网站可以转换字节码- >操作码。我正在使用以太扫描。
👉[https://etherscan . io/opcode-tool](链接)**

> 交易新手？试试[加密交易机器人](/coinmonks/crypto-trading-bot-c2ffce8acb2a)或者[复制交易](/coinmonks/top-10-crypto-copy-trading-platforms-for-beginners-d0c37c7d698c)

这就是我们得到的…

> [0]DUP1
> [2]push 1 0x 40
> [3]m store
> [4]call value
> [5]DUP1
> [6]is zero
> [9]push 2 0x 010
> [10]jump I
> [12]push 1 0x 00
> [13]DUP1
> [14]REVERT
> [15]dest jump
> [16]POP【POP👉请记住，EVM 将每个数字都视为十六进制数。“0x”表示该数字是十六进制数。现在在 EVM 的例子中，它不需要任何“0x ”,因为它将每个数字都视为十六进制。所以“0x40”在 EVM 我们可以把它仅仅看成“40”。*

这些不同的命令帮助了 EVM。下面是他们的意思
T1 1。PUSH1 0x40 - >这意味着将 1 字节的值“0x40”放入堆栈。现在 PUSH1 的十六进制值是“60”。所以命令变成了“6040”。
**2。MSTORE** - >这将分配 0x60 的内存，并将 0x40 移入其中。MSTORE 的十六进制代码是“0x52”。

> 所以这些的最终字节码变成…
> *604052*

这样，每一个字节码都有它们的意义，并告诉 EVM 该做什么。我们不需要知道每一个字节码对编写智能契约意味着什么。仅仅知道它做什么是好的。但是如果你想了解更多，这里有一个链接会很有帮助[ether VM . io]([https://www.ethervm.io/](https://www.ethervm.io/))。

在上一篇文章中我们看到了 ABI。好吧，智能合约只有在有人可以使用时才是好的。所以这里帮助 ABI。

💜**ABI**
——ABI 代表应用二进制接口。ABI 的代码不像字节码那样复杂，是人类可读的。
——ABI 长这样:

> **{
> 【inputs】:
> {
> 【internal type】:" string "、
> 【name】:" _ name "、
> 【type】:" string "
> }、
> {
> 【internal type ":" uint 256 "、
> " name ":" _ favorite number "、
> 【type】:" uint 256 "
> }**

-正如我们所看到的，这些只是 JSON 格式的文件，扩展名为。abi
-如果我们仔细观察，这些 abi 包含类似的类型:“函数”或“uint256”或“字符串”。
-这些文件包含关于智能合约的所有细节，如变量、变量类型、变量名称、函数名称、访问修饰符、可支付或不可支付等等。

Ex->当类型为“uint256”或“string”时，这意味着它是一个变量，但当类型为“function”时，这意味着它是一个函数。

-当 JavaScript 想要与智能合约交互时使用它，这就是 abi 的用武之地。没有 abi，就不会有任何交互，你不能调用任何函数或做任何事情。

💜让我们总结一下…

-智能合约需要字节码和 abi 才能运行。字节码是非人类可读的，它告诉 EVM 如何执行智能合同。
-字节码可以转换成人类可读的操作码。
-字节码中的每个数字在 EVM 中都意味着一个命令。EVM 认为每个数字都是十六进制的。
- ABI 是一个扩展名为. ABI 的 JSON 文件。它包含了关于变量、访问修饰符、函数、它们的返回类型等等的细节。
- ABI 让 JavaScript 调用智能合约中的函数成为可能。

👉请记住，如果您能编写正确的字节码和 ABI，您可以简单地将其部署在链上。

在下一篇文章中，我们将看到如何将智能合约转换成字节码和 ABI。我们还将了解可靠性的基础知识，以及如何编写智能合同和部署。

> **你好，我是 Tanisk Annpurna
> 我发布关于
> 的消息🚀web3、区块链、以太坊
> 🐦智能合同，可靠性
> 🎉JavaScript、ReactJS、NodeJS
> 关注并喜欢更多这样的帖子。！！✌️!！**

> 加入 Coinmonks [电报频道](https://t.me/coincodecap)和 [Youtube 频道](https://www.youtube.com/c/coinmonks/videos)了解加密交易和投资

# 另外，阅读

*   [币安期货交易](https://coincodecap.com/binance-futures-trading)|[3 comas vs Mudrex vs eToro](https://coincodecap.com/mudrex-3commas-etoro)
*   [如何购买 Monero](https://coincodecap.com/buy-monero) | [IDEX 评论](https://coincodecap.com/idex-review) | [BitKan 交易机器人](https://coincodecap.com/bitkan-trading-bot)
*   [CoinDCX 评论](/coinmonks/coindcx-review-8444db3621a2) | [加密保证金交易交易所](https://coincodecap.com/crypto-margin-trading-exchanges)
*   [红狗赌场评论](https://coincodecap.com/red-dog-casino-review) | [Swyftx 评论](https://coincodecap.com/swyftx-review) | [CoinGate 评论](https://coincodecap.com/coingate-review)
*   [Bookmap 评论](https://coincodecap.com/bookmap-review-2021-best-trading-software) | [美国 5 大最佳加密交易所](https://coincodecap.com/crypto-exchange-usa)