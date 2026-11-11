# Project CLI

A skill for setting up, maintaining, and improving a project's CLI for coding agents. It turns repeated project lookups and workflows into small, discoverable commands, then uses real session evidence to improve them.

It works with the project's existing runtime and tooling. It can extend an existing CLI or start one at `./bin/agent-cli`. It covers session mining, command design, agent instruction integration, focused validation, and honest before/after measurement.

## Install

Install to the current project with the [skills CLI](https://github.com/vercel-labs/skills/blob/main/docs/commands.md):

```sh
npx skills add tiaanduplessis/project-cli-skill --skill project-cli
```

Select your coding agent when prompted. For a global Codex install:

```sh
npx skills add tiaanduplessis/project-cli-skill --skill project-cli --agent codex --global
```

The repository is currently private. Installation needs GitHub access through your configured Git, GitHub CLI, or SSH authentication. An explicit SSH source is also available:

```sh
npx skills add git@github.com:tiaanduplessis/project-cli-skill.git --skill project-cli
```

You can inspect discovery without installing:

```sh
npx skills add tiaanduplessis/project-cli-skill --list
```

The same repository layout and install command can be used after a future public release. Compatibility with the installer does not imply a public skills.sh listing.

## Use

Invoke `project-cli` through your agent's skill mechanism. For example, in Codex:

```text
Use $project-cli to set up a small CLI for this repo. Inspect its existing
tooling and recent project sessions, choose useful recurring tasks, and
implement a read-only first slice. Report evidence and validation.
```

```text
Use $project-cli to fix our existing orient command. It reports an old test
runner after the manifest changed. Keep the current command names.
```

```text
Use $project-cli to improve our existing agent CLI from the last month's
project sessions. Check retries, missing commands, bypasses, and output cost.
Measure comparable tasks where evidence is available.
```

The skill works without transcripts; it labels source-derived candidates and missing measurements. It does not mandate a transcript provider, scanner, CLI framework, or new dependencies. A CLI is useful for stateless project work; existing browser or REPL session tools can remain the better choice for stateful work.

## Layout and maintenance

The installable unit is [`skills/project-cli/`](skills/project-cli/SKILL.md). Its references travel with the skill; there are no dependencies on this repository's root or the author's machine. `agents/openai.yaml` supplies optional Codex UI metadata; the instructions themselves are agent-neutral.

[`evals/scenarios.md`](evals/scenarios.md) contains behavioural evaluation cases for future revisions. Run relevant cases in disposable workspaces; review actual artifacts and actions as well as the final explanation. Packaging validation alone does not establish behavioural quality or token savings.

To check installer discovery from a local checkout:

```sh
npx skills add . --list
```

Keep private session data and project-specific generated CLIs out of this skill repository. Before making it public, review the full Git history for private material, choose an open-source licence, and validate installation from the public repository. Visibility changes and publication remain separate decisions.

## Background

Inspired by the project-specific CLI source guide supplied by Tiaan, including Jean-Michel Lemieux's repetition-to-command approach. The skill uses the practical ideas without treating reported savings or tool-count thresholds as universal results.

Related reading:

- [Mario Zechner: What if you don't need MCP at all?](https://mariozechner.at/posts/2025-11-02-what-if-you-dont-need-mcp/)
- [Ugo Enyioha: Writing CLI Tools That AI Agents Actually Want to Use](https://dev.to/uenyioha/writing-cli-tools-that-ai-agents-actually-want-to-use-39no)
- [Armin Ronacher: Your MCP Doesn't Need 30 Tools: It Needs Code](https://lucumr.pocoo.org/2025/8/18/code-mcps/)

These are background references, not required dependencies or instructions to fetch on every invocation.
