# 作为开发人员进入 Web3

> 原文：<https://medium.com/coinmonks/getting-into-web3-as-a-developer-1fa37b15f62b?source=collection_archive---------24----------------------->

进入 Web3 可能会很困难，这取决于你想在这个不确定的分散网络中做什么，从编写智能合同到创建 NFT 市场，从哪里开始呢？

![](img/47dc2020bed5419e3cb60c27066f9851.png)

我们将关注的堆栈(取决于您的需求):

*   HTML 和 CSS(核心技术)
*   JavaScript(编程语言)
*   反应(框架)
*   Next.js(框架)
*   数据库
*   Web3.js/Ethers.js(框架)
*   可靠性(编程语言)

只需学习这个列表中的前 3 个，你就已经能够制作网站和应用程序，添加 Web3.js 和 Solidity，你就有了一个能够与 Web3 交互的前端和后端功能的 Web 应用程序。

> 不知道什么时候买卖 cryp，试试[复制交易](http://coincodecap.com/go/bityard)。

# HTML/CSS

定义 *: HTML* (超文本标记语言)和 *CSS* (级联样式表)是构建网页的两大核心技术。

HTML 是 web 浏览器读取的内容，而 CSS 是 HTML 的组成部分。基本上 CSS 使网站和网络应用程序更漂亮。

为什么你应该知道这两个核心技术？它们是我们将要研究的基础材料。

```
<!DOCTYPE html PUBLIC “-//IETF//DTD HTML 2.0//EN”>
 <HTML>
 <BODY>
 <H1>Hi</H1>
 <P>This is a paragraph.</P> 
 </BODY>
 </HTML>
```

# Java Script 语言

他们中最酷的爸爸。在过去的 25 年中，JavaScript 越来越受欢迎，似乎仍然是首选。JavaScript 让你的网站流行起来，它让一切变得生动。它在你的网站/网络应用中创造了一个更加生动和互动的体验。JavaScript 的主要用途是 web 开发、web 应用程序、游戏开发等等。

在纯代码中，“Hello World”应该是这样的。

```
console.log(“Hello World!”);
```

在浏览器中，它需要看起来更像这样。

```
<!DOCTYPE html>
<html lang=”en-US”>
<head>
 <meta charset=”UTF-8">
 <meta http-equiv=”X-UA-Compatible” content=”IE=edge”>
 <meta name=”viewport” content=”width=, initial-scale=1.0">
 <title>A Simple Js Program</title>
</head>
<body>
 <h2 id=”demo”></h2><script>
 document.getElementById(‘demo’).innerHTML = “Hello World!”;
 </script>
</body>
</html>
```

我们的 JavaScript 位于两个脚本标记之间。

# 反应

拥有的超能力。React 真的很神奇，正如你可能已经从名字中发现的那样，它是反应式的。由 Meta(脸书)建造和维护。它是一个 JavaScript 库/框架，允许我们使用 React.js 在 Android 和 iOS 中创建原生移动应用程序。React 使用一种称为 JSX (JavaScript 语法扩展)的 HTML-in-JavaScript 语法。熟悉 HTML 和 JavaScript 将有助于你学习 JSX。

```
const root = ReactDOM.createRoot(document.getElementById('root'));
root.render(<h1>Hello, world!</h1>);
```

如你所见，我们的 HTML 现在存在于代码中。

# Next.js

具有一些漂亮特性的服务器端渲染框架，例如:

*   热代码重载
*   单个文件组件
*   自动选路
*   服务器渲染
*   自动吐码。

当检测到更改时，热代码重新加载将重新加载页面。

单个文件组件使生活变得更加容易，尤其是在设计特定组件的样式时。

自动路由，URL 被映射到文件系统，这样可以节省配置时间。

服务器呈现，允许您在将 HTML 发送到客户端之前在服务器端呈现 React 组件。

自动代码溢出有几种方式。加载页面只会加载特定页面所需的 JavaScript。这允许快速的页面加载，并且一旦被触发，将只为将来的页面请求 JavaScript。

# MongoDB

MongoDB 是一个 NoSQL 数据库，在 web 开发中很受欢迎，尤其是在 web3 领域，它用于存储和检索与分散式应用程序(dApps)和区块链项目相关的数据。

它以其灵活性和可伸缩性而闻名，这使得它非常适合处理复杂和分层的数据结构。

MongoDB 不同于传统的关系数据库，它使用基于文档的数据模型，可以水平伸缩，并且在数据操作方面更加灵活。

它的易用性和活跃的开发者社区也使它成为 dApp 开发者的好选择。

总的来说，MongoDB 是一个强大而灵活的数据库管理系统，非常适合在 web3 领域使用。其灵活的数据模型、可伸缩性和易用性使其成为需要以各种不同方式存储和检索大量数据的 dApp 开发人员的良好选择

# Web3.js

Web3.js 是一个 JavaScript 库，允许开发人员与分散式应用程序(dApps)和区块链网络进行交互。对于任何在 web3 领域工作的人来说，它都是一个关键工具，因为它使开发人员能够构建能够访问区块链数据并触发智能合约功能的 dApps。

Web3.js 用于创建 dApp 和区块链网络之间的连接，并允许开发人员发送交易和从智能合同中读取数据。它可以用于各种区块链网络，包括以太坊和 Hyperledger Fabric，并经常与其他技术(如 IPFS)结合使用。

使用 Web3.js 对 dApp 开发有几个好处。它允许开发人员轻松访问和操作区块链上的数据，并使他们能够触发智能合约功能和处理交易。它还为与不同的区块链网络进行交互提供了一个简单而一致的接口，使开发人员更容易构建可以部署在多个网络上的 dApps。

总的来说，Web3.js 对于在 Web3 领域工作的任何人来说都是一个必不可少的工具，因为它使开发人员能够构建可以与区块链网络和智能合同交互的 dApps。

Ethers.js 是另一种用法。

# 固态

Solidity 是一种编程语言，用于为以太坊区块链编写智能合约。它允许开发者创建可以存储和操作以太坊区块链上的数据的自动执行合同，并且是任何在 web3 领域工作的人的关键工具。

稳健对于撰写智能合同有几个好处。它是一种安全可靠的语言，已经过彻底的测试和审查，这有助于确保以可靠方式编写的合同将按预期发挥作用。它还具有强大的类型系统和对复杂逻辑的支持，这使得它非常适合于实施复杂的规则和业务逻辑。

除了安全性和可靠性之外，Solidity 还具有高度的灵活性，可以用来创建各种各样的智能合同。从在区块链上存储数据的简单合约，到处理复杂逻辑和与外部系统交互的更复杂合约，Solidity 提供了构建强大而可靠的智能合约所需的工具和功能。

总的来说，Solidity 对于任何在 web3 领域工作的人来说都是一个必不可少的工具，因为它允许开发人员创建安全可靠的智能合约，可以部署在以太坊区块链上。

# 总而言之:

通过学习这些编程语言、技能并知道如何在 web3 空间的上下文中使用它们，您将获得有价值的技能，这些技能可以应用于广泛的基于加密的项目和分散的应用程序。

是什么阻碍了你？

> 加入 Coinmonks [电报频道](https://t.me/coincodecap)和 [Youtube 频道](https://www.youtube.com/c/coinmonks/videos)了解加密交易和投资

# 另外，阅读

*   [Bookmap 点评](https://coincodecap.com/bookmap-review-2021-best-trading-software) | [美国 5 大最佳加密交易所](https://coincodecap.com/crypto-exchange-usa)
*   [加密交易机器人](/coinmonks/crypto-trading-bot-c2ffce8acb2a) | [硬币门评论](https://coincodecap.com/coingate-review)
*   最佳加密[硬件钱包](/coinmonks/hardware-wallets-dfa1211730c6) | [Bitbns 评论](/coinmonks/bitbns-review-38256a07e161)
*   [新加坡十大最佳加密交易所](https://coincodecap.com/crypto-exchange-in-singapore) | [购买 AXS](https://coincodecap.com/buy-axs-token)
*   [红狗赌场评论](https://coincodecap.com/red-dog-casino-review) | [Swyftx 评论](https://coincodecap.com/swyftx-review)