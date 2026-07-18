# grok-build-wiki

> [xai-org/grok-build](https://github.com/xai-org/grok-build) 源码的中文深度解读 Wiki —— 37 页文档、114 张架构图，把 xAI 的终端 AI 编程助手从里到外讲个明白。

[![在线阅读](https://img.shields.io/badge/在线阅读-GitHub%20Pages-blue)](https://linearuncle.github.io/grok-build-wiki/)
[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)

**📖 在线阅读（推荐，带侧边栏和全文搜索）：https://linearuncle.github.io/grok-build-wiki/**

## 这是什么

Grok Build（命令行里敲 `grok`）是 xAI 出品的终端 AI 编程助手：把 AI 聊天、代码编辑、命令执行揉进一个全屏 TUI，由 100+ 个 Rust crate 组成。

本仓库**不是源码**，而是 AI 通读整个源码仓库后自动生成的**中文解读 Wiki**。全部用大白话写，配 Mermaid 架构图，目标是让不同背景的读者都能看懂这套系统的设计：

- **使用者**：搞懂它能干什么、怎么上手
- **贡献者**：搞懂代码结构，知道改哪
- **研究者**：搞懂一个生产级 AI Agent 的架构决策

> ⚠️ Wiki 内容为 AI 生成，图中文字与代码均来自仓库真实内容，但仍建议对照源码核对关键结论。

## 内容地图

### 总览

| # | 页面 | 讲什么 |
|---|------|--------|
| 1 | [Grok Build 是什么](01-what-is-grok-build.md) | 定位：AI 聊天 + 改代码 + 跑命令三合一的 TUI，跟 ChatGPT、Copilot 的区别 |
| 2 | [5 分钟上手](02-quickstart.md) | 从安装到敲出第一个对话 |
| 3 | [代码仓库导览](03-repo-tour.md) | 100+ 个 Rust crate 的分布规律 |

### 核心架构

| # | 页面 | 讲什么 |
|---|------|--------|
| 4 | [整体架构：三层协作](04-architecture-overview.md) | TUI 交互层 → Agent 运行时 → 工具执行层 |
| 5 | [一次完整对话的旅程](05-one-full-turn.md) | 从你敲下问题到 AI 回复出现在屏幕上的全程数据流 |
| 6 | [会话管理：从出生到归档](06-session-lifecycle.md) | 会话的创建、分支、压缩、回退、关闭 |
| 7 | [工作区通信协议](07-workspace-types-protocol.md) | 客户端与服务端之间的 RPC 类型字典 |
| 8 | [上下文窗口管理](08-chat-state-context.md) | token 的精打细算：何时压缩、怎么塞最有用 |

### TUI 界面子系统

| # | 页面 | 讲什么 |
|---|------|--------|
| 9 | [终端渲染流水线](09-tui-rendering.md) | Markdown 回复怎么变成屏幕上有颜色的方块 |
| 10 | [滚动回溯引擎](10-scrollback-system.md) | 对话历史的「块」模型、搜索与文本选择 |
| 11 | [斜杠命令系统](11-slash-command-system.md) | 50+ 个 `/` 命令的注册、匹配与执行 |
| 12 | [Mermaid 图表渲染](12-mermaid-rendering.md) | 从 Mermaid 文本到 SVG 的全流程 |
| 13 | [语音输入](13-voice-input.md) | 按住说话、松手转文字：音频采集 → 流式 STT |
| 14 | [Minimal 模式](14-minimal-mode.md) | 不画 TUI 的纯文本精简模式 |

### Agent 运行时子系统

| # | 页面 | 讲什么 |
|---|------|--------|
| 15 | [Agent 调度核心](15-agent-runtime.md) | Run Loop：取任务、喂 LLM、分发工具调用 |
| 16 | [Goal 系统](16-goal-orchestration.md) | 模糊大任务怎么自动规划、分步执行、防止跑偏 |
| 17 | [对话压缩](17-compaction.md) | 窗口快爆时怎么总结老历史还不失忆 |
| 18 | [采样器与重试策略](18-sampler-and-retry.md) | SSE 流、限流、超时、熔断器 |

### 工具执行子系统

| # | 页面 | 讲什么 |
|---|------|--------|
| 19 | [工具箱：AI 的手和眼睛](19-tool-system.md) | 统一 Tool trait 与注册中心 |
| 20 | [终端执行与权限控制](20-terminal-tools.md) | AI 想跑 `rm -rf /` 时系统怎么拦 |
| 21 | [工作区与文件系统](21-filesystem-workspace.md) | 文件操作的统一抽象：本地磁盘、Git/JJ 感知、远程挂载 |
| 22 | [代码关系图引擎](22-codebase-graph.md) | tree-sitter 把仓库解析成 ScopeGraph |
| 23 | [LSP 集成](23-lsp-integration.md) | 给 AI 装上 IDE 的大脑：诊断、格式化、符号查询 |
| 24 | [代码溯源](24-hunk-attribution.md) | 这行代码是人写的还是 AI 写的 |

### 扩展与集成

| # | 页面 | 讲什么 |
|---|------|--------|
| 25 | [MCP 协议](25-mcp-integration.md) | 外部工具服务怎么被发现、启动、接入 Agent |
| 26 | [插件与钩子系统](26-plugins-and-hooks.md) | 插件像应用，钩子像自动化脚本 |
| 27 | [Agent Client Protocol](27-acp-protocol.md) | IDE / Web 前端怎么通过 JSON-RPC 连上 Agent |

### 横切关注点

| # | 页面 | 讲什么 |
|---|------|--------|
| 28 | [配置体系](28-config-system.md) | 用户配置、企业策略、MDM 托管配置三层合并 |
| 29 | [遥测与可观测性](29-telemetry.md) | 产品事件、性能追踪、错误上报三套体系 |
| 30 | [沙箱隔离](30-sandbox-security.md) | 把 AI 执行环境关进笼子 |
| 31 | [记忆系统](31-memory-system.md) | SQLite + 向量搜索的长期记忆，还有「Dream」整理机制 |
| 32 | [Leader 选举](32-leader-election.md) | 多实例操作同一目录时谁是唯一写入者 |

### 构建与发布

| # | 页面 | 讲什么 |
|---|------|--------|
| 33 | [自动更新子系统](33-update-autoupdate.md) | 静默检查、下载、校验、安装、重启 |
| 34 | [npm 二进制分发](34-npm-distribution.md) | optionalDependencies 按平台自动下载二进制 |
| 35 | [测试策略与基础设施](35-testing-strategy.md) | PTY 端到端测试、YAML 场景测试、trace replay |

### 参考

| # | 页面 | 讲什么 |
|---|------|--------|
| 36 | [用户命令与功能索引](36-command-reference.md) | 全部斜杠命令、快捷键、配置项速查 |
| 37 | [术语速查](37-glossary.md) | ACP、MCP、ScopeGraph、hunk tracker 这些行话 |

## 怎么读

- **想用它解决问题**：1 → 2 → 3 → 35
- **想给它改代码**：4 → 5 → 各模块页
- **想研究设计思想**：4 → 6 → 7 → 8

## 本地预览

站点用 [docsify](https://docsify.js.org/) 构建，纯静态、无需编译。任意静态服务器都能跑：

```bash
# 方式一：Python（系统自带）
cd grok-build-wiki
python3 -m http.server 3000

# 方式二：docsify-cli
npx docsify-cli serve .
```

然后打开 http://localhost:3000 。

## 仓库结构

```
├── index.html      # docsify 站点入口（含 Mermaid 渲染、全文搜索配置）
├── index.md        # Wiki 首页（完整目录 + 阅读路线）
├── _sidebar.md     # 侧边栏导航
├── .nojekyll       # 告诉 GitHub Pages 不要走 Jekyll 处理
├── 01-…-37-*.md    # 37 个章节，按阅读顺序编号
└── LICENSE         # MIT 许可证
```

## 免责声明

- 本 Wiki 为个人学习项目，与 xAI 官方无关，内容不代表官方文档。
- 全部内容由 AI 阅读源码后生成，可能存在理解偏差；关键结论请以 [xai-org/grok-build](https://github.com/xai-org/grok-build) 源码为准。

## License

[MIT](LICENSE) © linearuncle
