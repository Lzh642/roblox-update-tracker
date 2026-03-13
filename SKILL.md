---
name: roblox-update-tracker
description: >
  This skill tracks and analyzes Roblox engine release notes updates.
  It should be used when the user wants to fetch, summarize, or analyze Roblox release notes,
  community feedback from DevForum, or understand Roblox's update trends.
  Trigger keywords: "roblox更新", "roblox release notes", "roblox版本", "roblox追踪",
  "roblox社区反馈", "roblox更新方向", "UGC平台分析", "Luau更新", "Roblox安全",
  "roblox最新版本", "roblox有新版本吗", "roblox检查更新".
  IMPORTANT: Only trigger when "roblox" keyword is present in the user's query - do not trigger on generic version/update questions.
---

# Roblox Update Tracker Skill

## Purpose

Track Roblox engine release notes, summarize update content with accurate Chinese localization,
provide deep analysis of Lua/Luau programming changes with code examples,
analyze community feedback from DevForum, and deliver insights on Roblox's development direction
and UGC platform implications.

## When to Use

- When the user asks about latest Roblox updates or release notes
- When the user wants to track Roblox version changes
- When the user needs community feedback analysis from DevForum
- When the user asks about Roblox's update direction or UGC platform insights
- Trigger keywords: "roblox更新", "roblox release notes", "版本追踪", "Luau更新", "roblox最新"

## Reference Files (Same Directory)

This skill uses the following reference files in the same directory:

- **`api-keywords/`** — API 声明文件目录（按需加载）:
  - `luau-language.md` — Luau 语言关键词与特性（~700 tokens）
  - `api-classes.md` — 866 个 Roblox API 类名（~6k tokens）
  - `api-enums.md` — 556 个 Roblox 枚举名（~3.5k tokens）
- **`report-template.md`** — Markdown template for generating analysis reports (embedded in Step 6 below, read only if needed)
- **`version-history.md`** — Historical version records and trend analysis (read only recent sections: lines 1-200 for recent versions)
- **`config.example.json`** — Configuration example (optional, copy to workspace as `roblox-updates/config.json` to customize)

## Optional Configuration

Users can create `roblox-updates/config.json` to customize analysis behavior:

```json
{
  "focus": "programming",           // "programming" | "all" | "ui" | "security"
  "always_load_api": false,         // true: auto-load API declarations without asking
  "include_code_examples": true,    // Include Luau code examples in reports
  "include_community_feedback": true, // Fetch and analyze DevForum feedback
  "api_detail_level": "high"        // "high" | "medium" | "low"
}
```

**Config fields**:
- `focus`: Determines which sections to emphasize in reports
  - `"programming"`: Focus on Lua/Luau, API, code examples
  - `"all"`: Comprehensive analysis (default if config doesn't exist)
  - `"ui"`: Focus on UI/UX changes
  - `"security"`: Focus on security and safety updates
- `always_load_api`: Skip confirmation prompt when API declarations are needed
- `include_code_examples`: Controls whether to generate code examples (programming focus only)
- `include_community_feedback`: Controls whether to fetch DevForum feedback (can be disabled to save time)
- `api_detail_level`: Controls API analysis depth
  - `"high"`: Load full declarations, provide detailed signatures
  - `"medium"`: Load high-frequency keywords only
  - `"low"`: Skip API loading entirely

If `config.json` doesn't exist, defaults to `focus: "all"` with user prompts for API loading.

## Data Sources & Fetching Strategy

### ⚠️ Known Pitfalls

The zh-cn official docs page (`create.roblox.com/docs/zh-cn/...`) is a SPA (Single Page Application)
that dynamically renders content via JavaScript. Direct fetching often times out or returns only
the page shell without content.

### Recommended Fetching Order (三路并进)

1. **英文官方文档（优先，成功率高）**
   - URL: `https://create.roblox.com/docs/release-notes/release-notes-{VERSION}`
   - This returns JSON data that renders correctly

2. **DevForum 帖子列表（确定最新版本号）**
   - URL: `https://devforum.roblox.com/c/updates/release-notes`
   - Find the latest version number from topic titles

3. **DevForum 帖子详情（社区反馈）**
   - URL: `https://devforum.roblox.com/t/release-notes-for-{VERSION}/{TOPIC_ID}`
   - Fetch multiple pages: append `?page=2`, `?page=3` etc.
   - JSON endpoint: append `.json` for structured data

4. **Fallback: Web Search**
   - Search: `roblox "release notes for {VERSION}" site:devforum.roblox.com`

### Version Number Discovery

To find the latest version:
1. Fetch `https://devforum.roblox.com/c/updates/release-notes` — the top post title contains the latest version
2. Known latest as of 2026-03-12: **711**
3. Try incrementing: 712, 713... to check for newer versions

## Workflow

**Choose the appropriate path based on user intent:**

### Fast Path: Quick Version Check

**Use ONLY when the user asks about Roblox versions specifically** (must contain "roblox" keyword + version-related words):
- ✅ "roblox有新版本吗"
- ✅ "roblox最新版本是多少"
- ✅ "检查roblox更新"
- ❌ "有新版本吗" (too generic, don't trigger)
- ❌ "最新版本" (missing context, don't trigger)

**Workflow:**

1. **Check local tracking file**: Read `roblox-updates/LATEST_VERSION.txt`
   - If file exists → `local_version` = content (e.g., "711")
   - If file doesn't exist → `local_version` = null (user hasn't analyzed any version yet)

2. **Fetch remote latest version**: Fetch `https://devforum.roblox.com/c/updates/release-notes`
   - Parse the top post title to extract version number (e.g., "Release Notes for 712" → "712")
   - **Retry logic**: If fetch fails, retry up to 2 times with 3-second delays
   - **On failure**: Tell user "⚠️ 无法连接 DevForum，请稍后重试或检查网络" and STOP

3. **Compare and reply**:
   - **If `local_version` is null**:
     - "🆕 Roblox 当前最新版本是 {remote_version}，你还没有追踪过任何版本。是否需要分析？"
   - **If `local_version` == `remote_version`**:
     - "✅ Roblox 当前最新版本是 {remote_version}，你已追踪到最新版本。"
   - **If `local_version` < `remote_version`**:
     - "🆕 发现 Roblox 新版本 {remote_version}（你上次追踪的是 {local_version}），是否需要完整分析？"

4. **STOP** (do not proceed to full analysis unless user confirms)

**Important notes:**
- `LATEST_VERSION.txt` is the **user's local tracking record** (not the skill's default)
- `version-history.md` is a **reference file shipped with the skill** (for trend analysis only, NOT for determining what the user has analyzed)
- When a new user installs this skill, they start with NO `LATEST_VERSION.txt` (first-time setup)

### Full Path: Complete Analysis

**Use when the user asks for detailed analysis** (keywords: "分析", "详细", "更新了什么", "release notes"):

#### Step 0: Check for New Version

**CRITICAL: Always check for new versions first!**

1. **Check local tracking file**: Read `roblox-updates/LATEST_VERSION.txt`
   - If file exists → `local_version` = content (e.g., "711")
   - If file doesn't exist → `local_version` = null (first-time analysis)

2. **Fetch remote latest version**: Fetch `https://devforum.roblox.com/c/updates/release-notes`
   - Parse the top post title to extract version number
   - **Retry logic**: If fetch fails, retry up to 2 times with 3-second delays
   - **On failure**: Tell user "⚠️ 无法连接 DevForum，请稍后重试" and STOP

3. **Compare versions**:
   - **If `local_version` == `remote_version`**: 
     - Reply "✅ Roblox 无最新版本需要分析（当前最新: {remote_version}）" and STOP
   - **If `local_version` < `remote_version` OR `local_version` is null**: 
     - Continue to Step 1 (fetch and analyze new version)

4. **After successful report generation**: Update `LATEST_VERSION.txt` with `remote_version`

**Important terminology:**
- `LATEST_VERSION.txt` = **User's local tracking record** (what they have analyzed)
- `version-history.md` = **Skill reference file** (historical trends, NOT user's tracking state)
- DevForum listing = **Remote source of truth** (Roblox's actual latest version)

#### Step 1: Fetch Release Notes

1. Determine the latest version number from DevForum listing
2. Fetch the English official release notes page (see fetching strategy above)
   - **Retry logic**: If fetch fails, retry up to 2 times
   - **On failure**: Try fallback web search; if still fails, tell user "⚠️ 无法获取 release notes，请稍后重试" and STOP
3. Extract ALL items:
   - Every improvement with **Live** or **Pending** status
   - Every fix with **Live** or **Pending** status
   - Version number and date (from DevForum post timestamp if not in docs)

#### Step 2: Deep Analysis of Programming Updates (Smart Detection + On-Demand Loading)

**Goal**: Identify Lua/Luau programming-related updates and load API declarations only when needed.

**Workflow**:

0. **Read user config** (if exists): `roblox-updates/config.json`
   - Extract `focus`, `always_load_api`, `api_detail_level` settings
   - If file doesn't exist, use defaults: `focus="all"`, `always_load_api=false`

1. **Pre-scan release notes** (no file loading yet):
   - Extract potential API keywords using regex:
     - Pattern 1: `[A-Z][a-zA-Z0-9]+` (PascalCase names like `UIShadow`, `EditableImage`)
     - Pattern 2: `\b(const|type|local|function|return|Type Solver|Type Inference|Union Types|Intersection Types|Generic Types)\b` (Luau keywords)
   - Count matches: `keyword_count`

2. **Classify update type**:
   - **Programming-heavy update**: `keyword_count` ≥ 5
   - **General update**: `keyword_count` < 5

3. **Conditional loading**:

   **Case A: Programming-heavy update (≥5 keywords)**
   
   - **If `always_load_api` is true** → Skip prompt, proceed to load
   - **If `always_load_api` is false** → Ask user:
     ```
     🔍 检测到 {keyword_count} 个编程相关关键词（{list first 3 keywords}...），
     这可能是一个编程重度更新。是否加载完整 API 声明进行深度分析？
     
     [1] 是 - 加载完整声明（~10k tokens）
     [2] 否 - 仅使用提取的关键词生成概览
     
     提示：可在 roblox-updates/config.json 设置 "always_load_api": true 跳过此提示
     ```
   
   - **If user confirms OR `always_load_api=true`**:
     - Determine which files to load based on keyword types:
       - If keywords contain Luau语言特性 (const, type, Type Solver...) → Load `api-keywords/luau-language.md`
       - If keywords contain API类名 (UIShadow, EditableImage...) → Load `api-keywords/api-classes.md`
       - If keywords contain 枚举名 (ActionType, AdShape...) → Load `api-keywords/api-enums.md`
     - Cross-reference extracted keywords with loaded declarations
     - Generate detailed analysis with full signatures and code examples
   
   - **If user declines**:
     - Use extracted keywords only (no type info)
     - Generate simplified programming summary
     - Note: "完整 API 声明未加载，如需详细分析请重新运行并选择加载"

   **Case B: General update (<5 keywords)**
   
   - Skip API file loading
   - Use extracted keywords (if any) for lightweight analysis
   - Focus on high-level feature summary
   - Note: "本次更新编程相关内容较少，如需强制深度分析请使用命令：'详细分析 Lua 更新'"

4. **Extract programming details** (if API declarations loaded):
   - New API classes, methods, properties — provide full signatures
   - Luau language changes (keywords, type solver, syntax)
   - Security permission changes and their impact
   - Performance optimizations with quantification where possible
   - Breaking changes that require code migration

5. **Provide code examples** (if `include_code_examples` is true and API declarations loaded):
   - For every new API/feature, provide **working Luau code examples**
   - Show before/after for breaking changes
   - Include usage patterns and best practices

**User override commands**:
- "详细分析 Lua 更新" → Force programming mode, load all API declarations
- "只看概览" → Force general mode, skip API loading

#### Step 3: Fetch Community Feedback (Smart Pagination)

1. Find the DevForum thread (see fetching strategy)
   - **Retry logic**: If fetch fails, retry once
   - **On failure**: Note "⚠️ 无法获取社区反馈" in report, continue to Step 4
2. **Fetch ONLY the first page initially**
3. **Scan first page for high-value signals**:
   - Official Roblox staff responses (look for Roblox badge/flair)
   - High-engagement comments (>10 likes or replies)
   - Technical deep-dive discussions
4. **Decide whether to fetch more pages**:
   - **If high-value signals found**: Fetch pages 2-3
   - **If only low-engagement posts**: STOP at page 1
5. Categorize reactions:
   - 😊 Positive feedback (excitement, praise, appreciation)
   - 😟 Negative feedback / concerns (complaints, worries, criticism)
   - 💬 Technical discussions (deep dives, questions, clarifications)
   - 🔮 Feature requests and expectations
   - ⭐ Official Roblox staff responses (highlight these!)

#### Step 4: Trend Analysis (Limited History Read)

**Read only recent history** to avoid loading entire history file:

1. Read `~/.openclaw/skills/roblox-update-tracker/version-history.md` **lines 1-200 only** (covers ~10 most recent versions)
2. **Identify Themes**: Compare current updates with recent historical trends
3. **Strategic Directions**: Map updates to known directions (UI, Luau, Security, Performance, API)
4. **Predict Future**: Based on Pending items and community signals, predict upcoming changes
5. **Update History**: After generating report, append new version entry to `version-history.md`

Known trends (from `version-history.md`):
- UI System Modernization (catching up with CSS)
- Luau Engineering Evolution (scripting → engineering language)
- Security Defense in Depth (account → device → biometric)
- Performance Fine-tuning
- API Governance Normalization

#### Step 5: UGC Platform Insights

Analyze implications for other UGC platforms:
- Language design lessons (what worked/didn't work in Luau changes)
- UI system architecture (CSS parity strategy effectiveness)
- Security model design (defense-in-depth implementation)
- Creator-platform relationship management (governance model challenges)
- Community governance (RFC process lessons, transparency issues)

#### Step 6: Generate Report & Update Version History

**CRITICAL: This step has THREE mandatory actions!**

1. **Use embedded report template below** (no need to read `report-template.md` unless custom formatting needed)
2. **Fill in all sections** with analyzed data
3. **Save reports**:
   - `roblox-updates/release-notes-{VERSION}.md` — full detailed report
   - `roblox-updates/LATEST.md` — copy of the latest report (for quick access)
   - `roblox-updates/LATEST_VERSION.txt` — update with new version number (just the number, e.g., "711")
4. **⚠️ UPDATE `version-history.md`** (DO NOT SKIP!):
   - Add new version entry in the "已追踪版本" table with:
     - Version number and date
     - Key updates summary (concise, 2-3 items max)
     - Community sentiment (1 sentence)
     - DevForum link
   - If tracking multiple versions, update "近期重点方向" section to reflect new trends
   - Update "社区情绪趋势" section with new version's sentiment
5. **Create directory**: If `roblox-updates/` doesn't exist, create it first

**Embedded Report Template:**

```markdown
🎮 Roblox 版本 {VERSION} 更新说明
📅 发布时间：{DATE}
🔗 社区讨论：[DevForum 帖子](https://devforum.roblox.com/t/release-notes-for-{VERSION}/{TOPIC_ID}) | [官方文档](https://create.roblox.com/docs/release-notes/release-notes-{VERSION})

⭐ 主要改进（{TOTAL_COUNT}项更新，{LIVE_COUNT}项已上线）

---

## 🚀 新功能与改进

### Live (已上线)

[List all Live improvements here with Chinese translation + original English in parentheses]

### Pending (待上线)

[List all Pending improvements here]

---

## 🐛 问题修复

### Live (已修复)

[List all Live fixes here]

### Pending (待修复)

[List all Pending fixes here]

---

## 💻 编程相关深度分析

[Only include this section if API/Luau changes were detected]

### 新增 API

[List new API classes/methods/properties with Luau code examples]

### Luau 语言变化

[List Luau syntax/type system changes with before/after examples]

### 性能优化

[List performance improvements with quantification]

### ⚠️ Breaking Changes

[List breaking changes that require code migration]

---

## 💬 社区反馈分析

### 😊 积极反馈

[List positive community reactions]

### 😟 担忧与批评

[List concerns and criticisms]

### 💬 技术讨论焦点

[List key technical discussions]

### ⭐ 官方回应

[Highlight Roblox staff responses - these reveal official stance]

---

## 📊 趋势分析

### 本次更新方向

[Map updates to strategic directions: UI/Luau/Security/Performance/API]

### 与历史版本对比

[Compare with recent versions from version-history.md]

### 未来预测

[Based on Pending items and community signals, predict upcoming changes]

---

## 🌐 UGC 平台洞察

[Analyze implications for other UGC platforms]

- **语言设计启示**: ...
- **UI 系统架构**: ...
- **安全模型设计**: ...
- **创作者关系管理**: ...
- **社区治理**: ...

---

## 📚 相关资源

- [官方文档](https://create.roblox.com/docs/release-notes/release-notes-{VERSION})
- [DevForum 讨论](https://devforum.roblox.com/t/...)
- [API 参考](https://create.roblox.com/docs/reference/engine)
```

## Important Notes

- **Preserve Technical Terms**: Translate to Chinese but keep API names, class names, method names, enum names in English
- **Code Examples Required**: Provide Luau code examples for all new APIs — the user needs actionable code
- **Track Both Live and Pending**: Pending items preview future updates, include them
- **Highlight Official Responses**: Roblox staff responses from DevForum reveal official stance — call them out
- **Breaking Changes**: Always note breaking changes that require existing code migration
- **Use Reference Files**: Always read `api-keywords.md` before analysis; use `report-template.md` for output structure; cross-reference `version-history.md` for trends

## File Structure After Execution

```
workspace/
├── roblox-updates/
│   ├── release-notes-711.md
│   ├── release-notes-712.md
│   ├── LATEST.md
│   └── LATEST_VERSION.txt
└── [skill files in same dir]
    ├── SKILL.md
    ├── api-keywords.md
    ├── report-template.md
    └── version-history.md
```
