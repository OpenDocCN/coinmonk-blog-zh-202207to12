# CSC 开发工具包

> 原文：<https://medium.com/coinmonks/csc-development-toolkit-dd496cc1c95?source=collection_archive---------62----------------------->

嘿嘿嘿！

0Xlive 在这里

在本教程中，我将介绍 csc 工具包。csc toolkit 是一个开发工具集，用于在 coinex 智能链上测试和部署智能合同，其灵感来自 eth 工具集

![](img/ad5f5886c590e06e022a359764163a8c.png)

# CSC 开发工具包

通过使用 csc 开发工具，您可以:

*   将您的智能合约代码编译成二进制代码，以便部署
*   将编译好智能合同部署到以太网
*   出于测试目的，从控制台(命令行)调用智能合约方法

更改文件`config.js`中的配置值以使其工作:

*   `contractSourcePath`:包含您的*的文件夹路径。sol* 源文件
*   `builtContractPath`:构建目标文件夹路径
*   `deployedContractPath`:保存已部署合同信息的文件夹路径
*   `networks`:定义要部署的网络，并与已部署的智能合同进行交互。
    每个网络都在`networks`字典的一个条目中:`network_id: {...}`
    *例如* :
    `dev: { url: "http://localhost:8545", defaultAccount: { address: '0x...', privateKey: 'Your account private key' }, gas: 6500000 }`

# 编译智能合同

在您的终端中运行以下命令:

> `*node compile.js*`

这将编译`contractSourcePath`中的所有`.sol`文件。每个合同的字节码和定义将被保存为`<builtContractPath>/<contract_name>.json`文件。
我们将来会支持编译单个`.sol`文件

## Solc-JS

用于 [Solidity 编译器](https://github.com/ethereum/solidity)的 JavaScript 绑定。

使用在 [solc-bin 库](https://github.com/ethereum/solc-bin)中找到的 Emscripten 编译实体。

可以手动安装 Solc-JS。在终端类型中:

```
npm install solc
```

# 部署智能合同

在终端中运行以下命令:

> `*node deploy.js --contract=YourContractName [ --network=targetNetworkId ] [ --params='[param1, param2,...] ]'*`

因为:

*   —合同(必填):您的合同名称
*   — network(可选):您希望将合同部署到的网络的 Id(在配置键`networks`中定义)。如果跳过，将使用网络 id `default`。
*   — params(可选):将传递给智能协定的构造函数的参数值。如果智能协定的构造函数没有参数，请跳过此步骤。

这将找到合约`<builtContractPath>/YourContractName.json`定义(由编译过程生成),以将合约部署到所选网络。部署信息(*合同地址，交易哈希、...*)将被保存到`<deployedContractPath>/YourContractName_<networkId>.json`。例如:

```
node deploy.js --contract=TradeLog --network=dev --params='[12345, "Mr Fun"]'
```

(如果成功，部署信息将保存为`<deployedContractPath>/TradeLog_dev.json`)

> 交易新手？尝试[加密交易机器人](/coinmonks/crypto-trading-bot-c2ffce8acb2a)或[复制交易](/coinmonks/top-10-crypto-copy-trading-platforms-for-beginners-d0c37c7d698c)