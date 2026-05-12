# 🤝 欢迎贡献！(Contributing Guide)

> **这是属于大家的资源库**——每个人都能贡献一份力量 💪

无论你是 Web3 大佬还是刚入门的新人，只要有好资源、好内容，都欢迎加进来！**你不需要会 Git**，用 GitHub 网页也能提交。

---

## 📂 先看看——你的资源该放哪？

| 你的资源类型 | 放这里 |
|-------------|--------|
| Web3 基础教程、概念解释 | [`00-Start-Here/`](./00-Start-Here/) |
| 区块链原理、术语科普 | [`00-Start-Here/web3-basics/`](./00-Start-Here/web3-basics/) |
| 钱包安全、防骗指南 | [`00-Start-Here/wallet-security/`](./00-Start-Here/wallet-security/) |
| Dune SQL 查询、看板教程 | [`01-OnChain-Analysis/dune-analytics/`](./01-OnChain-Analysis/dune-analytics/) |
| Nansen / Arkham 分析技巧 | [`01-OnChain-Analysis/nansen-arkham/`](./01-OnChain-Analysis/nansen-arkham/) |
| 链上分析工具教程 | [`01-OnChain-Analysis/tools/`](./01-OnChain-Analysis/tools/) |
| 项目研报/深度分析 | [`02-Investment-Research/`](./02-Investment-Research/) |
| DeFi / GameFi / Infra 赛道 | [`02-Investment-Research/defi/`](./02-Investment-Research/defi/) |
| 代币经济学分析 | [`02-Investment-Research/tokenomics/`](./02-Investment-Research/tokenomics/) |
| L1/L2 基础设施分析 | [`02-Investment-Research/infrastructure/`](./02-Investment-Research/infrastructure/) |
| 好用工具推荐 | [`03-Resources-Collection/essential-tools.md`](./03-Resources-Collection/essential-tools.md) |
| 必读白皮书/书单 | [`03-Resources-Collection/must-read-whitepapers.md`](./03-Resources-Collection/must-read-whitepapers.md) |
| 优质社区/Discord/Twitter | [`03-Resources-Collection/communities.md`](./03-Resources-Collection/communities.md) |

> 💡 **不确定放哪？** 直接新建一个 Issue 告诉我们，或者先随便放，审核时会帮你调整！

---

## 🛠 贡献方式（由易到难）

### 😎 方式一：在网页上直接编辑（不需要装 Git！）

> 适合：修改错别字、补充内容、更新链接

1. 打开你想修改的文件 👉 点右上角 ✏️ 编辑按钮
2. 改完后在页面底部写清楚改了啥
3. 选择「Create a new branch」→ 提交 Pull Request
4. ✅ 完事！

### 🚀 方式二：上传新文件（网页操作）

> 适合：分享你的教程、研报、工具清单

1. 进入对应目录（比如 `01-OnChain-Analysis/dune-analytics/`）
2. 点右上角 **Add file** → **Create new file** 或 **Upload files**
3. 按下面的模板写内容
4. 提交 PR

### 💻 方式三：标准 Git 工作流

> 适合：批量上传、经常贡献的老手

```bash
# 1. Fork 本仓库
# 2. Clone 你的 Fork
git clone https://github.com/你的用户名/Akasha-Insight-Library.git
cd Akasha-Insight-Library

# 3. 创建新分支
git checkout -b add/my-awesome-resource

# 4. 添加文件或修改
# 5. 提交
git add .
git commit -m "Add: 我分享的资源名称"

# 6. 推送并提交 PR
git push origin add/my-awesome-resource
# 然后在 GitHub 上创建 Pull Request
```

---

## 📝 内容模板（复制用）

### 分享一篇教程/文章

```markdown
# 标题

> 一句话说明这篇教程教什么

## 背景
为什么这个主题重要？

## 正文
你的内容...

## 参考链接
- [链接1](url)
- [链接2](url)

---

> 贡献者：@你的名字
```

### 推荐一个工具

直接在 [`essential-tools.md`](./03-Resources-Collection/essential-tools.md) 里按表格格式加一行即可。

---

## 🚫 安全红线

| 禁止事项 | 说明 |
|---------|------|
| ❌ 带返佣链接 | 禁止植入 Referral 链接，除非特别标注 |
| ❌ 钓鱼链接 | 提交链接前请用 DefiLlama / CoinGecko 验证 |
| ❌ 恶意脚本 | 代码中不得包含私钥窃取逻辑 |
| ❌ 未授权翻译 | 翻译他人作品需注明原作者授权 |

---

## ⏳ 提交流程

```
提交 PR → 48h 内审核 → 合并 / 反馈修改建议 → 合并后社区公告 @你
```

| 状态 | 说明 |
|------|------|
| 🟢 已合并 | 你的内容已加入知识库，感谢贡献！ |
| 🟡 需修改 | 审查员会在 PR 里留言，改完即可合并 |
| 🔴 被关闭 | 可能内容重复/不符合方向/违反安全红线 |

---

## 🌟 贡献者荣誉

每次 PR 被合并，你会被记录在贡献者名单中！累计贡献：
- 🥉 3次 → 社区贡献者徽章
- 🥈 10次 → 核心贡献者身份
- 🥇 20次 → 维护者邀请

---

**感谢你让这个知识库变得更好！一起打造最棒的 Web3 资源库 🚀**
