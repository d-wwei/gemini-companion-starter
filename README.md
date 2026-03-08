# Gemini Companion Starter

[中文版 (Chinese)](./README.zh-CN.md)

A specialized "Bootstrap Prompt" designed for Gemini CLI. It transforms your command-line AI from a one-shot Q&A tool into a dedicated personal assistant with persistent memory, structured local context, and custom workflows.

## Why this project exists

Many extensive "AI Assistant prompts" encounter problems in a CLI environment because they:
- Over-rely on conversational history which is lost upon session restart.
- Read too many files on every turn, causing huge latency and token waste.
- Struggle with state tracking or suffer from "hallucinated" console-only edits rather than actual file operations.

**Gemini Companion Starter** solves this by establishing an explicit "external memory layer" right inside your file system. It treats daily logs, project context, and your specific communication style as tangible file assets in your workspace. This approach enables true long-term collaboration continuity, drawing inspiration from advanced agent memory architectures.

### What it changes

- Your preferences and rules are no longer scattered across chat logs.
- Temporary contexts (daily logs) and long-term project decisions are managed separately.
- The CLI tool greets your project with full awareness of your previous sessions and guidelines.

### Concrete Examples

1. **Persistent Style:** Tell it once to "be concise, state conclusions first" and it's saved in `.assistant/STYLE.md`.
2. **Architectural Decisions:** Document why "we chose SQLite over Postgres for now" in `.assistant/memory/projects/db.md` to avoid rehashing the topic tomorrow.
3. **Daily Scratchpad:** Keep unstructured thoughts and unfinished tasks in `.assistant/memory/daily/YYYY-MM-DD.md` without polluting global AI rules.

## Core Engineering Highlights

1. **Lazy Loading:** Prevents token waste by loading only the essential rules (`SYSTEM.md`, `runtime/inbox.md`) by default. Deeper project documents are read purely on an as-needed basis.
2. **Entity-Based State Tracking:** Eliminates fragility by checking the actual content of essential files rather than relying on an external, easily-desynced status flag.
3. **Safe Execution:** Strictly requires the LLM to use actual file-system tool calls to create configurations, suppressing "hallucinated" console code blocks.
4. **Explicit Archiving Triggers:** Prevents accidental memory overwriting. The assistant requires explicit commands (e.g., `"/done"`) to update daily logs and session summaries.

## Getting Started

1. Ensure you have a working installation of Gemini CLI (or an equivalent agentic CLI tool capable of file operations).
2. Navigate to your desired project directory: `cd /path/to/your/project`
3. Start your CLI tool session.
4. Copy the entire text content of [Gemini-Companion-Starter.md](./Gemini-Companion-Starter.md).
5. Paste it as a single prompt into the CLI.
6. Answer the 1-3 bootstrap questions the assistant asks you to set up your profiles.
7. Enjoy your new personalized CLI assistant!
