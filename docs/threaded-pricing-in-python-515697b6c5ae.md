# Python 中的线程定价

> 原文：<https://medium.com/coinmonks/threaded-pricing-in-python-515697b6c5ae?source=collection_archive---------22----------------------->

![](img/d3bdfbcfae3fe546e9ef1b5fbdbc37a8.png)

定价是 Defi 初学者会遇到的最棘手的功能之一。这是因为有很多地方可以得到价格，但是每个 Dex 会以不同的价格交换。价格数据的速度也非常重要，因为在一段时间内拥有尽可能多的价格将会产生更加详细的重采样结果。在这个例子中，我将把重点放在 BSC 上，因为 RPC 非常稳定，并且易于使用。

**理论:**

要了解 Dex 的价格，你首先要看看他们的*路由器*智能合约。在 Pancakeswap(几乎所有路由器都是相同的 btw)的情况下，我们将查看 Read 契约中的 **getAmountsOut** 函数。

[合同地址 0x 10 ed 43 c 718714 EB 63 D5 aa 57 b 78 b 54704 e 256024 e | BscScan](https://bscscan.com/address/0x10ED43C718714eb63d5aA57B78B54704E256024E#readContract)

这将使我们能够根据特定的途径来确定置换的价格。地址路径的格式如下[“输入”、“输出”]。然后我们输入想要输出的输入量。这对于获得稳定硬币的当前价格很重要。即(amount in = 1)([" Price _ you _ Are _ Looking-4 "，" USDC 契约"])
(记住很多路线不是 1:1，直达。当交换时，检查你正在使用的 Dex 的最佳路线。)

**代码:**

为了在 python 中实现这一点，我们需要用 web3 包调用智能契约。我们还需要 Dex 的 ABI 调用(所有可用的功能)和智能合同地址。设置应该如下所示。

```
from Web3 import web3#ABI
panabi = 'To long to post, just pull this from BSC SCAN'#Provider RPC
provider_bsc = 'https://bsc-dataseed.binance.org/'#Router Contract
router_pcsv2 = '0x10ED43C718714eb63d5aA57B78B54704E256024E'#By default trades from BUSD - CHEAPER
base_coin= '0xe9e7cea3dedca5984780bafc599bd69add087d56'
base_coin = Web3.toChecksumAddress(base_coin)
InputToken_address = "INPUT_TOKEN_SMART_CONTRACT_HERE"
InputToken_address = Web3.toChecksumAddress(InputToken_address)#setup Web3 connector
web3 = Web3(Web3.HTTPProvider(provider_bsc))#Setup the PancakeSwap contract
contract = web3.eth.contract(address=router_pcsv2, abi=panabi)#pricing Function
def get_price():
   #Current Price
   current_price = (contract.functions.getAmountsOut(1,[InputToken_address,base_coin]).call())[1] #float out price from Gwei, will be human reable, easier to set trade price
   current_price = float(web3.fromWei(current_price,'gwei'))
   print(current_price)
```

上面的代码会让你得到你的乐器的当前价格，但那只是在这个确切的时刻。在一个单独的线程中持续拉动价格才是正确的做法。

为此，导入线程模块，然后使用以下语法调用 get_price()函数。

```
import threadingif __name__ == "__main__":
   print(threading.active_count())
   print(threading.enumerate()) #Start Price Data Thread
   getPrice_thread = threading.Thread(target=get_price,)
   getPrice_thread.daemon = True
   getPrice_thread.start()
```

这个实现非常简单，只是为了展示如何在另一个线程中获取价格，以便在重采样或其他操作中保持价格不变。

(提示:如果您计划对时间帧进行重新采样，请在您的主线程中设置一个休眠，直到您构建了足够的蜡烛线来处理采样数据上的操作。使用单独的线程进行重采样。)

**改进思路:**

*   添加日志记录
*   将价格保存到数据帧，并为 OHLC 值重新取样
*   根据特定价格或指标触发买入/卖出

祝你在兔子洞里玩得愉快！

> 交易新手？尝试[加密交易机器人](/coinmonks/crypto-trading-bot-c2ffce8acb2a)或[复制交易](/coinmonks/top-10-crypto-copy-trading-platforms-for-beginners-d0c37c7d698c)