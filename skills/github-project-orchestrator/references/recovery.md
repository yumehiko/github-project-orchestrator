# Recover an unresponsive worker

## When to inspect

If a long operation's expected duration is known, choose a check time when delegating. Otherwise, about five minutes after the last meaningful progress is an initial inspection guideline, not an automatic interruption deadline. Inspect sooner on an error or stopped notification. Messages may be delayed during tool execution; lack of a reply alone does not establish a stall.

## Inspect, interrupt if warranted, resume the same worker

1. Inspect available agent state, active tools/processes, recent output, and artifact updates. If needed, ask the worker once for its current operation, progress, wait condition, and saved results.
2. Do not interrupt work that is progressing, still within expected duration, or waiting for approval. Choose the next check from the wait condition. If status is unavailable, record the stop state as unconfirmed and do not assign the same write to another worker.
3. If an operation exceeds expectations without progress and appears stalled, identify the operation and saved state, then use the host's supported interruption mechanism. Do not assume this stops application operations or child processes. Force-quitting an application with unsaved documents is not routine recovery.
4. Resume the same worker with the last confirmed state, unfinished operations, Issue/PR, specification revision, evidence location, and existing authorization. First determine whether the original operation is still running, completed, or never executed. Reconcile uncertain pushes, PR creation, and saves before executing only the remaining work. If still running, monitor rather than duplicate it.
5. If the worker cannot be reused, verify that the old worker and external writes have stopped and that changes are saved before handing off to a fresh-context worker. If verification is impossible, mark that task blocked and continue independent work.

After one recovery, if the same cause recurs, switch to diagnosis, another approach, or task splitting instead of repeating interruption and retry. Report limitations when required controls are unavailable. Record cause, action, and result in an Issue comment; update current state in the authoritative Project (or declared Issue fallback), and next action in the resume record. Do not publish process IDs or local paths.
