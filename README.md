# Gemini Recall

> Have a mind like a steel trap.
> 长期记忆，断点中继。

[中文说明](./README.zh-CN.md)

> **Looking for other CLI assistants?** 
> Check out the sister extensions: [Claude Recall](https://github.com/d-wwei/claude-recall) & [Codex Recall](https://github.com/d-wwei/codex-recall)

Turn Gemini CLI into a long-term collaboration system with durable memory and session continuity.

A deployment-ready package for people who want Gemini CLI to do more than answer one-off questions. It emphasizes long-term memory, growing collaboration chemistry over time, session handoff continuity, and fast recall of prior context.

## Why This Exists

Many "personal assistant" prompts look impressive in theory but behave poorly in real CLI environments. Common issues include:

- over-relying on chat history that disappears between sessions
- reading too many files on every turn
- blurring the line between suggested content and actual file operations
- mixing long-term rules with temporary task notes

This project is designed around those realities.

Instead of pretending the model will just remember everything, it pushes Gemini CLI to use the local file system as a durable collaboration surface.

## Key Features

This prompt transforms Gemini CLI from a powerful but amnesic one-shot tool into a long-term collaborator that builds rapport with you.

- **Long-term Memory Setup:** Automatically establishes persistent global rules (`~/.gemini/`) and project-specific memory (`.assistant/`).
- **Preference Accumulation:** Remembers how you like things done—your role, coding style, workflow, and recurring instructions.
- **Session Handoff Continuity:** Keeps work resumable across interruptions, so the next session can continue from the right breakpoint instead of restarting from scratch.
- **Global Memory Promotion:** As it discovers your habits and reusable knowledge during active projects, it can automatically promote them to your global profile.
- **Project-Specific Customization:** Every project can have its own tailored rules and context that override the global defaults.
- **Fast Recall:** Helps Gemini quickly recall your background, working style, recent decisions, and unfinished context when you re-enter a workspace.
- **Task-Level Resume Checkpoints:** For multi-step work of many kinds, Gemini can maintain a module-local `PROGRESS.md` so a new process can resume from the last accepted step or milestone instead of reconstructing progress from chat.
- **Layered Bootstrap Interview:** The startup interview now uses a compact 3-step script to capture naming, style, assistant role, ambiguity handling, work types, and memory boundaries without becoming a questionnaire.
- **Global Quick Mode:** When started from `$HOME`, Gemini writes directly to `~/.gemini/GEMINI.md` and skips redundant "sync to global" prompts.
- **Historical Project Scan:** First-time setup can scan prior `.assistant/` workspaces, extract project/session summaries, and register them into the global project index.

## Why OpenClaw-Inspired Memory For Gemini CLI

Gemini CLI is good at following instructions and working with local files, but it does not automatically become a long-term collaborative system just because you give it a long prompt.

OpenClaw-style memory systems are useful because they treat files as durable memory artifacts instead of leaving everything in chat history. That idea maps well to CLI assistants:

- stable preferences belong in structured files
- project decisions should survive across sessions
- temporary notes should stay separate from long-term memory

This repository applies that philosophy to Gemini CLI by using `.assistant/` as a project-local memory layout and `~/.gemini/GEMINI.md` as a global default behavior layer.

The result is not "turning Gemini into OpenClaw". The result is giving Gemini CLI a more structured memory surface so it can feel less stateless and more like a repeatable collaborator.

## What This Changes

- preferences and collaboration habits stop living only in chat history
- project context becomes inspectable and editable
- temporary context and durable memory are stored separately
- Gemini can re-enter a workspace with better continuity

## Concrete Examples

1. Style becomes stable across sessions.  
   Preferences such as "be concise, state conclusions first, then risks" can live in `.assistant/STYLE.md`.

2. Project decisions stop resetting.  
   If a project already decided to postpone a large refactor, that can live in `.assistant/memory/projects/architecture.md` instead of being re-debated from scratch.

3. Daily scratch notes stop polluting long-term rules.  
   Unverified notes or one-day context can live in `.assistant/memory/daily/YYYY-MM-DD.md`.

4. Workflow becomes personalized.  
   If you prefer "inspect first, edit second, summarize verification at the end", that can live in `.assistant/WORKFLOW.md`.

5. Interrupted work can resume cleanly.  
   A nearby `PROGRESS.md` can record which milestones or acceptance items are done, what is currently in progress, and what the next concrete step should be for the next Gemini process.

Use the generic template by default for content, video, research, operations, design, or mixed project work:

```md
status: in_progress
task: Produce launch video cut
module_path: content/video-launch/
project_type: video-editing

# 任务进度

## 已完成
- [x] 确认视频目标、时长和发布渠道
- [x] 整理可用素材与配音版本

## 进行中
- [ ] 精剪主版本时间线并对齐字幕

## 待做
- [ ] 输出 16:9 主版本
- [ ] 裁切 9:16 短视频版本
- [ ] 完成最终审校并导出交付文件

## 关键决策
- 主版本控制在 90 秒内，优先保留产品演示镜头
- 字幕风格统一使用品牌模板，避免重新设计一套样式

## 已知问题
- 第三段配音底噪偏重，可能需要降噪或重录

## 关键文件 / 素材
- raw/interview-a-roll/
- edits/launch-main.prproj
- assets/subtitles/final-cn.srt
```

Keep the development-specific template for software implementation work:

```md
status: in_progress
task: Add task-level progress recovery
module_path: packages/assistant-memory/

# 开发进度

## 已完成
- [x] 明确 `PROGRESS.md` 使用模块局部文件而不是 `.assistant/`
- [x] 加入恢复口令：继续上次进度 / 恢复进度
- [x] 定义恢复时的候选定位顺序

## 进行中
- [ ] 把恢复逻辑接入当前模块的初始化流程

## 待做
- [ ] 补充恢复话术模板
- [ ] 增加多候选 `PROGRESS.md` 的确认逻辑
- [ ] 完成一次中断恢复流程验证

## 关键决策
- `PROGRESS.md` 放在实际模块目录，避免所有任务共用一份中心状态文件
- 恢复时只读取最相关的 1-2 个候选，减少 token 消耗

## 已知问题
- 当前模块还没有验证“多个候选进度文件”时的选择行为
```
Use the generic template for milestone-based work. Use the development template when the task is driven by explicit acceptance items and code verification. For explicit recovery, the user can say `继续上次进度`, `恢复进度`, `resume progress`, or `continue from progress`.

When recovering, the assistant should locate the most relevant `PROGRESS.md` in this order: current working directory, most recently modified module, user-named module, then best keyword-matching module. It should read only the top 1-2 candidates instead of scanning every progress file in the repo.

The recommended resume wording is:

```text
我找到了这份进度记录：
- 已完成：...
- 进行中：...
- 下一步：...

要我按这份进度继续吗？
```

## Core Ideas

- File-backed memory instead of chat-only memory
- Lazy loading instead of reading every file on every turn
- Incremental edits instead of destructive rewrites
- Explicit file operations instead of hallucinated "pretend writes"
- Lightweight bootstrap instead of long onboarding forms

## What's Included

- [BOOTSTRAP_PROMPT.md](./BOOTSTRAP_PROMPT.md)  
  The main bootstrap prompt for Gemini CLI.

- [scripts/show-bootstrap.sh](./scripts/show-bootstrap.sh)  
  Prints the bootstrap prompt locally.

- [README.zh-CN.md](./README.zh-CN.md)  
  Chinese documentation.

## Deploy

1. Clone or download this repository.
2. Open `BOOTSTRAP_PROMPT.md`, or print it with:

```bash
./scripts/show-bootstrap.sh
```

3. Start Gemini CLI in the target workspace.
4. Paste the prompt into Gemini CLI in one message.
5. Let Gemini inspect the workspace, initialize memory, and begin lightweight bootstrap.

## Package Layout

```text
BOOTSTRAP_PROMPT.md
scripts/
  show-bootstrap.sh
```

This repository is now organized as a lightweight Gemini deployment pack rather than only a nested prompt file.

## Quick Start

1. Open [BOOTSTRAP_PROMPT.md](./BOOTSTRAP_PROMPT.md).
2. Copy the full prompt.
3. Start Gemini CLI in your target workspace.
4. Send the prompt in one message.
5. Let Gemini inspect the workspace, update config, and start lightweight bootstrap if needed.

## Recent Improvements

- richer first-round interview for long-term collaboration
- bootstrap trigger now prefers `BOOTSTRAP.md` plus core-file completeness instead of a single-file heuristic
- explicit global quick mode with no duplicate global-sync prompt
- fuller historical project and session scan flow
- clearer incremental-update behavior for existing `GEMINI.md`
- task-level `PROGRESS.md` checkpoints for interrupted implementation work

## How This Prompt Is Positioned

This repository currently provides one main prompt, not multiple variants.

It is best understood as a balanced working version:

- proactive enough to create files and initialize structure
- cautious enough to avoid unnecessary overwrites
- opinionated about memory structure and lazy loading

## Design Principles

- Global defaults should guide behavior, not claim permanent hard control
- Project-local memory should carry evolving context
- Temporary notes should not pollute long-term memory
- Small, durable files are better than giant prompt blobs
- Real CLI behavior matters more than idealized agent theory

## Important Boundaries

- `~/.gemini/GEMINI.md` should be treated as a default behavior layer, not a guaranteed permanent control layer.
- `.assistant/` is a project-local memory convention, not a native Gemini CLI protocol.
- Actual behavior still depends on runtime context, available tools, and the model's execution behavior.

## FAQ

### Why not just use a single giant prompt every time?

Because a giant prompt does not scale well across sessions.

As soon as work becomes ongoing, you need stable structure for:

- user preferences
- project decisions
- temporary notes
- unfinished tasks

Keeping that in files is more durable than repeating it in chat.

### Why not keep everything inside `GEMINI.md`?

Because global rules and project memory are different things.

`GEMINI.md` is better for defaults and stable behavior. Project-level files are better for local context that changes over time.

### Doesn't `.assistant/` add too much overhead?

It can, if overbuilt.

This project tries to keep the structure small and practical. The goal is not process for its own sake. The goal is reducing repeated friction.

### Is this trying to replace Gemini CLI?

No.

It is a prompt-layer workflow enhancement. Gemini CLI still does the actual work. This repository simply gives it a more durable collaboration structure.

## Who This Is For

This project is useful if you want Gemini CLI to behave more like:

- a repeatable project assistant
- a workspace-aware collaborator
- a tool that can preserve lightweight context across sessions

It is less useful if you only want fast one-off prompts with no local memory structure.

## Notes

If Gemini becomes too aggressive in your environment, tighten the prompt or trim the initialization scope.

If it is too passive, strengthen the execution instructions around file creation, validation, and bootstrap follow-through.
