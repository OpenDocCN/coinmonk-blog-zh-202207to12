# NFT 敏特 Dapp 对 CSC-第 2 部分

> 原文：<https://medium.com/coinmonks/nft-minter-dapp-on-csc-part2-694d71d6c470?source=collection_archive---------63----------------------->

嘿嘿嘿！

0Xlive 在这里

你有没有试过不用 React ro Vue 做单页 nft minter dapp？

我们想创建一个使用纯 JS 的 nft minter，可以很容易地部署在任何地方(甚至是博客)。

![](img/e5b1b7a4e886a92ae2e118fd5a4c681d.png)

Artwork by 0Xlive

## 描述

我们想创建一个铸造页面，每个用户都可以铸造自己的国王作为头像。我们需要 web3.js 与区块链互动，Metamask 钱包。

我们开始吧！

## Index.html

我们希望在单页中创建 nft minter dapp。我们的 dapp 应该有一个文本框来获取金额，连接钱包和薄荷按钮。创建一个 index.html 文件，把这个代码放在里面:

```
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta http-equiv="X-UA-Compatible" content="IE=edge" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Web3 Minting Demo</title>
  </head>
  <body>
    <button onclick="connectMetamask()">Connect To Metamask</button>
    <br />

    <label for="to_mint">number of nfts to mint (1-10):</label>
    <input
      id="to_mint"
      type="number"
      name="to_mint"
      value="1"
      min="1"
      max="10"
      required
    />
    <br />
    <button onclick="mint()">Mint</button>
  </body>
</html>
```

## Web3.js

我们希望在浏览器中使用 web3.js，因此我们应该在 Head 块中导入归档文件:

```
<script src="https://cdn.jsdelivr.net/npm/web3@latest/dist/web3.min.js"></script>
```

它会将 web3.js 导入到 browser.jsdeliver 的 cdn 是更好的选择。

## 明特

web.js 帮助我们与区块链交互。首先，我们需要设置合同地址:

```
const mainnetContract = "CONTRACT ADDRESS";
```

但这还不够！

要使用合同，我们需要合同 ABI。

> 交易新手？试试[加密交易机器人](/coinmonks/crypto-trading-bot-c2ffce8acb2a)或者[复制交易](/coinmonks/top-10-crypto-copy-trading-platforms-for-beginners-d0c37c7d698c)

## ABI

在[计算机软件](https://en.wikipedia.org/wiki/Computer_software)中，**应用二进制接口** ( **ABI** )是两个二进制程序模块之间的[接口](https://en.wikipedia.org/wiki/Interface_(computing))。通常，这些模块中的一个是[库](https://en.wikipedia.org/wiki/Library_(computing))或[操作系统](https://en.wikipedia.org/wiki/Operating_system)工具，另一个是用户正在运行的程序。

一个 *ABI* 定义了如何在[机器码](https://en.wikipedia.org/wiki/Machine_code)中访问数据结构或计算例程，这是一种低级的、硬件相关的格式。相比之下， [*API*](https://en.wikipedia.org/wiki/Application_programming_interface) 在[源代码](https://en.wikipedia.org/wiki/Source_code)中定义了这种访问，这是一种相对高级的、独立于硬件的、通常[人类可读的](https://en.wikipedia.org/wiki/Human-readable)格式。ABI 的一个常见方面是[调用约定](https://en.wikipedia.org/wiki/Calling_convention)，它决定了如何将数据作为输入提供给计算例程，或者从计算例程中读取数据作为输出。这方面的例子有 [x86 调用约定](https://en.wikipedia.org/wiki/X86_calling_conventions)。

遵守 ABI(可能是也可能不是官方标准化的)通常是编译器、操作系统或库作者的工作。然而，当用混合编程语言编写程序，或者甚至用不同的编译器编译用同一种语言编写的程序时，应用程序员可能不得不直接处理 ABI。(wikipedia.org)

```
const mainnetAbi = [PASTE CONTRACT ABI HERE!];
```

我们设定了每吨的价格

```
const cost = "60000000000000000"; //this is in Wei
```

要使用私钥签署交易，我们必须从元掩码请求访问 wallet

```
var Web3;
var window;

async function getAccounts() {
  try {
    let acc = await window.ethereum.request({
      method: "eth_requestAccounts",
    });

    return acc;
  } catch (e) {
    return [];
  }
}

async function connectMetamask() {
  if (window.ethereum) {
    try {
      const result = await this.getAccounts();
      if (Array.isArray(result) && result.length > 0) {
        let acc = result[0];
        return acc;
      } else {
        return false;
      }
    } catch (err) {
      return false;
    }
  } else {
    return false;
  }
}
```

现在是实现 Mint()函数的时候了

```
async function mint() {
  let totalToMint = document.getElementById("to_mint").value;
  var web3 = new Web3(Web3.givenProvider);
  window.contract = await new web3.eth.Contract(mainnetAbi, mainnetContract);
  const transactionParameters = {
    to: mainnetContract,
    from: (await this.getAccounts())[0],
    value: bigInt(cost).multiply(bigInt(totalToMint.toString())).toString(16),
    data: window.contract.methods.mintCraniums(totalToMint).encodeABI(),
  };
  try {
    await window.ethereum.request({
      method: "eth_sendTransaction",
      params: [transactionParameters],
    });
  } catch (error) {
    console.log(error);
  }
}
```

在上面的代码中，它将从 to_mint 元素中获取金额，并使用合同地址和合同 abi 与合同进行交互。最后使用 eth_sendTransaction 方法签署事务。

恭喜你！

我们已经使用 web3.js、metamask 和 pure js 创建了一个单页面 nft minter dapp