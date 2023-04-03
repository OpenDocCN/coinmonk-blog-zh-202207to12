# 如何创建信标代理

> 原文：<https://medium.com/coinmonks/how-to-create-a-beacon-proxy-3d55335f7353?source=collection_archive---------1----------------------->

Beacon proxy 是一种代理模式，其中多个代理引用单个智能协定，以便为它们提供实现协定的地址。为代理提供实现契约地址的契约称为信标契约。

关于什么是代理以及为什么应该使用它的更详细的解释，请查看这篇更早的文章。

当您有多个代理引用一个随我们的进展而升级的实现契约时，就使用信标代理。如果您要使用[透明代理](/@HashHaran/how-to-create-a-transparent-proxy-1683d21468bf)或 [UUPS 代理](/@HashHaran/how-to-create-an-uups-proxy-66eca257b2f9)，您将不得不一个接一个地升级所有代理中的实现契约地址。如果您的项目需要多个代理来引用同一个实现，那么 beacon 代理是一个不错的选择。

当我们试图升级使用信标模式部署的智能契约时，我们调用信标契约并升级存储在那里的实现契约地址。只有所有者地址能够进行这种升级，默认情况下，这是部署信标的地址。一旦该信标被更新，引用该信标的所有代理将开始引用新的契约，并且代理将被立即升级到新的实现。

让我们开始使用 UUPS 代理模式部署一个智能契约。

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
import { HardhatUserConfig } from "hardhat/config";
import "@nomicfoundation/hardhat-toolbox";
import "@openzeppelin/hardhat-upgrades";
import "@typechain/hardhat";const config: HardhatUserConfig = {
solidity: {
version: "0.8.9",
settings: {
optimizer: {
enabled: true,
runs: 200,
},
},
},
defaultNetwork: "localhost",
};export default config;
```

# 部署信标代理的实施合同

在 contracts 文件夹中，为我们将要部署信标代理的实现契约创建一个文件夹。

```
**mkdir contracts/beacon**
```

在 *contracts* 目录下创建一个名为 *VersionAware.sol* 的文件，并将以下代码复制粘贴到其中。

```
*// SPDX-License-Identifier: Unlicense* pragma solidity ^0.8.0;abstract contract VersionAware {string public versionAwareContractName;function getContractNameWithVersion() external pure virtual
returns (string memory);
}
```

在这里，我们创建了实现契约的基本框架，以便在部署和升级之后，我们可以轻松地看到版本升级。

在 *contracts/beacon* 目录下创建两个名为 *BeaconProxyPatternV1.sol* 和 *BeaconProxyPatternV2.sol* 的文件，并将下面的代码复制粘贴到其中。

*BeaconProxyPatternV1.sol*

```
*// SPDX-License-Identifier: Unlicense*pragma solidity ^0.8.0;import {Initializable} from "@openzeppelin/contracts-upgradeable/proxy/utils/Initializable.sol";import {VersionAware} from "../VersionAware.sol";contract BeaconProxyPatternV1 is Initializable, VersionAware {constructor() {
_disableInitializers();
}function initialize() external initializer {
versionAwareContractName = "Beacon Proxy Pattern: V1";
}function getContractNameWithVersion() public pure override returns (string memory){
return "Beacon Proxy Pattern: V1";
}
}
```

*BeaconProxyPatternV2.sol*

```
*// SPDX-License-Identifier: Unlicense* pragma solidity ^0.8.0;import {Initializable} from "@openzeppelin/contracts-upgradeable/proxy/utils/Initializable.sol";import {VersionAware} from "../VersionAware.sol";contract BeaconProxyPatternV2 is Initializable, VersionAware {constructor() {
_disableInitializers();
}function initialize() external initializer {
versionAwareContractName = "Beacon Proxy Pattern: V2";
}function getContractNameWithVersion() public pure override returns (string memory){
return "Beacon Proxy Pattern: V2";
}
}
```

我们的两个契约都是简单的可初始化契约，我们现在将使用 beacon 代理模式来部署它们。

在这里，我试图解释上述合同的一些高级细节。如果你在第一次阅读时理解有困难，考虑现在跳过它，以后再回来。典型的可升级协定不应该有构造函数，因为实现协定的构造函数永远不能在代理协定的上下文中运行。我们在这里添加了一个安全的构造函数，因为它没有设置任何存储变量。不初始化就退出协定会造成安全威胁。在构造函数中调用*_ disable initializer*方法使得实现契约不可初始化，这比让实现没有构造函数也不初始化要安全得多。注意，我在 V1 的*初始化*方法中使用了*初始化器*修饰符。这个修饰符确保这个 initialize 方法只被调用一次，就像 solidity 确保构造函数一样。还要注意，在 V2 中，*初始化*方法的修饰符是*重新初始化器(2)。*这里 2 代表实现契约的版本。必须使用*重新初始化器*修改器，而不是*初始化器*，因为代理契约已经在 V1 初始化过一次。关于所有可升级智能契约都应该继承的 Initializer.sol 契约，还有更多事情和细节需要了解。我将在以后写更多关于它的详细文章。

现在，运行以下命令来编译智能合约。

```
**npx hardhat compile**
```

# 信标代理部署和升级

首先，我们将使用 Beacon 代理部署版本 1，然后将其升级到版本 2。我们将使用一个安全帽脚本来做到这一点。在 scripts 文件夹中创建一个名为 *beacon.js* 的脚本，并将下面的代码复制粘贴到其中。

```
const { ethers, upgrades } = require("hardhat");async function main() {const BeaconProxyPatternV1 = await ethers.getContractFactory("BeaconProxyPatternV1");
const beacon = await upgrades.deployBeacon(BeaconProxyPatternV1, {unsafeAllow: ['constructor']});
await beacon.deployed();
console.log(`Beacon with Beacon Proxy Pattern V1 as implementation is deployed to address: ${beacon.address}`);
const beaconProxy1 = await upgrades.deployBeaconProxy(beacon.address, BeaconProxyPatternV1, []);let versionAwareContractName = await beaconProxy1.getContractNameWithVersion();console.log(`Proxy Pattern and Version from Proxy 1 Implementation: ${versionAwareContractName}`);const beaconProxy2 = await upgrades.deployBeaconProxy(beacon.address, BeaconProxyPatternV1, []);versionAwareContractName = await beaconProxy2.getContractNameWithVersion();console.log(`Proxy Pattern and Version from Proxy 2 Implementation: ${versionAwareContractName}`);const BeaconProxyPatternV2 = await ethers.getContractFactory("BeaconProxyPatternV2");const upgradedBeacon = await upgrades.upgradeBeacon(beacon.address, BeaconProxyPatternV2, {unsafeAllow: ['constructor']});console.log(`Beacon upgraded with Beacon Proxy Pattern V2 as implementation at address: ${upgradedBeacon.address}`);versionAwareContractName = await beaconProxy1.getContractNameWithVersion();console.log(`Proxy Pattern and Version from Proxy 1 Implementation: ${versionAwareContractName}`);versionAwareContractName = await beaconProxy2.getContractNameWithVersion();console.log(`Proxy Pattern and Version from Proxy 2 Implementation: ${versionAwareContractName}`);versionAwareContractName = await beaconProxy1.versionAwareContractName();console.log(`Proxy Pattern and Version from Proxy 1 Storage: ${versionAwareContractName}`);versionAwareContractName = await beaconProxy2.versionAwareContractName();console.log(`Proxy Pattern and Version from Proxy 2 Storage: ${versionAwareContractName}`);const initTx = await beaconProxy1.initialize();
const receipt = await initTx.wait();versionAwareContractName = await beaconProxy1.versionAwareContractName();console.log(`Proxy Pattern and Version from Proxy 1 Storage: ${versionAwareContractName}`);versionAwareContractName = await beaconProxy2.versionAwareContractName();console.log(`Proxy Pattern and Version from Proxy 2 Storage: ${versionAwareContractName}`);
}*// We recommend this pattern to be able to use async/await everywhere and properly handle errors.*main().catch((error) => {
console.error(error);
process.exitCode = 1;
});
```

我们首先使用 *deployBeacon* 方法部署信标契约。然后，我们使用 *deployBeaconProxy* 方法部署两个信标代理。两个代理引用同一个信标，该信标又引用实现契约。然后我们用*升级信标*方法升级信标契约。一旦版本 1 部署了代理，我们就在代理上调用*getContractNameWithVersion*函数。该方法将根据我们在契约的版本 1 中使该方法返回的内容返回一个字符串。升级结束后，我们再次调用*getContractNameWithVersion*函数来查看返回的字符串的变化。让我们运行脚本，看看结果。使用以下命令运行脚本。

```
**npx hardhat run scripts/beacon.js**
```

您应该会在控制台上看到以下内容。

```
Warning: Potentially unsafe deployment of BeaconProxyPatternV1You are using the `unsafeAllow.constructor` flag.Beacon with Beacon Proxy Pattern V1 as implementation is deployed to address: 0xe7f1725E7734CE288F8367e1Bb143E90bb3F0512
Warning: A proxy admin was previously deployed on this networkThis is not natively used with the current kind of proxy ('beacon').
    Changes to the admin will have no effect on this new proxy.Proxy Pattern and Version from Proxy 1 Implementation: Beacon Proxy Pattern: V1
Warning: A proxy admin was previously deployed on this networkThis is not natively used with the current kind of proxy ('beacon').
    Changes to the admin will have no effect on this new proxy.Proxy Pattern and Version from Proxy 2 Implementation: Beacon Proxy Pattern: V1
Warning: Potentially unsafe deployment of BeaconProxyPatternV2You are using the `unsafeAllow.constructor` flag.Beacon upgraded with Beacon Proxy Pattern V2 as implementation at address: 0xe7f1725E7734CE288F8367e1Bb143E90bb3F0512
Proxy Pattern and Version from Proxy 1 Implementation: Beacon Proxy Pattern: V2
Proxy Pattern and Version from Proxy 2 Implementation: Beacon Proxy Pattern: V2
Proxy Pattern and Version from Proxy 1 Storage: Beacon Proxy Pattern: V1
Proxy Pattern and Version from Proxy 2 Storage: Beacon Proxy Pattern: V1
Proxy Pattern and Version from Proxy 1 Storage: Beacon Proxy Pattern: V2
Proxy Pattern and Version from Proxy 2 Storage: Beacon Proxy Pattern: V1
```

耶！我们一升级信标，两个代理中的实现就升级了。我们可以通过查看升级前后由*getContractNameWithVersion*函数返回的字符串来验证这一点。

现在我想让你们注意结果中的一些东西。请注意，即使在升级之后，存储中的*versionawarecractname*变量的值也没有改变。这是因为当我们升级实现时，我们只更改智能合约的代码，存储保持不变。这是可取的，因为将一个智能合同的存储迁移到另一个智能合同并不简单。代理契约的存储用于存储实现智能契约中声明的变量(如果您不清楚为什么会这样，请阅读本文[中的](/@HashHaran/essential-guide-to-smart-contract-upgradeability-a257dac36525)部分，我将在那里解释这一点)。因此，即使在升级后,*versionawarecractname*存储变量的值仍然与版本 1 相同。在我们调用代理 1 上的 *initialize* 方法后，我们可以在下一行中看到*versionawarecontraname*的值已经改变。正是在 *initialize* 方法中，我们将该值设置为版本 2 的值。另外，请注意，在最后一行中，代理 2 中的*versionawarecractname*的值仍然与版本 1 中的值相同。这是因为我们只在代理 1 上调用了 *initialize* 方法，而不是在代理 2 上。

理解我们在上一节中演示的内容很重要。即使信标代理模式中的所有代理都引用实现协定地址的信标，并且仅升级信标就足以升级所有代理，也应该在每个代理协定上逐个调用后续版本的初始化器。否则， *initialize* 方法的效果在没有调用它的代理中看不到。(这是针对当前实现的 Openzeppelin 契约及其升级插件，它不是一个理想的开发者体验。我希望将来会出现一个更好的解决方案，所以我们不需要为此担心。)

感谢阅读。

# 我是谁？

我是一名全栈区块链开发者，对构建一个去中心化的、潜在的更具包容性的未来充满热情。有区块链发展的需求吗？

取得联系:📧hariharan @ aluminum . iitm . AC . in

[Github](https://github.com/HashHaran)

> 加入 Coinmonks [电报频道](https://t.me/coincodecap)和 [Youtube 频道](https://www.youtube.com/c/coinmonks/videos)了解加密交易和投资

# 另外，阅读

*   [折叠 App 回顾](https://coincodecap.com/fold-app-review) | [Kucoin 交易机器人](/coinmonks/kucoin-trading-bot-automate-your-trades-8cf0ca2138e0)
*   [如何匿名购买比特币](https://coincodecap.com/buy-bitcoin-anonymously) | [比特币现金钱包](https://coincodecap.com/bitcoin-cash-wallets)
*   [币安 vs FTX](https://coincodecap.com/binance-vs-ftx) | [最佳(SOL)索拉纳钱包](https://coincodecap.com/solana-wallets)
*   [比诺莫评论](https://coincodecap.com/binomo-review) | [斯多葛派 vs 3Commas vs TradeSanta](https://coincodecap.com/stoic-vs-3commas-vs-tradesanta)
*   [Capital.com 评论](https://coincodecap.com/capital-com-review) | [香港的加密借贷平台](https://coincodecap.com/crypto-lending-hong-kong)