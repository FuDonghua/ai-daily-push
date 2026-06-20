# 🤖 AI 每日热点飞书推送

> 全自动、零服务器成本的 AI 新闻日报 —— LangGraph Agent 每天早上 8:25 把全球 AI 热点整理成中文摘要，自动推送到飞书群。

## ✨ 特性

- 🤖 **Agent 驱动**：基于 LangGraph ReAct Agent，自主决定搜什么、读哪些源、怎么编排，不是写死的流水线
- 🔍 **双数据源**：DuckDuckGo 实时搜索（主）+ 5 家科技媒体 RSS（备用兜底）
- ⏰ **全自动**：GitHub Actions 定时触发，电脑关机也照跑
- 💰 **零成本**：纯 GitHub Actions + 免费模型额度，无需任何服务器
- 🇨🇳 **中文化**：英文源自动提炼成中文摘要，飞书卡片格式推送

## 🏗️ 工作流程

```
GitHub Actions（每天 08:25 北京时间）
        ↓
LangGraph ReAct Agent（自主决策）
   ├── 🔍 DuckDuckGo 搜索（主动找热点）
   └── 📰 RSS 抓取（Verge/TechCrunch/MIT/Ars/VentureBeat）
        ↓
GLM-4-flash 生成中文摘要
        ↓
飞书 Webhook 推送（HMAC-SHA256 签名校验）
```

## 🛠️ 技术栈

| 层 | 技术 |
|---|---|
| Agent 框架 | LangChain + LangGraph（`create_react_agent`） |
| LLM | 智谱 GLM-4-flash（OpenAI 兼容接口） |
| 数据采集 | DuckDuckGo Search + feedparser（RSS） |
| 推送 | 飞书自定义机器人 Webhook（HMAC-SHA256 签名） |
| 调度 | GitHub Actions（cron 定时） |
| 语言 | Python 3.11 |

## 📰 数据源

| 类型 | 来源 |
|---|---|
| 实时搜索 | DuckDuckGo |
| RSS 订阅 | The Verge AI · TechCrunch AI · MIT Tech Review · Ars Technica · VentureBeat AI |

## ⚙️ 配置（GitHub Secrets）

| Secret | 说明 |
|---|---|
| `QWEN_API_KEY` | 智谱 GLM 的 API Key |
| `FEISHU_WEBHOOK` | 飞书机器人 Webhook 地址 |
| `FEISHU_SECRET` | 飞书签名校验密钥 |

## 🚀 本地运行

```bash
pip install -r requirements.txt

# 设置环境变量
export QWEN_API_KEY=你的智谱key
export FEISHU_WEBHOOK=你的飞书webhook
export FEISHU_SECRET=你的飞书签名密钥

python main.py
```

## ⏰ 定时规则

- cron 表达式：`25 0 * * *`（UTC 00:25 = 北京时间 08:25）
- 支持在 GitHub Actions 页面手动 `Run workflow` 立即触发一次

## 📁 项目结构

```
├── main.py                      # Agent 主体：工具定义 + ReAct 编排 + 飞书推送
├── requirements.txt             # Python 依赖
└── .github/workflows/
    └── daily-push.yml           # GitHub Actions 定时任务
```
