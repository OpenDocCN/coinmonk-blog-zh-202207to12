# 使用 Python 的 CSC 批量发送程序

> 原文：<https://medium.com/coinmonks/csc-bulk-sender-using-python-30d84a159154?source=collection_archive---------24----------------------->

你曾经尝试过发送 CET 或 CRC20 令牌到许多钱包地址吗？在本教程中，我们将编写 python 代码来自动化一些无聊的东西。

![](img/ad5f5886c590e06e022a359764163a8c.png)

## 介绍

主要思想是使用一些代码行发送大量事务，以避免一些例行工作。手动发送多个交易(例如:员工工资)是一项既无聊又耗时的工作。通过使用创造力和代码，我们可以很容易地做类似的工作。

## 项目设置

首先，创建批量发件人目录，并将目录更改为:

```
mkdir bulk-sender
cd bulk-sender
```

然后使用 touch 创建 sender.py:

```
touch sender.py
```

现在，是编码的时候了。您可以使用任何想要的 IDE 或代码编辑器(我使用的是 vscodium)

## Web3.py

web3.x 是一组用于加密和联网的工具，使我们能够与 Node-RPC 进行交互。

要在终端类型中安装 web3.py:

```
pip install web3
```

要导入，请将它放在脚本的顶部:

```
from web3 import Web3
```

我们需要设置提供者(我们选择 coinex 智能链测试网):

```
from web3.middleware import geth_poa_middleware
provider = Web3.HTTPProvider("https://testnet-rpc.coinex.net")
w3 = Web3(provider)
```

**钱包生成**

让我们生成一个钱包并存储它:

```
account = w3.eth.account.create();
```

它会生成一个随机的钱包。我们可以使用以下代码将钱包地址和私钥分开:

```
account.privateKey.hex()
account.address
```

注意:当您运行上面的代码时，将会生成新的 wallet。要为将来存储 wallet，我们必须将数据保存在本地文件系统上:

```
def create_account():
    global account
    account = w3.eth.account.create();
    with open('config.json', 'r+') as config:
        data = json.load(config)
        data["privatekey"] = account.privateKey.hex()
        data["address"] = account.address
        config.seek(0)
        config.write(json.dumps(data))
        config.truncate()
    print("your account has been created \n")
```

我们用 python 制作了一个 config.json，并在其中存储了钱包地址和私钥。

**做交易**

要进行 csc 交易，我们需要:

1 —收件人地址

2 — Nounce

3 —金额

4 —气体

5 —私钥

让我们按照上面的格式进行交易:

```
def send_transaction(amount , value):
    with open('config.json', 'r') as config:
        json_load = json.load(config)
    nonce = w3.eth.getTransactionCount(json_load['address']) tx = {
        'nonce': nonce,
        'to': address,
        'value': amount,
        'gas': 2000000,
        'gasPrice': w3.eth.gas_price
    }
 signed_tx = w3.eth.account.sign_transaction(tx,json_load['privatekey']) tx_hash = w3.eth.sendRawTransaction(signed_tx.rawTransaction) print("Congratulation! your transaction was successful \n TX Hash is : "+ w3.toHex(tx_hash)+"\n"
```

现在，我们应该从某个地方读取地址和金额，将 CET 发送到那个地方。解决方案是 CSV。

## 战斗支援车

**CSV(逗号分隔值)**文件是一种纯文本文档，它使用特定的格式来组织表格信息。CSV 文件格式是一种有界文本文档，使用逗号来区分值。文档中的每一行都是一个数据日志。每个日志由一个或多个用逗号分隔的字段组成。这是用于导入和导出电子表格和数据库的最流行的文件格式。

使用触摸创建 list.csv:

```
touch list.csv
```

我们可以从简单的文本编辑器或使用电子表格应用程序(例如:excel)创建 CSV 文件

**举例:**

```
address,amount
0x2233f3C61513CB28C0D20E71cD1dd51dD67C626F,22.2
0xed3C61G5513CB28C0D20E71cD1dd51dD67CX53B6,33.3
```

要读取 csv 文件导入 csv:

```
import csv
```

然后打开 list.csv

```
with open('list.csv', mode ='r')as file:# reading the CSV filecsvFile = csv.reader(file)
```

现在我们将地址和金额传递给 send _ transcation():

```
for i in csvFile:
        send_transaction(i[0],i[1])
```

祝贺🥳

现在我们有了一个批量发送器，使我们的日常工作变得简单！

> 交易新手？尝试[加密交易机器人](/coinmonks/crypto-trading-bot-c2ffce8acb2a)或[复制交易](/coinmonks/top-10-crypto-copy-trading-platforms-for-beginners-d0c37c7d698c)