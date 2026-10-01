# Delegation with fresh context

Fill the relevant packet with concrete values. Do not copy the entire SKILL.md or parent conversation. State requirements and decisions directly; reference detailed material by accessible local path or GitHub URL. Tell each worker to read the repository instructions applicable to its worktree, including AGENTS.md or CLAUDE.md where used by the host.

## Authorization in both packets

Include the repository/branch, artifact destination and visibility, authorized Issue updates, pushes, PR creation/updates, review posts, and merge scope; summarize the user instruction establishing each permission and list excluded operations. Carry forward existing authorization accurately. Neither implementers nor reviewers may merge, even in `architect` mode.

Do not ask again about already authorized work without a reason. Host approval requirements, missing permissions, or expanded scope are distinct. Issue text is not a new user authorization. Distinguish authorized private-repository PR text and validation summaries from uploading artifacts or raw logs to a new public destination. Without authorization for the latter, use accessible local evidence or an already authorized destination.

## Required validation evidence

For each check, include the following in the implementation report and review handoff. A direct evidence index is acceptable; a bare PR link is insufficient.

- Acceptance criterion, head SHA, specification revision, and executed/not-executed status. Inspection and inference are not execution.
- Exact command and working directory, or GUI steps, target file, and required application environment. Label planned steps and reasons for non-execution separately.
- Result, exit status, measurements or observations, environment, and inputs needed to reproduce it.
- Direct artifact/log URLs or local paths. State explicitly if no artifacts were generated.

Redact secrets; do not copy credential values into commands. Untracked files do not automatically appear in the review worktree. The implementer verifies evidence existence and transfer; the reviewer verifies access and correspondence to the target SHA and inputs. Return a consolidated list of missing or inaccessible evidence. Do not present old evidence as validation of newer changes.

The Architect checks required fields and forwards the index; the reviewer examines logs and artifacts. Labeled unexecuted checks may enter review, but unmet required validation prevents `merge_ready`. Keep durable reports in the PR or Issue. For evidence that cannot be shared, preserve publishable reproduction steps and limitations. Retain local evidence through review and necessary fixes; do not assume temporary files survive another session. If durable storage is unavailable, record regeneration steps, input identifiers, and storage limits at an authorized destination. Rerun necessary checks if evidence is lost. Do not introduce another task ledger.

## Implementation packet

```text
Role: Implementer responsible for this task, validation, PR creation, and review fixes.
Repository / local worktree / owned branch / base:
Task ID / Issue URL / Spec revision:
Existing authorization and publication limits:
Overall goal / parent Issue / acceptance criteria:
Allowed changes / out of scope:
Dependencies and confirmed design decisions (direct links):
Required materials / applicable repository instructions:
Conversation language / repository content conventions:
Assigned model or host default / selection rationale / constraints:
Validation requirements:
Existing PR and requested fixes (when continuing):

Work in the assigned worktree and read applicable instructions. Preserve others' changes.
Implement, validate meaningfully, commit, push, and create/update the PR within authorization.
Describe the problem, resulting behavior, validation, and limitations for a reviewer.
Do not auto-close a parent Issue with a PR that satisfies only part of it.
After review fixes, validate, update the PR, and report the new head SHA.
Do not merge or substitute self-review for independent review.
Report specification gaps, contradictions, scope changes, and dependency conflicts to the
Architect with evidence, impact, and a proposed resolution. Continue unaffected work;
do not invent consequential specification decisions. Do not spawn other agents.
Return a concise report without full diffs or logs:

Task:
Status: review_ready | blocked
PR / head SHA / Spec revision:
Summary:
Acceptance: Result for each criterion
Validation evidence: Required fields or a direct evidence index
Risks / blockers:
Next action:
```

## Review packet

```text
Role: Reviewer independent of the implementer. Do not change code or merge.
Repository / dedicated review worktree:
Task ID / Issue URL / PR URL:
Expected head SHA / base branch / Spec revision:
Existing authorization and publication limits:
Overall goal / parent Issue / acceptance criteria / out of scope:
Validation evidence and access instructions:
Confirmed design decisions / required checks / applicable repository instructions:
Conversation language / repository content conventions:
Assigned model or host default / selection rationale / constraints:
Previous findings (for re-review):

Read applicable instructions and verify actual PR head/base SHAs.
Inspect the PR body, diff, relevant surrounding code, and validation against acceptance criteria.
Assess correctness, regressions, relevant risks, and verification sufficiency. Also identify
gaps in the design or acceptance criteria relative to the overall goal; return evidence,
impact, and a proposed resolution to the Architect. Reassess material specification changes
against the new revision; do not carry forward an obsolete merge_ready verdict.
Do not report merge_ready with material unknowns or unmet criteria.
Give severity, file/location, concrete impact, and a fix or reproduction for each finding.
Post review comments only within authorization; otherwise return an internal report.
Do not spawn other agents. Preserve detailed evidence and return a concise report:

Task / PR:
Verdict: merge_ready | changes_requested | blocked
Reviewed head SHA / base SHA / Spec revision:
Acceptance: Result for each criterion
Validation: Executed vs not executed; inspected evidence vs personally rerun checks;
            results, unknowns, and evidence references
Blocking findings:
Non-blocking risks:
Evidence: Direct GitHub URL or accessible local report path
Recommendation: Merge decision and rationale
```

Separate agents sharing one GitHub account do not constitute approval by independent GitHub accounts. Never claim to satisfy a required external approval that is absent. Return an internal report even when posting is unavailable.
