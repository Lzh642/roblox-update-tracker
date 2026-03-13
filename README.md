# 🎮 Roblox Update Tracker

An OpenClaw skill for tracking Roblox engine release notes with deep technical analysis, community feedback, and trend insights.

**OpenClaw 技能，用于追踪 Roblox 引擎更新日志，提供深度技术分析、社区反馈和趋势洞察。**

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![OpenClaw](https://img.shields.io/badge/OpenClaw-Skill-blue.svg)](https://openclaw.ai)

---

## 📖 语言 / Language

- [English](#english-documentation)
- [中文说明](#中文说明)

---

# English Documentation

## ✨ Features

- 🔍 **Automatic Version Detection** - Compares local tracking with remote latest version
- 📊 **Deep Programming Analysis** - Luau syntax updates, API changes with code examples
- 💬 **Community Feedback Analysis** - DevForum discussions, likes, and trending topics
- 📈 **Trend Prediction** - Historical comparison and future direction insights
- 🌐 **UGC Platform Insights** - Cross-platform analysis and design philosophy
- 🇨🇳 **Chinese Localization** - Accurate translation with original English terms preserved

## 📦 Installation

### Via OpenClaw Skills Manager

1. Open OpenClaw
2. Navigate to Skills Manager: `http://127.0.0.1:23001/skills`
3. Search for "Roblox Update Tracker"
4. Click "Install"

### Manual Installation

1. Download the latest release from [Releases](https://github.com/yourusername/roblox-update-tracker/releases)
2. Extract to `~/.openclaw/skills/roblox-update-tracker/`
3. Restart OpenClaw Gateway

```bash
openclaw gateway restart
```

## 🚀 Usage

### Quick Version Check

Ask OpenClaw:
- "Is there a new Roblox version?"
- "Roblox latest version"
- "Check Roblox updates"

**Result**: Compares your local tracking with the remote latest version and prompts for full analysis if updates are found.

### Full Deep Analysis

Ask OpenClaw:
- "Give me the latest Roblox updates"
- "Analyze Roblox version 712"

**Result**: Generates a comprehensive report including:
- 🎮 Version info + release date
- 🔗 DevForum post + official docs links
- ⭐ Update statistics (total + live count)
- 🚀 New features & improvements (Live/Pending)
- 💻 Programming deep dive (API signatures + code examples)
- 💬 Community feedback (positive/concerns/discussions)
- 📊 Trend analysis (historical comparison + predictions)
- 🌐 UGC platform insights (design philosophy + best practices)

## 📂 Output Files

Reports are saved to `~/.openclaw/workspace/roblox-updates/`:
- `release-notes-{VERSION}.md` - Full detailed report
- `LATEST.md` - Copy of the latest report
- `LATEST_VERSION.txt` - Local tracking version number

## 🛠️ Configuration (Optional)

Create `~/.openclaw/workspace/roblox-updates/config.json`:

```json
{
  "focus": "all",
  "always_load_api": false,
  "include_code_examples": true,
  "include_community_feedback": true,
  "api_detail_level": "high"
}
```

### Configuration Options

- `focus`: `"programming"` | `"all"` | `"ui"` | `"security"`
- `always_load_api`: Auto-load API declarations without prompts
- `include_code_examples`: Generate Luau code examples
- `include_community_feedback`: Fetch DevForum discussions
- `api_detail_level`: `"high"` | `"medium"` | `"low"`

## 📋 Requirements

- OpenClaw environment
- Network access to `create.roblox.com` and `devforum.roblox.com`

## 🧩 Technical Features

- **Smart API Detection**: Recognizes 866 Roblox classes + 556 enums
- **On-Demand Loading**: Loads API declarations only when programming updates are detected (saves tokens)
- **Multi-Source Data**: Official docs + DevForum community + historical version comparison
- **Chinese Localization**: Accurate technical term translation with English originals preserved

## 📖 Example Report

See [example report](https://github.com/yourusername/roblox-update-tracker/blob/main/examples/release-notes-712.md) for Roblox version 712.

## 🤝 Contributing

Contributions are welcome! Please see [CONTRIBUTING.md](CONTRIBUTING.md) for details.

## 📜 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

---

# 中文说明

## ✨ 功能特性

- 🔍 **自动版本检测** - 对比本地追踪版本与远程最新版本
- 📊 **深度编程分析** - Luau 语法更新、API 变更，附带代码示例
- 💬 **社区反馈分析** - DevForum 讨论、点赞和热议话题
- 📈 **趋势预测** - 历史版本对比和未来方向洞察
- 🌐 **UGC 平台洞察** - 跨平台对比分析和设计哲学
- 🇨🇳 **中文本地化** - 准确翻译技术术语，保留英文原文

## 📦 安装方法

### 通过 OpenClaw Skills 管理器

1. 打开 OpenClaw
2. 访问 Skills 管理器：`http://127.0.0.1:23001/skills`
3. 搜索 "Roblox Update Tracker"
4. 点击"安装"

### 手动安装

1. 从 [Releases](https://github.com/yourusername/roblox-update-tracker/releases) 下载最新版本
2. 解压到 `~/.openclaw/skills/roblox-update-tracker/`
3. 重启 OpenClaw Gateway

```bash
openclaw gateway restart
```

## 🚀 使用方法

### 快速版本检查

向 OpenClaw 询问：
- "roblox有新版本吗"
- "roblox最新版本"
- "检查roblox更新"

**结果**：对比本地追踪版本与远程最新版本，如有更新则询问是否需要完整分析。

### 完整深度分析

向 OpenClaw 询问：
- "给我roblox最新的更新"
- "分析roblox 712版本"

**结果**：生成综合报告，包含：
- 🎮 版本信息 + 发布时间
- 🔗 DevForum 帖子 + 官方文档链接
- ⭐ 更新统计（总数 + 已上线数量）
- 🚀 新功能与改进（Live/Pending 分类）
- 💻 编程深度分析（API 签名 + 代码示例）
- 💬 社区反馈分析（积极反馈/担忧/技术讨论）
- 📊 趋势分析（历史对比 + 未来预测）
- 🌐 UGC 平台洞察（设计哲学 + 最佳实践）

## 📂 输出文件

报告保存到 `~/.openclaw/workspace/roblox-updates/`：
- `release-notes-{VERSION}.md` - 完整详细报告
- `LATEST.md` - 最新报告副本
- `LATEST_VERSION.txt` - 本地追踪版本号

## 🛠️ 配置（可选）

创建 `~/.openclaw/workspace/roblox-updates/config.json`：

```json
{
  "focus": "all",
  "always_load_api": false,
  "include_code_examples": true,
  "include_community_feedback": true,
  "api_detail_level": "high"
}
```

### 配置选项

- `focus`: 分析重点 - `"programming"` | `"all"` | `"ui"` | `"security"`
- `always_load_api`: 自动加载 API 声明，无需提示
- `include_code_examples`: 生成 Luau 代码示例
- `include_community_feedback`: 抓取 DevForum 讨论
- `api_detail_level`: API 分析深度 - `"high"` | `"medium"` | `"low"`

## 📋 系统要求

- OpenClaw 环境
- 网络连接（需访问 `create.roblox.com` 和 `devforum.roblox.com`）

## 🧩 技术特性

- **智能 API 检测**：识别 866 个 Roblox 类 + 556 个枚举
- **按需加载**：仅在检测到编程更新时加载 API 声明（节省 token）
- **多源数据**：官方文档 + DevForum 社区 + 历史版本对比
- **中文本地化**：准确翻译技术术语，保留英文原文

## 📖 示例报告

查看 [Roblox 712 版本示例报告](https://github.com/yourusername/roblox-update-tracker/blob/main/examples/release-notes-712.md)。

## 🤝 贡献指南

欢迎贡献！请查看 [CONTRIBUTING.md](CONTRIBUTING.md) 了解详情。

## 📜 开源协议

本项目采用 MIT 协议 - 详见 [LICENSE](LICENSE) 文件。

---

## 🙏 鸣谢 / Acknowledgments

- [OpenClaw](https://openclaw.ai) - AI 助手平台
- [Roblox](https://roblox.com) - 游戏平台
- [DevForum](https://devforum.roblox.com) - 社区讨论

## 📞 支持 / Support

如遇到问题或有疑问：
- 提交 [Issue](https://github.com/yourusername/roblox-update-tracker/issues)
- 访问 [OpenClaw 文档](https://docs.openclaw.ai)

---

Made with ❤️ for the Roblox developer community  
为 Roblox 开发者社区用心打造 ❤️
