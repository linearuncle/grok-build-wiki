# grok-build 代码 Wiki

> SpaceXAI 出品、跑在终端里的 AI 编程助手，能理解你的代码库，帮你改文件、搜代码、执行命令。

**📅 生成时间：2026/7/17 18:42:55** · 共 37 页 / 114 张图 · 基于 [github.com/xai-org/grok-build](https://github.com/xai-org/grok-build)

## 🧭 怎么读这份 Wiki

- **使用者**（想用它解决问题）：[Grok Build 是什么](01-what-is-grok-build.md) → [5 分钟上手](02-quickstart.md) → [代码仓库导览](03-repo-tour.md) → [测试策略与基础设施](35-testing-strategy.md)
- **贡献者**（想给它改代码）：[整体架构：TUI → Agent → Workspace 三层协作](04-architecture-overview.md) → [一次完整对话的旅程](05-one-full-turn.md) → 各模块页
- **研究者**（想理解设计思想）：[整体架构：TUI → Agent → Workspace 三层协作](04-architecture-overview.md) → [会话管理：从出生到归档](06-session-lifecycle.md) → [工作区通信协议：RPC 类型字典](07-workspace-types-protocol.md) → [上下文窗口管理：token 的精打细算](08-chat-state-context.md)

## 📖 目录

### 总览

| # | 页面 | 类型 | 这一页讲什么 |
|---|------|------|--------------|
| 1 | [Grok Build 是什么](01-what-is-grok-build.md) | 总览 | 用大白话讲清楚 Grok Build 的定位：一个把 AI 聊天、代码编辑和终端命令揉在一起的 TUI 工具。读者看完能搞懂它跟 ChatGPT、Copilot 的区别。 |
| 2 | [5 分钟上手](02-quickstart.md) | 上手 | 从安装命令行到敲出第一个对话的实操流程，包括认证弹窗、选择工作目录和最基本的斜杠命令。 |
| 3 | [代码仓库导览](03-repo-tour.md) | 总览 | 一张图看清 100+ 个 Rust crate 的分布规律——哪些是界面、哪些是业务逻辑、哪些是公共库，帮读者建立方位感。 |

### 核心架构

| # | 页面 | 类型 | 这一页讲什么 |
|---|------|------|--------------|
| 4 | [整体架构：TUI → Agent → Workspace 三层协作](04-architecture-overview.md) | 架构 | 把这套系统拆成 TUI 交互层、Agent 运行时、工具执行层三层，外加贯穿的配置与遥测体系。讲清楚每一层的职责和层与层之间的交互协议。 |
| 5 | [一次完整对话的旅程](05-one-full-turn.md) | 核心流程 | 从你在终端敲下一个问题到屏幕上出现 AI 回复，全程追踪数据在各模块之间怎么流转、谁在什么时候调用谁。 |
| 6 | [会话管理：从出生到归档](06-session-lifecycle.md) | 概念故事 | 把会话想象成一个有生命周期的小生命——创建、分支、压缩、回退、关闭。讲清楚中间各种状态的转换条件和触发动作。 |
| 7 | [工作区通信协议：RPC 类型字典](07-workspace-types-protocol.md) | 概念故事 | 客户端和服务端之间传什么形状的数据——请求信封、响应块、事件推送，所有网络消息的类型定义都在 xai-grok-workspace-types 里，是整个分布式架构的类型基石。 |
| 8 | [上下文窗口管理：token 的精打细算](08-chat-state-context.md) | 概念故事 | ChatStateActor 是对话状态的守护神——估算每条消息的 token 数、决定什么时候触发压缩、怎么在有限窗口里塞进最多有用信息。Compaction 系统就靠它给信号。 |

### TUI 界面子系统

| # | 页面 | 类型 | 这一页讲什么 |
|---|------|------|--------------|
| 9 | [终端渲染流水线](09-tui-rendering.md) | 模块 | 从收到 AI 回复的一堆 Markdown 到屏幕上一个一个有颜色、有滚动条的方块，这中间经过了哪些加工站。 |
| 10 | [滚动回溯引擎](10-scrollback-system.md) | 模块 | 对话历史的「块」模型怎么设计、每个块类型怎么渲染、搜索和文本选择是怎么在终端上实现的。 |
| 11 | [斜杠命令系统](11-slash-command-system.md) | 模块 | 50+ 个 / 开头命令怎么注册、怎么被匹配到、执行后怎么反馈给用户，以及如何写一个自定义命令。 |
| 12 | [Mermaid 图表渲染](12-mermaid-rendering.md) | 模块 | 从 Mermaid 文本到 SVG 的全流程：解析成 AST、Dagre 分层布局、最终画成 SVG 标签，支持纯 Rust 引擎和 mmdc 子进程两种后端。 |
| 13 | [语音输入：按住说话、松手转文字](13-voice-input.md) | 模块 | 跨平台音频采集（macOS/Windows 用 cpal，Linux 用 arecord）→ WebSocket 流式发给 xAI STT 服务 → 实时返回识别结果。带心跳保活和自动重连。 |
| 14 | [Minimal 模式：不画 TUI 也能聊](14-minimal-mode.md) | 模块 | 给不喜欢全屏 TUI 的用户准备的精简模式——没有滚动回溯、没有面板，就是纯文本输入输出。用 xai-grok-pager-minimal 库实现，和完整 TUI 共享同一套底层逻辑。 |

### Agent 运行时子系统

| # | 页面 | 类型 | 这一页讲什么 |
|---|------|------|--------------|
| 15 | [Agent 调度核心](15-agent-runtime.md) | 模块 | ACP Session 内部那个永不停歇的 Run Loop：怎么从 prompt 队列取任务、怎么喂给 LLM、LLM 返回的工具调用怎么分发到 Extension。 |
| 16 | [Goal 系统：把大任务拆成小步骤](16-goal-orchestration.md) | 模块 | AI 接到一个模糊的大任务后，怎么自动规划、分步执行、检查是否跑偏——分类器、策略器、计划器、总结器都在这了。 |
| 17 | [对话压缩：给 LLM 的上下文瘦身](17-compaction.md) | 概念故事 | 聊到第 50 轮时 token 窗口快爆了怎么办？自动把老历史总结成摘要再塞回去，同时保证 AI 不会失忆。 |
| 18 | [采样器与重试策略](18-sampler-and-retry.md) | 模块 | 向 LLM API 发起请求时，Actor 模型怎么管理连接、解析 SSE 流、处理限流和超时，以及熔断器在什么情况下打开。 |

### 工具执行子系统

| # | 页面 | 类型 | 这一页讲什么 |
|---|------|------|--------------|
| 19 | [工具箱：AI 的手和眼睛](19-tool-system.md) | 模块 | 所有 AI 能调用的能力——读文件、搜代码、跑 bash、抓网页——都通过统一的 Tool trait 接入，注册中心负责发现和路由。 |
| 20 | [终端执行与权限控制](20-terminal-tools.md) | 模块 | AI 说想跑 rm -rf / 的时候，系统怎么拦住它——bash 工具从发起到执行的完整链路，包括权限策略的决策树。 |
| 21 | [工作区与文件系统](21-filesystem-workspace.md) | 模块 | Workspace 是所有文件操作的统一入口——本地磁盘、Git/JJ 感知、代码索引，甚至是远程 ACP 挂载都通过一个 trait 抽象。 |
| 22 | [代码关系图引擎](22-codebase-graph.md) | 模块 | 用 tree-sitter 把整个仓库解析成一张巨大的 ScopeGraph，记录每个符号的定义、引用和导入关系，支撑代码跳转和补全。 |
| 23 | [LSP 集成：给 AI 装上 IDE 的大脑](23-lsp-integration.md) | 模块 | Language Server Protocol 客户端怎么把代码诊断、格式化、符号查询这些 IDE 能力暴露给 AI——不是让 AI 猜代码有没有 bug，而是直接问 LSP。 |
| 24 | [代码溯源：谁写的这行——人还是 AI？](24-hunk-attribution.md) | 模块 | Git diff 拆成小块，每块打上来源标记。以后看代码库就能知道哪些是 AI 生成的、哪些是你亲手写的。基于 similar 文本差异算法和 gix 的 Git 操作。 |

### 扩展与集成

| # | 页面 | 类型 | 这一页讲什么 |
|---|------|------|--------------|
| 25 | [MCP 协议：接入外部工具服务](25-mcp-integration.md) | 模块 | 项目里 .mcp.json 文件声明的外部工具是怎么被发现、启动、建立连接，最终变成 Agent 可用工具的。 |
| 26 | [插件与钩子系统](26-plugins-and-hooks.md) | 模块 | 插件像 App Store 里的应用，钩子像自动化脚本。讲清楚它们的区别、怎么安装、怎么编写、触发时机和信任校验。 |
| 27 | [Agent Client Protocol：与编辑器通信](27-acp-protocol.md) | 模块 | 外部 IDE 或 Web 前端怎么通过 ACP 协议（基于 JSON-RPC）连上 Grok Agent，支持 stdio/HTTP/WebSocket 多种传输方式。 |

### 横切关注点

| # | 页面 | 类型 | 这一页讲什么 |
|---|------|------|--------------|
| 28 | [配置体系：三层优先级合并](28-config-system.md) | 模块 | 用户自己的 grok.toml、IT 管理员签发的策略文件、macOS MDM 推送的托管配置，三者怎么按优先级合并成最终生效配置。 |
| 29 | [遥测与可观测性](29-telemetry.md) | 模块 | 产品分析事件、性能追踪 span、Sentry 错误上报三套体系怎么共存在一个 crate 里，各自的数据流向哪里。 |
| 30 | [沙箱隔离](30-sandbox-security.md) | 模块 | 用 Linux namespace 和平台 API 把 AI 执行环境关进笼子——配置受控的文件系统视图和网络策略，防止越权操作。 |
| 31 | [记忆系统：AI 的长期小本本](31-memory-system.md) | 模块 | 基于 SQLite + 向量搜索的持久记忆，支持 BM25 全文检索和 KNN 语义搜索，还有一个「Dream」机制定期整理和遗忘旧记忆。 |
| 32 | [Leader 选举：多实例协作](32-leader-election.md) | 模块 | 同时开着好几个 grok-shell 操作同一个目录时，怎么选出唯一一个 Leader 负责写入，其他只做只读工作。 |

### 构建与发布

| # | 页面 | 类型 | 这一页讲什么 |
|---|------|------|--------------|
| 33 | [自动更新子系统](33-update-autoupdate.md) | 模块 | 应用怎么静默检查新版本、下载安装包、校验 SHA256、执行安装并重启，以及最低版本强制更新的阻断机制。 |
| 34 | [npm 二进制分发机制](34-npm-distribution.md) | 模块 | 总管包怎么通过 optionalDependencies 让 npm 自动下载正确平台的二进制，postinstall 脚本怎么把它们搬到正确位置。 |
| 35 | [测试策略与基础设施](35-testing-strategy.md) | 上手 | 怎么用 PTY 端到端测试模拟真实终端交互、YAML 场景驱动测试让非 Rust 程序员也能写用例、trace replay 确定性复现问题。 |

### 参考

| # | 页面 | 类型 | 这一页讲什么 |
|---|------|------|--------------|
| 36 | [用户命令与功能索引](36-command-reference.md) | 参考 | 所有斜杠命令的完整清单（从源码提取）、快捷键速查表、配置项字段说明和 CLI 包装入口的一览表。 |
| 37 | [术语速查](37-glossary.md) | 参考 | ACP、MCP、ScopeGraph、hunk tracker、Dream 这些项目内行话一句一句说清楚，避免新人看到缩写就头大。 |

## 🗂 仓库分区速览

| 区域 | 重要度 | 一句话说明 |
|------|--------|-----------|
| `xai-grok-pager/src` | ★★★★★ | 这是 AI 编程助手在终端里的完整 UI 层——负责把所有交互（聊天、斜杠命令、文件搜索、仪表盘、弹窗、滚动回溯）渲染成… |
| `xai-grok-shell/src` | ★★★★★ | 这是 Grok 命令行工具（grok-shell）的主代码区，负责把"在终端里跟 AI 对话、让它帮你改代码、执行命令"… |
| `codegen/xai-grok-tools` | ★★★★★ | 这是 Grok 的「工具箱」——把 AI 能调用的各种能力（读文件、搜代码、执行命令、网页抓取、LSP 诊断等）打包成统… |
| `crates/common` | ★★★★★ | 这是项目的公共基础库，像厨房里的一套共享刀具——各个业务模块都能从这里拿趁手的工具，比如熔断器、工具协议栈、日志追踪、对… |
| `codegen/xai-grok-workspace` | ★★★★★ | 这是 xAI Grok 本地工作区的核心库，负责管理你电脑上的项目文件夹——包括文件系统扫描、版本控制感知（Git/JJ… |
| `codegen/xai-grok-pager-render` | ★★★★★ | 这是 Grok 终端的“画面渲染引擎”——负责把 AI 对话、代码块、图片、视频预览这些东西画到终端屏幕上，同时处理键盘… |
| `codegen/xai-codebase-graph` | ★★★★★ | 把一整个代码仓库（支持 Rust、Python、TypeScript/JavaScript、Go）解析成一张巨大的关系图… |
| `codegen/xai-grok-workspace-types` | ★★★★☆ | 这个 crate 专门定义 xAI 工作区 API 在网络传输中使用的所有数据结构（请求、响应、事件、分块消息）。 |
| `codegen/xai-grok-telemetry` | ★★★★☆ | Grok Build 的遥测引擎——负责收集产品事件、性能追踪、错误上报这三件事，相当于给应用装了一个「黑匣子」加一个「… |
| `codegen/xai-grok-agent` | ★★★★☆ | 把 Agent 的定义、构建和系统提示词组装抽成一个独立的工具箱。 |
| `xai-grok-shell/tests` | ★★★★☆ | 这是整个 grok-build 项目的集成测试大本营，用真实场景去验证 xAI Grok Shell 这个代码生成工具的… |
| `codegen/xai-grok-sampler` | ★★★★☆ | xAI Grok 模型的采样推理层，负责把"发起补全请求"这件事包装成有状态的 Actor，内部搞定 HTTP 流式连接… |

> 另有 18 个次要区域（测试、文档、构建脚本等），正文模块页会按需涉及。

---

<sub>本 Wiki 由 AI 通读整个代码仓库后自动生成 · 图中文字与代码均来自仓库真实内容，但仍建议对照源码核对关键结论</sub>
