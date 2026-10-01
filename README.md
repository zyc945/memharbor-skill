# MemHarbor Skill

Turn a conversation into a reviewed memory draft, save approved changes, and retrieve them later. This folder is a complete standalone Skill; it does not include a server, credentials, or a hosted storage account.

## Install

First install and sign in to your chosen AI client, and confirm normal chat works. The skills CLI installs Skill files, not the client or its login.

From the project where you use your agent, install from the public Skill repository:

```sh
npx skills add zyc945/memharbor-skill --skill memharbor --agent codex --copy
```

This repository contains only the standalone Skill and does not require access to the private MemHarbor service source. For Claude Code or Cursor, replace `codex` with `claude-code` or `cursor`. Add `--global` to use the Skill across projects.

If you received a local copy instead, keep `SKILL.md`, `agents/`, and all `references/` together and install from its actual absolute path:

```sh
npx skills add /absolute/path/memharbor --skill memharbor --agent codex --copy
```

Both routes install the same Skill. Source-repository credentials are not required.

Without npm, copy the complete folder to your project's `.agents/skills/memharbor` (Codex) or `.claude/skills/memharbor` (Claude Code). Do not overwrite or nest into an existing installation; update the old copy deliberately. Open a new session and confirm the client discovers `memharbor`. A plugin containing this Skill does not need a second standalone installation.

## Choose where memory lives

- **Existing MemHarbor MCP:** connect the service through your client's MCP settings. Use a write-capable connection to save; read-only access can still retrieve and draft. Specify the connection if several are enabled.
- **Local Git vault:** use an independent directory with Git initialized and your Git author identity configured. Give the agent access to that directory. No MCP, Node service or cloud credentials are required. With no remote, memory is stored only on this machine.
- **Neither configured:** the Skill can draft, but cannot claim to have checked existing memories or saved anything. Ask the agent to help choose a storage target. Never paste tokens into the conversation or memory files.

## First memory

For a persistent local vault, choose a new directory separate from your software project (Bash/Zsh):

```sh
git init -b main "$HOME/memharbor-vault"
git -C "$HOME/memharbor-vault" var GIT_AUTHOR_IDENT
```

If Git reports no author identity, set your own `user.name` and `user.email` for this vault, using `git -C "$HOME/memharbor-vault" config ...`. Do not copy a fictitious identity or change global settings. Allow your agent to access this directory through its normal permission settings.

For a local vault, replace the path in this prompt:

```text
Use memharbor with /absolute/path/my-memory-vault only. This is a standalone
local Git vault; I authorize reading it and committing memory changes I approve.
Do not add a remote or use other memory services.
Draft this memory: I plan to read one technical book each week. I decided to
start with networking fundamentals. No book is selected and reading has not begun.
Check for duplicates, then show the draft without saving it.
```

Review it, then say: “Save the approved draft and read it back to verify.” Expect a real path and commit/save result. No need to enter UUIDs or Git SHAs yourself.

Update: “I selected Computer Networks but have not started reading. Update that memory, preserve the decision, and add reading chapter one as the next step. Save directly and verify.”

In a new session with the same vault/connection: “Use memharbor to find my reading plan and tell me my next step. Read only.” A persistent vault keeps the memory between sessions. A service launched with `--demo` does **not**; its synthetic data disappears when the process exits.

For MCP use, replace the local-vault instruction with the exact connection name. Normal conversation is not automatically saved. Writes require the approved draft or an explicit request to save the specified content.

Storage and recovery details are bundled in [getting started](references/getting-started.md) and [operations](references/vault-operations.md). The full source includes English/Chinese first-use guides for T1/T2 deployment and client configuration. No public source access is necessary for an already configured MCP service or this standalone T0 workflow.
