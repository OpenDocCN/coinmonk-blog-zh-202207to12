# CSC101- Slither

> 原文：<https://medium.com/coinmonks/csc101-slither-22a04ec8ac6f?source=collection_archive---------21----------------------->

我们已经在前面的教程中讨论了智能合同审计和常见漏洞。在这个实践教程中，我们想使用 slither 审计我们的智能合同。

![](img/78db353f85d00a7149d8bc2b6bc3c9de.png)

## 滑行

Slither 是一个用 Python 3 编写的 Solidity 静态分析框架。它运行一套漏洞检测器，打印关于合同细节的可视信息，并提供一个 API 来轻松编写定制分析。Slither 使开发人员能够发现漏洞，增强他们对代码的理解，并快速原型化定制分析。

# 特征

*   以较低的误报率检测易受攻击的可靠性代码(参见[奖杯](https://github.com/crytic/slither/blob/master/trophies.md)列表)
*   标识源代码中错误条件出现的位置
*   轻松集成到持续集成和块菌构建中
*   内置“打印机”可快速报告重要的合同信息
*   用 Python 编写定制分析的检测器 API
*   能够分析坚实度> = 0.4 的合同
*   中间表示( [SlithIR](https://github.com/trailofbits/slither/wiki/SlithIR) )支持简单、高精度的分析
*   正确解析 99.9%的公共安全代码
*   每个合同的平均执行时间不到 1 秒

## 装置

要在终端类型中安装 Slither:

```
pip3 install slither-analyzer
```

你也可以从 github 库安装它:

```
git clone https://github.com/crytic/slither.git && cd slither
python3 setup.py install
```

**注意:**如果你喜欢通过 git 安装 Slither，Slither 开发团队推荐使用 Python 虚拟环境，详见[开发者安装说明](https://github.com/trailofbits/slither/wiki/Developer-installation)。

你也可以使用 Docker 安装 slither:

使用`[eth-security-toolbox](https://github.com/trailofbits/eth-security-toolbox/)` docker 图像。它在一个映像中包含了我们所有的安全工具和 Solidity 的每个主要版本。`/home/share`将被安装到`/share`的集装箱中。

```
docker pull trailofbits/eth-security-toolbox
```

要共享容器中的目录:

```
docker run -it -v /home/share:/share trailofbits/eth-security-toolbox
```

## 使用

在 Truffle/Embark/Dapp/ether lime/hard hat 应用程序上运行 Slither:

```
slither .
```

对单个文件运行 Slither:

```
slither tests/uninitialized.sol
```

## 综合

*   对于 GitHub 动作集成，使用[滑动动作](https://github.com/marketplace/actions/slither-action)。
*   要生成降价报告，使用`slither [target] --checklist`。
*   要生成 GitHub 源代码高亮显示的 Markdown，使用`slither [target] --checklist --markdown-root https://github.com/ORG/REPO/blob/COMMIT/`(替换`ORG`、`REPO`、`COMMIT`)

使用[solc——如果您的合同需要旧版本的 solc，请选择](https://github.com/crytic/solc-select)。有关其他配置，请参见[用法](https://github.com/trailofbits/slither/wiki/Usage)文档。

我们可以在安全帽上使用滑板。就这么办吧！

`npx hardhat compile`

## 分析/审计

要分析智能合同，只需在终端中键入:

```
slither .
```

# 研究修复

通过使用 google 找到 slither 标记的问题，并研究和测试可能的修复方法，优先修复最严重的错误。

> 记住每次你改变一个 solidity 文件时，你都必须在扫描前清理并重新编译。

这个阶段可能非常复杂，为了完全理解、测试和建模控制流，您需要具备良好的开发知识水平，以便真正测试逻辑并确保它按预期工作。

实现这一点的最佳方式是通过自动化测试脚本，关于这一点的一篇好文章可以在[这里](https://ethereum.org/en/developers/docs/smart-contracts/testing/)找到。

> 交易新手？尝试[加密交易机器人](/coinmonks/crypto-trading-bot-c2ffce8acb2a)或[复制交易](/coinmonks/top-10-crypto-copy-trading-platforms-for-beginners-d0c37c7d698c)