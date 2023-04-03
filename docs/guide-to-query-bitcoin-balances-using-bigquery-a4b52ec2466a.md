# 使用 BigQuery 查询比特币余额指南

> 原文：<https://medium.com/coinmonks/guide-to-query-bitcoin-balances-using-bigquery-a4b52ec2466a?source=collection_archive---------15----------------------->

在本文中，我们将展示如何在 Google BigQuery 中查询比特币地址的余额。

特别是，我们将展示如何使用 **LEAD** 、 **UNNEST** 和 **GENERATE_DATE_ARRAY** 来填补区块链地址上没有交易的日期空白。

![](img/8407240e6a70a390bcc8ab4cd22747b6.png)

Extract on-chain Bitcoin data

# 第一步

我们将运用借记和贷记的会计概念，对区块链上的所有交易建立复式账簿。这遵循了[谷歌云网站](https://www.cloudskillsboost.google/focuses/8486?parent=catalog)上描述的想法。

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
```

# 第二步

条目按日期和地址分组。

“价值”的单位通过除以 10^8.转换成比特币

```
double_entry_book_grouped AS (
    SELECT date, address, SUM(value / POWER(10,8)) AS value
    FROM double_entry_book
    GROUP BY date, address
),
```

# 第三步

对于那些没有任何事务的日期，时间序列显示为完全缺失的记录。

*   **LEAD** —返回每个地址间隔后的日期

```
daily_balances_gappy AS (
    SELECT date, address,
    SUM(value) OVER (PARTITION BY address ORDER BY date) AS balance,
    LEAD(date, 1, CURRENT_DATE()) OVER (PARTITION BY address ORDER BY date) AS next_date
    FROM double_entry_book_grouped
),
```

# 第四步

为了创建合成行，我们将使用 **UNNEST** 将一个数组转换为一个表，其中数组中的每个元素都有一行，并使用 **GENERATE_DATE_ARRAY** 创建一个从‘2009–01–01’到当前日期的日期数组。

```
 all_dates AS (
    SELECT date FROM UNNEST(GENERATE_DATE_ARRAY('2009-01-01', CURRENT_DATE())) AS date
),
```

# 第五步

接下来，我们创建一个包含连续系列每日余额的表。合成行被**连接到原始时间序列上**以填补空白。

```
daily_balances_gapless AS (
    SELECT address, all_dates.date, balance
    FROM daily_balances_gappy
    JOIN all_dates ON daily_balances_gappy.date <= all_dates.date AND all_dates.date < daily_balances_gappy.next_date
  ),
```

# 第六步

计算所有地址的每日余额。

```
 all_daily_balances AS (
    SELECT DISTINCT date, SUM(balance) OVER(PARTITION BY date) AS balance
    FROM daily_balances_gapless
  )
```

# 第七步

以下查询获取“all_daily_balances”表中所有列的值。

```
 SELECT * FROM all_daily_balances
  ORDER BY date
```

# 结论

完整的查询如下所示。该查询使用 218.6 GB 在 BigQuery 中运行。

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
    JOIN all_dates ON daily_balances_gappy.date <= all_dates.date and all_dates.date < daily_balances_gappy.next_date
  ),

all_daily_balances AS (
    SELECT DISTINCT date, SUM(balance) OVER(PARTITION BY date) AS balance
    FROM daily_balances_gapless
)

SELECT * FROM all_daily_balances
ORDER BY date
```

在下一篇文章中，我们将展示如何使用这个查询来计算交易所的比特币余额。

***感谢阅读！***

如果您喜欢这篇文章，并想了解更多，请考虑关注我。我定期发布与链上分析、机器学习和 BigQuery 相关的主题。我尽量让我的文章简单而精确，尽可能提供代码、例子和模拟。