# 不死之谜

> 原文：<https://medium.com/coinmonks/ciphershastra-undead-puzzle-e12cc6185198?source=collection_archive---------18----------------------->

![](img/7537276fa100d13aeb8733c30a540f21.png)

Ciphershastra Challenge

![](img/50bf7988c854dbca9af87d7e0eb1caf2.png)

你可以在这里找到挑战:[https://ciphershastra.com/UnDeAD.html](https://ciphershastra.com/UnDeAD.html)

这个难题需要很多技巧来解决。我们唯一可用的外部函数是`deadOrAlive`函数。在这个函数中，我们必须满足几个检查，这样我们就可以在`UnDeAD`映射中将我们的散列地址设置为`true`，这证明我们已经通过了挑战。

# **死亡率**

我们发现的第一个检查是一个简单的网关，现在允许我们从同一个调用地址破解合同两次。下一个是对“死亡率”的检查。这个函数检查调用者地址在地址中是否有特定的模式。这个模式存储在`want`公共变量中。在查询它的时候，我们可以看到返回的字节是“0x0b100d”。这个模式需要出现在调用者的地址中才能通过检查。但是怎么做呢？这种检查表明，我们需要能够控制部署解决方案契约的地址。为了做到这一点，我们需要使用`CREATE2`操作码，该操作码通过使用部署的地址、部署的契约字节码和 Salt 作为生成最终部署地址的种子，在可预测的地址中部署契约。

通过修改 Salt，我们可以影响最终的部署地址。因此，我们只需要迭代 Salt，直到找到包含所需模式的地址，从而强制执行第一次检查。

在部署地址中查找生成特定模式的 salt 的脚本在[这里](https://gist.github.com/robercano/scripts/utils/findAddress.ts)可用。它将递增 salt，直到找到地址中的模式，然后返回 salt 和生成的地址。

这在[解决方案脚本](https://gist.github.com/robercano/tasks/solveUndead.ts)中使用，以找到生成正确地址的 salt。

然后我们准备了一个[工厂契约](https://gist.github.com/robercano/contracts/solutions/UndeadSolutionFactory.sol)来部署解决方案契约。它将使用 salt 生成具有所需模式的地址，并部署契约。

# **还没死**

第三项检查(我们现在跳过第二项)要求解决方案契约在调用函数`deadYet()`时返回字符串**“亡灵”**的编码字节。契约将返回字节`0x556e44654144`，这些字节是**“亡灵”**的 ascii 码。这在可靠性合同中很容易做到，但正如我们将在下一节中看到的，由于第二次检查，这将变得复杂。

# **迷你合同**

不死契约中的第二个检查强制解决方案契约只有 15 个字节的代码。如果我们在 Solidity 中创建契约并编译它，即使有大小优化，我们也会看到我们不能得到少于 140 字节的代码。这直接阻止了使用 Solidity 来创建解决方案契约。

我们可以创建它的另一种方法是使用 Yul，EVM 的汇编语言。一个最小的 Yul 合同应该是这样的:[https://gist . github . com/rober cano/55516 de 38 ba 3a 360 e 1050 eea6b 9f 60 c 7](https://gist.github.com/robercano/55516de38ba3a360e1050eea6b9f60c7)

`code`部分负责将运行时复制到区块链中，然后`runtime`做两件事:检查调用中没有发送值，然后使用选择器将调用分派给正确的函数。`code`段不能跳过，但`runtime`段可以尽量减少。我们可以移除`callvalue()`检查，然后使用`default`用例返回所需的值。这意味着契约将返回任何函数调用所需的字节，就像我们已经定义了`fallback()`函数来返回字符串“亡灵”的字节一样。

最终的 Yul 合同如下所示:

```
object "YulContract" {
  code {
    datacopy(0, dataoffset("runtime"), datasize("runtime"))
    return (0, datasize("runtime"))
  }
  object "runtime" {
    code {
      mstore(0,  0x556e44654144) // UnDeAD
      return(0, 0x20)
    }
  }
}
```

它将所需的字节存储在存储器地址 0:

```
mstore(0, 0x556e44654144) // UnDeAD
```

然后从那里返回 32 (0x20)字节(这正是不死契约所期望的):

```
return(0, 0x20)
```

之前的 Yul 代码是在 Remix 中编译的，产生的字节码被复制到解决方案脚本中:

```
0x600f80600d600039806000f3fe65556e4465414460005260206000f3
```

# **准备进攻！**

有了这一切，我们准备攻击亡灵契约。我们部署工厂契约，为当前部署者找到所需的 salt，并为 Yul 契约找到给定的字节码。然后使用找到的 Salt 通过工厂部署解决方案契约，最后将部署的解决方案契约地址传递给亡灵契约验证提交。瞧啊！

# Github 回购

你可以在我的 Github repo:[https://github.com/robercano/ciphershastra](https://github.com/robercano/ciphershastra)中找到这个解决方案以及其他密码破解挑战

# 关于我

我叫罗伯特·卡诺，你可以在 https://thesolidchain.com 的[找到我](https://thesolidchain.com)

> 交易新手？试试[密码交易机器人](/coinmonks/crypto-trading-bot-c2ffce8acb2a)或[复制交易](/coinmonks/top-10-crypto-copy-trading-platforms-for-beginners-d0c37c7d698c)