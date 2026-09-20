---
title: <Claude Code> 1. Setting Up Claude Code
date: 2026-03-11T10:04:20Z
lastmod: 2025-03-12T11:04:20Z
author: Jun Yeop(Johnny) Na
avatar: /images/favicon.svg
# authorlink: https://author.site
cover: logo.png
category: General Dev
categories:
  - Claude Code
  - Vibe Coding
  - Coding Agent
tags:
  - Claude Code
  - Programming
  - LLM

# nolastmod: true
draft: false
---

# Downloading Claude Code

```sh
# linux shell
curl -fsSL https://claude.ai/install.sh | bash

# powershell
irm https://claude.ai/install.ps1 | iex
```

![claude_code_install](./)

# Basic Settings

## /permissions

- set tools that can and cannot be run

## /terminal-setup

**enables `shift-enter` for newlines**

## /login

authenticate in Claude account.

# Claude Context

## Common Bash Commands, Files, Style Conventions

### Shared Across All Projects

- `/<enterprise root>/CLAUDE.md`
- `~/.claude/CLAUDE.md`

### Checked in

- `<project root>/CLAUDE.md`

### Not Checked in

- `<project root>/CLAUDE.local.md`

need to manually check in

## Pull In Context On Demand

- `~/.claude/commands/foo.md` -> `/user:foo`
- `<project root>/.claude/commands/foo.md` -> `/project:foo`
- `a/commands/foo.md` -> `project:a:foo`
- `a/commands/foo.py` -> `@a/foo.py`

## Permission Setting For Sharing

![permission](./permission.png)

## Keybindings

![keybindings](./keybindings.png)

# Tips

## 1. Run /init immediately

## 2. Structure: What, Domain, Validation

- Validation is important for AI to self-improve

## 3. CLAUDE.md deep dive

- always do X, never do Y - documnet Claude's mistakes

* let Claude update CLAUDE.md

## 4. Get Used to the Primitive Composites

- Skills: recurring workflow (Ex) get article from hacker news + save it to local file)
- MCPs: give repo URL and tell claude to install this MCP.

## 5. /chrome connects browser

## 6. autoformat with PostToolUse

## 7. escape twice puts you back to previous state
