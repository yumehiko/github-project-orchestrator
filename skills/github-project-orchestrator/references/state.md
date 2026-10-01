# GitHub state and resumption

Use Issues as execution units and assign authoritative records as follows. Do not create a competing TODO.md, JSON ledger, or other task database inside or outside the repository. Ephemeral session notes are fine; keep resumption information on GitHub.

| Information | Authoritative record |
| --- | --- |
| Goal, design, acceptance criteria, specification revision | Task Issue body |
| Parent/child relationships and execution dependencies | Sub-issues / Issue dependencies |
| Progress, priority, schedule | Fields in the designated Project |
| Delivery targets and membership | Milestone description / Issue milestone |
| Cross-task decisions and rationale | Discussion; Issue comments for small local decisions |
| Implementation, validation, review | PR; Issue comments before a PR exists |
| Decision authority, operating configuration, resume entry point | Coordination Issue |

## Initial setup

Find and reuse existing epics, coordination Issues, Projects, and Milestones. Ask only when the intended one is ambiguous. Create a Project when it helps manage substantial work; do not force every feature onto a small task. Respect existing ownership, fields, views, and naming. Check destination visibility before creation so private repository information is not exposed through a public Project.

Keep the coordination Issue concise, with settings and links rather than a duplicate task inventory:

```markdown
## Architect coordination
- Scope: Target Issues and overall completion criteria
- Base / Merge method: Branch and repository convention
- Merge decision: architect | user
- Authorization: Target, destination, permitted operations, limits, and instruction basis
- Project: URL and state-to-field mapping, or reason for not using one
- Roadmap / Work / Decisions: Links to views, Issues, and Discussions
- Model policy: Host capabilities, user constraints, assignment rationale and fallbacks
- Fallback: Unavailable features and substitute authoritative records, if any
- Updated: ISO timestamp

### Resume
- Next concrete action, unresolved questions, and current decision links.
- In user mode: PR, SHA, specification revision, and approval record.
```

Keep history in comments and only current information in the body. Read the latest version before editing and preserve others' changes. Replace stale status rather than appending contradictory instructions. Record logical worker roles; do not publish session-specific agent IDs, local absolute paths, or secrets. Put task-specific model assignments in the task's record rather than duplicating them throughout the Project.

## Projects and Milestones

Add actual task Issues to the Project; do not count their PRs as separate progress items. Convert draft items to Issues before execution. Designate one authoritative Project and avoid independently duplicating status elsewhere.

Start with Status and Priority; add Horizon (Now / Next / Later) if useful. Use a Table for overview and a Board for execution. Add dates or Iterations only when justified; never invent deadlines to populate a Roadmap. Use parent Issues for the overall Roadmap and execution units for daily work. If view configuration is unsupported, use available views and disclose what is unconfigured.

Use `backlog -> ready -> implementing -> reviewing -> merge_ready -> awaiting_user (user mode) -> done`, with separate `blocked` and `cancelled` states. Map coarse existing fields to the necessary detail and record the mapping. Use Issue status fields as authoritative only when no Project is used or during an explicitly declared outage/permission fallback. Mark an unavailable Project as stale in the coordination Issue. PR creation, review start, and automatic Issue closure are not sufficient evidence of completion: verify merge and acceptance. Cancellation is not completion.

Create Milestones for concrete targets such as an MVP, operational rollout, or a release. Record completion criteria and only justified due dates. Milestones are repository-level targets; use a Project for cross-repository coordination. Do not use Milestones as assignees or statuses. Avoid counting both Issues and PRs as separate accomplishments or relying only on completion percentages. Explain removed/deferred work and any changed target; close only when all criteria are met.

## Parent/child relationships and dependencies

Use Sub-issues for containment and Issue dependencies for prerequisites. Resolve dependency cycles through splitting or sequencing before assignment. Keep parent and child acceptance criteria in their own Issue bodies. A closed prerequisite that was cancelled or did not meet its criteria does not unblock dependent work.

Check actual CLI/API capabilities and permissions; do not invent flags or assume success. Distinguish Project permission failures from repository authentication failures. Do not expand permissions automatically. If a Project is essential, explain the needed permission and operation to the user. Otherwise record the limitation and fallback: Issue links for relationships, Issue fields for status, and comments for decisions. Do not create another Project to bypass permissions. When restoring the original feature, reconcile actual state and replace fallback fields with references so there is only one authoritative record.

## Discussions

Use Discussions for decisions affecting multiple Issues, comparisons, and durable design rationale. A comment is enough for a small single-Issue clarification. Reuse existing discussions and categories. Posting authorization does not authorize new mentions or notification requests to other people.

Record the question, related Issues, options and tradeoffs, Architect recommendation, decision owner, and conclusion (pending/decided/superseded). After a decision, record its reasons, impact, and resulting tasks. Creating a Discussion does not itself require waiting for the user. Decide within delegated authority; ask only where user intent is required. Votes or an accepted answer are not a substitute for user approval.

Mark blocked Issues and link the unresolved decision. Create a decision task when independent investigation is needed, and track the dependency. Do not leave tasks only in Discussions. Link resulting Issues and replacement decisions, preserving prior rationale.

## Resume and reconcile

From the coordination Issue, read unfinished Project items, relevant Issues/PRs, and needed decisions. Compare planned state with actual completion evidence and reflect outside updates, merges, or cancellations. Do not reread every Discussion or old conversation. After a partial update failure, record what remains unapplied, refetch, and repair only the missing changes. Report unsaved state if writing is unavailable.

Check whether earlier workers remain active. Start new workers from Issue records and evidence with fresh context, but do not duplicate implementation while an old worker or external write may still be running. Verify cessation and handoff first. Preserve the saved merge decision setting; if absent, use the user's instruction or announce the default `architect`. End with coordination/Project links, remaining work, and the next action.

## Design and acceptance changes

Receive a concise description of the gap, impact on the goal, evidence, and proposed fix. Within the user's scope:

- Amend an existing Issue when clarification is necessary to meet its original goal. Split independently acceptable work into linked tasks and record dependencies and completion impact.
- Ask when correctness depends on user intent or the change expands scope. Do not silently promote optional improvements to required acceptance criteria. Continue independent work.
- Never remove unmet criteria to declare completion. Splitting a missing requirement does not satisfy the original goal. For discoveries after merge, reopen the relevant Issue or create linked follow-up work and reassess the parent's completion.

Give each task a simple `Spec revision: N`, starting at 1. Increment only for material design or acceptance changes, not typos or progress updates. Propagate a parent change to affected child revisions.

Record before/after and rationale in an Issue comment. Use a Discussion for cross-task decisions and link its conclusion from the Issue. Update the body to the current specification. Send affected workers the new revision, change summary, and decision link. Invalidate existing `merge_ready` verdicts and reassess the new revision even if code is unchanged. Reconcile dependent Issues' state and criteria.
