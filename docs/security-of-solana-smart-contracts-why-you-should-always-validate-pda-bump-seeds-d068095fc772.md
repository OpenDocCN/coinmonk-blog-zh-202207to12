# 索拉纳智能合约的安全性:为什么你应该总是验证 PDA 碰撞种子

> 原文：<https://medium.com/coinmonks/security-of-solana-smart-contracts-why-you-should-always-validate-pda-bump-seeds-d068095fc772?source=collection_archive---------2----------------------->

程序派生地址(PDA)几乎用于所有的 Solana smart 合同。虽然有很好的资源(例如， [Solana Cookbook](https://solanacookbook.com/core-concepts/pdas.html#facts) )来理解 PDA，但是有一个重要的警告值得特别注意:

> **一个索拉纳程序的 PDA 可以有多个有效凸起的相同种子，给出多个不同的地址**(引自[推特](https://mobile.twitter.com/armaniferrante/status/1413635845865672706)作者 [@armaniferrante](http://twitter.com/armaniferrante) )

这具有至关重要的安全含义:***PDA 可以伪造*** *(通过提供不同的凹凸种子)* …