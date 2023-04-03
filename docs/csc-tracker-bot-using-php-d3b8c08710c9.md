# 使用 PHP 的 CSC 跟踪机器人

> 原文：<https://medium.com/coinmonks/csc-tracker-bot-using-php-d3b8c08710c9?source=collection_archive---------29----------------------->

当你使用社交媒体时，你有没有尝试过跟踪 CSC 网络。在本教程中，我们将使用 php 和 coinex.net API 服务编写一个 CSC 跟踪机器人

![](img/f1ca77a95c3273468213277dcf45bf7e.png)

## **电报**

Telegram 是一种流行的即时通讯服务，以其安全性而自豪。它拥有现代聊天平台的所有功能，包括**聊天机器人**:基于软件的代理，你可以通过编程读取和回复其他用户的消息。

## 设置你的电报机器人

创建电报机器人的第一步是建立机器人最终支持的配置文件。这也是获得电报 API 令牌的方法。

要设置新的机器人档案，请登录您的 Telegram 帐户，并与允许您创建和管理机器人的官方帐户**机器人父亲**(@机器人父亲)开始对话。在该对话中，输入`/newbot`命令。

机器人父亲会要求你为你的机器人选择一个显示名*和*用户名。用户名必须以“bot”结尾，并且必须是唯一的。在我们的例子中，我们已经确定了显示名称 *CSCTrcker_bot* 和用户名 *CSCTrackerBot*

一旦你登陆了一个有效的用户名，机器人父亲会自动注册你的机器人，并回复一个电报 API 的令牌，这个令牌是机器人独有的。确保不要与任何人共享您的令牌。

# 为你的机器人设置一个网络钩子

创建电报机器人的下一步是设置将与机器人通信的 webhook。Webhooks 是 API 告诉你发生了什么事情的方式，这样你就不必每隔几分钟(或几秒钟)就去查询 API，看看是否有新消息被发送。

Telegram 只使用一种 webhook，每当发生*任何*事件时，它都会向你发送一个`update`对象。

设置 webhook 非常简单。您只需要知道两件事:您的 API 令牌(您应该从第一步就有了)和您将托管 bot 的 URL。该网址将类似于`https://Example.com/Bot.php`。确保在 URL 的开头包含`https`;否则，电报不会发送 webhook。

现在，在常规的网络浏览器中，导航到`https://api.telegram.org/bot<yourtoken>/setwebhook?url=https://Example.com/Bot.php`。瞧，你的 webhook 现在已经启动并运行了！

# 让我们编码

是时候为我们的电报机器人写逻辑了。我会这样继续下去…

首先要做的是初始化一个变量，这将使我们能够轻松地调用电报 API。就这么简单`$path = "https://api.telegram.org/bot<yourtoken>`。

将你的 token api(从 bot father)粘贴到<yourtoken>。</yourtoken>

因为我们将通过 webhook 接收更新，所以让我们创建一个数组并填充更新数据:`$update = json_decode(file_get_contents("php://input"), TRUE)`

现在，为了以后方便起见，让我们从更新中提取两个关键的数据—聊天 ID 和消息(如果更新不是由新消息引起的，这个字段可能是空的，我们将在后面编写代码):

```
$chatId = $update["message"]["chat"]["id"];
$message = $update["message"]["text"];
```

如果你还没有猜到这个机器人应该做什么，我想让它告诉我我选择的位置的当前天气。为此，我将创建一个`/asset [Address]`命令。

为此，让我们创建一个`if`语句，看看消息是否以`/asset`开头。我们可以用`strpos()`函数来完成，它告诉我们子串在字符串中的位置:

```
if (strpos($message, "/asset") === 0) {
}
```

嵌套在该`if`语句中，让我们编写一些代码，通过截掉消息的前九个字符来提取位置(这是由`/asset` 命令使用的字符数，以及它后面的空格):

```
if (strpos($message, "/asset") === 0) {
$asset = substr($message, 9);
}
```

如果这个机器人被用于生产，我们必须添加一些输入清理，以确保位置采取正确的格式。但事实并非如此，所以我们不会为此担心。

现在我们将从 coinex.net 获得数据:

```
$curl = curl_init();
curl_setopt_array($curl, [
	CURLOPT_URL => "https://www.coinex.net/api/v1/transactions/".$message,
	CURLOPT_RETURNTRANSFER => true,
	CURLOPT_FOLLOWLOCATION => true,
	CURLOPT_ENCODING => "",
	CURLOPT_MAXREDIRS => 10,
	CURLOPT_TIMEOUT => 30,
	CURLOPT_HTTP_VERSION => CURL_HTTP_VERSION_1_1,
	CURLOPT_CUSTOMREQUEST => "GET",
	CURLOPT_HTTPHEADER => [
		"apikey: YOUR API KEY",
	],
]);

$response = curl_exec($curl);
$err = curl_error($curl);
curl_close($curl);
$data = json_decode($response, true);
```

这里我们应该实现某种错误处理，但是我不想麻烦。相反，让我们抱最好的希望，使用 Telegram API 发送我们的机器人的响应:

```
file_get_contents($path."/sendmessage?chat_id=".$chatId."&text="The list of assets:". $.data);
```

总而言之，下面是代码的样子:

```
<?php
$path = "https://api.telegram.org/bot<PasteYourTokenHere>";$update = json_decode(file_get_contents("php://input"), TRUE);$chatId = $update["message"]["chat"]["id"];
$message = $update["message"]["text"];$curl = curl_init();
curl_setopt_array($curl, [
	CURLOPT_URL => "https://www.coinex.net/api/v1/transactions/".$message,
	CURLOPT_RETURNTRANSFER => true,
	CURLOPT_FOLLOWLOCATION => true,
	CURLOPT_ENCODING => "",
	CURLOPT_MAXREDIRS => 10,
	CURLOPT_TIMEOUT => 30,
	CURLOPT_HTTP_VERSION => CURL_HTTP_VERSION_1_1,
	CURLOPT_CUSTOMREQUEST => "GET",
	CURLOPT_HTTPHEADER => [
		"apikey: YOUR API KEY",
	],
]);

$response = curl_exec($curl);
$err = curl_error($curl);
curl_close($curl);
$data = json_decode($response, true);if (strpos($message, "/asset") === 0) {
    $location = substr($message,6,42);
    $asset=$data;
    file_get_contents($path."/sendmessage?chat_id=".$chatId."&text="The list of assets:". $.data);
}?>
```

## 部署机器人

逻辑完成后，将代码保存为 PHP 文件。然后，上传文件到你可以上传你的机器人到一个共享主机，并设置网页挂钩。

> 交易新手？尝试[加密交易机器人](/coinmonks/crypto-trading-bot-c2ffce8acb2a)或[复制交易](/coinmonks/top-10-crypto-copy-trading-platforms-for-beginners-d0c37c7d698c)