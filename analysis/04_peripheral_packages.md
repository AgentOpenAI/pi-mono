# 外围软件包概览

## 1. 终端 UI 框架 (`packages/tui`)
这是一个定制构建的 CLI 应用程序渲染引擎，作为编码智能体的 UI 层。

### 主要特性
*   **差分渲染**: 仅更新更改的行以防止闪烁。
*   **组件架构**: 类似 React 的结构，具有 `Component` 接口 (`render(width)`) 和复合组件 (`Container`, `Box`, `Editor`)。
*   **高级终端支持**: 支持 Kitty/iTerm2 协议以显示内联图像。
*   **输入处理**: 通过 `matchesKey` 进行健壮的按键检测（包括修饰符）。

### 用途
它抽象了原始 ANSI 转义码的复杂性，允许开发者以声明方式构建富文本界面（仪表板、聊天窗口）。

## 2. Mom: Slack 机器人 (`packages/mom`)
"Master Of Mischief" (Mom) 是一个基于 Agent Core 的 Slack 机器人封装，专为自主性设计。

### 核心概念
*   **自我管理**: 与标准编码智能体不同，Mom 设计为在 Docker 沙箱内安装自己的依赖项（通过 `apk`, `npm`）。
*   **技能 (Skills)**: Mom 通过将脚本写入 `skills/` 目录来创建自己的“工具”。这是一种“持久能力获取”的形式。
*   **双重记忆**:
    *   `log.jsonl`: 无限的仅追加历史记录（事实来源）。
    *   `context.jsonl`: 发送给 LLM 的实际窗口（已压缩）。
*   **事件**: 支持触发智能体的计划任务（Cron 作业）。

## 3. Pods: vLLM 部署工具 (`packages/pods`)
一个 DevOps 工具，用于简化在云 GPU 提供商（DataCrunch, RunPod）上运行开源 LLM。

### 功能
*   **自动化设置**: 配置 Ubuntu Pod，安装驱动程序并设置 vLLM。
*   **智能存储**: 挂载共享 NFS 卷，以便模型只需下载一次并在 Pod 之间共享。
*   **抽象**: 无论底层硬件或模型（Qwen, GLM, Llama）如何，都暴露出标准的 OpenAI 兼容 API (`/v1/chat/completions`)。

## 4. Web UI (`packages/web-ui`)
一个用于构建基于浏览器的智能体界面的 Web 组件库。

### 特性
*   **Artifacts (工件)**: 在沙盒 iframe 中渲染 LLM 生成的 HTML/SVG 代码块（类似于 Claude Artifacts）。
*   **浏览器工具**: 包含客户端 JavaScript REPL 和文档文本提取功能。
*   **存储**: 使用 IndexedDB 完全在浏览器中持久化会话和 API 密钥（本地优先，隐私保护）。
*   **CORS 代理**: 内置支持处理跨域请求，以便直接从浏览器连接到外部 LLM 提供商。

## 总结
这些软件包展示了核心 `agent` 和 `ai` 模块的多功能性。相同的核心逻辑驱动着 TUI CLI、Slack 机器人和 Web 应用程序，证明了架构上的关注点分离是有效的。
