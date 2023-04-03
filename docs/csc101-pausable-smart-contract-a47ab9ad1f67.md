# CSC101-可暂停智能合同

> 原文：<https://medium.com/coinmonks/csc101-pausable-smart-contract-a47ab9ad1f67?source=collection_archive---------19----------------------->

在可暂停的智能合约中，我们可以暂停智能合约中的功能。智能合约的所有者有权暂停或启动智能合约中的某项功能。可暂停智能合同的使用案例是什么？

![](img/78db353f85d00a7149d8bc2b6bc3c9de.png)

假设我们正在创建一个 NFT 铸币智能合约，我们希望暂停铸币一段时间以持有 NFT，我们可以禁用将暂停该功能的功能。

可暂停智能合约的另一个用例可以是初始硬币发行(ico)。当我们为用户创建新令牌并希望限制用户不得交易这些令牌时。

如果您的合约有任何错误，并且黑客试图利用它，可暂停智能合约会很有用。作为智能合约的所有者，您可以暂停合约，以便我们可以阻止智能合约的滥用。

因此，让我们看看如何编写一个可暂停的智能合同。在代码解释之后，我将解释可暂停智能合约的缺点。

```
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.10;

contract Pausable {

    address public owner;
    bool public isPaused;

    constructor() {
        owner = msg.sender;
    }

    modifier onlyOwner() {
      require(msg.sender == owner, "On owner have access");
      _;
    }

    function setPaused(bool _paused) public onlyOwner {
        isPaused = _paused;
    }

    function withdraw(address payable _recipient) public onlyOwner {
        require(isPaused == false, "Contract Paused");
        payable(_recipient).transfer(address(this).balance);
    }
}
```

例如，我采用了一个智能合约，它将智能合约金额转移给调用该函数的用户。为了展示可暂停智能合约的功能，我必须选择该示例代码。

我们来破解密码。

```
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.10;

contract Pausable {
```

在前 2 行中，我们已经添加了将要编译的许可证和可靠性版本。在那之后，我们将我们的智能合约命名为 **Pausable**

```
address public owner;
bool public isPaused;

constructor() {
   owner = msg.sender;
}
```

我们初始化了两个状态变量 owner 和 **isPaused** ，之后，我们在构造函数 **msg.sender** 中初始化了 owner 地址，这是一个全局变量，包含将要部署智能联系人的用户的钱包地址。

```
modifier onlyOwner() {
      require(msg.sender == owner, "On owner have access");
      _;
    }
```

之后，我们添加了一个修饰符，它检查调用函数的人的所有权条件，调用方应该是智能契约的所有者。

```
function setPaused(bool _paused) public onlyOwner {
        isPaused = _paused;
    }
```

**setPaused** 功能用于更新 **isPaused** 状态变量。 **isPaused** 是一个布尔变量，所以我们可以对它设置 true 或 false。只有智能合约的所有者被授权调用这个函数，因为已经使用了我们在上面创建的 onlyOwner 修饰符。

```
function withdraw(address _recipient) public onlyOwner {
        require(isPaused == false, "Contract Paused");
        payable(_recipient).transfer(address(this).balance);
 }
```

之后，我们有一个撤销功能，该功能将检查 **isPaused** 状态，然后将智能合同金额转移给调用该功能的用户。

```
require(isPaused == false, "Contract Paused");
```

这一行确保了作为智能合约的所有者，我对这一特定功能拥有可中止的控制权。我可以随时暂停和启动该功能，这样我们就可以在智能合约中添加可暂停的功能。

这就是我们如何创建可暂停的功能。正如我前面提到的，可暂停功能也有一些缺点，所以让我们来看看这些缺点。

## 可暂停智能合同的缺点

正如我们所知，智能合约代码是公开的，任何人都可以阅读它，因此暂停功能会影响用户的信任，因为在看到智能合约的代码后，用户会知道您可以在智能合约中暂停该功能。假设您有 ICO 智能合约，并且您已经在其中添加了该功能，因此用户可能没有兴趣购买令牌，因为您可以随时暂停撤销功能。

一般来说，如果用户知道你的智能合约是可暂停的，那么用户就不会有兴趣使用你的 dApp。为了解决这个问题，我们还可以实现基于时间的暂停，这将确保在一定时间后暂停将被删除，并且所有者无法控制暂停合同。

我们将在接下来的教程中学习基于时间的暂停。

> 交易新手？试试[加密交易机器人](/coinmonks/crypto-trading-bot-c2ffce8acb2a)或者[复制交易](/coinmonks/top-10-crypto-copy-trading-platforms-for-beginners-d0c37c7d698c)