### CLI Best Practices

---

#### Legend
- **Repository** (concept): a directory of files.
- **Git**: `.git/`; optional metadata enabling version control.
- **CLI**: Command-Line Interface
  - Claude Code - `.claude/`
  - Codex - `.codex/`

#### Project structure
- Keep source/docs at repo root or subdirs.
- `.git/` = VCS database only; never store project files there.
- `.claude/`, `.codex/` = tool state; treat as ephemeral.

#### Git usage
- Commit human-authored, stable intent.
- Ignore tool-generated, churny state.
- `.gitignore`:  
&nbsp;&nbsp;&nbsp; `.claude/*`\
&nbsp;&nbsp;&nbsp; `.codex/*`\
&nbsp;&nbsp;&nbsp; `!.claude/rules.md`\
&nbsp;&nbsp;&nbsp; `!.codex/rules.md`  

#### Agent rules
- Prefer one canonical rules file: AI_RULES.md.
- Keep it committed for traceability and sharing.
- Use symlinks or pointers if tools require tool-specific paths.

#### Tool scope
- Launch CLI at repo root to avoid multi-project indexing.
- Do not share caches/indexes across machines.

#### Sharing
- Share policy (rules), not implementation (indexes, logs).
- Git is the default sync mechanism; others are optional.
