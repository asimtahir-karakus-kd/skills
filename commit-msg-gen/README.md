# commit-msg-gen

Generate conventional commit messages without scope from staged changes.

## What This Skill Does

- Reads your staged git diff
- Infers the best conventional commit type
- Produces a concise commit message without scope
- Appends the primary file/class name in this format:

```text
type: description in `FileName`
```

## Requirements

- VS Code with GitHub Copilot Chat
- Git repository with staged changes
- MCP Git server from https://github.com/modelcontextprotocol/servers/tree/main/src/git
- MCP git tool support in chat (used internally by the skill)

## MCP Tool Requirement

This skill relies on staged diff data retrieved by the configured MCP Git integration.

Tool mapping note:
- This is provided by the MCP Git server from the modelcontextprotocol/servers repository.

Why required:
- The model needs the actual staged patch to infer intent (`fix`, `feat`, `refactor`, etc.)
- It needs changed file context to include the filename suffix (`in `FileName``)

If no staged changes exist, message generation quality drops or cannot proceed.

## Install / Setup

### 0) Configure MCP Git server

Use the official MCP Git server:

- Source: https://github.com/modelcontextprotocol/servers/tree/main/src/git

Then ensure your chat client exposes the staged diff tool as `#git_diff_staged`.

### 1) Create skill folder

Place files in this exact path:

```text
~/.copilot/skills/commit-msg-gen/
```

On Windows, this resolves to:

```text
C:\Users\<your-user>\.copilot\skills\commit-msg-gen\
```

### 2) Add required files

- `SKILL.md` (required)
- `README.md` (this file, optional but recommended)

### 3) Reload VS Code

Run:
- `Developer: Reload Window`

This ensures the new/updated skill is discovered.

## How To Use

### 1) Stage your changes

```bash
git add <files>
```

### 2) Run the skill with slash command

Primary command:

```text
/commit-msg-gen
```

Optional typed hint:

```text
/commit-msg-gen fix
```

You can still use natural language prompts, but slash command is the recommended flow.

### 3) Use the generated message

```bash
git commit -m "fix: handle empty state in `OrderForm`"
```

## Output Rules

- Conventional commit format
- No scope section
- Keep subject concise
- Include file/class suffix when clear
- Prefer under 100 characters when requested

## Type Selection Guide

- `fix`: bug fix, incorrect behavior
- `feat`: new behavior/feature
- `refactor`: code restructure with no behavior change
- `docs`: documentation only
- `style`: formatting/style only
- `test`: tests added/updated
- `chore`: tooling/build/dependency updates

## Troubleshooting

- No output from staged diff:
  - Ensure files are staged (`git status`)
  - Re-run the slash command `/commit-msg-gen`
- Wrong commit type:
  - Add hint in prompt, e.g. `use type fix`
- Too long message:
  - Ask: `limit to 100 chars`

## Examples

```text
fix: handle null response when loading dashboard data in `DashboardLoader`
feat: add export CSV action for report table in `ReportTable`
refactor: simplify input validation flow in `FormValidator`
```
