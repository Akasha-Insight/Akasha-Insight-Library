# Dune SQL 基础入门

> Dune 是目前最流行的链上数据查询平台，支持 Ethereum、Solana、Polygon 等多链数据查询。

---

## 1. Dune 数据架构

```
链上原始区块数据 → Dune 解码/索引 → 标准化的数据表
                                                ↓
                             用户通过 SQL 查询 → 可视化看板
```

### 核心表结构

```sql
-- Ethereum 主网常用表
ethereum.transactions     -- 所有交易记录
ethereum.blocks           -- 区块信息
ethereum.logs             -- 事件日志（如 ERC20 Transfer）
ethereum.traces           -- 内部调用（更细粒度的调用追踪）

-- 合约专属表（Dune 自动解码，无需手动解析）
-- 格式：项目名_协议名_版本.EvmTableName
uniswap_v3_ethereum.Pair_evt_Swap    -- Uniswap V3 交易事件
aave_v2_ethereum.LendingPool_evt_Deposit  -- Aave 存款事件

-- 价格数据表（高频更新）
prices.usd                -- 代币 USD 价格（每分钟）
```

---

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
  AND value > 100 * 1e18   -- 大于 100 ETH
ORDER BY value DESC
```

> 🔍 这个查询会返回过去 24 小时内所有超过 100 ETH 的大额转账，按金额从高到低排列。

---

## 3. 实用查询模板

### 📊 查询任意代币的前 10 持有者

```sql
WITH transfers AS (
  SELECT
    `to` AS holder,
    SUM(value) AS received
  FROM erc20_ethereum.evt_Transfer
  WHERE contract_address = 0x...  -- 替换为目标代币合约地址
  GROUP BY `to`
)
SELECT
  holder,
  received / 1e18 AS balance
FROM transfers
ORDER BY balance DESC
LIMIT 10
```

### 💰 追踪特定地址的所有交易

```sql
SELECT
  block_time,
  CASE
    WHEN `from` = 0x... THEN '📤 流出'
    ELSE '📥 流入'
  END AS direction,
  value / 1e18 AS amount,
  tx_hash
FROM ethereum.transactions
WHERE `from` = 0x... OR `to` = 0x...  -- 目标地址
ORDER BY block_time DESC
LIMIT 50
```

### 🔥 查看 Uniswap V3 最新交易

```sql
SELECT
  block_time,
  amount0 / 1e18 AS token0_amount,
  amount1 / 1e18 AS token1_amount,
  amountUSD AS usd_value
FROM uniswap_v3_ethereum.Pair_evt_Swap
WHERE contract_address = 0x...  -- 交易对合约
ORDER BY block_time DESC
LIMIT 20
```

---

## 4. Dune 实用技巧

| 技巧 | 说明 |
|------|------|
| **CTE (WITH 子句)** | 把复杂查询拆解成多步，易读易调试 |
| **分区裁剪** | 始终加 `block_time` 过滤——大幅加速，省 credits |
| **JOIN 价格表** | 用 `prices.usd` 把代币数量转成 USD 估值 |
| **Fork 看板** | 看到好的公开看板直接 Fork，在此基础上改 |

---

## 5. 推荐的公开看板

| 看板 | 作者 | 说明 |
|------|------|------|
| Dune 官方教程 | @dune | 新手必看，涵盖基础 SQL 到高级查询 |
| ETH 巨鲸追踪 | @hildobby | 大额资金实时监控 |
| DeFi 总览 | @rchen | TVL / Volume / Fees 一屏看全 |
| NFT 市场分析 | @sealaunch | 蓝筹 NFT 交易数据和趋势 |

> 在 Dune 搜索栏搜索以上作者名即可找到。

---

## 学习建议

1. **先 Fork 一个漂亮看板** → 看它的 SQL → 理解逻辑
2. **改一个参数**（比如时间范围）→ 看效果
3. **自己写一个简单查询** → 从 `SELECT * FROM ethereum.transactions LIMIT 10` 开始
4. **加入 [Dune Discord](https://discord.gg/dune)** → 在 `#queries` 频道学大佬写法

---

> 🔙 [返回链上分析](./../README.md)
