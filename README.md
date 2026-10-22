# TypeScript 7 migration skill

A reusable Codex skill and optional migration-agent role for upgrading a TypeScript repository to a stable TypeScript 7 release. It guides a repository-specific migration; it is not an executable upgrade script.

The workflow checks the installed compiler and dependent tools, updates the project with its own package manager, fixes any compatibility errors, and verifies type checks, unit tests, builds, and browser tests. It was used for the Orbit web migration from TypeScript 5.9.3 to 7.0.2, including a clean CI install and Chromium/WebKit runs. Each new repository still needs its own compatibility review and tests.

## Install the skill

Copy [`SKILL.md`](./SKILL.md) into your Codex skills directory:

```sh
mkdir -p ~/.codex/skills/typescript7-migration
cp SKILL.md ~/.codex/skills/typescript7-migration/SKILL.md
```

Then, in a TypeScript project, ask Codex to use `typescript7-migration` to upgrade the repository on a new branch and run its checks. The skill itself does not choose a model or change approvals.

## Optional migration agent

The agent role in [`.codex/agents/typescript7-migrator.toml`](./.codex/agents/typescript7-migrator.toml) gives a delegated Codex agent a focused migration brief. It is a reusable configuration created from the Orbit migration workflow; the original Orbit migration was a one-time delegated task, not a saved agent.

To enable the role in another project, copy the agent file to that project's `.codex/agents/` directory and add the following table to its `.codex/config.toml` (merge it with any existing configuration rather than overwriting the file):

```toml
[agents.typescript7_migrator]
description = "Upgrade a TypeScript project to stable TypeScript 7 and verify its full test pipeline."
config_file = "agents/typescript7-migrator.toml"
```

The complete example is in [`.codex/config.toml`](./.codex/config.toml). Codex resolves `config_file` relative to the declaring config file, as described in the [official configuration reference](https://developers.openai.com/codex/config-reference). The role does not run on its own; ask Codex to delegate a TypeScript migration to it when you want that work.

## Scope

- Reads and preserves repository instructions and unrelated changes.
- Uses the repository's package manager and updates its lockfile.
- Checks TypeScript 7 compatibility for compiler-API consumers and framework tooling.
- Runs the project's existing checks and reports any remaining limits.

No API keys, project credentials, or private Orbit source files are included.
