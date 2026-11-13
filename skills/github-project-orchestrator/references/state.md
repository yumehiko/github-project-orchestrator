# GitHub state and resumption

Use Issues as execution units and assign authoritative records as follows. Do not create a competing TODO.md, JSON ledger, or other task database inside or outside the repository. Ephemeral session notes are fine; keep resumption information on GitHub.

| Information | Authoritative record |
| --- | --- |
| Goal, design, acceptance criteria, specification revision | Task Issue body |
| Parent/child relationships and execution dependencies | Sub-issues / Issue dependencies |
| Task state, priority, blockers, justified task dates | Task Issue, using existing labels or one concise management section |
| Roadmap stages, completion criteria, aggregate progress | Milestone description and its task Issues |
| Substantial decisions and rationale | Discussion; Issue comments for small task-local decisions, PR for merge approval |
| Implementation, validation, review | PR; Issue comments before a PR exists |
| Decision authority, operating configuration, resume entry point | Coordination Issue |

## Initial setup

Find and reuse existing epics, coordination Issues, and Milestones. Ask only when the intended one is ambiguous. The normal path is Milestone -> Issue -> PR; a small standalone task does not require a Milestone. Use a Project only when explicitly requested or already part of the repository workflow. Respect existing conventions and check destination visibility before creating records so private information is not exposed.

Keep the coordination Issue concise, with settings and links rather than a duplicate task inventory:

```markdown
## Architect coordination
- Scope: Target Issues and overall completion criteria
- Base / Merge method: Branch and repository convention
- Merge decision: architect | user
- Authorization: Target, destination, permitted operations, limits, and instruction basis
- Roadmap / Work / Decisions: Links to Milestones, Issues, and relevant Discussions
- Project (only if used): URL, purpose, and authoritative field mapping
- Model policy: Host capabilities, user constraints, assignment rationale and fallbacks
- Fallback: Unavailable features and substitute authoritative records, if any
- Canonical checkout: Logical location / intended branch / remote (keep machine-local paths private)
- Cleanup: pending | complete | blocked, retained materials and next action when needed
- Updated: ISO timestamp

### Resume
- Next concrete action, unresolved questions, and current decision links.
- In user mode: PR, SHA, specification revision, and approval record.
```

Keep history in comments and only current information in the body. Read the latest version before editing and preserve others' changes. Replace stale status rather than appending contradictory instructions. Record logical worker roles; do not publish session-specific agent IDs, local absolute paths, or secrets. Put task-specific model assignments in the task's record rather than duplicating them across coordination records.

## Issues: executable tasks and current state

Keep acceptance criteria, priority, dependencies, blockers, and current task state in the Issue by default. For fields owned by an established Project workflow, follow the explicit ownership mapping under Optional Projects below. Reuse existing labels or one concise management section; do not track the same field in both. Use `backlog -> ready -> implementing -> reviewing -> merge_ready -> awaiting_user (merge approval in user mode) -> done`, with separate `blocked` and `cancelled` states. A task waiting on a prerequisite decision is blocked and links to that decision. Track only meaningful state, not a second running narrative.

PR creation, review start, and automatic Issue closure do not establish completion. Verify merge and acceptance for implementation tasks. Investigation or decision tasks without code finish when their own deliverable and acceptance criteria are met; do not invent a PR for them. Cancellation is not completion. Record why a closed task was cancelled or superseded rather than counting it as achieved work.

## Milestones: roadmap stages and aggregate progress

Use Milestones for concrete stages such as an MVP, operational rollout, or release. Record the stage's intended outcome, completion criteria, and order or prerequisites in its description. Add a due date only when justified. Keep executable work in assigned Issues and derive progress from those Issues; do not maintain per-task status or priority lists in the Milestone description.

Do not use Milestones as assignees or statuses. Avoid counting a task Issue and its PR as separate accomplishments. Parent and child Issues can also distort counts: use criteria, not percentages alone, to determine completion. Explain deferred, removed, or cancelled work and any change to the stage's target. Close a Milestone only when its overall completion criteria are satisfied. Do not introduce Projects automatically for larger or cross-repository work; link the relevant repository Milestones and Issues unless a Project is requested or already established.

## Optional Projects

Projects are not part of the default workflow. When explicitly requested or already established, use them for a concrete need such as a shared board or cross-repository view. Before updating, record which location owns each field. Default to Issue-owned state and priority with the Project serving as a view. If the established workflow owns those fields in Project fields, preserve that convention and replace duplicate Issue fields with a reference. Never require manual updates to two copies of the same field.

Do not delete or abandon an existing Project merely because it is optional. If moving its authoritative fields to Issues is requested, reconcile the latest values, record the new source of truth, and mark the old fields as retired or reference-only. For a temporary access failure, name the substitute authoritative location (normally the Issue), record when ownership switches and which Project fields are stale, and reconcile before restoring ownership. Do not expand permissions or create another Project to bypass access controls. Ask for authorization only when the requested workflow needs unavailable permissions. Unsupported optional views must not block otherwise feasible work.

## Parent/child relationships and dependencies

Use Sub-issues for containment and Issue dependencies for prerequisites. Resolve dependency cycles through splitting or sequencing before assignment. Keep parent and child acceptance criteria in their own Issue bodies. A closed prerequisite that was cancelled or did not meet its criteria does not unblock dependent work.

Check actual CLI/API capabilities and permissions; do not invent flags or assume success. Record unavailable relationships as Issue links and unavailable Discussions as Issue comments, without creating another task ledger. Distinguish feature permissions from authentication failures. When a feature becomes available, reconcile and replace fallback records with references instead of keeping competing copies.

## Discussions: decisions, not a waiting queue

Choose the destination by the substance of the decision, not by whether a human must answer:

- A small clarification affecting one task stays in that Issue's comments.
- Approval to merge a reviewed change stays on the PR; record approval received in the conversation there when authorized. Apply the PR/SHA/specification requirements in merge.md.
- Use a Discussion for a policy affecting multiple tasks, a substantial design choice requiring comparison, or a concept that is not yet an executable task. Reuse existing discussions and categories.

Record the question, related Issues, concrete options and tradeoffs, the Architect's recommendation, decision owner, and conclusion (pending/decided/superseded). Before asking a human to decide, complete the feasible investigation and make the options and recommendation reviewable. Explain what needs their judgment. Decide within delegated authority; neither creating a Discussion nor labelling it pending automatically means waiting for the user. Votes and accepted answers are not substitutes for required user approval. Posting authorization does not authorize new mentions or notification requests.

If a decision blocks execution, record the blocker and decision link in the affected Issue; do not duplicate its progress in the Discussion. Continue unaffected work. Create an investigation Issue only when there is concrete research or another independently acceptable deliverable, not just to mirror a pending Discussion.

After a decision, record the conclusion and rationale, create or update any executable Issues, link them, and unblock affected work when its prerequisites are satisfied. Apply specification revision and re-review rules when acceptance criteria change. Preserve prior conclusions and link replacements. Do not leave executable tasks only in Discussions.

## Resume and reconcile

From the coordination Issue, read relevant Milestones, unfinished task Issues, linked PRs, and needed decisions. Read a Project only if the recorded workflow uses it. Compare task state and stage criteria with actual completion evidence and reflect outside updates, merges, or cancellations. Do not reread every Discussion or old conversation. After a partial update failure, record what remains unapplied, refetch, and repair only the missing changes. Report unsaved state if writing is unavailable.

Check whether earlier workers remain active. Start new workers from Issue records and evidence with fresh context, but do not duplicate implementation while an old worker or external write may still be running. Verify cessation and handoff first. Preserve the saved merge decision setting; if absent, use the user's instruction or announce the default `architect`. Reconcile any pending post-merge cleanup using [cleanup.md](cleanup.md); a merged Issue alone does not prove the local checkout was synchronized or worktrees removed. End with coordination/Milestone/Issue links as relevant, remaining work, and the next action.

## Design and acceptance changes

Receive a concise description of the gap, impact on the goal, evidence, and proposed fix. Within the user's scope:

- Amend an existing Issue when clarification is necessary to meet its original goal. Split independently acceptable work into linked tasks and record dependencies and completion impact.
- Ask when correctness depends on user intent or the change expands scope. Do not silently promote optional improvements to required acceptance criteria. Continue independent work.
- Never remove unmet criteria to declare completion. Splitting a missing requirement does not satisfy the original goal. For discoveries after merge, reopen the relevant Issue or create linked follow-up work and reassess the parent's completion.

Give each task a simple `Spec revision: N`, starting at 1. Increment only for material design or acceptance changes, not typos or progress updates. Propagate a parent change to affected child revisions.

Record before/after and rationale in an Issue comment. Use a Discussion for cross-task decisions and link its conclusion from the Issue. Update the body to the current specification. Send affected workers the new revision, change summary, and decision link. Invalidate existing `merge_ready` verdicts and reassess the new revision even if code is unchanged. Reconcile dependent Issues' state and criteria.
