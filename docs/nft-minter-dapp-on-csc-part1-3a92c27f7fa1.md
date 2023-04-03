# NFT 敏特 Dapp 上 CSC-第 1 部分

> 原文：<https://medium.com/coinmonks/nft-minter-dapp-on-csc-part1-3a92c27f7fa1?source=collection_archive---------53----------------------->

嘿嘿嘿！

0Xlive 在这里

你有没有试过不用 React ro Vue 做单页 nft minter dapp？

我们想创建一个使用纯 JS 的 nft minter，可以很容易地部署在任何地方(甚至是博客)。

![](img/b30fe678ce94f785b4d5bf40007ade33.png)

Artwork by 0Xlive

## 项目设置

首先，我们需要为我们的项目制作一个目录。在终端类型中:

```
mkdir nft
```

然后将目录更改为 nft，并用 npm 初始化一个项目:

```
cd nft
npm init --yes
```

然后我们去安装安全帽:

```
npm install --save-dev hardhat
```

安装完成后，使用 npx 运行 hardhat

```
npx hardhat
```

选择`"Create a basic sample project”` 并输入“是”😃

注意:它会询问您关于`hardhat-waffle , ethereum-waffle and hardhat-ethers`的信息，顺便说一下，您可以使用以下说明手动安装它们:

```
npm install --save-dev @nomiclabs/hardhat-waffle ethereum-waffle chai @nomiclabs/hardhat-ethers ethers
```

并按下回车键

我们将使用 openzeppelin 库来简化它

```
npm install @openzeppelin/contracts
```

我们会在 NFT.sol 中编写一些代码

```
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.10;// Import the openzepplin contracts
import "@openzeppelin/contracts/token/ERC721/ERC721.sol";// GameItem is  ERC721 signifies that the contract we are creating imports ERC721 and follows ERC721 contract from openzeppelin
contract nft is ERC721 { constructor() ERC721("simple nft", "NFT") {
        // mint an NFT to yourself
        _mint(msg.sender, 1);
    }
}
```

该编译了！

```
npx hardhat compile
```

如果没有错误，我们将开始部署:)

> 交易新手？试试[密码交易机器人](/coinmonks/crypto-trading-bot-c2ffce8acb2a)或[复制交易](/coinmonks/top-10-crypto-copy-trading-platforms-for-beginners-d0c37c7d698c)

# 安全帽配置

首先，在`scripts`文件夹下创建一个名为`run.js`的新文件

将这段代码放在 run.js 中:

```
const { ethers } = require("hardhat");async function main() {
  /*
A ContractFactory in ethers.js is an abstraction used to deploy new smart contracts,
so nftContract here is a factory for instances of our GameItem contract.
*/
  const nftContract = await ethers.getContractFactory("NFT"); // here we deploy the contract
  const deployedNFTContract = await nftContract.deploy(); // print the address of the deployed contract
  console.log("NFT Contract Address:", deployedNFTContract.address);
}// Call the main function and catch if there is any error
main()
  .then(() => process.exit(0))
  .catch((error) => {
    console.error(error);
    process.exit(1);
  });
```

现在，hardhat.config.json 应该是这样的:

```
require("[@nomiclabs/hardhat-waffle](http://twitter.com/nomiclabs/hardhat-waffle)");// This is a sample Hardhat task. To learn how to create your own go to
// [https://hardhat.org/guides/create-task.html](https://hardhat.org/guides/create-task.html)
task("accounts", "Prints the list of accounts", async (taskArgs, hre) => {
  const accounts = await hre.ethers.getSigners();for (const account of accounts) {
    console.log(account.address);
  }
});module.exports = {
  solidity: "0.8.10",
  networks: {
    csc: {
      url: "[https://testnet-rpc.coinex.net](https://testnet-rpc.coinex.net)",
      accounts: ["YOUR-PRIVATE_KEY"],
    }
  }
};
```

要部署合同，请在您的终端中键入:

```
npx hardhat run scripts/run.js --network csc
```

祝贺🥳

您在 CSC 网络上部署了第一台 NFT。

要使用我们的契约，我们需要契约 ABI，我将在下一部分详细解释。

注:每次进行少量更改时，您都应该更新合同 ABI