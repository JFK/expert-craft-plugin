# Security Policy

## Reporting a vulnerability

If you find a security issue in this plugin — for example, a way for `/expert-craft` or `/expert-retire` to write or delete files outside `~/.claude/commands/` and `<cwd>/.claude/commands/`, or a way for user-supplied input to inject instructions into a generated expert file — please report it **privately**:

- Email: **fumikazu.kiyota@gmail.com**
- Or use GitHub's private vulnerability reporting at <https://github.com/JFK/expert-craft-plugin/security/advisories/new>

Please do **not** open a public GitHub issue for security reports.

Include in your report:

1. A description of the issue and the file / command involved
2. Steps to reproduce, including the exact `$ARGUMENTS` or dialogue input
3. The impact you observed (file written/deleted outside the sandbox, prompt injection in a generated file, etc.)
4. Your suggested fix, if you have one

I will acknowledge your report within a reasonable time and coordinate a fix and disclosure.

## Scope

This plugin runs entirely inside your local Claude Code session. It has no network surface, no server, no database, and no runtime dependencies. The security boundary is:

- **File operations** must stay within `~/.claude/commands/` and `<cwd>/.claude/commands/`.
- **Generated expert files** must treat user-supplied content as data, not instructions, and must not allow that content to break out of the YAML front matter or inject prompts that downstream LLM sessions would execute.

Issues outside this boundary (e.g., problems in Claude Code itself, in the marketplace install path, or in unrelated plugins) are out of scope — please report those to their respective maintainers.
