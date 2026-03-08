# Gemini CLI Assistant Initialization Prompt
[中文版 README (Chinese Version)](#gemini-cli-个人助理一键初始化)

Transform your Gemini CLI from a one-shot Q&A tool into a long-term personal assistant system with memory, custom workflows, and user preferences.

## What is this?
This is a comprehensive "Bootstrap Prompt" designed specifically for the [Gemini CLI](https://github.com/GoogleCloudPlatform/gemini-cli) (or similar command-line LLM tools). By pasting this prompt into an initialized Gemini CLI session, you trigger an automated setup process that creates:
1.  **Global Behavior Rules (`~/.gemini/`):** Establishes default operational guidelines, memory policies, and auto-bootstrap behaviors across all your projects.
2.  **Project-Level Assistant Context (`.assistant/`):** Initializes a local directory structure for the current workflow, including style preferences, contextual templates, and daily memory logs.
3.  **Bootstrap Dialogue:** Automatically starts an interactive conversation to learn your name, role, and communication style to populate the newly created configuration files.

## Features
*   **Lazy Loading Strategy:** Prevents token waste by only loading necessary files (`SYSTEM.md`, `runtime/inbox.md`) by default, reading deeper context only when triggered.
*   **Entity-Based State Tracking:** Eliminates fragility by checking the actual content of essential configuration files rather than relying on an external status flag.
*   **Safe Execution Rules:** Strictly requires the LLM to use actual file-system tool calls to create configurations, suppressing "hallucinated" console outputs.
*   **Explicit Archiving Triggers:** Prevents accidental memory overwriting by requiring explicit commands (e.g., `"/done"`, `"总结会话"`) to update daily logs and session summaries.
*   **Git Protection:** Automatically adds the `.assistant/` directory to `.gitignore` to prevent leaking personal AI contexts or API configurations into your public codebases.

## How to use
1.  Ensure you have a working installation of Gemini CLI (or an equivalent agentic CLI tool capable of file operations).
2.  Navigate to your desired project directory: `cd /path/to/your/project`
3.  Start your CLI tool.
4.  Copy the entire content of `Gemini-CLI-Assistant.md` and paste it as a single prompt into the CLI.
5.  Answer the 1-3 bootstrap questions the assistant asks you.
6.  Enjoy your new personalized CLI assistant!

---

# Gemini CLI 个人助理一键初始化
[English README (英文版)](#gemini-cli-assistant-initialization-prompt)

将你的 Gemini CLI 从“一次性问答工具”永久转变为一个“具备长期记忆和个性化工作流的个人助理系统”。

## 介绍
这是一个专为 [Gemini CLI](https://github.com/GoogleCloudPlatform/gemini-cli)（或类似支持文件操作的终端大模型工具）设计的“引导提示词 (Bootstrap Prompt)”。将这段提示词发给全新启动的 Gemini CLI，它会自动执行一系列操作：
1.  **建立全局法则 (`~/.gemini/`)：** 配置核心响应逻辑、记忆管理策略，确保在你的机器上的所有工作区都遵循统一的助理行为规范。
2.  **初始化项目记忆 (`.assistant/`)：** 生成当前项目特有的上下文目录结构，存放你的沟通偏好、工作流定义以及日常思考碎片。
3.  **无缝进入 Bootstrap 对话：** 自动向你提问以收集诸如称呼、岗位角色、输出风格偏好等信息，并将结果固化到本地配置中。

## 核心特性
*   **记忆懒加载 (Lazy Loading)：** 告别每次对话都长篇大论地读取所有历史记录。默认仅读取极少量核心规则（`SYSTEM.md` 等），按需加载具体项目上下文，极大降低工具调用风暴与 Token 消耗。
*   **实体级状态判定：** 抛弃脆弱易错的独立状态配置文件，直接通过判断 `USER.md` 等核心文件是否拥有实质性内容（>3行），来决定是否需要重新开启引导模式。
*   **防呆执行策略：** 在提示词层面严格约束 LLM 的行为，强制要求它必须调用底层的系统工具去**真实创建写入文件**，严禁只在对话框里打印代码块或脑补执行结果。
*   **显式归档触发器：** 摒弃模糊的任务结束判定。规定只有在用户明确发送 `"/done"`、`"结束"` 或 `"总结会话"` 等指令时，才允许复写 `last-session.md` 并沉淀当日记忆。
*   **隐私安全防线：** 若在 Git 仓库内初始化，会自动将 `.assistant/` 目录注入 `.gitignore`，防止你的 AI 交互习惯或敏感上下文被意外 Commit 出去。

## 使用指南
1.  确保你已正确安装并配置好了 Gemini CLI（或其他具备底层读写能力的 Agent 工具）。
2.  在终端中进入你想初始化的项目目录：`cd /path/to/your/project`
3.  启动你的命令行 AI。
4.  复制本项目中 `Gemini-CLI-Assistant.md` 文件的全部内容，一次性发送给它。
5.  跟随它的引导，回答 1-3 个有关于你个人偏好的小问题。
6.  初始化完成，开始体验有记忆的长期协作吧！
