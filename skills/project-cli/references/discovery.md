# Discover repeated project work

## Establish coverage

Identify the repository root, relevant worktrees or renamed paths, and the history the user asked to inspect. If no window is specified, a recent month is a practical starting point; state the actual cutoff and sampling limits. Use only accessible, authorised sources. Do not install a scanner, enable a background hook, or export sessions to a service just to obtain a first pass.

Find session sources from the active tool's configuration or available session API. Common local starting points include `~/.claude/projects/` and `~/.codex/sessions/`, but location and schema vary. Other agents and orchestration tools may expose histories through their own APIs or exports. Inspect a small record sample before choosing an extractor.

Scope each session using its workspace metadata, working directory, or explicit repository association. A shared Git common directory can establish worktree identity. A matching basename or a mention of the project in conversation is insufficient. Sessions can change directories; separate events from other projects and flag ambiguous attribution. Do not mine unrelated client histories. Exclude the current analysis session, whose own searches would inflate the counts. Never replay transcript commands.

Record the period, sources, included sessions, exclusions, unreadable records, truncation, and unknown attribution. Do not call a sample “all sessions.” If there are no usable sessions, say so and inspect current scripts, documentation, and the user's recurring tasks instead.

## Extract once, reason over aggregates

For large histories, use a streaming parser or an existing local scanner. Extract tool calls and their corresponding results, not every occurrence of a command string in prose. Keep original sessions unchanged.

Useful signals:

| Signal | What to extract |
| --- | --- |
| Orientation | Questions answered through several layout, configuration, or documentation reads |
| Repeated lookups | Read paths, search patterns, and the task they answered |
| Repeated execution | Actual command, working directory, arguments that change its meaning |
| Failure and retry | Matched result, status or error, next attempt, eventual resolution if visible |
| Missing CLI capability | Unknown subcommand or flag, help lookup, fallback to lower-level tools |
| Bypass | Task answered by raw reads or commands despite an existing matching CLI command |

Normalise repository prefixes and harmless formatting. Preserve distinctions such as project, environment, test selector, read versus write, and success versus failure. Retain counts of both total invocations and distinct sessions; a loop in one session is different from daily repetition. Deduplicate exported copies and delegated copies when identifiers permit; otherwise disclose possible double counting.

A repeated command is not evidence that it failed. Link tool call IDs to results when available, or mark status unknown. Do not infer elapsed time or tokens from record count.

Sanitise before printing or saving extracts: replace credentials, query values containing secrets, personal data, and sensitive payloads. Prefer task labels and repo-relative source paths over raw shell lines. Sanitisation is not a guarantee; inspect any evidence selected for a commit. Keep detailed local evidence outside tracked content.

## Select the operation

Use a compact candidate report:

| Task | Calls / distinct sessions | Failure evidence | Sanitised evidence | Proposed operation | Decision |
| --- | --- | --- | --- | --- | --- |

Evidence should identify a local session/event or a current project source, enough to audit the count without copying private content. Label unobserved candidates as hypotheses rather than assigning them invented counts.

Choose operations that reduce decisions or round trips. Several reads to discover package commands might become `orient`; repeated test failures caused by using the wrong runner might become `tests run`. A frequently used search that already answers the task cheaply may need no wrapper. Do not hide a real application defect with retries or a command alias.

Before implementing, identify the operation's inputs, authoritative source, output, cost, side effects, and how correctness will be checked. Reuse existing scripts or API clients where practical. A CLI is less useful when it must recreate a long-lived browser or REPL session; keep that work in the existing session tool and expose only useful stateless operations.

For OpenAPI- or HAR-derived commands, select endpoints that solve observed tasks rather than generating the entire service surface. Treat HAR cookies, headers, query strings, and bodies as sensitive. Derive the contract from sanitised data, keep authentication external, and stay within existing service permissions.
