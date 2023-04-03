# CSC 101 —枚举、结构和常数

> 原文：<https://medium.com/coinmonks/csc-101-enums-structs-and-constants-8e82b51fe8b0?source=collection_archive---------26----------------------->

**实度中的枚举**代表**可枚举**。它们是用户定义的类型，包含一组常量的可读名称，称为**成员。**

简单地说，结构是一种定义新的自定义类型的方法，它包含其他类型的成员。这是一个用 Solidity 编写的简单结构的例子。

常量是不可修改的变量。

![](img/78db353f85d00a7149d8bc2b6bc3c9de.png)

## 枚举

枚举是创建用户定义的数据类型的方式，它通常用于为整型常量提供名称，这使得契约更易于维护和阅读。枚举用几个预定义值中的一个来限制变量，枚举列表中的这些值被称为*枚举*。

**格式:**

```
enum <enumerator_name> { 
            element 1, element 2,....,element n
}
```

这里有几个其他可能的例子。

```
*enum Direction { Left, Right, Straight, Back }
enum Color { Red, Green, Blue}
enum Volt{ ON, OFF }
enum Size { Large , Medium , Small }*
```

**注意**:当我们需要指定智能合约的当前状态/流程时，枚举特别有用。例如，如果需要打开智能合约以准备好接受用户的存款。

可以按如下方式访问枚举值:

```
contract MyContract{
    enum Size{ Large,Medium,Small}
    Size public size=Size.Large;
    constructor() public {

    }
    function getSize() public view returns (Size) {
    return size;
}
```

## 结构体

Solidity 中的 Structs 允许你创建更复杂的具有多种属性的数据类型。您可以通过创建一个**结构**来定义自己的类型。

结构可以在协定之外声明，并在另一个协定中导入。通常，它用于表示一个记录。为了定义一个结构*，使用了 struct* 关键字，它创建了一个新的数据类型。

**格式**:

```
struct <structure_name> {  
   <data type> variable_1;  
   <data type> variable_2; 
}
```

**例子:**

```
struct Employee {string name;uint Age;uint id;}
```

## 声明结构

可以按如下方式声明结构:

`Employee person;`

为了给结构赋值，我们可以这样做:

```
Employee person= Employee("John Doe","43 ",1);
```

## 常数

常量是不可修改的变量。它们的值是硬编码的，使用常量可以节省汽油成本。

```
address public constant MY_ADDRESS = 0x000000.....;
uint public contant unit = 12.2;
string public constant Name = "John Doe";
```

注意:当你想从自己的钱包里取出合同中的硬币或代币时，常量是很有用的。得到捐赠，税收和更多。

> 交易新手？尝试[加密交易机器人](/coinmonks/crypto-trading-bot-c2ffce8acb2a)或[复制交易](/coinmonks/top-10-crypto-copy-trading-platforms-for-beginners-d0c37c7d698c)