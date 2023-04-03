# 如何创建透明代理

> 原文：<https://medium.com/coinmonks/how-to-create-a-transparent-proxy-1683d21468bf?source=collection_archive---------5----------------------->

代理模式是一种智能合约设计模式，用于使智能合约可升级。请注意，智能合约本身是不可变的，为了使它们可升级，一些高级的可靠性基础工作是必要的。有关智能合同升级能力的详细概述和透明代理模式的解释，请阅读[这篇](/@HashHaran/essential-guide-to-smart-contract-upgradeability-a257dac36525)更早的文章。在本文中，我们将重点关注使用透明代理模式实际部署您的契约。

这个 [Github 仓库](https://github.com/HashHaran/hardhat-upgrades)有这篇文章的代码，可以随意保存以备将来使用。如果您不想从头开始进行设置，可以克隆它。

# 项目设置

我们将使用[安全帽](https://hardhat.org/)用于我们的以太坊开发工作流程。在本演练中，我在 windows 机器上使用 WSL2。如果使用不同的设置，您可以稍微调整这些步骤。我们开始吧！

运行以下命令创建一个目录，并将其初始化为节点项目。

```
**mkdir hardhat-upgrades && cd hardhat-upgrades****npm init -y**
```

在这个目录中安装 hardhat。

```
**npm i --save-dev hardhat**
```

在目录中初始化一个 hardhat 类型的脚本项目。运行下面的命令并选择“*创建一个 TypeScript 项目”*选项。对于其余的提示，请使用默认参数。

```
**npx hardhat**
```

安装开放的 zeppelin hardhat 升级插件，我们将使用它来轻松部署我们的代理。

```
**npm i --save-dev @openzeppelin/hardhat-upgrades**
```

还要安装来自 Open zeppelin 的可升级合同，它是 Open zeppelin 合同的可升级对应物。

```
**npm i --save-dev @openzeppelin/contracts-upgradeable**
```

将以下内容复制并粘贴到 hardhat.config.ts 文件中。这将做这个项目所需的基本安全帽设置。

```
import { HardhatUserConfig } from "hardhat/config";import "@nomicfoundation/hardhat-toolbox";import "@openzeppelin/hardhat-upgrades";import "@typechain/hardhat";const config: HardhatUserConfig = {solidity: {version: "0.8.9",settings: {optimizer: {enabled: true,runs: 200,},},},defaultNetwork: "localhost",};export default config;
```

# 使用透明代理部署的实施合同

在 contracts 文件夹中为我们将要部署透明代理的实现契约创建一个文件夹。

```
**mkdir contracts/transparent**
```

在 *contracts* 目录下创建一个名为 *VersionAware.sol* 的文件，并将以下代码复制粘贴到其中。

```
*// SPDX-License-Identifier: Unlicense*pragma solidity ^0.8.0;abstract contract VersionAware {string public versionAwareContractName;function getContractNameWithVersion()externalpurevirtualreturns (string memory);}
```

在这里，我们创建了实现契约的基本框架，以便在部署和升级之后，我们可以轻松地看到版本升级。

在 *contracts/transparent* 目录下创建两个名为*transparentproxypatternv 1 . sol*和*transparentproxypatternv 2 . sol*的文件，并将下面的代码复制粘贴到其中。

*transparentproxypatternv 1 . sol*

```
*// SPDX-License-Identifier: Unlicense*pragma solidity ^0.8.0;import {ERC1967UpgradeUpgradeable} from "@openzeppelin/contracts-upgradeable/proxy/ERC1967/ERC1967UpgradeUpgradeable.sol";import {VersionAware} from "../VersionAware.sol";contract TransparentProxyPatternV1 is ERC1967UpgradeUpgradeable, VersionAware {constructor() {_disableInitializers();}function initialize() external initializer {versionAwareContractName = "Transparent Proxy Pattern: V1";}function getContractNameWithVersion()publicpureoverridereturns (string memory){return "Transparent Proxy Pattern: V1";}}
```

*transparentproxypatternv 2 . sol*

```
*// SPDX-License-Identifier: Unlicense*pragma solidity ^0.8.0;import {ERC1967UpgradeUpgradeable} from "@openzeppelin/contracts-upgradeable/proxy/ERC1967/ERC1967UpgradeUpgradeable.sol";import {VersionAware} from "../VersionAware.sol";contract TransparentProxyPatternV2 is ERC1967UpgradeUpgradeable, VersionAware {constructor() {_disableInitializers();}function initialize() external reinitializer(2) {versionAwareContractName = "Transparent Proxy Pattern: V2";}function getContractNameWithVersion()publicpureoverridereturns (string memory){return "Transparent Proxy Pattern: V2";}}
```

我们有两个实现合同版本 1 和 2。请注意，版本 1 和版本 2 的结构不必相同，升级也能工作。请注意，这两个合同都是从开放的 zeppelin 可升级包的*ERC 1967 可升级*合同继承的。阅读本文顶部链接的早期文章，了解更多关于 ERC1967 标准的信息。

我们有两个实现合同版本 1 和 2。请注意，版本 1 和版本 2 的结构不必相同，升级也能工作。注意，这两个合同都继承了来自开放的 zeppelin 可升级包的*ERC 1967 可升级*合同。阅读本文顶部链接的早期文章，了解更多关于 ERC1967 标准的信息。

在这里，我试图解释上述合同的一些高级细节。如果你在第一次阅读时理解有困难，考虑现在跳过它，以后再回来。典型的可升级协定不应该有构造函数，因为实现协定的构造函数永远不能在代理协定的上下文中运行。我们在这里添加了一个安全的构造函数，因为它没有设置任何存储变量。不初始化就退出协定会造成安全威胁。在构造函数中调用*_ disable initializer*方法使得实现契约不可初始化，这比让实现没有构造函数也不初始化要安全得多。注意，我在 V1 的*初始化*方法中使用了*初始化器*修饰符。这个修饰符确保这个 initialize 方法只被调用一次，就像 solidity 确保构造函数一样。还要注意，在 V2 中，*初始化*方法的修饰符是*重新初始化器(2)。*此处 2 代表实施合同的版本。必须使用*重新初始化器*修改器，而不是*初始化器*，因为代理契约已经在 V1 初始化过一次。关于所有可升级智能契约都应该继承的 Initializer.sol 契约，还有更多事情和细节需要了解。以后我会写更多关于它的详细文章。

现在，运行以下命令来编译智能合约。

```
**npx hardhat compile**
```

# 透明代理部署和升级

首先，我们将使用透明代理部署版本 1，然后将其升级到版本 2。我们将使用一个安全帽脚本来做到这一点。在脚本文件夹中创建一个名为 *transparent.js* 的脚本，并将下面的代码复制粘贴到其中。

```
const { ethers, upgrades } = require("hardhat");async function main() {const TransparentProxyPatternV1 = await ethers.getContractFactory("TransparentProxyPatternV1");const transparentProxyPatternV1 = await upgrades.deployProxy(TransparentProxyPatternV1, [], {kind: 'transparent', unsafeAllow: ['constructor']});await transparentProxyPatternV1.deployed();console.log(`Transparent Proxy Pattern V1 is deployed to proxy address: ${transparentProxyPatternV1.address}`);let versionAwareContractName = await transparentProxyPatternV1.getContractNameWithVersion();console.log(`Proxy Pattern and Version: ${versionAwareContractName}`);const TransparentProxyPatternV2 = await ethers.getContractFactory("TransparentProxyPatternV2");const upgraded = await upgrades.upgradeProxy(transparentProxyPatternV1.address, TransparentProxyPatternV2, {kind: 'transparent', unsafeAllow: ['constructor'], call: 'initialize'});console.log(`Transparent Proxy Pattern V2 is upgraded in proxy address: ${upgraded.address}`);versionAwareContractName = await upgraded.getContractNameWithVersion();console.log(`Proxy Pattern and Version: ${versionAwareContractName}`);}main().catch((error) => {console.error(error);process.exitCode = 1;});
```

我们首先部署带有代理和代理管理的智能合约版本 1。Open zeppelin 升级 *deployProxy* 方法为我们解决了所有这些问题。一旦用代理部署了版本 1，我们就在代理上调用*getContractNameWithVersion*函数。该方法将根据我们在契约的版本 1 中使该方法返回的内容返回一个字符串。然后我们继续用*升级代理*方法升级这个合同。升级结束后，我们再次调用*getContractNameWithVersion*函数来查看返回的字符串的变化。让我们运行脚本，看看结果。使用以下命令运行脚本。

```
**npx hardhat run scripts/transparent.js**
```

您应该会在控制台上看到以下内容。

```
Warning: Potentially unsafe deployment of TransparentProxyPatternV1You are using the `unsafeAllow.constructor` flag.Transparent Proxy Pattern V1 is deployed to proxy address: 0x9fE46736679d2D9a65F0992F2272dE9f3c7fa6e0
Proxy Pattern and Version: Transparent Proxy Pattern: V1
Warning: Potentially unsafe deployment of TransparentProxyPatternV2You are using the `unsafeAllow.constructor` flag.Transparent Proxy Pattern V2 is upgraded in proxy address: 0x9fE46736679d2D9a65F0992F2272dE9f3c7fa6e0
Proxy Pattern and Version: Transparent Proxy Pattern: V2
```

耶！结果在意料之中。实施合同已成功升级到版本 2。这就是您如何使用透明代理模式部署和升级未来版本的契约，无论您的智能契约变得多么复杂。

感谢阅读。

# 我是谁？

我是一名全栈区块链开发者，对构建一个去中心化的、潜在的更具包容性的未来充满热情。有区块链发展的需求吗？

取得联系:📧hariharan @ aluminum . iitm . AC . in

[Github](https://github.com/HashHaran)

> 交易新手？尝试[加密交易机器人](/coinmonks/crypto-trading-bot-c2ffce8acb2a)或[复制交易](/coinmonks/top-10-crypto-copy-trading-platforms-for-beginners-d0c37c7d698c)