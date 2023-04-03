# 生锈了

> 原文：<https://medium.com/coinmonks/getting-rusty-3d8154091f48?source=collection_archive---------19----------------------->

所以，欢迎来到另一个博客，在介绍了 Solidity 之后，是时候去另一个著名的，可能是未来最好的区块链网络洗澡了——Solana 支持的语言 RUST。我们将在两篇文章中了解更多关于铁锈的知识，另一篇将是这篇文章的延续。

Rust 已经与以太坊共享一个平行空间，预计在未来，Rust 将比任何其他区块链网络都更适应，因为其母网络 Solana 在安全性、速度和并发性方面相对更好。

## 铁锈的历史

Rust 源于 Mozilla 员工 Graydon Hoare 于 2006 年开始的一个个人项目。Mozilla 于 2009 年开始赞助该项目，并于 2010 年正式宣布该项目。同年，工作从最初的用 [OCaml](https://en.wikipedia.org/wiki/OCaml) 编写的[编译器](https://en.wikipedia.org/wiki/Compiler)转移到基于用 Rust 编写的 [LLVM](https://en.wikipedia.org/wiki/LLVM) 的[自托管编译器](https://en.wikipedia.org/wiki/Self-hosting_(compilers))。新的 Rust 编译器命名为 rustc，在 2011 年成功地[编译了自己](https://en.wikipedia.org/wiki/Bootstrapping_(compilers))。第一个编号为[的预 alpha 版本](https://en.wikipedia.org/wiki/Software_release_life_cycle#Pre-alpha)的编译器 Rust 0.1 于 2012 年 1 月发布。
Rust 的[类型系统](https://en.wikipedia.org/wiki/Type_system)，在 Rust 的 0.2、0.3 和 0.4 版本之间变化很大。0.2 版本首次引入了[类](https://en.wikipedia.org/wiki/Class_(computer_programming))，0.3 版本通过使用接口增加了[析构函数](https://en.wikipedia.org/wiki/Destructor_(computer_programming))和[多态](https://en.wikipedia.org/wiki/Polymorphism_(computer_science))。在 Rust 0.4 中，增加了[性状](https://en.wikipedia.org/wiki/Trait_(computer_programming))作为提供[遗传](https://en.wikipedia.org/wiki/Inheritance_(object-oriented_programming))的手段；接口与特征统一起来，作为一个独立的特性被移除。类也被移除并被实现和[结构化类型](https://en.wikipedia.org/wiki/Record_(computer_science))的组合所取代。除了常规的[静态类型化](https://en.wikipedia.org/wiki/Static_typing)，在 0.4 版本之前，Rust 还通过[契约](https://en.wikipedia.org/wiki/Design_by_contract)支持[类型状态分析](https://en.wikipedia.org/wiki/Typestate_analysis)。它在 0.4 版中被删除，尽管同样的功能可以通过利用 Rust 的[类型系统](https://en.wikipedia.org/wiki/Type_system)来实现。

2014 年 1 月， [*多布博士期刊*](https://en.wikipedia.org/wiki/Dr._Dobb%27s_Journal) 的主编安德鲁·宾斯托克(Andrew Binstock)在评论 Rust 成为除了语言 [D](https://en.wikipedia.org/wiki/D_(programming_language)) 、 [Go](https://en.wikipedia.org/wiki/Go_(programming_language)) 、 [Nim](https://en.wikipedia.org/wiki/Nim_(programming_language)) (然后是 Nimrod)之外的 [C++](https://en.wikipedia.org/wiki/C%2B%2B) 的竞争对手的机会时，根据宾斯托克的说法，虽然 Rust“被广泛认为是一种非常优雅的语言”，但由于它在不同版本之间反复变化，采用速度变慢了。第一个[稳定版本](https://en.wikipedia.org/wiki/Stable_release)，Rust 1.0，于 2015 年 5 月 15 日公布

## 了解 RUST 计划的要点

**声明变量-** 在 Rust 中，创建变量也被称为声明变量。
设 num = 1；
这里， **let** 关键字是初始化器， **num** 是变量名， **1** 是值， **=** 符号是赋值运算符。我们也可以说 1 是所有者为 num 的值。

**打印报表-**

**fn main(){
println！(“你好”)；
}
可以理解为 Rust 语言的全球范围。为了打印任何东西，我们需要写“println！”。这是一个命令，它触发编译器打印参数中给定的值。**

现在，打印任意变量-
**fn main(){
让 num = 1；
println！(" {} "，num)；
}** 这里我们给了一个占位符{}，它将作为预留的空白空间，由编译器遇到的最新变量填充。
同样，如果我们必须打印更多的变量和字符串值，那么-
**println！(“num1 是{}，num2 是{}”，num1，num 2)；**
这里，第一个占位符将由 num1 变量值填充，第二个占位符由 num2 值填充，每次填充都将按顺序进行。

**Rust 中的命名约定-**

可以使用以下规则定义变量名-

*   由字母、数字、下划线组成，
*   必须以字母或下划线开头，
*   大写字母和小写字母是不同的，因为 Rust 区分大小写，
*   不得是任何保留的关键字。

常见命名约定-
-骆驼案例(如 someVariable)
-蛇案例(如 some_variable)

**Rust 中的关键字-** 关键字是预定义的保留字，用于编程，在编译器中有特殊含义。
关键字是语法的一部分，不能用作标识符。

**Rust 中的数据类型-**
Rust 中有两种数据类型。
-标量类型
-复合类型
*Scaler 类型-* 它代表单个值。一共有 4 种主定标器数据类型-

*   整数，
*   浮点，
*   布尔型，
*   character
    example-let num:bool = true；

**整数
大小有符号无符号** 8 位 i8 u8
16 位 i16 u16
32 位 i132 u32
64 位 i64 u64
128 位 i128 u128
还有一个大小声明——isSize:isSize 以特定系统的处理器大小来响应。如果是 i64 处理器，那么 isSize=64 字节。由于每个字节有 8 位，所以 i8 可以存储 64 位的值，以此类推。

取值范围可以- ***-(2^n — 1)到+(2^n — 1) -1*** 为无符号，
和 0 到 ***+(2^n — 1) -1*** 为有符号。

**浮点-**-
它用来存储十进制数值。
有两种类型的十进制存储类型声明-
f32 和 f64。
需要注意的是，当尾部十进制数值较少时，我们使用 f32，当尾部十进制数值较多时，我们使用 f64。对于存储 5.6，我们将使用 f32，对于 3.56，我们将使用 f64。

**布尔型** let bool _ true:bool = true；
设 bool _ false:bool = false；

我们可以在 Char 中存储任何东西，包括字母、数字、字符、下划线和空格。 **让 var _ char:char = ' 2ab _&7c '；**

> 加入 Coinmonks [电报频道](https://t.me/coincodecap)和 [Youtube 频道](https://www.youtube.com/c/coinmonks/videos)了解加密交易和投资

# 另外，阅读

*   [BlockFi vs 摄氏](/coinmonks/blockfi-vs-celsius-vs-hodlnaut-8a1cc8c26630) | [Hodlnaut 点评](/coinmonks/hodlnaut-review-best-way-to-hodl-is-to-earn-interest-on-your-bitcoin-6658a8c19edf) | [KuCoin 点评](https://coincodecap.com/kucoin-review)
*   [Bitsgap 评审](/coinmonks/bitsgap-review-a-crypto-trading-bot-that-makes-easy-money-a5d88a336df2) | [Quadency 评审](/coinmonks/quadency-review-a-crypto-trading-automation-platform-3068eaa374e1) | [Bitbns 评审](/coinmonks/bitbns-review-38256a07e161)
*   [加密复制交易平台](/coinmonks/top-10-crypto-copy-trading-platforms-for-beginners-d0c37c7d698c) | [Coinmama 审核](/coinmonks/coinmama-review-ace5641bde6e)
*   [印度的加密交易所](/coinmonks/bitcoin-exchange-in-india-7f1fe79715c9) | [比特币储蓄账户](/coinmonks/bitcoin-savings-account-e65b13f92451)
*   [OKEx vs KuCoin](https://coincodecap.com/okex-kucoin) | [摄氏替代品](https://coincodecap.com/celsius-alternatives) | [如何购买 VeChain](https://coincodecap.com/buy-vechain)