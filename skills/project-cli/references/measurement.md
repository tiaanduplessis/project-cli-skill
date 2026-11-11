# Measure and improve

## Compare an equivalent task

Define a task with an observable correct answer or outcome: for example, identify the service entry point and the exact focused test command. Record the repository revision, agent/model and relevant settings, task prompt, available instructions, cache state, and session boundary. State material differences instead of treating unlike runs as comparable.

If practical, capture the baseline before introducing the CLI. Use a fresh session for each before and after orientation task so prior context does not supply the answer. Compare equivalent checkout states and include the CLI's help/discovery and output in its cost. If a manual run substitutes for an agent session, label it a command benchmark; it does not measure agent token savings.

Collect only observable metrics:

| Metric | Definition |
| --- | --- |
| Correctness | Required answer or outcome checked against current project sources |
| Tool calls | Consistently counted invocations needed to finish the task |
| Elapsed seconds | Same defined start and finish; distinguish command time from whole-task time |
| Tokens | Provider-reported usage with input, output, and cached tokens distinguished where available |
| Retries | Failed or misdirected attempts before the correct result, with a consistent definition |

Do not derive tokens from character counts and present them as measured usage. Do not call a shorter output an improvement if it omits required facts. Report the number of comparable runs and variability when repeated measurements are available. One run is a preliminary observation.

Use a per-task result table, leaving unknown values explicit:

| Task | Correct before / after | Calls before / after | Seconds before / after | Tokens before / after | Retries before / after | Evidence and limits |
| --- | --- | --- | --- | --- | --- | --- |

Calculate percentage savings only from known, comparable, nonzero baselines. If history is missing, record an after-only measurement and a reproducible baseline procedure. Do not reconstruct a fictional before run. A controlled comparison using an isolated checkout without the new CLI is possible when justified and authorised; disclose that it is reconstructed, not historical.

## Learn from actual use

Review recent project sessions using [discovery.md](discovery.md), with a stated window and coverage:

- **Unknown commands or flags:** determine the task the agent expected, then improve grammar, help, or coverage. Do not add an alias for every typo.
- **Bypasses:** inspect whether the matching command was undiscoverable, incomplete, stale, or less useful than the raw tool. A bypass can be rational.
- **Retries and failures:** reproduce the cause. Fix wrong defaults, argument handling, or diagnostics rather than suppressing errors.
- **Verbose answers:** keep the needed facts and add selectors or artifacts where they reduce cost. Recheck correctness after trimming.
- **Possible dead commands:** check human use, scripts, CI, docs, and history coverage before proposing removal. A month without an observed call is a clue, not proof of obsolescence.

Prefer one measured improvement over a growing catalogue. Keep any enduring benchmark or sanitised decision record in the project's existing documentation conventions. Do not introduce production telemetry, transcript collection hooks, or automated monthly edits without a request.
