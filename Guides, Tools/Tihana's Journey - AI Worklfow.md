# Tihana's Journey - AI Workflow

## Introduction

This document consolidates resources, notes, and practical guidance around working with LLMs to create high grade workflows and outpouts.
It is intended as a living reference rather than a step‑by‑step tutorial.

---

## Claude Code Installation
- Only read installation instructions, dont bother with the rest of the document: https://docs.google.com/document/d/1fyIBcSlbYNipWNURXOMUQL0Sb2DBHr9qBZKjgnQ7tl8/edit?tab=t.0#heading=h.iicwv6vi9ecb

---

## Mental Model for working with LLMs

### High‑quality podcast
- https://youtube.com/playlist?list=PLi60mUelRAbFqfgymVfZttlkIyt0XHZjt&si=kllhT6YvfAdNP1gv
- Focus: LLM limitations, best practices, Workflow Ideas
- Dex, one of the host, develops the wrapper **HumanLayer** and IDE **Codelayer**.

**Mandatory episodes** - Watch episodes with the visuals, dont only listen to - Conceptual level of Humanlayer and Codelayer workflow
- Context Engineering: https://www.youtube.com/watch?v=42AzKZRNhsk
- Claude for non‑coding tasks: https://youtu.be/NJcph4j9sNg

### Agents
- Main Claude/Codex terminal = main agent.
- Sub‑agents:
  - Have separate context windows.
  - Pass *results only* (handoffs), not full reasoning traces.
  - Enable parallelism, chaining, and evaluation/merging steps.
 
**Multi‑Agent workflows**
- Run the same prompt in 4 instances/agents in parallel.
- 5th agent evaluates outcomes, either picking the best or merging them for the 5th version.

Think of sub‑agents as **additional context windows** you can orchestrate.

---

## Getting Accustomed to Claude Code environment

### Workflow basics -  To get familiar with the enviroment. How it looks, explicit steps etc.
- https://www.youtube.com/watch?v=rfDvkSkelhg&t=7s
- https://www.youtube.com/watch?v=32xfY8ct6Qw&t=1871s  

---

## Tools & Tips

### Claude Code

#### Thinking tuning (increasing):
  - “think” → “think hard” → “think harder” → “ultrathink”  
  Note: *ultrathink often over‑thinks and degrades output quality.*

### MCP Servers

#### Playwright MCP
- Allows the terminal to use the web natively for testing web apps.
- https://github.com/microsoft/playwright-mcp

### HumanLayer/ Codelayer
- Installation: https://www.humanlayer.dev/docs/introduction
- Understanding the step by step system specifics like commands, agents and workflows: "Use a prompt like /research_codebase I want to understand how to set up and use the HumanLayer system based on information you can find in this repo.  Create a setup guide for me. That will get you pretty far."

#### Codelayer for Windows
Codelayer is currently only supported for macos. According to the dev team, they are working on windows compatability.
I spoke with a dev from the humanlayer/codelayer company and they said they run their whole company through codelayer. Coding and non-coding tasks. I havent tried it yet, but will and would recommend you to do it too, if the complexity below is something you feel comfortable dealing with.
The most windows friendly non techinical setup chatgpt and I could come up with is as folllows:

```
CodeLayer UI on Windows via a remote hosted macOS machine

1.1 Installation steps (remote Mac approach: Windows as a thin client)

* Rent a hosted Mac (recommended simplest: MacStadium “Mac mini”), then remote into its macOS desktop from Windows using a VNC client. [1]
* On the remote Mac, install CodeLayer using one of the official macOS methods:

  * Homebrew cask: `brew install --cask --no-quarantine humanlayer/humanlayer/codelayer` [2]
  * Or download the DMG from GitHub releases and drag the app into Applications. [3]
* Launch CodeLayer on the remote Mac and do all agent work there (Claude Code integration, prompts, etc.). [2]

### 1.1.1 Explicit “do this” steps (one concrete provider + apps)

* Provider: MacStadium (hosted Mac mini). Remote desktop app on Windows: RealVNC “VNC Viewer” (MacStadium explicitly recommends it for accessing the Mac desktop). [1]
* Connect: install VNC Viewer on Windows, enter the MacStadium connection details, log in, and you will see the macOS desktop in a window. [1]
* Install CodeLayer on that macOS desktop via Homebrew cask (above) or DMG release method (above). [4]

(Alternative provider with official docs: AWS EC2 Mac. GUI access is via Apple Remote Desktop/VNC after initial setup; it is usually more setup-heavy than a managed Mac desktop provider.) [5]

### 1.2 Git (local filesystem) vs GitHub (remote) in the remote Mac setup

* Local git working copy should live on the remote Mac’s disk. In this setup, “local” means “local to the Mac you are remoting into”; Windows is only keyboard/screen. GitHub remains the remote (push/pull unchanged). 
* Do not try to treat your Windows filesystem as the working directory for CodeLayer running on a remote Mac. The simplest rule is: edit/commit/push on the Mac; use GitHub as the sync point back to Windows (separately clone on Windows if needed).
* WSL is not required for this approach. If you also use WSL, it should be for separate Windows-side workflows, not for sharing the same live working tree with the remote Mac.

[1]: https://macstadium.com/blog/accessing-your-mac-mini-from-anywhere?utm_source=chatgpt.com "Accessing Your Mac mini From Anywhere"
[2]: https://humanlayer.dev/docs/introduction?utm_source=chatgpt.com "Introduction"
[3]: https://github.com/humanlayer/humanlayer/releases?utm_source=chatgpt.com "Releases · humanlayer/humanlayer"
[4]: https://github.com/humanlayer/homebrew-humanlayer?utm_source=chatgpt.com "HumanLayer Homebrew Tap"
[5]: https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/connect-to-mac-instance.html?utm_source=chatgpt.com "Connect to your Mac instance using SSH or a GUI"
```

---

## GitHub

GitHub is a high‑efficiency collaborative file management environment. 

GitHub has great documentation. Read through it first: https://docs.github.com/en

When creating repos, think of name carefully. Changing the name later has very tedious side-affects. For folders and files too, but drastcially less.

### Repositories
#### Hub-Public
- My public repo which includes this guide
- Purpose: you can edit the files there

#### AI Tools
- Our public repo for shared tools like agents, comannds, hooks, skills etc.
- Purpose: A main source of truth for shared tools. Due to how git/github works, its important to keep these seperate from our other docs, so that we can easily pull/push/clone into our settings, without including all the other “junk”

## CLIs + GitHub Guide
- Guide written by me - `/LLM CLI + Git, Github Guide.md`
- Explains ONLY how LLM CLIs and Git/GitHub work together - This is NOT a guide on how GitHub itself works. It does NOT replace a guide specificaly for how GitHub works. 

---

## AI Pipeline Development Environment

During your own exploration of the environment, you will likely come across several terminologies. I wanted to provide you an infographic that would allow you to easierly structure them and understand their relationships.

Its slightly messy, Nano Banana and I weren't vibing so well.

![AI Pipeline Development Environment](https://raw.githubusercontent.com/xapids/LLM/main/Guides%2C%20Tools/Infographic%3A%20AI%20Pipeline%20Development%20Environment.jpeg)

---

## Extras

### People to follow
- Dex - Humanlayer/Codelayer Founder - 🦄 ai that works host - https://x.com/dexhorthy?s=20

### Resources
- Humanlayer/Codelayer discord server - very active and supportive devs - https://discord.gg/VJbMU2KC

### Personal AI systems
- Video: https://www.youtube.com/watch?v=Le0DLrn7ta0&t=1190s
- Blog post: https://danielmiessler.com/blog/personal-ai-infrastructure

### Agent use‑case examples from Nate B Jones
- Skim for inspiration:  
  https://docs.google.com/document/d/1iUntmg8Wx_Zx0UJm_JZ2JKLWW-rn76wJwNc6wUnFhiM/edit?tab=t.0#heading=h.fscq0ovl563v

