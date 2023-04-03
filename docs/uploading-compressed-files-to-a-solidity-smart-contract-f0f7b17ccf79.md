# 将压缩文件上传到 Solidity Smart 合同

> 原文：<https://medium.com/coinmonks/uploading-compressed-files-to-a-solidity-smart-contract-f0f7b17ccf79?source=collection_archive---------3----------------------->

![](img/e485315ba269f6161471a55e5ceb1400.png)

# 创新正在召唤

通过使用来自 open AI 的令人惊讶的新 GPT 聊天，一个充满想法和知识的脑袋将它们联系在一起，我能够在创纪录的时间内制作出这个指南。

如果你还没看过这个，请点击这里的来玩[。](https://chat.openai.com/chat)

# 创建 React 应用程序

本指南假设你已经安装了节点，如果你没有，你可以从[这里](https://nodejs.org/en/download/)安装。

在你的桌面上为我们的前端应用程序创建一个新的目录，打开一个新的终端窗口和 cd 到那个目录。

在这个新目录中，在终端中键入以下内容，创建一个新的 react 应用程序:

```
npx create-react-app FileCompressor
```

安装后，您的新目录结构将被构建。

# 添加前端上传表单

转到 src/app.js 并导航到 app 函数，将此逻辑放入其中:

```
 import React from "react";

function App(props) {
  const handleChange = e => {
    const fileReader = new FileReader();
    fileReader.readAsText(e.target.files[0], "UTF-8");
    fileReader.onload = e => {
      if(e.target !== null) {
        console.log("e.target.result", e.target.result);
      }
    };
  };

  return (
    <>
      <h1>Upload file</h1>

      <input type="file" onChange={handleChange} />
    </>
  );
}

export default App;
```

这将创建一个名为 handleChange 的新函数，每次有文件上传到前端时都会调用这个函数。

# Base64

我们的文件目前没有 chain 可以理解的格式，我们接下来需要将它转换成 base64 字符串，以便存储在 chain 上。

回到您的 app.js 文件，我们将在其中向 handleChange 方法添加 base64 逻辑:

```
import React from "react";

function App(props) {
  const handleChange = e => {
    const fileReader = new FileReader();
    fileReader.readAsDataURL(file)
    fileReader.onload = () => {
      console.log(fileReader.result);
    }
    fileReader.onerror = (error) => {
      console.log(error);
    }
  };

  return (
    <>
      <h1>Upload file</h1>

      <input type="file" onChange={handleChange} />
    </>
  );
}

export default App;
```

到目前为止，我们有一个文件上传表单，它接受输入，将文件转换为 base64 字符串，并记录输出。这样很好，但是使用压缩来降低字节数更好。

**LZ 字符串压缩**

首先安装 lz 压缩库:

```
npm i lzma-js
```

安装后，将 app.js 修改为类似于下面的示例，这将把压缩的 base64 字符串记录到终端:

```
import React from "react";
import LZMA from "lzma-js";

function App(props) {
  const handleChange = e => {
    const fileReader = new FileReader();
    fileReader.readAsDataURL(file)
    fileReader.onload = () => {
      // Convert the base64 encoded string to a Uint8Array
      const uint8Array = Uint8Array.from(atob(fileReader.result), c => c.charCodeAt(0));

      // Compress the Uint8Array using LZMA-JS
      LZMA.compress(uint8Array, 9, (result, error) => {
        if (error) {
          // Handle error
        } else {
          // The compressed data is a Uint8Array, so you need to convert it to a base64 encoded string
          const compressedBase64String = btoa(String.fromCharCode(...result));
          console.log(compressedBase64String); // Outputs the compressed base64 encoded string
        }
      });
    }
    fileReader.onerror = (error) => {
      console.log(error);
    }
  };

  return (
    <>
      <h1>Upload file</h1>

      <input type="file" onChange={handleChange} />
    </>
  );
}

export default App;
```

**智能合约**

这是一个简单的智能契约，它存储一个字节 32 字符串的数组，在本例中，我们的字节 32 字符串是文件。

```
// SPDX-License-Identifier: MIT 

pragma solidity ^0.8.10;

contract FileStorage { 
  mapping (address => bytes32[]) userFiles;

  function addFile(string calldata _file) public { 
    userFiles[msg.sender].push(_file);
  }

  function deleteFile(uint arrayIndex) public {
    delete userFiles[msg.sender][arrayIndex];
  }

  function getFiles() public view returns (bytes32[] memory) {
    return userFiles[msg.sender];
  }
}
```

这是最低限度，所以对于生产情况，你可能需要更多，但上述合同将足以满足本教程的目的。部署完成后，您就可以将 react 应用程序连接到分散的文件数据库了。

# 连接 Dapp

首先为您部署的合同获取 abi，并存储在一个名为 abi.json 的新文件中。

```
[
 {
  "inputs": [
   {
    "internalType": "string",
    "name": "_file",
    "type": "string"
   }
  ],
  "name": "addFile",
  "outputs": [],
  "stateMutability": "nonpayable",
  "type": "function"
 },
 {
  "inputs": [
   {
    "internalType": "uint256",
    "name": "arrayIndex",
    "type": "uint256"
   }
  ],
  "name": "deleteFile",
  "outputs": [],
  "stateMutability": "nonpayable",
  "type": "function"
 },
 {
  "inputs": [],
  "name": "getFiles",
  "outputs": [
   {
    "internalType": "string[]",
    "name": "",
    "type": "string[]"
   }
  ],
  "stateMutability": "view",
  "type": "function"
 }
]
```

如果您返回到 app.js 文件，最后一步将是将文件字符串的控制台日志替换为智能合约调用。首先，我们引入契约 abi，填写契约地址，通过前端连接到元掩码，然后在连接建立后允许存储用户文件。

```
import React from "react";
import LZMA from "lzma-js";

function App(props) {
  const abi = require('./abi.json');
  const handleChange = e => {
    const fileReader = new FileReader();
    const CONTRACT_ADDRESS = "0xof";
    fileReader.readAsDataURL(file)
    fileReader.onload = () => {
      // Convert the base64 encoded string to a Uint8Array
      const uint8Array = Uint8Array.from(atob(fileReader.result), c => c.charCodeAt(0));

      // Compress the Uint8Array using LZMA-JS
      LZMA.compress(uint8Array, 9, (result, error) => {
        if (error) {
          console.log(error);
        } else {
            // The compressed data is a Uint8Array, so you need to convert it to a base64 encoded string
            const compressedBase64String = btoa(String.fromCharCode(...result));
            const { ethereum } = window;
            const provider = new ethers.providers.Web3Provider(ethereum);
            const signer = provider.getSigner()
            const connectedContract = new ethers.Contract(CONTRACT_ADDRESS, abi, signer);

            let tx = async() => await connectedContract.addFile(compressedBase64String);

            console.log(tx); // Outputs the result of the smart contract call
        }
      });
    }
    fileReader.onerror = (error) => {
      console.log(error);
    }
  };

  return (
    <>
      <h1>Upload file</h1>

      <input type="file" onChange={handleChange} />
    </>
  );
}

export default App;
```

这就是压缩 word 文档并将其存储在 chain 上所需的全部内容，而不仅仅是 word 文档，尽管几乎任何文件类型都可以被基于浏览器的上传程序所理解。

要返回您的文件并解压缩，您将需要创建一个新的方法来查询智能契约 getFiles 函数并循环每个函数，在每个迭代中，您只需调用下面的方法，并以适合您的用例的任何方式存储解压缩的数据:

```
LZMA.decompress(properties, inStream, outStream, outSize);
```

这种方法有很多可能性，但像往常一样，这只是一个指南，引导您建立坚实的基础和理解，但绝不是生产就绪。

去创造吧！

> 交易新手？试试[加密交易机器人](/coinmonks/crypto-trading-bot-c2ffce8acb2a)或者[复制交易](/coinmonks/top-10-crypto-copy-trading-platforms-for-beginners-d0c37c7d698c)