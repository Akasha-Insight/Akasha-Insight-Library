# Dune SQL 基础入门

> Dune 是目前最流行的链上数据查询平台，用 SQL 查询 Ethereum、Solana 等多链数据

## 1. Dune 数据架构

```
链上原始区块数据 → Dune 解码/索引 → 标准化的数据表
                                                ↓
                             用户通过 SQL 查询 → 可视化看板
```

### 核心表结构

```sql
-- Ethereum 主网交易表
ethereum.transactions     -- 所有交易记录
ethereum.blocks           -- 区块信息
ethereum.logs             -- 事件日志（ERC20 Transfer 等）
ethereum.traces           -- 内部调用（更细粒度）

-- 合约专属表（Dune 自动解码）
-- 格式：项目名_协议名_版本.EvmTableName
-- 例如：
uniswap_v3_ethereum.Pair_evt_Swap   -- Uniswap V3 交易事件
aave_v2_ethereum.LendingPool_evt_Deposit  -- Aave 存款事件

-- 价格数据
prices.usd               -- 代币 USD 价格（按分钟）
```

## 2. 第一个查询：最近 24h 大额交易

```sql
SELECT
  block_time,
  `from` AS sender,
  `to` AS receiver,
  value / 1e18 AS eth_amount,
  tx_hash
FROM ethereum.transactions
WHERE
  block_time > NOW() - INTERVAL '24' HOUR
  AND value > 100 * 1e18  -- 大于 100 ETH
ORDER BY value DESC
```

## 3. 常用查询模板

### 📊 查询代币前 10 持有者

```sql
WITH transfers AS (
  -- ERC20 Transfer 事件
  SELECT
    `to` AS holder,
    SUM(value) AS received
  FROM erc20_ethereum.evt_Transfer
  WHERE contract_address = 0x...  -- 替换为代币合约地址
  GROUP BY `to`
)
SELECT
  holder,
  received / 1e18 AS balance
FROM transfers
ORDER BY balance DESC
LIMIT 10
```

### 💰 追踪某个地址的活动

```sql
SELECT
  block_time,
  CASE WHEN `from` = 0x... THEN '流出' ELSE '流入' END AS direction,
  value / 1e18 AS amount,
  tx_hash
FROM ethereum.transactions
WHERE
  `from` = 0x...  -- 目标地址
  OR `to` = 0x...
ORDER BY block_time DESC
LIMIT 50
```

### 🔥 Uniswap 最新交易

```sql
SELECT
  block_time,
  amount0 / 1e18 AS token0_amount,
  amount1 / 1e18 AS token1_amount,
  amountUSD AS usd_value
FROM uniswap_v3_ethereum.Pair_evt_Swap
WHERE contract_address = 0x...  -- 交易对合约地址
ORDER BY block_time DESC
LIMIT 20
```

## 4. Dune 实用技巧

| 技巧 | 说明 |
|------|------|
| **CTE (WITH 子句)** | 把复杂查询分解成多步，易读易调试 |
| **分区裁剪** | 加 `block_time` 过滤条件加速查询 |
| **JOIN 价格表** | 用 `prices.usd` 把代币数量转成 USD 估值 |
| **公共看板 Fork** | 看到好的看板直接 Fork，在此基础上改 |

## 5. 推荐 Dune 看板

> （搜索以下关键词可以找到经典看板）

| 看板 | 作者 | 说明 |
|------|------|------|
| Dune 官方教程看板 | @dune | 新手必看 |
| ETH 巨鲸追踪 | @hildobby | 大额资金监控 |
| DeFi 协议总览 | @rchen | TVL/Volume 全览 |
| NFT 市场分析 | @sealaunch | NFT 交易数据 |

## 💡 学习建议

1. 先 Fork 一个漂亮看板 → 看它的 SQL → 理解逻辑
2. 改一个参数（比如时间范围）→ 看效果
3. 尝试自己写一个简单的查询 → 从 `SELECT * FROM ethereum.transactions LIMIT 10` 开始
4. 加入 [Dune 官方 Discord](https://discord.gg/dune) → 在 `#queries` 频道学习大佬写法

> 📚 **下一步：** 多写多练，加入 [Dune 官方 Discord](https://discord.gg/dune) 学习大佬的查询技巧

---

**⬅️ [返回链上分析](./../README.md)**
