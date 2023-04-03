# 在 CSC Dapp 上使用 RainbowKit 进行身份验证-第 1 部分

> 原文：<https://medium.com/coinmonks/authentication-using-rainbowkit-on-csc-dapp-part1-151ea22f0e1b?source=collection_archive---------42----------------------->

身份验证是 dapp 开发中最重要的部分之一，RainbowKit 是将钱包连接到 dapp 的最佳方式之一！

在本教程中，我们想介绍 RainbowKit，并打算在我们的 Dapp 中实现 RainbowKit。

![](img/ad5f5886c590e06e022a359764163a8c.png)

coinex,org

## RaibowKit

Rainbowkit 是连接为每个人设计、为开发者打造的钱包的最佳方式之一。siqqest Web3 团队正在使用 RainbowKit 改进他们的产品，取悦他们的用户，并在构建时节省时间。

RainbowKit 为开发人员提供了一种快速、简单且高度可定制的方式，为他们的应用程序添加了出色的钱包体验。我们处理困难的事情，这样开发者和团队可以专注于为他们的用户构建令人惊叹的产品和社区。

## 特征

## 钱包管理

为您的 dapp 提供开箱即用的钱包管理。除了处理钱包的连接和断开，RainbowKit 支持许多钱包，交换连接链，解析地址到 ENS，显示余额等等！

## 可定制的

您可以调整 RainbowKit UI 以匹配您的品牌。您可以从一些预定义的强调颜色和边框半径配置中进行选择。对于更高级的用例，你可以提供一个完全自定义的主题，呈现你自己的按钮，并省略某些功能。包括黑暗模式。

## 行业标准

为了更好地与大多数产品互操作，我们依赖于 [ethers](https://github.com/ethers-io/ethers.js) 和[wag mi](https://wagmi.sh/)——这个领域最常用的库。

**其他功能:**

*   易于安装
*   内置主题
*   自定义钱包列表
*   App Store 和 Google Play 集成
*   自定义连接按钮
*   定制链

RainbowKit 是一个 [React](https://reactjs.org/) 库，可以轻松地将钱包连接添加到您的 dapp。它直观、反应灵敏且可定制。

## 装置

您可以使用您选择的软件包管理器，使用以下命令之一来搭建新的 rainbow kit+[wagmi](https://wagmi.sh)+[next . js](https://nextjs.org)应用程序:

## NPM

```
npm init @rainbow-me/rainbowkit@latest
```

## 故事

```
yarn create @rainbow-me/rainbowkit@latest
```

## PNPM

```
pnpm create @rainbow-me/rainbowkit@latest
```

这将提示您输入项目名称，生成一个包含样板项目的新目录，并安装所有必需的依赖项。

或者，您可以手动将 RainbowKit 集成到现有项目中。

## 手动设置

安装 RainbowKit 及其对等依赖项， [wagmi](https://wagmi-xyz.vercel.app/) 和 [ethers](https://docs.ethers.io) 。

```
npm install @rainbow-me/rainbowkit wagmi ethers
```

## 导入

进口 RainbowKit、wagmi 和 ethers。

```
import '@rainbow-me/rainbowkit/styles.css';import {
  getDefaultWallets,
  RainbowKitProvider,
} from '@rainbow-me/rainbowkit';import {
  chain,
  configureChains,
  createClient,
  WagmiConfig,
} from 'wagmi';import { alchemyProvider } from 'wagmi/providers/alchemy';import { publicProvider } from 'wagmi/providers/public';
```

## 安装ˌ使成形

配置所需的链并生成所需的连接器。您还需要设置一个`wagmi`客户端。

```
import { alchemyProvider } from 'wagmi/providers/alchemy';import { publicProvider } from 'wagmi/providers/public';const { chains, provider } = configureChains([chain.mainnet, chain.polygon, chain.optimism, chain.arbitrum],[alchemyProvider({ apiKey: process.env.ALCHEMY_ID }),publicProvider()]);const { connectors } = getDefaultWallets({appName: 'My RainbowKit App',chains});const wagmiClient = createClient({autoConnect: true,connectors,provider})
```

阅读更多关于使用`wagmi`配置链和提供商的信息。

## 包装提供商

用`RainbowKitProvider`和`[WagmiConfig](https://wagmi.sh/docs/provider)`包装你的应用。

```
const App = () => {return (<WagmiConfig client={wagmiClient}><RainbowKitProvider chains={chains}><YourApp /></RainbowKitProvider></WagmiConfig>);
};
```

## 添加连接按钮

然后，在你的应用程序中，导入并渲染`ConnectButton`组件。

```
import { ConnectButton } from '@rainbow-me/rainbowkit';export const YourApp = () => {return <ConnectButton />;};
```

RainbowKit 现在将处理您的用户的钱包选择，显示钱包/交易信息，并处理网络/钱包切换。

## 连接按钮

这是主要成分。它负责呈现连接/断开按钮，以及链交换 UI。

```
import { ConnectButton } from '@rainbow-me/rainbowkit';export const YourApp = () => {return <ConnectButton />;};
```

> 交易新手？尝试[加密交易机器人](/coinmonks/crypto-trading-bot-c2ffce8acb2a)或[复制交易](/coinmonks/top-10-crypto-copy-trading-platforms-for-beginners-d0c37c7d698c)