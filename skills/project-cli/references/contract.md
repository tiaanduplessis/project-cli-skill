# CLI contract and verification

Apply the relevant parts to the project's actual commands. Preserve a stable existing contract unless the task requires a change.

## Invocation and help

- Resolve project paths from the CLI's location or an explicit project argument, rather than assuming the caller's current directory. Define behaviour outside a checkout and in worktrees.
- Use consistent grammar. For a growing CLI, `noun verb` such as `tests run` makes a browsable tree; a small `orient` command need not gain a redundant level.
- Top-level help lists commands and a short purpose. Command help shows required and optional inputs, defaults, side effects, one realistic example, machine output, and relevant exit codes.
- `--help` succeeds without credentials, starting services, or executing the command's main action. Validate flags and required values before side effects. Unknown commands and flags fail with a useful next step.
- Run without prompts. A missing required input fails with an explanation. A confirmation flag may acknowledge an action; it never replaces credentials, permissions, or the user's authority.

## Output

Read-only queries should offer terse text and `--json`. For execution wrappers, define whether JSON is a result summary, a stream, or a path to captured logs; do not silently mix child logs with a JSON document.

- JSON mode emits a documented, parseable shape on stdout, including an empty result. Keep field names and types stable; use numeric durations with units and ISO 8601 timestamps.
- Put diagnostics, progress, and warnings on stderr. Disable colour and terminal formatting in machine output.
- Define failures in JSON mode too. One reasonable shape is `{"error":{"code":"NOT_FOUND","message":"Unknown test target: api","next":"Run agent-cli tests list --json"}}` with a nonzero exit status. A stdout JSON error must remain a single valid document; stderr must not expose secrets.
- Prefer flat results where they help, but retain meaningful relationships instead of forcing everything flat. Avoid mandatory metadata that overwhelms tiny answers.
- Bound potentially large output. Offer filters, pagination, or a summary plus an artifact path. Machine output must explicitly report truncation and how to obtain the remainder; do not truncate JSON text into invalid JSON or silently omit results.
- Artifacts must use a writable, project-appropriate local location and avoid collisions. Never overwrite an arbitrary user file or commit captured logs by default. Return a usable path and useful follow-up command. Stream large outputs rather than accumulating them in memory.
- Add quiet bare-value output, stdin, batching, or JSON Lines when the workflow benefits from them; do not implement flags with no consumer.

## Failures and execution

Choose and document exit codes. For example: `0` success, `2` usage, `3` missing resource, `4` denied, `5` conflict, and `1` other failure. Existing project conventions take precedence.

Distinguish “no matches” from failure to inspect the source. A permission or network error must not become an empty success result. Report the failed operation and a safe corrective command; sanitise echoed inputs. Do not swallow child stderr. Execution wrappers must propagate failure, by preserving child status or applying a documented mapping, and handle signals without leaving owned child processes running.

Pass child arguments as an argument vector instead of interpolated shell code. Define any `--` forwarding boundary. Bound network waits and retries; do not blindly retry non-idempotent operations. Keep secrets in the existing environment or credential store, and out of help examples, source, JSON, and logs.

## Freshness

Read current manifests, project configuration, directory structure, or maintained scripts rather than embedding their current values in code. Keep curated facts tied to sources and a cheap drift check. Where caching is justified, expose its age and invalidation or refresh behaviour. Missing or stale sources should produce an explicit warning or failure, never fabricated certainty.

## Writes and expensive commands

For requested write operations, provide a structured plan or diff by default and an explicit apply or confirmation flag. The preview path must not perform the write. Identify the target and environment, validate preconditions again at apply time, and make repeated execution idempotent where possible. A changed target or plan must not inherit an earlier confirmation silently.

Keep preview and apply semantics consistent with existing project tooling. Do not weaken its guards. A test or build wrapper should clearly declare its resource and artifact effects; inspect host capacity before running expensive validation and choose a scope that fits current workloads.

## Focused checks

Exercise observable behaviour using a disposable project fixture where practical:

- Launch from the supported working directories, with no reliance on the author's home paths or untracked files; check executable mode and required runtime availability.
- Verify top-level and changed subcommand help, normal text output, parseable JSON including empty results, invalid inputs, and errors with the documented status.
- Change an authoritative input and confirm the answer updates, or that a stale cache is disclosed.
- For child wrappers, verify argument boundaries, child failure and stderr, and any documented log capture or signal handling.
- For writes, confirm preview leaves the target unchanged, then apply only to an authorised disposable target and check idempotency or detectable conflict.

Use existing project checks and test conventions. Do not add a broad snapshot suite that merely freezes help wording. Inspect generated files and diffs for secrets before delivery.
