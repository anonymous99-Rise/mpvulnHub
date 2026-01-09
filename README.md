# 微信安全文章归档系统 v1.2

[![GitHub Actions](https://github.com/adminlove520/mpVulnHub/actions/workflows/update_today.yml/badge.svg)](https://github.com/adminlove520/mpVulnHub/actions)
[![License](https://img.shields.io/badge/license-MIT-blue.svg)](LICENSE)

> 🚀 **微信公众号安全文章知识库** - 自动抓取、分类、归档微信公众号安全文章，构建专业安全知识库。支持 **Feishu** 和 **Discord** 实时推送！

## ✨ 核心功能

### 🔔 多渠道实时推送 (New)
- **双模推送**：
    - **单篇推送 (Normal Mode)**：发现新文章立即通过富文本卡片/Embed推送，包含标题、分类、链接、来源。
    - **日报汇总 (Daily Report Mode)**：每日任务结束时推送汇总报告，统计当日数据并链接到 GitHub 报告页。
- **平台支持**：
    - **飞书 (Feishu)**：使用精美的**富文本卡片**，支持颜色随机主题和互动链接。
    - **Discord**：使用 **Embed** 格式，美观清晰。
- **启动通知**：系统启动时发送状态通知卡片。

### 🔍 智能内容识别
- **多维度关键词匹配**：覆盖威胁情报、漏洞利用、安全运营、应急响应、溯源分析等11个专业领域。
- **配置化管理**：所有关键词配置已迁移至 `config.yaml`，方便自定义调整。
- **智能去重机制**：避免重复文章，确保知识库质量。

### 📊 专业报告生成
- **威胁态势分析**：自动分析安全威胁分布和趋势。
- **详细匹配规则**：展示所有关键词分类和匹配逻辑。
- **完整文章列表**：按数据源分组展示所有匹配文章。

### 🗂️ 智能文件管理
- **分层目录结构**：`doc/年/年-月/年-W周/年-月-日/文章.md`
- **数据持久化**：通过`data.json`记录处理历史，支持断点续传。
- **Markdown转换**：自动将微信文章转换为标准Markdown格式。

## 📰 数据来源

| 数据源 | 描述 | 更新频率 |
|--------|------|----------|
| **ChainReactors** | GitHub安全文章聚合，专注于漏洞复现和技术分析 | 每日 |
| **BruceFeIix** | 安全文章收集，涵盖威胁情报和安全运营 | 每日 |
| **Doonsec** | 安全资讯RSS，实时推送安全事件和漏洞预警 | 实时 |

## ⚙️ 配置说明

### 配置文件 `config.yaml`
项目通过 `config.yaml` 进行统一管理：
- **notification**: 配置飞书/Discord 推送开关、WebHook 环境变量名、底部 Footer 文案。
- **threat_analysis_categories**: 自定义各类安全关键词（如漏洞利用、威胁情报等）。

### 环境变量
在 GitHub Secrets 或本地环境变量中配置：
- `FEISHU_WEBHOOK`: 飞书群组机器人的 WebHook 地址。
- `DISCORD_WEBHOOK`: Discord WebHook 地址。

## 🚀 使用方法

### 📦 安装依赖
```bash
pip install -r requirements.txt
```

### ▶️ 常用命令

```bash
# 运行今日抓取（自动推送通知）
python3 run.py

# 指定日期抓取
python3 run.py --date 2025-01-20

# 补录历史数据（强制覆盖 & 静默模式）
# 适用于补全 doc 目录缺失文件，--force 忽略去重，--no-notify 避免消息轰炸
python3 run.py --range 2024-01-01 2024-01-09 --force --no-notify
```

### 📊 命令行参数

| 参数 | 说明 | 示例 |
|------|------|------|
| `--date` | 指定日期抓取 | `--date 2025-01-20` |
| `--range` | 指定日期范围 | `--range 2025-01-01 2025-01-31` |
| `--history` | 抓取历史数据 | `--history` |
| `--force` | **[New]** 强制处理，忽略 `data.json` 重复检查 | `--force` |
| `--no-notify` | **[New]** 静默模式，不发送任何推送 | `--no-notify` |

### 🤖 自动化工作流

1.  **Update Today**: 每日/每4小时自动运行，获取最新文章并推送。
2.  **Backfill History**: 手动触发 (Workflow Dispatch)，用于批量补录历史数据。支持输入日期范围，并默认开启静默模式。

## 📁 文件结构
```
wxvuln/
├── run.py                 # 主程序
├── config.yaml            # [New] 配置文件
├── data.json              # 数据记录文件
├── doc/                   # 文章存储目录 (YYYY/MM/Week/Day)
├── md/                    # 每日报告目录
├── bin/                   # 工具目录 (wechatmp2markdown)
└── README.md
```

## 🤝 贡献指南
欢迎提交 Issue 或 PR！
- 如果需要添加新的关键词，请直接修改 `config.yaml`。
- 如果需要接入新的通知渠道，请在 `run.py` 中扩展 `send_notification` 相关逻辑。