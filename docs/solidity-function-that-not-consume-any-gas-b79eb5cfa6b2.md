# 不消耗任何气体的坚固功能

> 原文：<https://medium.com/coinmonks/solidity-function-that-not-consume-any-gas-b79eb5cfa6b2?source=collection_archive---------6----------------------->

> 我为什么写这个博客？我有一个假设，只有整数(读操作)不需要汽油费，但随着我探索更多的合同，下面是几个不同的函数，不消耗任何汽油。

一般带有`view`或`pure`修饰符的函数在外部调用**时不会产生任何气费。如果从另一个函数内部调用，那么`pure`和`view`函数仍然**消耗 gas。****

## **1.打印整数而不读取**

**`pure`修饰符意味着函数不能读取(状态)变量。**

```
//SPDX-License-Identifier: MIT
pragma solidity 0.8.15;contract basic1 {
    function printHello() public pure returns (uint) {
        return 5;
    }
}
```

## **2.读取并打印一个整数**

**`view`修饰符只能允许读取(状态)变量**

```
//SPDX-License-Identifier: MIT
pragma solidity 0.8.15;contract basic1 { uint val = 5; function printHello() public pure returns (uint) {
        return val;
    }
}
```

## **3.在不读取的情况下打印字符串**

```
//SPDX-License-Identifier: MIT
pragma solidity 0.8.15;contract basic1 {
    function printHello() public pure returns (string memory) {
        return "Hello World"
    }
}
```

## **4.打印带读数的字符串**

```
//SPDX-License-Identifier: MIT
pragma solidity 0.8.15;contract basic1 {
    string strdata = "Hello World";
    function printHello() public view returns (string memory) {
        return strdata;
    }
}
```

> **加入 Coinmonks [电报频道](https://t.me/coincodecap)和 [Youtube 频道](https://www.youtube.com/c/coinmonks/videos)了解加密交易和投资**

# **另外，阅读**

*   **[印度最佳 P2P 加密交易所](https://coincodecap.com/p2p-crypto-exchanges-in-india) | [柴犬钱包](https://coincodecap.com/baby-shiba-inu-wallets)**
*   **[8 大加密附属计划](https://coincodecap.com/crypto-affiliate-programs) | [eToro vs 比特币基地](https://coincodecap.com/etoro-vs-coinbase)**
*   **[最佳以太坊钱包](https://coincodecap.com/best-ethereum-wallets) | [电报上的加密货币机器人](https://coincodecap.com/telegram-crypto-bots)**
*   **[交易杠杆代币的最佳交易所](https://coincodecap.com/leveraged-token-exchanges) | [购买 Floki](https://coincodecap.com/buy-floki-inu-token)**
*   **[3Commas 对 Pionex 对 Cryptohopper](https://coincodecap.com/3commas-vs-pionex-vs-cryptohopper) | [Bingbon 评论](https://coincodecap.com/bingbon-review)**