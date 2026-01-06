# Claude Code, CLIs, and GitHub — Practical Notes

## Introduction

This document consolidates resources, notes, and practical guidance around **Claude Code**, **CLIs**, **MCP servers**, **HumanLayer**, and **Git/GitHub** workflows.  
It is intended as a living reference rather than a step‑by‑step tutorial.

---

## Getting Started with Claude Code

**Installation**
- Instructions: https://docs.google.com/document/d/1fyIBcSlbYNipWNURXOMUQL0Sb2DBHr9qBZKjgnQ7tl8/edit?tab=t.0#heading=h.iicwv6vi9ecb

**Agent use‑case examples**
- Skim for inspiration:  
  https://docs.google.com/document/d/1iUntmg8Wx_Zx0UJm_JZ2JKLWW-rn76wJwNc6wUnFhiM/edit?tab=t.0#heading=h.fscq0ovl563v

**Using Claude for documents**
- Spreadsheets, PowerPoints, Word docs:  
  https://natesnewsletter.substack.com/p/finally-ai-can-do-excel-and-powerpointget

---

## Understanding Claude Code

**Core concepts**
- Videos:
  - https://www.youtube.com/watch?v=JC2dR2RHcEM&t=7s
  - https://www.youtube.com/watch?v=rfDvkSkelhg&t=7s

**Full workflow walkthrough**
- Video: https://www.youtube.com/watch?v=32xfY8ct6Qw&t=1871s  
- Support document:  
  https://aiadvantage.notion.site/Claude-Code-Ultimate-Starter-Kit-25a6426aaf6980ce8a37f19ce2f4a3be

---

## MCP Servers

**Context7**
- Purpose: Pulls up‑to‑date official documentation into Claude Code.
- Recommendation: Use Ref if budget allows (saves context window).
- Links:
  - https://github.com/upstash/context7
  - https://ref.tools/mcp
  - https://github.com/ref-tools/ref-tools-mcp

**Playwright MCP**
- Allows the terminal to use the web natively for testing web apps.
- https://github.com/microsoft/playwright-mcp

**Zen MCP**
- Enables calling multiple LLM APIs within the same chat.
- https://github.com/BeehiveInnovations/zen-mcp-server

**Multi‑model workflows**
- Run the same task across multiple LLMs.
- Hand off results to a sub‑agent for evaluation and merging.
- Example: Deep research + social search + general web search, merged by Claude.

---

## Practical Tips

- Thinking budget phrases (increasing):
  - “think” → “think hard” → “think harder” → “ultrathink”  
  Note: *ultrathink often over‑thinks and degrades output quality.*

- Use **“proactive”** for MCP servers or sub‑agents that should run automatically.

---

## GitHub Repositories

Creating a GitHub repository gives you:
- Versioned file storage
- Full history of changes
- Ability to branch, revert, and experiment safely
- Private or public repositories

Effectively, GitHub becomes a high‑efficiency file management system for all CLI‑driven work.

See the CLI + Git/GitHub guide linked below.

---

## Intermediary / Advanced Material

**High‑quality podcast & philosophy**
- Focus: Claude Code limitations, best practices, context engineering.
- One host develops **HumanLayer**.

**Recommended episodes**
- Context Engineering: https://www.youtube.com/watch?v=42AzKZRNhsk
- Claude for non‑coding tasks: https://youtu.be/NJcph4j9sNg
- HumanLayer background ("Hear from Founders"):  
  https://www.ycombinator.com/companies/humanlayer

**Optional**
- Vision models + PDFs: https://youtu.be/sqJrl09dDmI
- Model comparison: https://youtu.be/OawyQOrlubM

---

## Tooling Add‑Ons

**CCStatusLine**
- Shows live context window usage in terminal.
- Aim to stay below ~40%.

**HumanLayer**
- Installation: read all READMEs carefully.
- Sub‑agents and commands often replace “skills” for coding.

**Key concept**
- Main Claude terminal = main agent.
- Sub‑agents:
  - Have separate context windows.
  - Pass *results only* (handoffs), not full reasoning traces.
  - Enable parallelism, chaining, and evaluation/merging steps.

Think of sub‑agents as **additional context windows** you can orchestrate.

---

## CLIs and GitHub Guide

Guide written by the author:
- Focus: How CLIs and Git/GitHub work together (not a Git manual).
- Fully editable:
  https://github.com/xapids/LLM/blob/c72c906014c57fcc70074cb3c96f3a054f070e23/CLI%20%2B%20Git%2C%20Github%20Guide.md

---

## If You Have Extra Time

**Personal AI systems**
- Video: https://www.youtube.com/watch?v=Le0DLrn7ta0&t=1190s
- Blog post: https://danielmiessler.com/blog/personal-ai-infrastructure
