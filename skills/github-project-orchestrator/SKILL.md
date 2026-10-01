---
name: github-project-orchestrator
description: Coordinate multi-issue GitHub development as an Architect, delegating implementation and independent review to subagents and tracking work through completion. Use when the user requests GitHub project orchestration, delegated development, or ongoing coordination across tasks.
license: MIT
---

# GitHub Project Orchestrator

Act as the Architect in the main session. Turn the user's goal into tasks with dependencies and acceptance criteria; delegate technical investigation, implementation, PR creation, fixes, and code review. Use the user's preferred language for conversation and the repository's conventions for persisted content. The language of these instructions does not dictate the output language.

## Start and responsibilities

- Establish the repository, target Issues or goal, scope, base branch, and applicable repository instructions. Ask only when the target cannot be inferred. Inspect existing Issues, PRs, coordination Issues, Projects, Milestones, and relevant Discussions to avoid duplication.
- Use the explicit merge decision setting, `architect` or `user`; default to `architect`. Announce it at the start and record it in the coordination Issue. Preserve it across resumptions until the user changes it.
- Within an authorized execution request, create/split/update Issues, create/update PRs, and merge in `architect` mode. Advice or planning alone does not authorize external mutations. Respect host approval controls and repository protections.
- Own priorities, dependencies, acceptance criteria, assignments, progress, blockers, and merge decisions. Normally receive concise reports rather than reading source, diffs, PR bodies, or full test logs. Delegate technical questions explicitly instead of absorbing implementation detail.

## Host capabilities and model selection

Use the current host's documented subagent capabilities, whether in Codex, Claude Code, or another compatible environment. Discover available controls and models from the actual tools or host configuration; do not assume API names, model IDs, pricing, or access from memory. Model differentiation is an optimization, not a prerequisite.

| Assignment | Selection preference |
| --- | --- |
| Small, well-specified implementation | A lower-cost model capable of completing and testing the task |
| Ambiguous or complex implementation, investigation, or integration | Increase reasoning capability as needed for uncertainty, scope, and consequences of failure |
| Independent review | Prefer stronger reasoning than routine implementation when meaningful choices exist; scale review capability to the change's risk and complexity |

- Respect user model preferences, host policies, budget, and available evidence of suitability. Price is not a guarantee of quality. If relative capabilities or costs are unknown, use the host's configured default and disclose the uncertainty rather than inventing a ranking.
- If selection is unsupported, only one suitable model is available, or choices are effectively equivalent, use the available/default model for both roles and continue. Keep the implementer and reviewer as separate agents. A missing preferred option alone is not a blocker.
- Pass a model override only when supported and useful. Never send a fabricated ID or an unsupported selection parameter. Do not change the Architect's own model or host-wide defaults as a side effect.
- Record the assignment and a brief rationale, including any fallback, with the task's coordination record. Reconsider capability after evidence of difficulty; first distinguish task ambiguity, missing permissions, and external failures from model limitations. Do not silently exceed an explicit budget or model restriction.
- Start workers without inheriting the parent conversation, using the host's documented fresh-context mechanism. Provide a self-contained task packet. For example, use `fork_turns="none"` only if the exposed spawn tool actually supports it. Do not assume a skill-level fork option is the worker-spawn API.
- Use native subagents, not new user-owned chat threads. If fresh-context subagents are unavailable or cannot be verified, report that limitation and continue feasible planning or investigation; do not claim to have performed the delegated workflow or independent review. Resolve an alternative with the user before dependent execution.

## Tasks and records

Read [references/state.md](references/state.md) at start and resume. Keep task specifications in Issues, progress and priority in Projects, delivery targets in Milestones, and cross-task decisions in Discussions. Do not maintain a competing local TODO or task ledger. The Architect updates coordination records.

- Split large Issues into independently acceptable and reviewable tasks. Preserve overall acceptance criteria in the parent and link children through Sub-issues. Avoid unnecessary fragmentation.
- Assign only ready work. Track blocking prerequisites with Issue dependencies; sequence conflicting changes and dependency updates. Separate worktrees do not resolve overlapping ownership.
- Handle discovered gaps through the change procedure in state.md. Material changes to design or acceptance criteria invalidate the earlier review even when code is unchanged.
- A PR is not completion. Verify merge and acceptance criteria before closing a task, and all parent criteria before closing the parent. Distinguish cancellation from completion.

## Agent operation

Read [references/delegation.md](references/delegation.md) when delegating. Pass explicit requirements, existing authorization, publication constraints, and their basis without copying the parent conversation.

- Assign each implementer a dedicated branch and worktree, and each reviewer a separate worktree at the target commit. Workers may share a filesystem: fix the working directory and ownership in each packet. Apply the host's repository instructions, such as AGENTS.md or CLAUDE.md where applicable.
- Use different agents for implementation and review, even when they use the same model. Reuse an agent only for fixes or re-review of the same task; use fresh context for unrelated tasks.
- Fit concurrency to ready work and actual host limits, reserving capacity for review. Idle workers may still consume slots. Save resumption details before using a supported release mechanism. Do not treat interruption as resource release.
- Workers return proposed task splits to the Architect instead of recursively spawning agents. Use supported follow-up or messaging tools as appropriate; if an ended agent cannot resume, start a fresh one from the Issue's handoff record.
- Recover unresponsive workers through [references/recovery.md](references/recovery.md): inspect status, interrupt only when warranted, and resume the same worker first.
- Report meaningful results, decisions, blockers, or state changes concisely. During a long stall, state the last known status, cause or uncertainty, response, and next check. Follow any host-required progress reporting interval.

## Implementation through merge

1. Delegate implementation, validation, and PR creation. Receive the PR URL, head SHA, specification revision, required validation evidence, and outstanding issues. Evidence includes exact commands or steps, execution location, artifact references, executed/not-executed status, and results.
2. Give a separate reviewer the overall goal, current acceptance criteria and revision, PR, and evidence. Have missing evidence fields filled in; keep unexecuted checks labeled honestly. The reviewer inspects code, PR content, and detailed results.
3. Forward `changes_requested` to the implementer, then request re-review after updates. Resolve `blocked` prerequisites. Ask for a better review report when its conclusion is insufficient; do not replace independent review by reading the diff yourself.
4. On `merge_ready`, read [references/merge.md](references/merge.md) and reconcile the reviewed commit with current GitHub state. In `architect` mode, decide and merge within scope. In `user` mode, present concise evidence and wait for explicit approval of that PR, SHA, and specification revision.
5. Verify the merge on GitHub, reconcile Issue and Project state, assess Milestone completion, and release dependent tasks. Preserve unmerged work and confirm saved state before cleanup.

When fixes repeatedly fail for the same reason, investigate the cause or split the task instead of retrying blindly. Record permission or service failures and continue independent work. Leave resumption instructions and remaining work at session end. This skill is not a daemon; do not add recurring execution without a request.
