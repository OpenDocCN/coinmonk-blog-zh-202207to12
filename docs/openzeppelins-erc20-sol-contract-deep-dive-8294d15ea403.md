# OpenZeppelins ERC20.sol 合同深潜

> 原文：<https://medium.com/coinmonks/openzeppelins-erc20-sol-contract-deep-dive-8294d15ea403?source=collection_archive---------17----------------------->

# ERC20 标准

ERC20 标准是一组规则，定义了令牌在以太坊区块链上的行为方式。它指定了令牌协定必须实现的一组功能和事件，以便被视为 ERC20 令牌。开发该标准是为了确保不同的令牌可以互换，并且可以以类似的方式使用，而不管契约的具体实现如何。

# OpenZeppelins ERC20 合同

OpenZeppelin 的 ERC20 契约是由 OpenZeppelin 库提供的 ERC20 标准的开源实现。这是一份可靠的合同，可以作为构建您自己的 ERC20 令牌的起点。该契约包括 ERC20 标准中定义的所有必需的功能和事件，以及一些标准不要求但在 ERC20 令牌中常用的附加功能。

# open zeppelin er C20 合同的使用案例

OpenZeppelin 的 ERC20 合同的用例包括构建和部署您自己的 ERC20 令牌，使用该合同作为参考实现来了解 ERC20 令牌的工作方式，并在该合同的基础上为您的令牌添加附加功能。

# ERC20 标准和 OpenZeppelin 实现的区别

普通 ERC20 标准和 OpenZeppelin 实现之间的主要区别在于，后者包含了超出标准最低要求的附加特性和功能。这些附加功能包括铸造和刻录令牌的功能，以及各种访问控制功能，这些功能允许您设置谁可以传输和管理令牌的限制。

# 使用 OpenZeppelin 的 ERC20 合同的优势

使用 OpenZeppelin 的 ERC20 合同的一些优势包括:它是 ERC20 标准的一个经过充分测试和广泛使用的实现，它包括超出标准最低要求的附加特性和功能，并且它是开源的，可以自由使用和修改。此外，OpenZeppelin 提供了一系列其他合同和工具，可以与 ERC20 合同结合使用，以构建更复杂、功能更丰富的令牌系统。

# 合同的预演

```
// SPDX-License-Identifier: MIT
```

行`**// SPDX-License-Identifier: MIT**`是一个注释，指的是 MIT 许可证的 SPDX 许可证标识符。

```
pragma solidity ^0.4.24;
```

在 Solidity 中，`**pragma**`是一个指令，它指定了用来编写合同的 Solidity 版本。`**pragma**`指令通常放在 Solidity 文件的顶部，指示编译合同所需的 Solidity 的最低版本。

```
import "./IERC20.sol";
import "../../math/SafeMath.sol";
```

`**import**`指令用于将另一个实体文件中的代码包含在当前文件中。这通常用于包含外部库或在契约之间重用代码。

在 OpenZeppelin ERC20 契约中，`**import**`指令用于包含`**IERC20.sol**`和`**SafeMath.sol**`文件。

*   `**IERC20.sol**`文件定义了 ERC20 令牌标准的接口，其中包括 ERC20 令牌必须实现的必需和可选功能。OpenZeppelin ERC20 契约实现了这个接口，这允许它被其他契约和应用程序识别为 ERC20 令牌。
*   `**SafeMath.sol**`文件定义了一组数学函数，用于防止在处理大整数值时发生整数溢出和下溢。OpenZeppelin ERC20 契约使用这些函数来确保对`**uint256**`值执行算术运算是安全的。

```
contract ERC20 is IERC20 {
```

在 Solidity 中，`**contract**`关键字用于定义一个新的合同。

声明的`**is IERC20**`部分表示`**ERC20**`契约实现了`**IERC20**`接口。

Solidity 中的接口是一个契约，它定义了一组函数及其签名，但不提供这些函数的实现。接口通常用于指定其他契约必须遵循的一组通用规则或行为。

在这种情况下，`**IERC20**`接口定义了 ERC20 令牌必须实现的必需和可选功能。通过声明`**ERC20**`契约实现了`**IERC20**`接口，该契约表明它将为`**IERC20**`接口中定义的所有功能提供一个实现。

```
using SafeMath for uint256;
```

在 Solidity 中，`**uint256**`是一个无符号整数类型，可以保存从 0 到 2^256 - 1 的值。`**SafeMath**`库是一个数学函数集合，旨在防止在对`**uint256**`值执行算术运算时发生整数溢出和下溢。

为`**uint256**`使用`**SafeMath**`库意味着你正在使用库提供的算术函数的安全版本，而不是内置的可靠性函数。例如，您可以使用`**SafeMath**`库中的`**add**`函数，而不是使用内置的`**+**`运算符将两个`**uint256**`值相加。

使用`**SafeMath**`函数可以帮助保护您的契约免受因整数溢出或下溢而导致的漏洞的影响。在 Solidity 中处理大整数值时，使用`**SafeMath**`库是一个很好的实践。

```
mapping (address => uint256) private _balances;

mapping (address => mapping (address => uint256)) private _allowed;

uint256 private _totalSupply;
```

OpenZeppelin ERC20 契约中的`**_balances**`映射是一个私有变量，用于存储每个用户的余额。该映射由用户的以太坊地址决定，其值是用户在令牌中的余额。

`**_allowed**`映射是一个私有变量，用于存储每个用户代表另一个用户传输令牌的权限。该映射由正在存储其许可的用户的以太坊地址键控，并且该值是由被授权代表第一用户传送令牌的用户的以太坊地址键控的另一个映射。这个内部映射的值是允许第二用户代表第一用户传送令牌。

`**_totalSupply**`变量是一个私有变量，用于存储已经创建的令牌总数。

这些变量用于跟踪用户的余额和限额以及已创建的令牌总数。它们是私有变量，这意味着只能从协定内部或从它的继承协定中访问它们。

```
function totalSupply() public view returns (uint256) {
    return _totalSupply;
  }
```

`**totalSupply**`功能是一个公共查看功能，允许用户查看代币的总供应量。该函数不接受任何参数。

该函数只是返回令牌的总供应量。

它是一个公共视图函数，这意味着它可以被任何契约或外部参与者调用，并且不修改契约的状态。视图函数通常用于从合同中读取数据，而不会产生完整事务的开销。

```
function balanceOf(address owner) public view returns (uint256) {
    return _balances[owner];
  }
```

`**balanceOf**`功能是一个公共查看功能，允许一个用户查看另一个用户的余额。该函数有一个参数:

1.  `**owner**`:您要查看其余额的用户的以太坊地址。

该函数只是返回`**owner**`地址的余额。

```
function allowance(
    address owner,
    address spender
   )
    public
    view
    returns (uint256)
  {
    return _allowed[owner][spender];
  }
```

`**allowance**`功能是一个公共查看功能，允许一个用户查看另一个用户代表他们转移代币的许可。该函数有两个参数:

1.  `**owner**`:主人的以太坊地址。
2.  `**spender**`:挥霍者的以太坊地址。

该函数简单地返回允许`**spender**`地址代表`**owner**`地址传输令牌。

```
function transfer(address to, uint256 value) public returns (bool) {
    require(value <= _balances[msg.sender]);
    require(to != address(0));

    _balances[msg.sender] = _balances[msg.sender].sub(value);
    _balances[to] = _balances[to].add(value);
    emit Transfer(msg.sender, to, value);
    return true;
  }
```

`**transfer**`功能是一个公共功能，允许用户将指定数量的代币从自己的余额转移到另一个用户的余额。该函数有两个参数:

1.  `**to**`:要增加余额的用户的以太坊地址。
2.  `**value**`:您要转移的代币数量。

该功能首先检查`**value**`参数是否小于或等于调用者的余额(`**msg.sender**`)以及`**to**`地址是否不是零地址(`**0x0**`)。如果这些检查中的任何一个失败，该函数将抛出一个错误并退出。

如果检查通过，该函数将调用者的余额减少指定的`**value**`，并将`**to**`地址的余额增加相同的数量。然后，它发出一个`**Transfer**`事件来表明传输已经发生。

`**transfer**`功能是在用户之间转移代币的主要功能，用于将代币从一个用户的余额直接转移到另一个用户的余额。

```
/**
   * Beware that changing an allowance with this method brings the risk that someone may use both the old
   * and the new allowance by unfortunate transaction ordering. One possible solution to mitigate this
   * race condition is to first reduce the spender's allowance to 0 and set the desired value afterwards:
   */
  function approve(address spender, uint256 value) public returns (bool) {
    require(spender != address(0));

    _allowed[msg.sender][spender] = value;
    emit Approval(msg.sender, spender, value);
    return true;
  }
```

`**approve**`功能是一个公共功能，允许一个用户授权另一个用户代表他们转移指定数量的令牌。该函数有两个参数:

1.  `**spender**`:您要授权代表您转移代币的用户的以太坊地址。
2.  `**value**`:您要授权`**spender**`转移的令牌数量。

该功能首先检查`**spender**`地址不是零地址(`**0x0**`)。如果检查失败，该函数将抛出一个错误并退出。

如果检查通过，该函数设置`**spender**`地址的允许值，以代表调用者(`**msg.sender**`)向指定的`**value**`传送令牌。然后，它会发出一个`**Approval**`事件，表示津贴已经更新。

`**approve**`功能通常与`**transferFrom**`功能结合使用，以允许用户授权其他地址代表他们传输令牌。

```
function transferFrom(
    address from,
    address to,
    uint256 value
  )
    public
    returns (bool)
  {
    require(value <= _balances[from]);
    require(value <= _allowed[from][msg.sender]);
    require(to != address(0));

    _balances[from] = _balances[from].sub(value);
    _balances[to] = _balances[to].add(value);
    _allowed[from][msg.sender] = _allowed[from][msg.sender].sub(value);
    emit Transfer(from, to, value);
    return true;
  }
```

`**transferFrom**`功能是一个公共功能，允许用户将指定数量的代币从另一个用户的余额转移到第三个用户的余额。该函数有三个参数:

1.  `**from**`:您要减少余额的用户的以太坊地址。
2.  `**to**`:要增加余额的用户的以太坊地址。
3.  `**value**`:您要转移的代币数量。

该功能首先检查`**value**`参数是否小于或等于`**from**`地址的余额，以及是否小于或等于调用者(`**msg.sender**`)代表`**from**`地址传送令牌的许可。它还检查`**spender**`地址不是零地址(`**0x0**`)。如果这些检查中的任何一项失败，该函数将抛出一个错误并退出。

如果检查通过，该功能将按指定的`**value**`减少`**from**`地址的余额，并按相同的量增加`**to**`地址的余额。它还减少了调用者通过`**value**`代表`**from**`地址传输令牌的许可。最后，它发出一个`**Transfer**`事件来指示传输已经发生。

`**transferFrom**`功能通常与`**approve**`功能结合使用，以允许用户授权其他地址代表他们传输令牌。

```
function increaseAllowance(
    address spender,
    uint256 addedValue
  )
    public
    returns (bool)
  {
    require(spender != address(0));

    _allowed[msg.sender][spender] = (
      _allowed[msg.sender][spender].add(addedValue));
    emit Approval(msg.sender, spender, _allowed[msg.sender][spender]);
    return true;
  }
```

`**increaseAllowance**`功能是一个公共功能，允许一个用户增加另一个用户代表他们转移代币的许可。该函数有两个参数:

1.  `**spender**`:您要增加其权限的用户的以太坊地址。
2.  `**addedValue**`:您想要增加津贴的数量。

该功能首先检查`**spender**`地址不是零地址(`**0x0**`)。如果检查失败，该函数将抛出一个错误并退出。

如果检查通过，该函数增加指定的`**addedValue**`代表调用者`**msg.sender**`传送令牌的`**spender**`地址的许可。然后，它发出一个`**Approval**`事件，表明津贴已经更新。

`**increaseAllowance**`功能通常与`**decreaseAllowance**`功能结合使用，以允许用户调整另一个用户代表他们转移代币的许可。

```
function decreaseAllowance(
    address spender,
    uint256 subtractedValue
  )
    public
    returns (bool)
  {
    require(spender != address(0));

    _allowed[msg.sender][spender] = (
      _allowed[msg.sender][spender].sub(subtractedValue));
    emit Approval(msg.sender, spender, _allowed[msg.sender][spender]);
    return true;
  }
```

`**decreaseAllowance**`功能是一个公共功能，允许一个用户减少另一个用户代表他们转移代币的许可。该函数有两个参数:

1.  `**spender**`:您要减少其津贴的用户的以太坊地址。
2.  `**subtractedValue**`:您希望减少津贴的数量。

该功能首先检查`**spender**`地址不是零地址(`**0x0**`)。如果检查失败，该函数将抛出一个错误并退出。

如果检查通过，该函数将减少指定的`**subtractedValue**`代表调用者`**msg.sender**`传送令牌的`**spender**`地址的允许值。然后，它会发出一个`**Approval**`事件，表明津贴已经更新。

`**decreaseAllowance**`功能通常与`**increaseAllowance**`功能结合使用，以允许用户调整另一个用户代表他们转移代币的许可。

```
function _mint(address account, uint256 amount) internal {
    require(account != 0);
    _totalSupply = _totalSupply.add(amount);
    _balances[account] = _balances[account].add(amount);
    emit Transfer(address(0), account, amount);
  }
```

`**_mint**`功能是一个内部功能，允许用户创建指定数量的新代币并将它们添加到自己的余额中。该函数有两个参数:

1.  `**account**`:要增加余额的用户的以太坊地址。
2.  `**amount**`:您想要创建的令牌数量。

该功能首先检查`**account**`地址不是零地址(`**0x0**`)。如果检查失败，该函数将抛出一个错误并退出。

如果检查通过，该功能将令牌的总供应量增加指定的`**amount**`，并将`**account**`地址的余额增加相同的数量。然后，它发出一个`**Transfer**`事件来指示传输已经发生。

`**_mint**`功能通常用于创建新代币，并将它们添加到用户的余额中。它被标记为`**internal**`，这意味着它只能从契约内部或者从它继承的契约中被调用。

```
function _burn(address account, uint256 amount) internal {
    require(account != 0);
    require(amount <= _balances[account]);

    _totalSupply = _totalSupply.sub(amount);
    _balances[account] = _balances[account].sub(amount);
    emit Transfer(account, address(0), amount);
  }
```

`**_burn**`功能是一种内部功能，允许用户从自己的余额中烧掉指定数量的代币。该函数有两个参数:

1.  `**account**`:您要减少余额的用户的以太坊地址。
2.  `**amount**`:你要销毁的代币数量。

该功能首先检查`**account**`地址不是零地址(`**0x0**`)并且`**amount**`参数小于或等于`**account**`地址的余额。如果这些检查中的任何一个失败，该函数将抛出一个错误并退出。

如果检查通过，该功能将按指定的`**amount**`减少令牌的总供应量，并按相同的数量减少`**account**`地址的余额。然后，它发出一个`**Transfer**`事件来指示传输已经发生。

`**_burn**`函数通常用于销毁不再需要或被错误铸造的令牌。它被标记为`**internal**`，这意味着它只能从契约内部或者从它继承的契约中被调用。

```
function _burnFrom(address account, uint256 amount) internal {
    require(amount <= _allowed[account][msg.sender]);

    // Should <https://github.com/OpenZeppelin/zeppelin-solidity/issues/707> be accepted,
    // this function needs to emit an event with the updated approval.
    _allowed[account][msg.sender] = _allowed[account][msg.sender].sub(
      amount);
    _burn(account, amount);
  }
}
```

`**_burnFrom**`功能是一种内部功能，允许用户从另一个用户的余额中销毁指定数量的代币。该函数有两个参数:

1.  `**account**`:您要减少余额的用户的以太坊地址。
2.  `**amount**`:你想要销毁的代币数量。

该函数首先检查`**amount**`参数是否小于或等于调用者(`**msg.sender**`)代表`**account**`地址传输令牌的允许值。如果检查失败，该函数将抛出一个错误并退出。

如果检查通过，该函数将减少调用者根据指定的`**amount**`代表`**account**`地址传输令牌的许可。然后它调用`**_burn**`函数从`**account**`地址的余额中销毁指定数量的令牌。

`**_burnFrom**`功能通常与`**approve**`和`**transferFrom**`功能结合使用，允许用户授权其他地址代表他们传输和销毁一定数量的令牌。该函数被标记为`**internal**`，这意味着它只能从契约内部或从它继承的契约中调用。

# 参考

[ERC20 — OpenZeppelin 文档](https://docs.openzeppelin.com/contracts/4.x/erc20)

[open zeppelin-contracts/ERC 20 . sol 在主 open zeppelin/open zeppelin-contracts](https://github.com/OpenZeppelin/openzeppelin-contracts/blob/master/contracts/token/ERC20/ERC20.sol)

> 交易新手？在[最佳密码交易所](/coinmonks/crypto-exchange-dd2f9d6f3769)上尝试[密码交易机器人](/coinmonks/crypto-trading-bot-c2ffce8acb2a)或[复制交易](/coinmonks/top-10-crypto-copy-trading-platforms-for-beginners-d0c37c7d698c)