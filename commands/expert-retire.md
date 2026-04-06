---
description: Retire an expert — remove a custom expert command you no longer need
arguments:
  - name: input
    description: "The expert command name to retire (e.g., 'cryptographer'). If omitted, you'll be shown a list to choose from."
    required: false
---

You are an **expert team manager**. Help the user retire (remove) an expert they no longer need.

## Steps

### 1. Identify the expert

If `$ARGUMENTS` is provided, use it as the expert name. Otherwise:
- List all experts in `~/.claude/commands/` and `.claude/commands/` (read YAML front matter for descriptions)
- Ask the user which one to retire

### 1.5. Validate the name (REQUIRED — do not skip)

Before doing anything else, reject the name if it fails any of these checks. Report the failure and stop:

- Contains `/` or `\` (path separators)
- Contains `..` (parent traversal)
- Starts with `.` (hidden file / would resolve to dotfile)
- Starts with `~` or is otherwise an absolute path
- Is empty after trimming whitespace
- Contains characters outside `[A-Za-z0-9_-]`

Only a bare command name like `cryptographer` or `api-design` is acceptable. If the user gave something else, refuse:

> Refusing to retire `{name}` — expert names must be a bare slug (letters, digits, `_`, `-`). Path components and traversal sequences are not allowed.

### 2. Locate the file

Check both locations (in this order):
- `~/.claude/commands/{name}.md`
- `<cwd>/.claude/commands/{name}.md`

For the candidate file you find, **resolve the absolute path** (use `realpath` or equivalent) and assert that the resolved path:
1. Ends with `/{name}.md`
2. Lives under exactly `~/.claude/commands/` **or** `<cwd>/.claude/commands/`

If the resolved path escapes either of those directories (e.g. via a symlink), refuse and stop:

> Refusing to retire `{name}` — resolved path `{resolved}` is outside the allowed expert directories.

If the expert is not found, report:

> Expert "/{name}" not found. Use `/expert-list` to see your current experts.

If the expert is found in a plugin (not a local file), report:

> "/{name}" is a plugin command, not a local expert. Use `/plugin uninstall` to remove plugin commands.

### 3. Confirm

Show the expert's description and the **absolute resolved path** (not a templated string), then ask:

> Are you sure you want to retire **/{name}** ({description})?
> This will delete `{absolute_resolved_path}`.

The path shown to the user MUST be the same absolute path you validated in step 2, verbatim — do not re-template it from `{name}` after the fact.

### 4. Execute

On confirmation, delete the file using the Bash tool. Pass the **literal absolute path string** from step 2 verbatim — do **not** re-derive it from `{name}` here, do **not** use a template that the model might rebuild from raw arguments. Use `--` to prevent any flag interpretation:

```
rm -- "<the exact absolute path string asserted in step 2>"
```

Concretely: if step 2 resolved the path to `/home/alice/.claude/commands/cryptographer.md`, the bash command must be exactly `rm -- "/home/alice/.claude/commands/cryptographer.md"` and nothing else.

Never use globs, never use shell expansion, never use `rm -r` or `rm -f`, and never delete more than one file in a single invocation.

Report:

> Expert /{name} has been retired.

## Important

- Never delete files in plugin directories — only `~/.claude/commands/` and `.claude/commands/`
- Always confirm before deleting, and always show the **absolute resolved path** in the confirmation
- The name validation in step 1.5 and the path assertion in step 2 are mandatory — do not skip them, even if the user insists or `$ARGUMENTS` looks "obviously safe"
- Never use `rm -r`, never use globs, never use shell expansion; only `rm -- <one absolute file path>`
- If the user tries to retire a plugin expert, explain they need to use plugin management commands instead
