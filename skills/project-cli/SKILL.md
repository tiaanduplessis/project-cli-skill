---
name: project-cli
license: MIT
description: "Set up, maintain, or improve a repository-specific CLI for coding agents. Use to turn repeated project lookups and workflows into commands, mine project sessions for candidates, or measure an existing agent CLI's usefulness. Ordinary CLI product development is outside this skill."
---

# Project CLI

Make repeated project work cheap to discover, execute, and verify through a small CLI owned by the project.

## Choose the work

Inspect the project's instructions, existing CLI and scripts, runtime, and source of truth before proposing commands. Extend an established CLI when it already serves this purpose. When a task runner such as a Makefile, justfile, or package scripts already gives agents a discoverable entry point, extending it may be enough; a new binary is not the goal. Use `./bin/agent-cli` only as a default for a new CLI; keep the project's language, naming, and dependency conventions.

Match the user's request:

- **Set up:** identify useful recurring tasks, implement a small first slice, and make the agent discover it.
- **Maintain:** fix a broken or stale command, its output, or its integration. Stay focused on that defect; a full transcript scan is unnecessary.
- **Improve:** investigate repeated work, failed CLI attempts, verbosity, and usage; change commands based on that evidence and compare outcomes.

A short discovery pass is enough for a clear request. Do not require transcripts or a benchmark before fixing a known problem. If history is unavailable, derive explicitly labelled candidates from current project tooling and the user's examples, then continue.

## Find useful commands

For session mining or command selection, read [discovery.md](references/discovery.md). Report the project, date window, sessions covered, and gaps. Keep transcripts local and extract compact, sanitised evidence instead of loading entire histories.

Separate repeated *tasks* from repeated strings. A frequent `cat`, `rg`, or failing command does not automatically deserve a wrapper. Prefer a command that answers the underlying question or runs the correct project workflow. Keep preferences and prohibitions in agent instructions.

Rank candidates by recurrence across sessions, cost, failure rate, and maintenance burden. An `orient` command often helps, but include it only when project discovery is a real need. Choose a coherent first slice rather than a prescribed number of commands.

## Build or change the CLI

Read [contract.md](references/contract.md) for output, errors, help, safety, and validation. Use the existing runtime and standard library or dependencies already present. Avoid adding a framework or generated abstraction without a concrete project need.

Compute answers from maintained project sources at invocation time. Reuse authoritative scripts rather than duplicating their configuration. Keep curated explanations small and tie them to explicit files so drift can be detected. Never present an old summary as current repository state.

Start new CLIs with read-only lookups unless the user requests execution or writes. Test runners, builds, network calls, and service startup have side effects even when they do not edit source. For an existing CLI, preserve its supported operations and safety controls. Authoring a write command does not authorise running it against live systems.

Use a few meaningful checks to verify the changed contract: execution without local state beyond the files this change adds, relevant help paths, parsed JSON, error and exit status, underlying command failure, and source changes reflected in output. Check only the applicable behaviours; do not run a whole project suite for a small wrapper change.

## Make it discoverable

Add or refine a short instruction in the file the project's agents actually read, preserving its ownership and symlink conventions. For example, adapting the path and command names:

> For project lookups and workflows, use `./bin/agent-cli`: `orient` for layout and commands, `tests run <target>` for focused tests, and `--help` for the rest. Fall back to the underlying tools when no command fits, and report missing or stale answers.

Do not duplicate an instruction across files that already include or link to each other. Name only the few commands agents need most so they can skip a help round trip; keep the full catalogue in CLI help, loaded on demand. Avoid forcing a redundant CLI call before unrelated work or a command the agent already knows.

## Measure and keep it useful

For measurement or ongoing improvement, read [measurement.md](references/measurement.md). Compare the same task and correctness as well as cost. Report unavailable metrics as unavailable; never claim savings from an estimate or a differently scoped run.

Use new sessions to spot missing commands, incorrect flag guesses, retries, bypasses, and excessive output. Fix the command or its discoverability before adding louder instructions. Suggest a review cadence only when useful; do not schedule recurring jobs without a request. Check coverage and callers before deprecating an apparently unused command.

## Deliver

Report what changed, the evidence behind command choices, the checks run, and any measured comparison or remaining gaps. Keep raw transcripts, credentials, private payloads, and local evidence paths out of committed files and shared reports. Follow the user's existing commit, push, and publication boundaries; this skill supplies no additional authority.
