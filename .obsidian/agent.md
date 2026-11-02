# Developer Knowledge Agent Guide (Obsidian + CLI + AI)

## Purpose

This document defines how an AI coding assistant (e.g., Copilot, CLI agents, or local LLM tools) should help maintain a structured developer knowledge base using Obsidian.

The user is an Android developer working on a chat application with multiple modules.
The goal of this system is to maintain a continuously updated developer knowledge base containing:

* Daily development logs
* Task tracking
* Debugging notes
* Architecture documentation
* Technical learnings
* Bug fixes and solutions

The system should minimize friction and automate note creation wherever possible.

---

# Vault Structure

All notes must follow this directory structure.

```
Remote/dev-vault/

Remote/daily/
Remote/modules/
Remote/debugging/
Remote/architecture/
Remote/snippets/
Remote/ai-summaries/
Remote/tasks/
```

Explanation:

Remote/daily/
Daily development logs.

Remote/modules/
Knowledge specific to each project module.

Remote/debugging/
Bug investigations and fixes.

Remote/architecture/
System architecture and technical explanations.

Remote/snippets/
Useful commands, code snippets, and reusable solutions.

Remote/ai-summaries/
AI generated summaries of work sessions.

tasks/
Task notes and feature planning.

---

# Daily Note Format

Daily notes must be named using the following format:

YYYY-MM-DD.md

Example:

```
daily/2026-04-01.md
```

Each daily note must follow this template.

```
# YYYY-MM-DD

## Modules Worked
- module name

## Tasks
- description

## Debugging Notes
Issue:
Cause:
Fix:

## Commits
- commit hash message

## Learnings
- new concept learned

## Notes
- extra context
```

The AI assistant should automatically populate sections whenever possible.

---

# Module Knowledge Notes

Each module must have its own folder.

Example:

```
modules/chat-core/
modules/websocket/
modules/notifications/
modules/ui-compose/
```

Important notes inside a module:

```
message-flow.md
architecture.md
known-issues.md
debug-history.md
```

Bug fixes should always follow the format:

```
Issue
Cause
Fix
Prevention
```

---

# Debugging Notes Format

All debugging notes must follow this structure.

```
Title: short description of issue

Module:
Date:

Issue:
Describe the observed problem.

Cause:
Root cause analysis.

Fix:
Steps taken to resolve it.

Verification:
How the fix was verified.

Related Commits:
commit references
```

---

# AI Assistant Responsibilities

The AI agent should assist in the following ways.

## 1. Convert Commit Logs into Notes

When commit history is available, summarize commits and append them to the daily note.

Example format:

```
## Commits
- a12d3f Fix websocket reconnect issue
- b55f11 Improve message pagination
```

---

## 2. Generate Daily Work Summaries

Based on:

* commit logs
* task lists
* debugging notes
* terminal logs

The AI should produce a short summary.

Example:

```
### AI Summary

Today's work focused on improving websocket reconnection reliability.
Several fixes were made to message retry handling and Compose UI state updates.
```

This should be stored under:

```
ai-summaries/YYYY-MM-DD-summary.md
```

---

## 3. Convert Debug Sessions into Knowledge Notes

If debugging steps appear in logs or chat prompts, convert them into structured debugging notes and store them under:

```
debugging/
```

The goal is to build a searchable bug history.

---

## 4. Extract Technical Learnings

When explanations or technical discussions occur, convert them into learning notes under:

```
architecture/
```

Examples:

```
okhttp-websocket-lifecycle.md
compose-recomposition-behavior.md
android-workmanager-retry-strategy.md
```

---

## 5. Organize Notes Automatically

If a note contains:

Architecture explanation → move to architecture/
Bug investigation → move to debugging/
Reusable command → move to snippets/

---

# CLI Workflow Expectations

The AI agent should assume the developer frequently works in terminal and may use commands such as:

```
devnote
note "message"
dev-summary
```

These commands correspond to:

devnote → create/open daily note
note → append quick note to daily log
dev-summary → generate AI summary of the day

The AI should assist in generating scripts when requested.

---

# Example Quick Note Format

Terminal quick notes should appear like this inside the daily file.

```
- 14:10 Fixed websocket reconnect issue
- 15:20 Debugged compose recomposition loop
```

---

# Knowledge Base Goals

This system should evolve into a long-term developer knowledge base.

After several months it should contain:

* Complete bug history
* Architecture decisions
* Module-level documentation
* Technical learnings
* Past debugging solutions

The AI assistant should prioritize:

1. Structured notes
2. Minimal manual effort
3. Automation whenever possible
4. Searchable debugging knowledge

---

# AI Behavior Guidelines

When assisting the developer:

1. Prefer structured markdown output.
2. Follow the folder structure defined in this document.
3. Avoid unstructured notes.
4. Convert conversations into knowledge when useful.
5. Suggest improvements to the knowledge base when patterns are detected.

---

# End of Agent Guide
