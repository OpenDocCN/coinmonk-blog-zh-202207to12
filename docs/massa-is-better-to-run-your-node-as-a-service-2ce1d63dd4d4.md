# Massa —更好地将您的节点作为服务来运行

> 原文：<https://medium.com/coinmonks/massa-is-better-to-run-your-node-as-a-service-2ce1d63dd4d4?source=collection_archive---------1----------------------->

在遵循官方文档 [Massa 节点安装](https://docs.massa.net/en/latest/testnet/running.html)的同时，您将从命令行进入节点执行，如下所示

```
./massa-node -p <PASSWORD> |& tee logs.txt
```

或者

```
RUST_BACKTRACE=full cargo run --release -- -p <PASSWORD> |& tee logs.txt
```

它会一直工作到你不关闭你的终端或者你想使用*屏幕。*我的建议很简单——创建 Massa 节点服务(下面是在 Ubuntu 上)。为此，请执行以下命令:

```
sudo tee /etc/systemd/system/massad.service > /dev/null << EOF
[Unit]
Description=Massa Node
After=network-online.target
[Service]
User=root
WorkingDirectory=/root/massa/massa-node
ExecStart=/root/.cargo/bin/cargo run --release -- -p <YOURPASSWORD>
Restart=on-failure
RestartSec=3
LimitNOFILE=65535
[Install]
WantedBy=multi-user.target
EOF
```

然后使用命令来启用和启动该服务(如果您喜欢使用 *sudo* ):

```
systemctl daemon-reload
systemctl enable massad
systemctl start massad
```

之后，您可能需要检查日志:

```
journalctl -u massad -f
```

正如您可能看到的，这非常简单，现在您的节点将在服务器启动后启动，如果在 3 秒内崩溃，将重新启动。请记住，即使没有创建服务，您的节点通常也应该首先正常工作。

祝运行您的节点愉快！

ps:加入 crew3 Massa quests runs 和我们一起找乐子—[https://massalabs.crew3.xyz/invite/rAcng8NGQxGJoXl1gwqpi](https://massalabs.crew3.xyz/invite/rAcng8NGQxGJoXl1gwqpi)

> *交易新手？试试* [*加密交易机器人*](/coinmonks/crypto-trading-bot-c2ffce8acb2a) *或* [*复制交易*](/coinmonks/top-10-crypto-copy-trading-platforms-for-beginners-d0c37c7d698c) *上* [*最好的加密交易*](/coinmonks/crypto-exchange-dd2f9d6f3769)

> 加入 Coinmonks [电报频道](https://t.me/coincodecap)和 [Youtube 频道](https://www.youtube.com/c/coinmonks/videos)获取每日[加密新闻](http://coincodecap.com/)

# 另外，阅读

*   [免费加密信号](/coinmonks/free-crypto-signals-48b25e61a8da) | [加密交易机器人](/coinmonks/crypto-trading-bot-c2ffce8acb2a)
*   [杠杆代币的终极指南](/coinmonks/leveraged-token-3f5257808b22)
*   [16 款最佳折叠电动自行车](/coinmonks/top-17-folding-electric-bikes-5e296f0918cb)
*   [28 款最佳电动自行车点评](/coinmonks/the-28-best-electric-bikes-review-and-buying-guide-in-2023-7bb3146cb403)
*   前三名[币安期货交易机器人](/coinmonks/top-3-binance-futures-trading-bots-e6031f84b3f9)