# Playwright CLI Installation Guide

How to install the Playwright CLI for use with Claude Code, and why it's the best browser automation option available.

## Installation

Three commands to get up and running:

### 1. Install the Playwright CLI

```bash
npm install -g @anthropic-ai/playwright-cli
```

Installs the CLI globally so it's available from any project.

### 2. Install the browser engine

```bash
npx playwright install chromium
```

Downloads the Chromium browser engine that Playwright uses under the hood. To use a different engine (Firefox, WebKit, Edge), substitute the browser name or check the [Playwright docs](https://playwright.dev/docs/browsers).

### 3. Install the Claude Code skill

```bash
playwright-cli install --skills
```

### Note: Global Installation for all Claude Projects

```bash
claude mcp add -s user playwright -- npx @playwright/mcp@latest
```

Installs the Playwright skill into your Claude Code project so it knows how to use the CLI. The skill is a living document -- you can edit, audit, or recreate it with the skill creator.

> **Note:** This installs the skill to the current project only (`.claude/skills/`). To make it available globally across all projects, copy the skill to `~/.claude/skills/playwright-cli/`.

## Comparison: CLI vs MCP Server vs Claude in Chrome

All three approaches can interact with a browser programmatically, but they differ significantly in efficiency.

| Feature | Playwright CLI | Playwright MCP Server | Claude in Chrome Extension |
|---|---|---|---|
| **Token usage** | Lowest | ~90,000 more tokens for the same task | Highest |
| **Headless support** | Yes | Yes | No |
| **Parallel execution** | Yes | Yes | No (one tab at a time) |

### Why the token difference is so large

All three tools rely on the browser's **accessibility tree** -- the structure that maps a website so assistive technologies (like screen readers) can navigate it.

The difference is in how that tree reaches Claude Code:

- **MCP Server** -- Sends the *entire* accessibility tree into Claude Code's context on every interaction. The tree is large, so each pass is a massive token dump.
- **CLI** -- Saves the full accessibility tree to **disk**, then sends only a **summary** to Claude Code with just the information it needs. Far fewer tokens.
- **Chrome Extension** -- Takes **screenshots** of the web page. Images are the most token-expensive format of all, making this the costliest option. It also cannot run headless or in parallel.

### Bottom line

The CLI can do everything the MCP server can do -- and more -- at a fraction of the token cost. The Chrome extension is the least efficient option across every dimension.

## Demo

See [DEMO.md](DEMO.md) for a hands-on demo that runs 3 parallel sub-agents to test a contact form from different angles (happy path, validation, edge cases).

## Source

Based on: [Playwright CLI for Claude Code](https://www.youtube.com/watch?v=I9kO6-yPkfM) by Chase Lean (Chase AI Plus)
