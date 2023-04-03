# 模糊智能合同

> 原文：<https://medium.com/coinmonks/fuzzing-smart-contracts-552fab1afb43?source=collection_archive---------17----------------------->

## 在以太坊区块链，模糊智能合约正变得越来越混合:组合工具允许模糊覆盖更多的代码路径并发现更多的 bug。

澳大利亚昆士兰大学的汤姆·马尔科姆[在他们的博客中写道，要改进一种常见的智能合同模糊器](https://blog.trailofbits.com/2022/12/08/hybrid-echidna-fuzzing-optik-maat/)、[和](https://github.com/crytic/echidna)。他们将鼹鼠与符号执行框架[玛特](https://github.com/trailofbits/maat)结合起来，以创建混合鼹鼠，作为 [Optik](https://github.com/crytic/optik) 的一部分，来发现智能合约中的更多漏洞。