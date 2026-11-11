# Project CLI

A skill for setting up, maintaining, and improving a project's CLI for coding agents. It turns repeated project lookups and workflows into small, discoverable commands, then uses real session evidence to improve them.

It works with the project's existing runtime and tooling. It can extend an existing CLI or start one at `./bin/agent-cli`. It covers session mining, command design, agent instruction integration, focused validation, and honest before/after measurement.

## Install

Requires [Node.js 22.20 or later](https://github.com/vercel-labs/skills#install-a-skill) with npm/npx. Installation has been checked with `skills@1.8.1` for Codex and Claude Code.

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

This should list one skill named `project-cli`.

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

Keep private session data and project-specific generated CLIs out of this skill repository.

## Contribute

Edit the skill in `skills/project-cli/` and check discovery with the command above. For behaviour changes, use the relevant evaluation scenarios. Keep the root `LICENSE` and the bundled `skills/project-cli/LICENSE.txt` identical.

Report bugs or suggest improvements in [Issues](https://github.com/tiaanduplessis/project-cli-skill/issues). Include the agent, installer version, and a small sanitised reproduction; leave out raw transcripts, credentials, and client data.

## Background

Inspired by Jean-Michel Lemieux's repetition-to-command approach and the writing below. The skill uses the practical ideas without treating reported savings or tool-count thresholds as universal results.

Related reading:

- [Mario Zechner: What if you don't need MCP at all?](https://mariozechner.at/posts/2025-11-02-what-if-you-dont-need-mcp/)
- [Ugo Enyioha: Writing CLI Tools That AI Agents Actually Want to Use](https://dev.to/uenyioha/writing-cli-tools-that-ai-agents-actually-want-to-use-39no)
- [Armin Ronacher: Your MCP Doesn't Need 30 Tools: It Needs Code](https://lucumr.pocoo.org/2025/8/18/code-mcps/)

These are background references, not required dependencies or instructions to fetch on every invocation.

## Licence

[MIT](LICENSE). The licence is also included in the installed skill bundle.
