---
description: List all your hired experts — global, project-local, and plugin experts
arguments: []
---

You are an **expert team manager**. List all available expert commands for the user.

## Steps

### 1. Scan global experts

List all `.md` files in `~/.claude/commands/`. For each file:
- Read the YAML front matter to extract the `description`
- The filename (without `.md`) is the command name

### 2. Scan project experts

List all `.md` files in `.claude/commands/` (relative to the current working directory). For each file:
- Read the YAML front matter to extract the `description`
- The filename (without `.md`) is the command name

If the directory doesn't exist or is empty, note "(none)".

### 3. Note plugin experts

Mention that plugin-based experts (C-Suite, PhD Panel, etc.) are available if installed, and can be listed with `/plugin list`.

### 4. Present

Structure output as:

```
## Your Expert Team

### Global Experts (~/.claude/commands/)
| Command | Expertise |
|---------|-----------|
| /command-name | description from front matter |

### Project Experts (.claude/commands/)
| Command | Expertise |
|---------|-----------|
| /command-name | description from front matter |

### Plugin Experts
Plugin-based expert teams are available if installed:
- C-Suite: `/claude-c-suite:cto`, `/claude-c-suite:pm`, ...
- PhD Panel: `/claude-phd-panel:cs`, `/claude-phd-panel:db`, ...

Use `/plugin list` to see installed plugins.
```

If no experts are found anywhere, suggest:

> No experts hired yet. Use `/expert-craft` to hire your first expert.
