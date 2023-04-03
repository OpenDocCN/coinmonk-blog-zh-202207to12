# 如何创建 UUPS 代理

> 原文：<https://medium.com/coinmonks/how-to-create-an-uups-proxy-66eca257b2f9?source=collection_archive---------6----------------------->

# 什么是 UUPS 代理？

UUPS 代表通用可升级代理标准，最初记录在 [EIP1822](https://eips.ethereum.org/EIPS/eip-1822) 中。在这个代理模式中，升级实现契约地址的责任在于实现契约本身。相反，在透明代理模式中，升级实现契约地址的责任是代理契约和代理管理契约的责任。关于透明代理模式的更详细的解释，请查看[这篇](/@HashHaran/essential-guide-to-smart-contract-upgradeability-a257dac36525)文章。

当我们尝试升级使用 UUPS 代理模式部署的智能契约时，代理契约会调用实现契约。它检查升级的用户是否有权限升级实现，然后继续将实现合同地址更改为新的地址。

与透明代理模式相比，推荐使用 UUPS 代理模式，因为它消除了透明代理模式中所需的代理管理契约，从而节省了初始部署期间的开销。使用 UUPS 代理模式时，有一点需要注意。如果您使用非 UUPS 兼容的实现来升级您的代理，您将永远无法升级它。这样的错误可能是致命的，因为你已经失去了可升级性，这是我们修复这种错误的后门。

让我们开始使用 UUPS 代理模式部署一个智能契约。

这个 [Github 仓库](https://github.com/HashHaran/hardhat-upgrades)有这篇文章的代码，可以随意保存以备将来使用。如果您不想从头开始进行设置，可以克隆它。

# 项目设置

我们将使用[安全帽](https://hardhat.org/)用于我们的以太坊开发工作流程。在本演练中，我在 windows 机器上使用 WSL2。如果使用不同的设置，您可以稍微调整这些步骤。我们开始吧！

运行以下命令创建一个目录，并将其初始化为节点项目。

```
**mkdir hardhat-upgrades && cd hardhat-upgradesnpm init -y**
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

# 使用 UUPS 代理部署的实施合同

在 contracts 文件夹中，为我们将要部署 UUPS 代理的实现契约创建一个文件夹。

```
**mkdir contracts/uups**
```

在 *contracts* 目录下创建一个名为 *VersionAware.sol* 的文件，并将以下代码复制粘贴到其中。

```
*// SPDX-License-Identifier: Unlicense*pragma solidity ^0.8.0;abstract contract VersionAware {string public versionAwareContractName;function getContractNameWithVersion()externalpurevirtualreturns (string memory);}
```

在这里，我们创建了实现契约的基本框架，以便在部署和升级之后，我们可以轻松地看到版本升级。

在 *contracts/uups* 目录下创建两个名为 Uups *ProxyPatternV1.sol* 和 Uups *ProxyPatternV2.sol* 的文件，并将下面的代码复制粘贴到其中。

*UupsProxyPatternV1.sol*

```
*// SPDX-License-Identifier: Unlicense* pragma solidity ^0.8.0;import {UUPSUpgradeable} from "@openzeppelin/contracts-upgradeable/proxy/utils/UUPSUpgradeable.sol";
import {OwnableUpgradeable} from "@openzeppelin/contracts-upgradeable/access/OwnableUpgradeable.sol";
import {VersionAware} from "../VersionAware.sol";contract UupsProxyPatternV1 is
UUPSUpgradeable,
OwnableUpgradeable,
VersionAware{constructor() {
_disableInitializers();
}function initialize() external initializer {
versionAwareContractName = "UUPS Proxy Pattern: V1";
*///@dev as there is no constructor, we need to initialise the OwnableUpgradeable explicitly* __Ownable_init();
}*///@dev required by the OZ UUPS module* function _authorizeUpgrade(address) internal override onlyOwner {}function getContractNameWithVersion()
public
pure
override
returns (string memory){
return "UUPS Proxy Pattern: V1";
}
}
```

*UupsProxyPatternV2.sol*

```
*// SPDX-License-Identifier: Unlicense* pragma solidity ^0.8.0;import {UUPSUpgradeable} from "@openzeppelin/contracts-upgradeable/proxy/utils/UUPSUpgradeable.sol";
import {OwnableUpgradeable} from "@openzeppelin/contracts-upgradeable/access/OwnableUpgradeable.sol";
import {VersionAware} from "../VersionAware.sol";contract UupsProxyPatternV2 is
UUPSUpgradeable,
OwnableUpgradeable,
VersionAware{constructor() {
_disableInitializers();
}function initialize() external reinitializer(2) {
versionAwareContractName = "UUPS Proxy Pattern: V2";
*///@dev as there is no constructor, we need to initialise the OwnableUpgradeable explicitly* __Ownable_init();
}*///@dev required by the OZ UUPS module* function _authorizeUpgrade(address) internal override onlyOwner {}function getContractNameWithVersion()
public
pure
override
returns (string memory){
return "UUPS Proxy Pattern: V2";
}
}
```

注意，在两个契约中，我们都继承了*uupsupgradable . sol*，这使得契约 UUPS 升级兼容。从该契约继承要求我们实现 *_authorizeUpgrade* 函数，该函数用于决定是否允许调用者升级。

在这里，我试图解释上述合同的一些高级细节。如果你在第一次阅读时理解有困难，考虑现在跳过它，以后再回来。典型的可升级协定不应该有构造函数，因为实现协定的构造函数永远不能在代理协定的上下文中运行。我们在这里添加了一个安全的构造函数，因为它没有设置任何存储变量。不初始化就退出协定会造成安全威胁。在构造函数中调用*_ disable initializer*方法使得实现契约不可初始化，这比让实现没有构造函数也不初始化要安全得多。注意，我在 V1 的*初始化*方法中使用了*初始化器*修饰符。这个修饰符确保这个 initialize 方法只被调用一次，就像 solidity 确保构造函数一样。还要注意，在 V2 中，*初始化*方法的修饰符是*重新初始化器(2)。*这里 2 代表实现契约的版本。必须使用*重新初始化器*修改器，而不是*初始化器*，因为代理契约已经在 V1 初始化过一次。关于所有可升级智能契约都应该继承的 *Initializer.sol* 契约，还有更多事情和细节需要了解。我将在以后写更多关于它的详细文章。

现在，运行以下命令来编译智能合约。

```
**npx hardhat compile**
```

# UUPS 代理部署和升级

首先，我们将使用 UUPS 代理部署版本 1，然后将其升级到版本 2。我们将使用一个安全帽脚本来做到这一点。在 scripts 文件夹中创建一个名为 *uups.js* 的脚本，并将下面的代码复制粘贴到其中。

```
const { ethers, upgrades } = require("hardhat");async function main() {const UupsProxyPatternV1 = await ethers.getContractFactory("UupsProxyPatternV1");const uupsProxyPatternV1 = await upgrades.deployProxy(UupsProxyPatternV1, [], {kind: 'uups', unsafeAllow: ['constructor']});await uupsProxyPatternV1.deployed();console.log(`UUPS Proxy Pattern V1 is deployed to proxy address: ${uupsProxyPatternV1.address}`);let versionAwareContractName = await uupsProxyPatternV1.getContractNameWithVersion();console.log(`UUPS Pattern and Version: ${versionAwareContractName}`);const UupsProxyPatternV2 = await ethers.getContractFactory("UupsProxyPatternV2");const upgraded = await upgrades.upgradeProxy(uupsProxyPatternV1.address, UupsProxyPatternV2, {kind: 'uups', unsafeAllow: ['constructor'], call: 'initialize'});console.log(`UUPS Proxy Pattern V2 is upgraded in proxy address: ${upgraded.address}`);versionAwareContractName = await upgraded.getContractNameWithVersion();console.log(`UUPS Pattern and Version: ${versionAwareContractName}`);}*// We recommend this pattern to be able to use async/await everywhere and properly handle errors.*main().catch((error) => {
console.error(error);
process.exitCode = 1;
});
```

我们首先部署带有代理的智能合约的版本 1。Open zeppelin 升级 *deployProxy* 方法为我们解决了这个问题。一旦用代理部署了版本 1，我们就在代理上调用*getContractNameWithVersion*函数。该方法将根据我们在契约的版本 1 中使该方法返回的内容返回一个字符串。然后我们继续用*升级代理*方法升级这个合同。升级结束后，我们再次调用*getContractNameWithVersion*函数来查看返回的字符串的变化。让我们运行脚本，看看结果。使用以下命令运行脚本。

```
**npx hardhat run scripts/uups.js**
```

您应该会在控制台上看到以下内容。

```
Warning: Potentially unsafe deployment of UupsProxyPatternV1You are using the `unsafeAllow.constructor` flag.Warning: A proxy admin was previously deployed on this networkThis is not natively used with the current kind of proxy ('uups').
    Changes to the admin will have no effect on this new proxy.UUPS Proxy Pattern V1 is deployed to proxy address: 0xe7f1725E7734CE288F8367e1Bb143E90bb3F0512
UUPS Pattern and Version: UUPS Proxy Pattern: V1
Warning: Potentially unsafe deployment of UupsProxyPatternV2You are using the `unsafeAllow.constructor` flag.UUPS Proxy Pattern V2 is upgraded in proxy address: 0xe7f1725E7734CE288F8367e1Bb143E90bb3F0512
UUPS Pattern and Version: UUPS Proxy Pattern: V2
```

耶！结果在意料之中。实施合同已成功升级到版本 2。这就是您如何使用 UUPS 代理模式部署和升级未来版本的契约，无论您的智能契约有多复杂。

感谢阅读。

# 我是谁？

我是一名全栈区块链开发者，对构建一个去中心化的、潜在的更具包容性的未来充满热情。有区块链发展的需求吗？

取得联系:📧hariharan @ aluminum . iitm . AC . in

[Github](https://github.com/HashHaran)

> 交易新手？尝试[加密交易机器人](/coinmonks/crypto-trading-bot-c2ffce8acb2a)或[复制交易](/coinmonks/top-10-crypto-copy-trading-platforms-for-beginners-d0c37c7d698c)