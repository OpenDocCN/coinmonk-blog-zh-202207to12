# 如何在 CSC 上创建自己的众筹合同-第 1 部分

> 原文：<https://medium.com/coinmonks/how-to-create-your-own-crowdsale-cotract-on-csc-part1-b571420d8f7?source=collection_archive---------6----------------------->

让我们假设我们有一个项目，并希望在区块链(而不是集中交易所)独立推出销售计划。你如何给很多人分发很多代币？答案是众卖合约。在本教程中，我们将在 csc 上创建一个批量销售合同。

![](img/0c758e50bd1cc3d271d82e72287a9306.png)

photo by bianca fazacas

## 项目设置

首先，我们为我们的项目创建一个目录。在终端类型中:

```
mkdir crowdsale
cd crowdsale
```

这里我们做了两件事:
我们修改了 Open-Zeppelin 的旧众卖智能合约，使其符合 Solidity 0.6
我们在它的基础上写出了自己的众卖

随着 OpenZeppelin 接近 Solidity 0.6，众卖合约被删除。有些人倾向于给令牌智能合约本身添加一个
“mint Token”功能或类似的东西，但这将是糟糕的设计。我们应该
添加一个单独的处理令牌分发的众筹合同。

要安装 Openzeppelin 库，请在终端中键入:

```
npm install @openzeppelin/contracts
```

## CRC20

我们的 token.sol 是 crc-20。以太网上的 crc-20 与 erc-20 相同

ERC20 是以太坊区块链上用于实现令牌的智能合约的技术标准。ERC 代表以太坊评论请求，20 是分配给该请求的号码。

```
pragma solidity >=0.8.10;
import “@openzeppelin/contracts/token/ERC20/ERC20.sol”;
contract MyToken is ERC20 {
constructor(uint256 initialSupply) ERC20(“coinex smart chain token”, “CST”) public {
_mint(msg.sender, initialSupply);
_setupDecimals(0);
}
}
```

让我们为我们的令牌创建众筹合同

## 众筹合同

创建一个名为“TokenSale.sol”的文件，并将它放入:

```
pragma solidity ^0.8.10;
import “./Crowdsale.sol”;
contract MyTokenSale is Crowdsale {
KycContract kyc;
constructor(
uint256 rate, // rate in TKNbits
address payable wallet,
IERC20 token
)
Crowdsale(rate, wallet, token)
public
{ }
}
```

您可以使用 Hardhat、Tuffle 和 csc 工具包进行编译和测试

如果你喜欢安全帽，你可以阅读这篇文章:

[https://medium . com/coin monks/introduction-to-smart-contract-development-and-dapp-development-on-CSC-part 3-e 5 ddbe 2c 23 fa](/coinmonks/introduction-to-smart-contract-development-and-dapp-development-on-csc-part3-e5ddbe2c23fa)

如果你想用松露，你可以用这篇文章:

[](/coinmonks/smart-contract-deployment-on-csc-using-truffle-152d09cd1abb) [## 使用 Truffle 在 CSC 上部署智能合同

### 您是否曾经尝试过使用 truffle 在 coinex 智能链上部署您的智能合约？在本教程中，我们希望…

medium.com](/coinmonks/smart-contract-deployment-on-csc-using-truffle-152d09cd1abb) 

为了测试令牌，我们想稍微改变一下我们通常的设置。让我们使用 chai 的 expect 来测试令牌从
所有者到另一个帐户的转移。

然后在 tests 文件夹中创建我们的测试。创建一个名为/tests/MyToken.test.js 的新文件:

```
const Token = artifacts.require(“MyToken”);
var chai = require(“chai”);
const BN = web3.utils.BN;
const chaiBN = require(‘chai-bn’)(BN);
chai.use(chaiBN);
var chaiAsPromised = require(“chai-as-promised”);
chai.use(chaiAsPromised);
const expect = chai.expect;
contract(“Token Test”, async accounts => {
const [ initialHolder, recipient, anotherAccount ] = accounts;
it(“All tokens should be in my account”, async () => {
let instance = await Token.deployed();
let totalSupply = await instance.totalSupply();
//old style:
//let balance = await instance.balanceOf.call(initialHolder);
//assert.equal(balance.valueOf(), 0, “Account 1 has a balance”);
//condensed, easier readable style:
await expect(instance.balanceOf(initialHolder)).to.eventually.be.a.bignumber.equal(totalSupply);
});
});
```

在上面的代码中，我们使用 chai js 来测试我们的契约。我们将在下一篇文章中讨论柴

为了让我们的 crowdsale 智能合约发挥作用，我们必须将所有的钱发送到合约。这是在我们的 truffle 安装中的迁移
阶段完成的:
问题是现在测试失败了。让我们将标准的 Truffle 测试套件更改为 openzeppelin 测试套件:

```
 var MyToken = artifacts.require(“./MyToken.sol”);
var MyTokenSales = artifacts.require(“./MyTokenSale.sol”);
module.exports = async function(deployer) {
let addr = await web3.eth.getAccounts();
await deployer.deploy(MyToken, 1000000000);
await deployer.deploy(MyTokenSales, 1, addr[0], MyToken.address);
let tokenInstance = await MyToken.deployed();
await tokenInstance.transfer(MyTokenSales.address, 1000000000);
};
```

## KYC 智能合同

首先，我们将添加一个处理白名单的 KYC 智能合约。在 contracts/KycContract.sol 中增加
以下内容。

```
pragma solidity ^0.8.10;
import “@openzeppelin/contracts/access/Ownable.sol”;
contract KycContract is Ownable {
mapping(address => bool) allowed;
function setKycCompleted(address _addr) public onlyOwner {
allowed[_addr] = true;
}
function setKycRevoked(address _addr) public onlyOwner {
allowed[_addr] = false;
}
function kycCompleted(address _addr) public view returns(bool) {
return allowed[_addr];
}
}
```

将 Crowdsale.sol 和 KycContrct.sol 导入 MyTokenSale.sol:

```
pragma solidity ^0.8.10;
import “./Crowdsale.sol”;
import “./KycContract.sol”;
contract MyTokenSale is Crowdsale {
KycContract kyc;
constructor(
uint256 rate, // rate in TKNbits
address payable wallet,
IERC20 token,
KycContract _kyc
)
Crowdsale(rate, wallet, token)
public
{
kyc = _kyc;
}
function _preValidatePurchase(address beneficiary, uint256 weiAmount) internal view override {
super._preValidatePurchase(beneficiary, weiAmount);
require(kyc.kycCompleted(beneficiary), “KYC not completed yet, aborting”);
} 
```

## 部署

让我们自动进行部署:

```
var MyToken = artifacts.require(“./MyToken.sol”);
var MyTokenSales = artifacts.require(“./MyTokenSale.sol”);
var KycContract = artifacts.require(“./KycContract.sol”);
require(‘dotenv’).config({path: ‘../.env’});
module.exports = async function(deployer) {
let addr = await web3.eth.getAccounts();
await deployer.deploy(MyToken, process.env.INITIAL_TOKENS);
await deployer.deploy(KycContract);
await deployer.deploy(MyTokenSales, 1, addr[0], MyToken.address, KycContract.address);
let tokenInstance = await MyToken.deployed();
await tokenInstance.transfer(MyTokenSales.address, process.env.INITIAL_TOKENS);
};
```

您可以在 npm 中安装 dotenv:

```
# install locally (recommended)
npm install dotenv --save
```

Dotenv 是一个零依赖模块，它将环境变量从一个`.env`文件加载到`[process.env](https://nodejs.org/docs/latest/api/process.html#process_process_env)`中。将配置存储在独立于代码的环境中是基于[十二因素应用](http://12factor.net/config)方法的。

祝贺🥳

我们已经编写了 dapp 的后端代码，在下一部分中，我们将使用前端代码

> 交易新手？试试[密码交易机器人](/coinmonks/crypto-trading-bot-c2ffce8acb2a)或[复制交易](/coinmonks/top-10-crypto-copy-trading-platforms-for-beginners-d0c37c7d698c)