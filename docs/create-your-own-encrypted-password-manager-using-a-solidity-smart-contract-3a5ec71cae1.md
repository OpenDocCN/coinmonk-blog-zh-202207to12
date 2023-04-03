# 使用 Solidity 智能合同创建您自己的加密密码管理器

> 原文：<https://medium.com/coinmonks/create-your-own-encrypted-password-manager-using-a-solidity-smart-contract-3a5ec71cae1?source=collection_archive---------5----------------------->

![](img/a7c4d73f2cd827abaec3d5249e2359a1.png)

随着智能合约的使用增加，它在金融科技和通用系统中占据主导地位，我们需要了解这些公共链上个人信息的安全性，这一点至关重要。

本指南绝不是生产就绪，但它将向您展示您需要了解如何在智能合同中存储加密数据的基础，使不良行为者更难从链中解密和泄露用户信息。

安装一个 NodeJS 环境，并在 Src 目录中设置一个名为 Encrypt.js 的简单脚本。

在项目目录中打开一个新的终端窗口，并运行以下命令:

```
npm i dotenv --save
npm i crypto-js --save
```

打开 Encrypt.js，导入 dotenv 和 crypto-js 库，然后保存:

```
require('dotenv').config();

const CryptoJS = require('crypto-js');
```

在项目目录的顶层创建一个新文件，文件名为“”。env”。

⚠️ **确保如果使用这个项目的 git 库添加这个文件到你的. gitignore.**

将以下内容复制到您的新。env 文件，设置您的密钥(这是用于加密和解密)一旦设置保存此文件:

```
SECRET_KEY="YOURSECRETKEYGOESHERE"
```

返回 Encrypt.js 并添加以下方法，该脚本现在应该如下所示:

```
require('dotenv').config();

const CryptoJS = require('crypto-js');

const encryptWithAES = (text) => {
  const passphrase = process.env.SECRET_KEY;
  return CryptoJS.AES.encrypt(text, passphrase).toString();
}; export encryptWithAES;

const decryptWithAES = (ciphertext) => {
  const passphrase = process.env.SECRET_KEY;
  const bytes = CryptoJS.AES.decrypt(ciphertext, passphrase);
  const originalText = bytes.toString(CryptoJS.enc.Utf8);
  return originalText;
}; export decryptWithAES;
```

到目前为止，我们已经有了加密和解密文本的方法。环境文件。我们的下一步是创建保存我们数据的智能契约。

```
// SPDX-License-Identifier: MIT
// PasswordManager 
pragma solidity 0.8.17;

import "@openzeppelin/contracts/access/Ownable.sol";
import "@openzeppelin/contracts/utils/Address.sol";
import "@openzeppelin/contracts/utils/Strings.sol";

contract PasswordManager is Ownable {
    using Strings for uint256;

    struct MyAccounts {
        string accountName;
        string accountUsername;
        string accountEmail;
        string accountPassword;
        uint256 id;
    }

    MyAccounts[] private accounts;

    // ===== Check Caller Is User =====
    modifier callerIsUser() {
        require(tx.origin == msg.sender, "[Error] Function cannot be called by a contract");
        _;
    }

    // ===== Validate Entry Values =======
    function checkValues(
        string calldata accountName, 
        string calldata accountUsername, 
        string calldata accountEmail, 
        string calldata accountPassword
    ) internal pure {
        unchecked {
            require(bytes(accountName).length > 0, "[Error] Account Name Can't Be Blank");
            require(bytes(accountUsername).length > 0, "[Error] Account Userame Can't Be Blank");
            require(bytes(accountEmail).length > 0, "[Error] Account Email Can't Be Blank");
            require(bytes(accountPassword).length > 0, "[Error] Account Password Can't Be Blank");
        }
    }

    // ====== Get New ID =========
    function generateID() internal view returns(uint256) {
        unchecked {
            return accounts.length + 1;
        }
    }

    // ===== Add New Account Record =======
    function addNewAccount(
        string calldata accountName, 
        string calldata accountUsername, 
        string calldata accountEmail, 
        string calldata accountPassword
    ) public callerIsUser() onlyOwner {
        checkValues(accountName, accountUsername, accountEmail, accountPassword);
        accounts.push(MyAccounts(accountName, accountUsername, accountEmail, accountPassword, generateID()));
    }

    // ===== Upate Account Record =======
    function updateAccount(
        uint256 recordID, 
        string calldata accountName, 
        string calldata accountUsername, 
        string calldata accountEmail, 
        string calldata accountPassword
    ) public callerIsUser() onlyOwner {
        checkValues(accountName, accountUsername, accountEmail, accountPassword);
        accounts[recordID] = MyAccounts(accountName, accountUsername, accountEmail, accountPassword, recordID);
    } 

    // ===== Delete Account Record =======
    function deleteAccount(
        uint256 recordID
    ) public callerIsUser() onlyOwner {
        delete accounts[recordID];
    } 

    // ===== Get Account Record =======
    function getAccountByID(
        uint256 recordID
    ) public view callerIsUser() onlyOwner returns (MyAccounts memory) {
        return accounts[recordID];
    } 

    // ===== Get All Accounts ======
    function allAccounts() public view callerIsUser() onlyOwner returns (MyAccounts[] memory) {
        return accounts;
    } 
}
```

使用 remix 或 hardhat 将此合同部署到 testnet，一旦启用，我们可以使用 ethers.js 将我们的加密 dapp 连接到我们的合同。在 Src 目录中创建一个新文件，并将其命名为 PasswordManager.js。

在这个新文件中，我们首先需要使我们的加密和解密函数可用于管理器文件，将下面的部分复制到管理器文件中并保存。

```
const { decryptWithAES, encryptWithAES } = require('./Encrypt.js');
```

保存 PasswordManager.js 并在 src 目录中创建一个名为 abi.json 的新文件。在该文件中，您将粘贴您的合同 abi。如果使用上面的合同，它将如下所示:

```
[
 {
  "anonymous": false,
  "inputs": [
   {
    "indexed": true,
    "internalType": "address",
    "name": "previousOwner",
    "type": "address"
   },
   {
    "indexed": true,
    "internalType": "address",
    "name": "newOwner",
    "type": "address"
   }
  ],
  "name": "OwnershipTransferred",
  "type": "event"
 },
 {
  "inputs": [
   {
    "internalType": "string",
    "name": "accountName",
    "type": "string"
   },
   {
    "internalType": "string",
    "name": "accountUsername",
    "type": "string"
   },
   {
    "internalType": "string",
    "name": "accountEmail",
    "type": "string"
   },
   {
    "internalType": "string",
    "name": "accountPassword",
    "type": "string"
   }
  ],
  "name": "addNewAccount",
  "outputs": [],
  "stateMutability": "nonpayable",
  "type": "function"
 },
 {
  "inputs": [],
  "name": "allAccounts",
  "outputs": [
   {
    "components": [
     {
      "internalType": "string",
      "name": "accountName",
      "type": "string"
     },
     {
      "internalType": "string",
      "name": "accountUsername",
      "type": "string"
     },
     {
      "internalType": "string",
      "name": "accountEmail",
      "type": "string"
     },
     {
      "internalType": "string",
      "name": "accountPassword",
      "type": "string"
     },
     {
      "internalType": "uint256",
      "name": "id",
      "type": "uint256"
     }
    ],
    "internalType": "struct PasswordManager.MyAccounts[]",
    "name": "",
    "type": "tuple[]"
   }
  ],
  "stateMutability": "view",
  "type": "function"
 },
 {
  "inputs": [
   {
    "internalType": "uint256",
    "name": "recordID",
    "type": "uint256"
   }
  ],
  "name": "deleteAccount",
  "outputs": [],
  "stateMutability": "nonpayable",
  "type": "function"
 },
 {
  "inputs": [
   {
    "internalType": "uint256",
    "name": "recordID",
    "type": "uint256"
   }
  ],
  "name": "getAccountByID",
  "outputs": [
   {
    "components": [
     {
      "internalType": "string",
      "name": "accountName",
      "type": "string"
     },
     {
      "internalType": "string",
      "name": "accountUsername",
      "type": "string"
     },
     {
      "internalType": "string",
      "name": "accountEmail",
      "type": "string"
     },
     {
      "internalType": "string",
      "name": "accountPassword",
      "type": "string"
     },
     {
      "internalType": "uint256",
      "name": "id",
      "type": "uint256"
     }
    ],
    "internalType": "struct PasswordManager.MyAccounts",
    "name": "",
    "type": "tuple"
   }
  ],
  "stateMutability": "view",
  "type": "function"
 },
 {
  "inputs": [],
  "name": "owner",
  "outputs": [
   {
    "internalType": "address",
    "name": "",
    "type": "address"
   }
  ],
  "stateMutability": "view",
  "type": "function"
 },
 {
  "inputs": [],
  "name": "renounceOwnership",
  "outputs": [],
  "stateMutability": "nonpayable",
  "type": "function"
 },
 {
  "inputs": [
   {
    "internalType": "address",
    "name": "newOwner",
    "type": "address"
   }
  ],
  "name": "transferOwnership",
  "outputs": [],
  "stateMutability": "nonpayable",
  "type": "function"
 },
 {
  "inputs": [
   {
    "internalType": "uint256",
    "name": "recordID",
    "type": "uint256"
   },
   {
    "internalType": "string",
    "name": "accountName",
    "type": "string"
   },
   {
    "internalType": "string",
    "name": "accountUsername",
    "type": "string"
   },
   {
    "internalType": "string",
    "name": "accountEmail",
    "type": "string"
   },
   {
    "internalType": "string",
    "name": "accountPassword",
    "type": "string"
   }
  ],
  "name": "updateAccount",
  "outputs": [],
  "stateMutability": "nonpayable",
  "type": "function"
 }
]
```

接下来我们需要导入。我们将用于链上交互的 env 文件和 libs 导入到我们的 PasswordManager.js 文件中，我们还将导入新的 abi 文件:

```
require('dotenv').config();

const { decryptWithAES, encryptWithAES } = require('./Encrypt.js');
const { ethers, providers } = require('ethers');
const abi = require('./abi.json');
```

接下来获取您部署的合同地址，并存储到 PasswordManager.js 中的一个常量中

```
const contractAddress = "0xC36442b4a4522E871399CD717aBDD847Ab11FE88"
```

保存文件，转到 env 并为您的钱包私钥(签署交易的那个)添加这一行，然后保存。

```
SECRET_KEY="YOURSECRETKEYGOESHERE"
PRIVATE_KEY="YOURWALLETPRIVATEKEYGOESHERE"
```

现在回到我们的密码管理器文件，我们将添加我们的第一个契约方法调用:

```
const signer = new ethers.Wallet(
    process.env.PRIVATE_KEY,
    providers.getDefaultProvider('testnet')
);

const contract = new ethers.Contract(contractAddress, abi, signer);

const addNewAccount = async (
    accountName, 
    accountUsername,
    accountEmail,
    accountPassword
) => {
    const a = await contract.callStatic.addNewAccount(
      accountName, 
      accountUsername,
      accountEmail,
      accountPassword
    )
    console.log(a);
}

export addNewAccount;
```

现在我们将加密我们的输入，这可以用前端表单数据代替，但在这种情况下，我们将使用纯代码。

```
const encryptName = encryptWithAES("Facebook");
const encryptUsername = encryptWithAES("Arkay92");
const encryptEmail = encryptWithAES("arkay@gmail.com");
const encryptPassword = encryptWithAES("Password!");
```

最后，我们需要用加密的参数调用我们的方法:

```
addNewAccount(encryptName, encryptUsername, encryptEmail, encryptPassword);
```

这就是我们如何在 chain 上存储加密数据。要进行解密，我们只需在契约中查询我们想要的记录 ID，并对返回的记录运行解密函数 decryptWithAES()。

PasswordManager.js 的完整代码示例:

```
require('dotenv').config();

const { decryptWithAES, encryptWithAES } = require('./Encrypt.js');
const { ethers, providers } = require('ethers');
const abi = require('./abi.json');

const encryptName = encryptWithAES("Facebook");
const encryptUsername = encryptWithAES("Arkay92");
const encryptEmail = encryptWithAES("arkay@gmail.com");
const encryptPassword = encryptWithAES("Password!");

const contractAddress = "0xC36442b4a4522E871399CD717aBDD847Ab11FE88";

const signer = new ethers.Wallet(
    process.env.PRIVATE_KEY,
    providers.getDefaultProvider('testnet')
);

const contract = new ethers.Contract(contractAddress, abi, signer);

const addNewAccount = async (
    accountName, 
    accountUsername,
    accountEmail,
    accountPassword
) => {
    const a = await contract.callStatic.addNewAccount(
      accountName, 
      accountUsername,
      accountEmail,
      accountPassword
    )
    console.log(a);
}

export addNewAccount;

addNewAccount(encryptName, encryptUsername, encryptEmail, encryptPassword);
```

我们还可以做很多很多事情来使它更加安全、可扩展和可靠，但这是有趣的部分。去创作吧！

> 交易新手？试试[加密交易机器人](/coinmonks/crypto-trading-bot-c2ffce8acb2a)或者[复制交易](/coinmonks/top-10-crypto-copy-trading-platforms-for-beginners-d0c37c7d698c)