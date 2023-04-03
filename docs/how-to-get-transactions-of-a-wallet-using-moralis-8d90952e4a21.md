# 如何使用 Moralis 进行钱包交易

> 原文：<https://medium.com/coinmonks/how-to-get-transactions-of-a-wallet-using-moralis-8d90952e4a21?source=collection_archive---------12----------------------->

![](img/8286575a6c1a38173da841d66dd4456a.png)

在本教程中，我们将回顾如何利用 Moralis 事务 API 和 NodeJS 来检索钱包的所有本地事务。

要设置该应用程序，请遵循以下说明。

1.  **创建一个 *package.json* 文件，如下图**

```
npm init -y
```

2.**安装** `**moralis**` **和** `**@moralisweb3/evm-utils**` **依赖关系如下**

```
npm i moralis @moralisweb3/evm-utils
```

3.**您的 *package.json* 应该包含以下内容**

```
{
  "name": "moralis-transactions",
  "version": "1.0.0",
  "description": "",
  "main": "server.js",
  "scripts": {
    "start": "node server.js",
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "keywords": [],
  "author": "",
  "license": "ISC",
  "dependencies": {
    "@moralisweb3/evm-utils": "^2.7.4",
    "dotenv": "^16.0.3",
    "express": "^4.18.2",
    "moralis": "^2.7.4"
  }
}
```

4.**创建一个*。包含道德规范*API _ KEY*的 env* 文件**

```
API_KEY = “Your Moralis API Key”
```

5.**启动*server . js*中的服务器**

```
require('dotenv').config();
const express = require('express');

const app = express();

app.use(express.json());

app.listen(8080, async () => {
    console.log('server started');
});
```

6.**启动*server . js*中的 Moralis**

```
require('dotenv').config();
const express = require('express');
const Moralis = require('moralis').default;

const app = express();
const { API_KEY } = process.env;

app.use(express.json())

async function startMoralis() {
    try{
        await Moralis.start({
            apiKey: API_KEY,
        });
    }catch(err) {
        console.log(err);
    }
}

app.listen(8080, async () => {
    startMoralis();
    console.log('server started');
})
```

7.**添加一个 GET 端点 */transactions* 来获取一个钱包的所有交易**

```
require('dotenv').config();
const express = require('express');
const Moralis = require('moralis').default;
const { EvmChain } = require('@moralisweb3/evm-utils');

const app = express();
const { API_KEY } = process.env;

app.use(express.json())

async function startMoralis() {
    try{
        await Moralis.start({
            apiKey: API_KEY,
        });
    }catch(err) {
        console.log(err);
    }
}

app.get('/transactions', async (req, res) => {
    const chain = EvmChain.ETHEREUM;
    const { address } = req.body;
    try{
     const response = await Moralis.EvmApi.transaction.getWalletTransactions({
        address,
        chain,
     });
     res.status(200).send(response.toJSON());
    }catch(err) {
        console.log(err);
    }
})

app.listen(8080, async () => {
    startMoralis();
    console.log('server started');
})
```

**8。使用 Postman** 测试端点

测试结果如下所示。在`req.body.`给出钱包地址，交易按块号降序排列。在响应中，将首先显示最近的交易。

![](img/316bb75734612f8411a150aa86852eb6.png)

# 摘要

至此，我们已经讨论了如何利用 Moralis 事务 API 和 NodeJs 来获取所有的钱包事务。开一个 Moralis 账户，玩玩他们的 API。

> 交易新手？尝试[加密交易机器人](/coinmonks/crypto-trading-bot-c2ffce8acb2a)或[复制交易](/coinmonks/top-10-crypto-copy-trading-platforms-for-beginners-d0c37c7d698c)