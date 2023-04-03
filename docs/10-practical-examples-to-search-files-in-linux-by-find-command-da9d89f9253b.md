# 用 Find 命令在 Linux 中搜索文件的 10 个实例

> 原文：<https://medium.com/coinmonks/10-practical-examples-to-search-files-in-linux-by-find-command-da9d89f9253b?source=collection_archive---------2----------------------->

## 在 Linux 中查找文件的 GNU Find 命令示例

![](img/15659b32e0f8afd3011feda7950b4bf6.png)

Photo by [Gabriel Heinzer](https://unsplash.com/@6heinz3r?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

## 介绍

作为 Linux 用户或管理员，您可能经常会遇到在操作系统的不同目录中查找文件的需求。手动扫描整个目录结构以找到所需的文件并不容易，因为在一台 Linux 机器中可能有成百上千个目录。实现这一目的最常见和最有效的命令是 Linux find 命令。在这里，我将列出 10 个实际例子，说明如何在 Linux 中根据不同的标准查找文件。

注意:命令以**粗体**显示，而输出不是。

1.  在当前工作目录下找到所有名为 **execute.py** 的文件，

> **找到。-名称" execute.py"**

2.在整个根目录中查找所有大于 1GB 的文件，

> **find/-type f-size+1G** /proc/k core
> /root/big file . txt

3.在整个根目录中通过权限 777 查找文件，

> **find/-type f-perm 777** /root/ia empty . txt

4.查找扩展名为的多个文件。整个根目录中的 cpp。

> **find / -type f -name "*。CPP "** /root/testfile . CPP

5.查找用户“admin”拥有的根目录下的空文件，但忽略目录/proc 下的文件。为此我们使用- **空**选项。

> **find/-path/proc-prune-o-type f-user admin-empty
> /proc
> /var/spool/mail/admin
> /var/tmp/empty file . txt
> /tmp/admin file . txt**

6.在当前目录及其下的 1 个目录中查找空文件。不应在该目录之外更深入地搜索空文件。我们为此使用- **maxdepth** 选项，

> **find/root-max depth 2-type f-empty
> /root/test file . CPP
> /root/test/empty new . txt**

7.在用户“admin”拥有的/var/tmp 目录中查找超过 90 天未修改的文件。如果有的话，将错误输出重定向到 null，这样我们就看不到错误，

> **find/var/tmp/-type f-mtime+90-user admin 2>/dev/null** /var/tmp/admin new file . txt

8.与 7 相同，但另外删除找到的文件(始终小心使用 remove 命令。你不想删除任何需要的东西)，

> **find/var/tmp/-type f-mtime+90-user admin | xargs/bin/RM**

9.在目录/var 下找到用户“admin”拥有的所有文件目录，但不显示目录/var/tmp/test 中的任何内容。为此，我们使用选项- **修剪**。请记住，在 Linux 中，一切都是文件，包括目录。因此，如果找到了目录 test，不要进入它。

> **find/var-path/var/tmp/test-prune-o-user admin** /var/spool/mail/admin
> /var/tmp/adminfilenew . txt
> /var/tmp/test

10.使用-iname(忽略命名中的大小写)选项在根目录中找到文件“passwd”，并对其执行 grep 以显示以单词“admin”开头的行。

> **find/-iname " passwd "-exec grep-I '^admin' { } \；2>/dev/null** admin:x:1002:1003::/home/admin:/bin/bash

## **结论**

查找命令是一个非常有用的工具，用于搜索和查找文件以及对它们执行操作。我们只是触及了冰山一角。它仍然有绝大多数的选择。您还使用过“查找”命令的其他方式吗？请在评论中告诉我。

> 交易新手？尝试[加密交易机器人](/coinmonks/crypto-trading-bot-c2ffce8acb2a)或[复制交易](/coinmonks/top-10-crypto-copy-trading-platforms-for-beginners-d0c37c7d698c)