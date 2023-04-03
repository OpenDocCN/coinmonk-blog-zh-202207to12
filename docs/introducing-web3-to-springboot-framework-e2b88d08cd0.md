# 将 Web3 引入 Springboot 框架

> 原文：<https://medium.com/coinmonks/introducing-web3-to-springboot-framework-e2b88d08cd0?source=collection_archive---------5----------------------->

![](img/8e813f09d62a1f48ee7942f5f12f3886.png)

去中心化身份( [DID](https://www.w3.org/TR/did-core/) )是 Web3 的主要特性之一，与 Web2 用户的区别在于所有权和控制权属于用户本身。

本文将展示如何实现基于 Web3 身份的认证和授权程序，以及如何将其与 [Springboot 框架](https://spring.io/projects/spring-boot)集成

# Web3 身份

Web3 身份由一个只有所有者知道的*私钥*和一个任何人都知道的*公钥*组成。

*私钥*被所有者用来签署数据——例如在区块链的交易——公钥*被签名的接收者用来验证数据。*

![](img/8435d08da1114285ca4b8936712eea51.png)

Web2 身份的不同之处在于，你不需要第三方来证明你的身份——例如脸书、谷歌、Okta 等。—使用您和身份管理员( [IAM](https://en.wikipedia.org/wiki/Identity_management) )都知道的*用户名/密码*

> 在 Web3 中，密码用于锁定或加密存储私钥的钱包，而不是用于身份验证。

# 身份证明

Web3 中的用户通过用他们的*私钥*生成的签名来证明他们的身份。

*   应用程序可以要求用户通过请求来证明他们的身份*，这意味着用户必须通过请求生成签名。一个例子是基于区块链网络的 [Dapps](https://en.wikipedia.org/wiki/Decentralized_application) 。*
*   应用程序可以要求用户只需证明一次身份就能获得访问权限，例如传统的*登录—* 。在这种情况下，用户对任意文本进行签名，并将该签名用于发送到应用程序的所有请求。

![](img/9205eba9a8346d54580e17f6c0baaac0.png)

在 Web3 生态系统中，*用户*被认证，确认*签名*属于*公钥—* 记住，给定的*公钥*的签名可以仅用*私钥* *—* 生成

与*用户名/密码*的不同之处在于，身份证明是在起点完成的，而不是在目的地完成的，*私钥*由用户保护——起点——不同于*用户名/密码*由应用程序保护——目的地———

## 数字签名

数字签名可以使用编程语言(Javascript、Go、Java 等)中实现的数学函数手动生成。—

Example with Javascript

通常这个任务由[钱包](https://en.wikipedia.org/wiki/Cryptocurrency_wallet)完成，钱包实现签名功能，用[钱包](https://en.wikipedia.org/wiki/Cryptocurrency_wallet)中存储的*私钥*之一对数据进行签名。

## 使用元掩码签名

Metamask 提供了一个浏览器[钱包](https://en.wikipedia.org/wiki/Cryptocurrency_wallet)，作为 Chrome 或 Firefox 的扩展安装。[元掩码](https://metamask.io/)存储*私钥*并签署从浏览器发送到支持[以太坊虚拟机](https://ethereum.org/en/developers/docs/evm/)的区块链网络的交易

此外，[元掩码](https://metamask.io/)能够签署任意数据。下图显示了[元掩码](https://metamask.io/)对两个动作的响应。

![](img/96ebd7bd14e485d81f64cc3c73750d0b.png)

# 授权服务器

授权服务器也称为 [IAM](https://en.wikipedia.org/wiki/Identity_management) ，是一个管理给定应用程序中用户的*认证*和*授权*的系统。

*   认证是验证用户身份的过程。例如，在基于*用户名/密码*的系统中，通过提供*密码*来证明*用户名*的身份。
*   **授权**是为给定的已认证*身份*确定授予权限的过程。例如授予给定*用户*的*权限*或*角色*。

## Web3 身份认证

Web3 身份的认证依赖于只有*身份*所有者可以创建的签名的验证。

下图显示了一个使用用户的 Web3 身份签名来验证请求的示例:

![](img/ba5f5ac2a9b5ab92f28be0de4672899a.png)

> 请记住，授权服务器不保护私钥，也不期望用户发送公钥和私钥来登录

以下代码显示了签名和签名验证的示例:

Example of signing and validation

## 基于 NFT 所有权的授权

授权服务器通常将授予的权限存储为角色、组等。与用户相关联。

在纯 Web3 dapp 中，授予的权限将从令牌或 NFT 等区块链数据中获得。例如，只有拥有活动或聚会的 NFT 的用户才能看到聚会的位置。

下图显示了使用 NFT 所有权管理授权的建议

![](img/df5553a324964fcea7eb85c39aa7b24f.png)

## 授权服务器的实现

授权服务器的提出是受 [OAuth](https://en.wikipedia.org/wiki/OAuth) 协议的启发

![](img/736086fa40a392752d5a86dac3ea9a47.png)

用户在授权服务器上证明他们的身份，授权服务器用用户的身份和权限导出一个 [JWT](https://en.wikipedia.org/wiki/JSON_Web_Token) 。应用程序使用 [JWT](https://en.wikipedia.org/wiki/JSON_Web_Token) 来授权访问私有资源。

与标准的 [OAuth](https://en.wikipedia.org/wiki/OAuth) 服务器不同，Web3 授权服务器不提供*登录页面*，也不存储任何用户数据。*登录页面*被用户的[钱包](https://en.wikipedia.org/wiki/Cryptocurrency_wallet)所取代——例如[元掩码](https://metamask.io/)——授权来源于 NFT 所有权。

![](img/0948a3244ede7cfcf3981802210d14c2.png)

让我们看看它是如何工作的:

1.  用户试图登录应用程序。显示的只是一个按钮，没有*用户名*和*密码*字段。

![](img/3743935e8ea81996afdf5dfc02ba732c.png)

2.用户点击按钮，并被要求用[元掩码](https://metamask.io/)签署一个*消息*。

![](img/48a729d8ff9aaf49ca7625bb5d9e29b5.png)

3.*签名*被发送到授权服务器，授权服务器验证签名并重定向到回调 url，产生 [JWT](https://en.wikipedia.org/wiki/JSON_Web_Token)

```
[http://localhost:9080/authorize?token=eyJ0eXAiO...H6BD1g23-A](http://localhost:9080/authorize?token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiJ9.eyJzdWIiOiIweGQyMjU0OWVDZjg2RTQzNkE3NDIzOERBMzI2NDE1MTg4N0U2NmFhMkUiLCJzY29wZSI6WyJMZXZlbC1CIl0sImV4cCI6MTY2NzgxMzEwOX0.uNDbCbIi4kSxnp_x0HYQ6_GoButF_ibkaqKRDI6SxJ7lh-WUp8Gf24n7_HzYUxRrdu-KOZ5-Ec67390ok0qCoWPIttH-n_Y6tvIOFe7Wyx8YyjWN89gKwtAlAutKq6VdT2a8STIc8xI6lBbpX-OtJzljl7AoE5ThN52ehY0R1vblAMSevTkRe7cLQhrH-f_838yQwFwzuS5G1XpR-_jwfONGfiANF8oYOaO9ohh7opxTJWJRnUhFKJDaK_4e2VSwmP4jgK6TVRooaEJ51JFephXJwa6P4B_41GxiEthLxm4RfyudM6Ma24jmeyK8oNC7NTsdoLeXnUPrH6BD1g23-A)
```

![](img/9b6952b924a5c99b7d1c33b041f40555.png)

## JSON Web 令牌的模式

由 [JWT](https://en.wikipedia.org/wiki/JSON_Web_Token) 提供的有效载荷中的数据是:

用户的**主题** : *地址*或*公钥*

**范围**:用户拥有的 NFT 的名称。

**exp** :令牌的到期日期

![](img/58941177491cbbc6548135c2b1c3c329.png)

Example of JWT payload

## 与 Springboot 框架集成

Springboot 与 Web3 身份的集成在于 [Spring 安全项目](https://github.com/spring-projects/spring-security)

*   *Spring security filters* 可用于从报头获取 [JWT](https://en.wikipedia.org/wiki/JSON_Web_Token) 或签名。
*   *Spring 安全认证提供者*可用于验证签名，并通过授权映射 NFT 所有权。

# 资源和参考资料

GitHub 中的项目库[https://github.com/formentor-studio/spring-security-web3](https://github.com/formentor-studio/spring-security-web3)

> 交易新手？尝试[加密交易机器人](/coinmonks/crypto-trading-bot-c2ffce8acb2a)或[复制交易](/coinmonks/top-10-crypto-copy-trading-platforms-for-beginners-d0c37c7d698c)