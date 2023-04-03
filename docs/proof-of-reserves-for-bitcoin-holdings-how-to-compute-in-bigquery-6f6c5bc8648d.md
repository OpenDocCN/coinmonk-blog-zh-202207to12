# 比特币持有量的储备证明——如何在 BigQuery 中计算

> 原文：<https://medium.com/coinmonks/proof-of-reserves-for-bitcoin-holdings-how-to-compute-in-bigquery-6f6c5bc8648d?source=collection_archive---------24----------------------->

FTX 的垮台震惊了整个密码界。如果 crypto 坚持其核心原则——不要相信、核实并总是自行保管你的资产，这场灾难本来是可以避免的。

随着 FTX 危机的蔓延，集中交易所正在寻求重建投资者信心。主要交易所已经分享了它们的热钱包和冷钱包地址的细节，作为它们对透明度和促进生态系统信任的承诺的一部分。

在本文中，我们将使用 Google BigQuery 通过交易所披露的钱包地址来查询比特币余额。您可以参考此[列表](https://trigolabs.io/btc-wallet-addresses/)获取 BTC 钱包地址。

![](img/25e8540103441b69fed2027c15317285.png)

Proof of reserves on cryptocurrency exchanges

为了便于您参考，币安的 BTC 钱包地址如下所示:

*   [34xp 4 vrocgjym 3 xr 7 ycvpfhocnxv 4 twseo](https://explorer.btc.com/btc/search/34xp4vRoCGJym3xR7yCVPFHoCNxv4Twseo)
*   [3 lyjfcfpxyjremsask 2 jkn 69 lweykzexb](https://explorer.btc.com/btc/search/3LYJfcfHPXYJreMsASk2jkn69LWEYKzexb)
*   [3m 219 kr 5 venen b 47 ewrpfwyb 5 jq 2 djxrp 6](https://explorer.btc.com/btc/search/3M219KR5vEneNb47ewrPfWyb5jQ2DjxRP6)
*   [BC 1 QM 34 LSC 65 zpw 79 lxes 69 zkqmk 6 ee 3 ewf 0j 77s 3h](https://explorer.btc.com/btc/address/bc1qm34lsc65zpw79lxes69zkqmk6ee3ewf0j77s3h)

如果您不想运行查询，您可以在[比特币仪表盘](https://trigolabs.io/bitcoin/)上查看 BTC 余额。确保选择“选择类别”作为交换，并选择“交换”作为币安余额。

计算币安交易所 BTC 余额的完整查询如下所示。代码细节在[使用 BigQuery](/@vivbellavita/guide-to-query-wallet-balances-of-bitcoin-in-bigquery-a4b52ec2466a) 查询比特币余额指南中讨论。

**其中**用于“double_entry_book_grouped”中，以尽早对数据进行分区，从而减少计算资源并提高性能。

*   **其中** **包含 _substr(`address `，` string`)** —用于搜索 BTC 钱包地址

```
WITH double_entry_book AS (
    -- debits
    SELECT array_to_string(inputs.addresses, ",") AS address, -inputs.value AS value, 
    DATE(block_timestamp) AS date
    FROM `bigquery-public-data.crypto_bitcoin.inputs` AS inputs
    UNION ALL
    -- credits
    SELECT array_to_string(outputs.addresses, ",") AS address, outputs.value AS value, 
    DATE(block_timestamp) AS date
    FROM `bigquery-public-data.crypto_bitcoin.outputs` AS outputs
), 

double_entry_book_grouped AS (
    SELECT date, address, SUM(value / POWER(10,8)) AS value
    FROM double_entry_book

    -- Binance
    WHERE contains_substr(`address`, '34xp4vRoCGJym3xR7yCVPFHoCNxv4Twseo') OR
    -- Binance 2
    contains_substr(`address`, '3LYJfcfHPXYJreMsASk2jkn69LWEYKzexb') OR
    -- Binance 3
    contains_substr(`address`, '3M219KR5vEneNb47ewrPfWyb5jQ2DjxRP6') OR
    -- Binance 4
    contains_substr(`address`, 'bc1qm34lsc65zpw79lxes69zkqmk6ee3ewf0j77s3h')

    GROUP BY date, address
),

daily_balances_gappy AS (
    SELECT date, address,
    SUM(value) OVER (PARTITION BY address ORDER BY date) AS balance,
    LEAD(date, 1, CURRENT_DATE()) OVER (PARTITION BY address ORDER BY date) AS next_date
    FROM double_entry_book_grouped
),

all_dates AS (
    SELECT date FROM UNNEST(GENERATE_DATE_ARRAY('2009-01-01', CURRENT_DATE())) AS date
),

daily_balances_gapless AS (
    SELECT address, all_dates.date, balance
    FROM daily_balances_gappy
    JOIN all_dates ON daily_balances_gappy.date <= all_dates.date AND all_dates.date < daily_balances_gappy.next_date
),

all_daily_balances AS (
    SELECT DISTINCT date, SUM(balance) OVER(PARTITION BY date) AS balance
    FROM daily_balances_gapless
)

SELECT * FROM all_daily_balances
ORDER BY date
```

# 结论

您可以通过将**中的字符串替换为其各自的钱包地址来尝试将此代码用于其他交换，其中包含 _substr(`address `，` string`)** 。此处提供了各大交易所的 BTC 钱包地址[。](https://trigolabs.io/btc-wallet-addresses/)

注意安全。记住“不是你的钥匙，不是你的硬币”。

另请参阅:

*   [使用 BigQuery 访问比特币块数据的简单方法](/dev-genius/an-easy-way-to-access-bitcoin-block-data-using-bigquery-2f5d9be6ae63)
*   在 BigQuery 中计算平均、中间和总比特币交易费用
*   比特币:使用 BigQuery 监控矿工收入

***感谢阅读！***

如果您喜欢这篇文章并想了解更多，请考虑关注我。我定期发布与链上分析、机器学习和 BigQuery 相关的主题。我尽量让我的文章简单而精确，尽可能提供代码、例子和模拟。