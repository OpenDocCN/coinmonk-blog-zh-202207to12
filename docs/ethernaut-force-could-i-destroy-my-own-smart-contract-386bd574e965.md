# Ethernaut-Force —我可以销毁我自己的智能合同吗？

> 原文：<https://medium.com/coinmonks/ethernaut-force-could-i-destroy-my-own-smart-contract-386bd574e965?source=collection_archive---------38----------------------->

区块链——每个信息被永久保存的地方。永远？你确定吗？如果你的回答是“是”，我很遗憾地说“你错了”。Solidity 为我们提供了具有自我解释名称的功能，该功能允许我们从区块链中删除智能合同。这个函数是*自毁*，这个函数作为另一个智能合约的参数地址。

*自毁*功能从区块链中删除智能合同，并将合同中存储的所有剩余以太网发送到作为参数给出的地址。

该函数可能被恶意使用，以强制向任何合同发送以太网。为什么这会很危险？把额外的以太送到某个地方会有什么问题吗？让我们看看来自[的例子](https://solidity-by-example.org/hacks/self-destruct/)。

Example of malicious use of *selfdestruct* function.

我希望这篇文章对你有用。如果你有任何想法，我如何能使我的帖子更好，请告诉我。我随时准备学习。你可以在 [LinkedIn](https://pl.linkedin.com/in/szymon-skrzy%C5%84ski-881462214) 和 [Telegram](https://t.me/eszymi) 上和我联系。

如果你想和我谈论这个话题或者我写的其他话题，请随意。我乐于交谈。

快乐学习！

> 交易新手？尝试[加密交易机器人](/coinmonks/crypto-trading-bot-c2ffce8acb2a)或[复制交易](/coinmonks/top-10-crypto-copy-trading-platforms-for-beginners-d0c37c7d698c)