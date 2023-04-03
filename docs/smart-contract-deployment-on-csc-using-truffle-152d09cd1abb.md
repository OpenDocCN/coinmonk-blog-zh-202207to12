# 使用 Truffle 在 CSC 上部署智能合同

> 原文：<https://medium.com/coinmonks/smart-contract-deployment-on-csc-using-truffle-152d09cd1abb?source=collection_archive---------25----------------------->

您是否曾经尝试过使用 truffle 在 coinex 智能链上部署您的智能合约？在本教程中，我们希望安装和配置 truffle，并使用 [truffle](https://trufflesuite.com) 在 coinex 智能链测试网上部署智能合约。

![](img/c50ec71ae9c025bc8f3952a1bd09bbde.png)

truffle

## 松露

一个世界级的开发环境，测试框架和资产管道为区块链使用以太坊虚拟机(EVM)，旨在使生活作为一个开发者更容易。有了松露，你会得到:

*   内置智能合约编译、链接、部署和二进制管理。
*   快速开发的自动化合同测试。
*   可脚本化、可扩展的部署和迁移框架。
*   部署到任意数量的公共和私有网络的网络管理。
*   使用 [ERC190](https://github.com/ethereum/EIPs/issues/190) 标准，通过 EthPM & NPM 进行封装管理。
*   用于直接合同通信的交互式控制台。
*   支持紧密集成的可配置构建管道。
*   在 Truffle 环境中执行脚本的外部脚本运行程序。

[https://trufflesuite.com](https://trufflesuite.com)

## 安装 Truffle

在您可以使用 Truffle 之前，您必须使用 npm 安装它。打开一个终端，使用以下命令进行全局安装。

```
npm install -g truffle
```

## 要求

*   节点`latest`版本
*   npm `latest`版本

## 项目设置

我们可以创建一个裸项目模板，但是在这个例子中，我们将使用[元硬币框](https://trufflesuite.com/boxes/metacoin)。

首先，我们为我们的项目创建一个目录。在终端类型中:

```
mkdir coin
cd coin
```

1.  下载(“拆箱”)元硬币盒:

```
truffle unbox metacoin
```

**注意**:你可以使用`truffle unbox <box-name>`命令下载任何其他的松露盒子。

我们的松露项目结构是这样的:

*   `contracts/`[担保合同目录](https://trufflesuite.com/docs/truffle/getting-started/interacting-with-your-contracts)
*   `migrations/`:可脚本化部署文件[的目录](https://trufflesuite.com/docs/truffle/getting-started/running-migrations#migration-files)
*   `test/`:测试文件目录，用于[测试您的应用程序和合同](https://trufflesuite.com/docs/truffle/testing/testing-your-contracts)
*   `truffle.js`:松露[配置文件](https://trufflesuite.com/docs/truffle/reference/configuration)

注意:你可以查看[松露文档](https://trufflesuite.com/docs)了解更多细节。

## 测试

在终端中，运行可靠性测试:

```
truffle test ./test/TestMetaCoin.sol
```

您将看到以下输出:

```
TestMetacoin
    √ testInitialBalanceUsingDeployedContract (71ms)
    √ testInitialBalanceWithNewMetaCoin (59ms) 2 passing (794ms)
```

运行 JavaScript 测试:

```
truffle test ./test/metacoin.js
```

您将看到以下输出:

```
Contract: MetaCoin
    √ should put 10000 MetaCoin in the first account
    √ should call a function that depends on a linked library (40ms)
    √ should send coin correctly (129ms) 3 passing (255ms)
```

## 收集

编译智能合同:

```
truffle compile
```

您将看到以下输出:

```
Compiling .\contracts\ConvertLib.sol...
Compiling .\contracts\MetaCoin.sol...
Compiling .\contracts\Migrations.sol...Writing artifacts to .\build\contracts
```

## 配置块菌

没有任何附加功能的默认 Truffle 配置如下所示:

```
module.exports = {
  rpc: {
    host: "127.0.0.1",
    port: 8545
  }
};
```

这告诉 Truffle，默认情况下，它应该在主机`127.0.0.1`和端口`8545`连接到网络客户端

为了确保 Truffle 知道您想要部署到的网络，我们可以为实时网络添加一个特定的配置:

```
module.exports = {
  networks: {
    "csc": {
      network_id: 53,
      host: "https://testnet-rpc.coinex.net",
      port: 8546
    }
  },
  rpc: {
    host: "127.0.0.1",
    port: 8545
  }
};
```

## 部署到 CSC

现在还不是在 csc 网络上部署的时候。为此，我们可以使用以下命令进行部署:

```
$ truffle migrate --network csc
```

注意，我们要求使用`"csc"`网络，这是我们在配置中定义的名称

祝贺🥳

现在，我们可以在 csc 上轻松部署我们的智能合同！

> 交易新手？尝试[加密交易机器人](/coinmonks/crypto-trading-bot-c2ffce8acb2a)或[复制交易](/coinmonks/top-10-crypto-copy-trading-platforms-for-beginners-d0c37c7d698c)