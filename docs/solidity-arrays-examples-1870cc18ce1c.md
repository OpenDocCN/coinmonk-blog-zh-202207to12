# 坚固性——数组+示例

> 原文：<https://medium.com/coinmonks/solidity-arrays-examples-1870cc18ce1c?source=collection_archive---------5----------------------->

![](img/49a7718d7446f8e61a083a1eaa997d6d.png)

Photo by [Markus Spiske](https://unsplash.com/es/@markusspiske?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/data-array?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

在 Solidity 中，数组采用了与 JavaScript 类似的结构和风格，但是仍然有一些复杂的语言特有的细节。这篇文章是 Solidity 数组的入门，并试图总结其中的一些特性。一如既往，如果我做错了什么，请告诉我。

## 介绍

Solidity 中的数组是一种引用类型，这意味着它们*引用*现有数据。这与值类型形成对比，值类型传递该值的独立副本以供使用。因此，这意味着引用类型可以通过多个不同的名称(即它们的每个引用)来修改。这类似于 JavaScript 引用类型。

在下面的例子中，`z`和`y`都引用了`x`，因此当调用`f()`时，它们都可以改变(中的第三个元素)`x`:

在 Solidity 中，引用类型由结构、数组和映射组成，使用起来比基本值类型(int、bools 等)更复杂，因为数据位置也必须显式声明:或者`memory`、`storage`或者`calldata`。唯一的例外是状态变量，自动假设，只能是`storage`。前一篇[文章](/coinmonks/solidity-storage-vs-memory-vs-calldata-8c7e8c38bce)探讨了其中的一些差异。我们将只在这里探索数组。

总的来说:

*   `memory` —生存期限于外部函数调用，可变，作用域在函数内(非持久，可修改)
*   `storage` —其生存期限于包含它的*契约*的生存期，是可变的，并且是存储所有状态变量的位置(持久的，可变的)
*   `calldata` —类似于`memory`，不可变，是一个包含函数参数的特殊数据位置(非持久、不可修改)

> 交易新手？试试[加密交易机器人](/coinmonks/crypto-trading-bot-c2ffce8acb2a)或者[复制交易](/coinmonks/top-10-crypto-copy-trading-platforms-for-beginners-d0c37c7d698c)

# 数据位置效应

数组(和其他引用类型)的数据位置对赋值行为有影响:

*   **赋值在** `**storage**` **和** `**memory**` **(或从** `**calldata**` **)之间总是创建一个独立的副本**

在下面的例子中，即使`x`是一个`storage`数组，并且`g`接受`memory`数组，也可以在内部调用`g(x)`(通过在外部调用`f()` ),因为`g(x)`调用在`memory`中传递了一个单独的、独立的`x`副本。

*   **从** `**memory**` **到** `**memory**` **的赋值只创建引用。这意味着对一个内存变量的更改在引用相同数据的所有其他内存变量中也是可见的**

在下面的例子中，因为`z`和`zz` `memory`数组引用相同的底层`memory`数组(`y`)，所以对一个`memory`变量的更改会影响引用相同数据的所有其他变量。

*   **赋值从** `**storage**` **到一个局部** `**storage**` **变量也只赋值一个引用**

在下面的例子中，局部`storage`变量`z`引用`x`，因此能够通过调用`f()`更新第`0`个索引中`x`的值。私有函数`h(y)`也可以更新`x`，因为再次传递的是对`x`的引用，而不是副本。

*注意，如果`h`既不是`private`也不是`internal`，那么这个例子甚至无法编译，因为在那种情况下`h`的函数参数将被要求要么是`memory`要么是`calldata`——局部`storage`变量只能作为对现有存储变量的引用传入函数，而不能用于公共函数。它们特别适用于 1)代替引用变量的操作，或 2)在库函数中访问存储数据。

*   **对** `**storage**` **的所有其他赋值总是复制——包括对状态变量的赋值，即使局部变量本身只是一个引用**

在下面的例子中，允许将状态变量`x`赋给`memory`或`calldata`数组，因为整个数组都被复制到了`storage`中。它们被显示为副本(而不是引用),因为更新`x[0]`对`y`或`z`都没有影响。

## 动态大小与固定大小阵列

数组可以具有编译时的固定大小，也可以具有动态大小。对于固定大小`k`(即 5 个元素)和元素类型`T`(即`uint256`)，固定大小数组用`T[k]`(即`uint256[5]`)表示，动态大小数组用`T[]`(即`uint256[]`)表示。

所有的`memory`数组都有固定的大小——然而，动态数组也可以依赖于运行时参数。

数组可以有任何类型的元素，包括映射或结构，尽管同样的类型限制适用于这些引用类型——映射只能存储在`storage`数据位置。

数组也可以是多维的。例如，`uint[][5] memory x`将创建一个由 5 个动态数组`uint`组成的`memory`数组。与 JS 中一样，索引是从零开始的，访问方向与声明的方向相反，这意味着要访问**第二个**动态数组中的**第三个** `uint`，需要使用`x[1][2]`。

与其他状态变量类型一样，将状态数组指定为`public`会自动在 Solidity 中创建一个 getter。但是，在 getter 中还必须包含一个数字索引作为必需的参数(否则，将不知道“获取”哪个元素，因为不会返回整个数组，以避免高开销):

*   因此，对于数组`uint[] public x`，第三个元素将通过调用`x(2)`来访问

请注意，试图访问超过其长度的数组会导致断言失败。

## 初始化数组

**动态存储器阵列**可以使用`new`操作符进行初始化。然而，尽管是动态的，`memory`数组不能**也不能**调整大小——需要的大小必须提前确定，或者完全复制到新的`memory`数组中进行更新。新分配的数组总是用该类型的默认值初始化(即`uint256`用`0`):

要更新这些值，必须单独分配元素:

**静态大小(固定大小)的内存数组**可以由数组文字初始化——一个或多个表达式的逗号分隔列表，形式为`[…]`。数组文字被解释为静态大小的内存数组，其长度等于表达式的数量。数组的基本类型是列表中第一个表达式的类型，以便其他表达式可以隐式转换为它。否则抛出一个类型错误:`Unable to deduce common type for array elements`。注意，列表**中的一个元素必须**显式地是该类型——只有一个所有元素都可以隐式转换的类型是不够的。

例如，下面是一个`uint8[3] memory`，因为`1`的类型是显式的`uint8`，列表中剩余的值(`2`、`3`)可以隐式转换为`uint8`。

`[1, 2, 3]`

相反，要创建一个`uint[3] memory`，其中一个元素必须显式转换为`uint`:

以下是不允许的，因为`bool`不能隐式转换为`uint`。

固定大小的内存阵列也不能分配给动态大小的内存阵列:

## 数组成员

数组以类似于 JavaScript 的方式拥有成员，除了一些例外:

*   `length`:数组长度，适用于所有数组类型

*   `push()`和`push(x)`:两者都仅适用于存储阵列

如果没有参数，它会在数组末尾追加一个初始化为零的元素，并返回对该元素的引用。

通过参数，它追加到数组的末尾，并且不返回任何内容。

*   `pop()`:也仅适用于存储阵列。它在移除的元素上隐式调用一个`delete`。与 JS 不同，它不返回那个元素。

## 数组切片

使用`x[start:end]`，数组的连接部分也可以作为片来访问。数组值将在`x[start]`到`x[end-1]`之间返回。索引必须是`uint256`类型或者可以隐式转换成它。索引访问不是相对于底层数组，而是相对于切片的开始(并且只对动态`calldata`数组可用)。

## 其他阵列详细信息

*   避免悬空引用:避免在数组中留下对已被删除或移动的存储项的引用，例如弹出元素。
*   bytes 和 string 作为数组:都是特殊数组(`bytes`类似于`bytes1[]`，但在`calldata`和`memory`中打包得很紧)；`string`等于`bytes`，但不允许`length`或索引访问。

希望这给了 Solidity 数组一些好的见解，以及与 JavaScript 的一些关键区别。感谢阅读！

## 参考资料:

*   [https://docs.soliditylang.org/en/v0.8.15/types.html#arrays](https://docs.soliditylang.org/en/v0.8.15/types.html#arrays)
*   [https://docs . soliditylang . org/en/v 0 . 8 . 15/contracts . html # getter-functions](https://docs.soliditylang.org/en/v0.8.15/contracts.html#getter-functions)