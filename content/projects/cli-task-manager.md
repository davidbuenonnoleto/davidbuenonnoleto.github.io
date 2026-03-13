---
title: "CLI Task Manager"
date: 2024-01-10
draft: false
tags: ["go", "cli", "productivity"]
github: "https://github.com/davidbuenonnoleto"
description: "A fast, keyboard-driven CLI task manager with project tagging, priorities, and SQLite storage."
---

A minimal, blazing-fast task manager for the terminal. No cloud sync, no subscriptions — just a clean SQLite-backed CLI that stays out of your way.

## Features

- **Project tagging** — organize tasks by project or context
- **Priority levels** — P1/P2/P3 with visual indicators
- **Due dates** — with overdue highlighting
- **Fuzzy search** — find tasks instantly by keyword
- **Export** — dump tasks to Markdown or JSON

## Usage

```bash
task add "Fix rate limiting bug" --project api-gateway --priority 1
task list --project api-gateway
task done 42
task search "rate limit"
```

## Tech Stack

- **Go** — core CLI
- **SQLite** (via `modernc.org/sqlite`) — zero-dependency storage
- **Cobra** — CLI framework
- **Lip Gloss** — terminal styling
