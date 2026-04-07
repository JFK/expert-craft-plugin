# Claude: Expert Craft Plugin

A [Claude Code](https://docs.anthropic.com/en/docs/claude-code) plugin that lets you create custom domain expert skills through interactive dialogue. Hire experts, summon them for code reviews and questions, and manage your expert team — all from the Claude Code CLI.

## Commands

| Command | What it does |
|---------|-------------|
| `/expert-craft` | Hire a new expert through interactive dialogue |
| `/expert-list` | List all your hired experts |
| `/expert-retire` | Remove an expert you no longer need |

## Installation

```bash
/plugin marketplace add JFK/expert-craft-plugin
/plugin install expert-craft
```

## Usage

### Hire an expert

```
/expert-craft "cryptographer"
/expert-craft
```

The command walks you through an interactive dialogue to define:
1. **Role** — What kind of expert (e.g., cryptographer, accessibility specialist)
2. **Focus areas** — What they review (e.g., key management, protocol verification)
3. **Principles** — Their core judgment criteria (e.g., "Never roll your own crypto")
4. **Scope keywords** — Review mode scopes (e.g., 'full', 'keys', 'protocols')

The expert is saved as a local command and immediately available.

### Summon an expert

Once hired, summon any expert by name:

```
/cryptographer Is this TLS implementation secure?
/cryptographer                                      # Full review
```

Each expert supports **question mode** (ask a specific question) and **review mode** (comprehensive analysis), just like C-Suite and PhD Panel commands.

### Manage your team

```
/expert-list                    # See all hired experts
/expert-retire cryptographer    # Remove an expert
```

## Where experts live

- **Global**: `~/.claude/commands/` — available in all projects
- **Project**: `.claude/commands/` — available only in the current project

## Works with C-Suite & PhD Panel

Experts created with this plugin can cross-reference findings from [Claude C-Suite](https://github.com/JFK/claude-c-suite-plugin) and [Claude PhD Panel](https://github.com/JFK/claude-phd-panel-plugin) when run in the same session.

## Related plugins

- **[claude-c-suite-plugin](https://github.com/JFK/claude-c-suite-plugin)** — Executive perspectives (CEO/CTO/CSO/CFO/CMO/CLO/etc.) for code review. Use it for built-in business roles, then `expert-craft` for project-specific custom experts.
- **[claude-phd-panel-plugin](https://github.com/JFK/claude-phd-panel-plugin)** — Academic expert reviews (CS, DB, Stats, Data Science, Distributed Systems, PL theory). Use it for theory-grounded analysis alongside your custom experts.

## License

MIT
