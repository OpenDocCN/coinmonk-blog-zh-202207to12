# 如何在 CSC 上使用 web3.js 和 metamask 创建捐赠系统

> 原文：<https://medium.com/coinmonks/how-to-create-donation-system-using-web3-js-and-metamask-on-csc-b4ee9837227e?source=collection_archive---------8----------------------->

嘿嘿嘿

0Xlive 在这里

在本教程中，我们将使用 web3.js 和 coinex 智能链上的 metamask 为我们的网站创建一个捐赠系统。

![](img/a06978ee5f3898954c8e40fea3a34ff9.png)

How to donate crypto source: coinbase.com

## Web3.js

这是以太坊兼容的 [JavaScript API](https://github.com/ethereumproject/wiki/wiki/JavaScript-API) ，它实现了[通用 JSON RPC](https://github.com/ethereumproject/wiki/wiki/JSON-RPC) 规范。它以可嵌入 js 和 meteor.js 包的形式提供给 component。

将此内容放入 head 标记中以导入 web3.js:

```
 <script src=”https://cdnjs.cloudflare.com/ajax/libs/web3/1.7.3/web3.min.js" integrity=”sha512-Ws+qbaGHSFw2Zc1e7XRpvW+kySrhmPLFYTyQ95mxAkss0sshj6ObdNP3HDWcwTs8zBJ60KpynKZywk0R8tG1GA==” crossorigin=”anonymous” referrerpolicy=”no-referrer”></script>
```

## donate.html

让我们创建一个名为 donate.html 的文件。我们将在此创建捐赠表格。

让我们创建一个名为付款的部分:

```
<section id=”payment” class=”payment”></section>
```

将付款表格放入付款部分:

```
<form class=”credit-card”><div class=”form-header”><h4 class=”title”>Payment</h4></div><div class=”form-body”><input class=”card-number” id=”Amount” type=”number” placeholder=”Amount”><button class=”pay-button” type=”submit”> <a href=”#”>Buy</a></button><div id=”status”></div></div></form>
```

让我们设置 web3.js 和 metamask。打开

```
window.addEventListener('load', async () => {if (window.ethereum) {window.web3 = new Web3(ethereum);try {await ethereum.eth_requestAccounts;initPayButton()} catch (err) {$('#status').html('User denied account access', err)}} else if (window.web3) {window.web3 = new Web3(web3.currentProvider)initPayButton()} else {$('#status').html('No Metamask (or other Web3 Provider) installed')}})
```

有一个向 metamask 提交数据的付费按钮。将此代码写入脚本标记:

```
const initPayButton = () => {$(‘.pay-button’).click(() => {const paymentAddress = ‘Enter Your Address’const amountEth = document.getElementById(“Amount”).value;web3.eth.sendTransaction({to: paymentAddress,value: web3.toWei(amountEth, ‘ether’)}, (err, transactionId) => {if (err) {console.log(‘Payment failed’, err)$(‘#status’).html(‘Payment failed’)} else {document.getElementById(“status”).innerHTML = transactionId;$(‘#status’).html(‘Payment successful’)}})})}
```

恭喜你！

我们已经使用 js 为我们的网站/博客创建了一个捐赠系统

你也可以查看我之前关于 csc 加密支付系统的文章:

[https://medium . com/coin monks/simple-crypto-payment-system-using-PHP-and-coinex-net-API-45d 38 cf 2131](/coinmonks/simple-crypto-payment-system-using-php-and-coinex-net-api-45d38cf2131)

> 交易新手？尝试[加密交易机器人](/coinmonks/crypto-trading-bot-c2ffce8acb2a)或[复制交易](/coinmonks/top-10-crypto-copy-trading-platforms-for-beginners-d0c37c7d698c)