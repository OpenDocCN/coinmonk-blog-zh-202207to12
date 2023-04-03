# Solidity 编译器安装简介

> 原文：<https://medium.com/coinmonks/an-intro-to-solidity-compiler-installation-ff6322cf6e6e?source=collection_archive---------43----------------------->

在我们的 YouTube 上观看视频的同时，享受这个流的资源！

YouTube:[https://youtu.be/FsojJtwjiG4](https://youtu.be/FsojJtwjiG4)

不和:【https://discord.gg/J73qhkj7kr】T2

推特:【https://twitter.com/CryptoverseDAO】

linktree:[https://linktr.ee/cryptoversedao](https://linktr.ee/cryptoversedao)

Solidity 编译器安装:

到目前为止，稳健对于智能合约的重要性可能已经很明显了。然而，在任何实体教程中设置实体的环境是很重要的。安装 solidity 编译器的常用方法可以提供其工作的详细印象。以下是设置 Solidity 环境的不同方法及其独特的功能。

版本控制

solidity 区块链设置的第一个例子是版本控制，最重要的是语义版本控制。solidity 的不同版本遵循语义版本化，为相关的发布提供夜间开发构建的便利。每夜开发构建不能保证功能，并且可能包括未记录的和潜在的损坏的修改。此外，可靠性最佳实践还意味着使用最新发布的版本。

再搅拌

Remix 是几乎所有 solidity 教程中推荐的工具，用于快速学习智能合约和 solidity。它提供了一个在线集成开发环境或 IDE 来编写 Solidity smart 契约，然后部署和运行它们。

您可以在线访问 Remix IDE，而不需要任何额外的安装。此外，它还允许离线使用，方便地评估夜间构建，而无需安装各种 Solidity 版本。有趣的是，当用户需要额外的编译选项或必须处理更大的合同时，他们也可以选择命令行 Solidity 编译器软件。

Node.js/国家预防机制

建立使用 Solidity 的环境的最简单的方法是“npm”对于名为 solc-js 的 Solidity 编译器的安装，您可以依赖 npm 来获得更好的便利性和灵活性。与访问编译器的方式相比，solc-js 程序提供的功能也相对有限。用户可以通过使用存储库中的 solc-js 来访问文档，以便灵活学习。

这种在 solidity 教程中设置编译器的方法的重要亮点是 solc-js 编译器是基于 C++solc 的。solc-js 项目利用 Emscripten，并确保两者都利用相似的编译器源代码。它可以直接用于基于 Remix 的 JavaScript 项目。solc-js 存储库提供了使用它所需的文档。

> 交易新手？试试[加密交易机器人](/coinmonks/crypto-trading-bot-c2ffce8acb2a)或者[复制交易](/coinmonks/top-10-crypto-copy-trading-platforms-for-beginners-d0c37c7d698c)

Docker 图像

下一个灵活的设置 solidity 区块链环境的方法是使用 Docker 镜像。用户可以选择拉一个 Docker 图像，然后开始使用它进行 Solidity 编程。使用 Docker image 建立一个坚固的环境最大的好处是步骤简单。第一步包括应用命令来拉 solidity 的 docker 图像。命令是，

$docker pull 以太坊/solc:稳定

Docker 的可靠性示例中的第二步是指下载 Docker 映像后对其进行验证。下面的命令可以帮助你安装一个带有 Docker 镜像的 Solidity 编译器和环境。

$docker 运行以太坊/solc:稳定版

通过完成这两个步骤，您可以找到以下输出，

$ docker 运行以太坊/solc:稳定版

solc，solidity 编译器命令行接口版本:0 . 5 . 2+commit . 1 df 8 f40 c . Linux . g++

![](img/1980e0210aa47a9a7b12b2fc66e7de99.png)

二进制包

学生们也很可能碰到二进制包作为设置坚固性的首选方法。此外，在 solidity 教程中需要注意的是，你可以在 Solidity 官方网站上轻松找到它们。Solidity 的官方网站也有 Ubuntu 的 PPAs，并帮助获得最新的稳定版本。

Solidity 还提供了一个 snap 包，用于 Solidity 安装所有兼容的 Linux 发行版。snap 包支持严格的限制，从而为 snap 包提供了一个高度安全的环境，尽管有一些限制。这些限制仅集中在访问/media 和/home 目录中的文件。

![](img/98bbe5e7694cc81ce92d7b48868068ea.png)