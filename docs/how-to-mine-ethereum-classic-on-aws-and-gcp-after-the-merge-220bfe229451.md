# AWS 和 GCP“合并”后的密码挖掘

> 原文：<https://medium.com/coinmonks/how-to-mine-ethereum-classic-on-aws-and-gcp-after-the-merge-220bfe229451?source=collection_archive---------4----------------------->

## 是的，即使在以太坊切换到股权证明之后，你仍然可以在公共云中挖掘密码！

![](img/266e4237700ef9a757629ff74a36ca7d.png)

Crypto mining on AWS and GCP

当“大”以太坊(ETH)在 9 月 15 日切换到股权证明时，将不再可能在 GPU 上挖掘它。我们都知道。然而，仍然有其他硬币，例如以太坊经典(等)，我们可以继续挖掘。我们当然可以在像 AWS 或 GCP 这样的公共云中实现。

# 以太坊采矿和“合并”

很长一段时间，以太坊(ETH)曾经是 GPU 上最受欢迎的加密硬币。人们建造“采矿设备”——配备了大量英伟达或 AMD 镭龙图形处理器的专用计算机——并全天候运行它们，以赚取新的联邦理工学院硬币。建立这些钻井平台的成本是巨大的，几千甚至几万美元。对于许多想成为矿工的人来说，这种前期成本显然是一个很大的障碍。

欢迎*云端挖掘*。在云中——无论是 AWS 还是 GCP——我们可以按小时租用 GPU 容量，而不需要大量的前期投资来购买设备。多年来，我一直维护着 [**AWS 以太坊矿工**](https://github.com/mludvig/aws-ethereum-miner) 的开源模板，最近又维护了 [**GCP 以太坊矿工**](https://github.com/mludvig/gcp-ethereum-miner) 的开源模板，供任何人使用。

然而，在 2022 年 9 月 15 日，在一场名为*合并*的活动中，区块链以太坊将从*工作证明*(即采矿)转换为*股权证明* (PoS)。关于 PoS 的这一变化已经写了很多，我不打算在这里重复。底线是以太坊(ETH)采矿将不再可能在 9 月 15 日之后。

> 这是云中加密挖掘的终结吗？**当然不是！**

![](img/00303540f96617bfe2c6386e1c9abd92.png)

# AWS 和 GCP 的以太坊经典采矿

虽然开采 ETH 将很快结束，但仍有其他加密硬币有待开采。现在我已经更新了 AWS 和 GCP 的模板，支持以太坊经典，合并后仍然可以开采的硬币之一。不客气:)

开始之前，请记住以下几点:

1.  你*可能需要，也可能不需要*一个新的**钱包地址**，这要看情况。
    MetaMask 用户可以对 ETH 和 ETC 使用相同的地址，非常简单。
    分类账户用户*应该*创建一个新的 ETC 地址，但是有办法将发送到您的 ETH 分类账户地址的资金转移到 ETC 地址，所以如果您犯了一个错误，没什么大不了的。其他人——检查你的钱包或交易所，或者简单地为 ETC 创建一个新的地址以确保安全。
2.  ETC 盈利能力比 ETH 盈利能力波动更大，我们预计当矿商开始重新配置他们的钻机时，将会有更大的波动。自己研究一下采矿对你是否有利可图。一旦事情在一两个月内解决，我计划做一个新的分析，类似于我的旧帖子[新的 AWS 实例，使 ETH mining 有利可图](/coinmonks/new-aws-instance-that-makes-eth-mining-profitable-1dd87183cce7)。现在 DYOCA(自己做成本分析:-)

# AWS 快速入门

关于如何在 AWS 上开始挖掘，我就不一一赘述了。如果你是 AWS 或加密挖掘的新手，请参考我以前的文章，例如 [**在 AWS 上挖掘以太坊——完全指南**](/coinmonks/mining-ethereum-on-aws-the-complete-guide-6d1211b90da9) 。

![](img/158776c61153468d043a45b9979ee363.png)

以下是快速操作方法:

1.  登录您的 AWS 帐户。在进行下一步之前，请执行此操作。
2.  转到 [AWS 以太坊矿工 GitHub 库](https://github.com/mludvig/aws-ethereum-miner)并向下滚动到[快速入门部分](https://github.com/mludvig/aws-ethereum-miner#quick-start)。
3.  选择您想要开始采矿的地区，然后单击“[启动默认 VPC](https://console.aws.amazon.com/cloudformation/home?region=us-east-1#/stacks/new?stackName=ethminer&templateURL=https://s3.us-west-2.amazonaws.com/cnl4uehyq6/ethminer/template-eth-default-vpc.yml) ”链接。它将打开一个 CloudFormation 控制台，您可以在其中配置采矿堆栈—选择硬币(ETH 或 ETC)，设置您的钱包地址和所需的哈希值。
4.  单击 CloudFormation 控制台向导启动堆栈。正在创建，请稍候，这可能需要 5 到 10 分钟。
5.  打开 cloud formation“Outputs”选项卡，导航至“Dashboard URL”。大约 15 到 20 分钟后，你应该开始看到一些采矿进展。哈希拉特将逐渐上升，至少需要几个小时才能稳定下来。

在*成本浏览器*中密切关注你的 AWS 支出，记住云开采会很快变得非常昂贵。知道自己在做什么！

# GCP 快速启动

再次参考我的另一篇帖子——[**GCP 以太坊采矿**](/coinmonks/easy-ethereum-mining-on-gcp-576f0aaaeeed)——详细的分步教程。如果你是一个熟练的 GCP 用户，这里是快速纲要。

![](img/47134243b16ca035767217765ecedb54.png)

1.  登录到您的 GCP 帐户，并选择具有足够高的运行 GPU 实例配额的项目。
2.  打开 CloudShell，克隆 [GCP 以太坊矿工 Github 库](https://github.com/mludvig/gcp-ethereum-miner) :
    `git clone https://github.com/mludvig/gcp-ethereum-miner.git`
3.  编辑`terraform.tfvars`文件以满足您的需求。
4.  从`terraform init && terraform apply -auto-approve`开始矿工
5.  转到[以太矿等仪表板](https://etc.ethermine.org)并输入您的钱包地址。大约需要 15 到 20 分钟，你才会看到一些活动。然后，在接下来的几个小时里，它会逐渐上升。给它时间。

注意，GCP 对他们平台上的加密挖掘有点敏感。我们的 terraform 模板经过精心制作，不会引起他们的注意，但一如既往，使用它需要您自担风险！

**重要提示:**除非你 100%知道自己在做什么，否则不要修改模板。即使是一个小小的改变，比如选择不同的采矿软件或不同的采矿池，也可能会让你出错。

一如既往地注意你的开销！GCP 矿业会变得非常昂贵，非常快速，所以确保你知道你在做什么。

# 下一步是什么？

以太坊转移到 PoS 将不可避免地在加密挖掘领域引起涟漪。许多 GPU 所有者将开始尝试其他硬币，价格和盈利能力将在一段时间内大幅波动。局势可能需要几个月才能稳定下来。

一旦“合并”尘埃落定，我打算在云环境中研究不同硬币的盈利能力。订阅我的 [**云矿工邮件列表**](http://eepurl.com/h2_oZ5) 或成为我的 [**中型邮箱订阅者**](https://michael-ludvig.medium.com/subscribe) 之一以保持更新。最后，如果你是新手，请考虑订阅我的 [**会员链接**](https://michael-ludvig.medium.com/membership) 来支持我的工作。

## 开心挖矿！:-)

**如果你喜欢这个故事，想支持我这个作家，可以考虑用我的** [**会员链接**](https://michael-ludvig.medium.com/membership) **订阅 Medium。谢谢大家！**