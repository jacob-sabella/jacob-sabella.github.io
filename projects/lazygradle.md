---
layout: page
title: lazygradle
permalink: /projects/lazygradle/
---

**Python · Textual TUI · MIT**
Source: [github.com/jacob-sabella/lazygradle](https://github.com/jacob-sabella/lazygradle) · PyPI: [`lazygradle`](https://pypi.org/project/lazygradle/)

A Textual-based terminal UI for browsing Gradle tasks, running them, and reviewing output — inspired by IntelliJ IDEA's Gradle tool panel, but keyboard-driven and terminal-native.

## Workflow

- Cache metadata for multiple Gradle projects, switch between them
- Search the task list with `/`, drill into task details
- Run tasks with saved parameter + env-var configurations
- Review recent task history and structured output in a dedicated Task Manager tab

## Features

- Multi-project support with cached Gradle metadata
- Fast keyword search, keyboard-first task execution
- Saved task configs (CLI flags + env vars), reusable across runs
- Recent-task history with re-run
- Vim-style navigation in Task Output: motions, visual selection, yank
- Contextual key guide accessible from the command palette
- Optional file logging via env var

## Install

```sh
pip install lazygradle
lazygradle
```
