# Merge from independent review evidence

The Architect reconciles the worker's report with current GitHub state rather than rereading source, diffs, or PR bodies.

Before merging, verify:

- The report identifies the intended repository and PR, returns `merge_ready`, and clearly addresses acceptance, validation, and remaining risks.
- The current Issue specification revision matches the report. Material design or acceptance changes require reassessment even with an unchanged SHA; routine progress updates do not.
- The PR is open, is not a draft, and targets the intended base branch.
- The current head SHA is the reviewed SHA. A changed head requires re-review.
- If the base SHA changed after review, the reviewer has assessed the impact and performed any necessary additional validation.
- Required checks, required GitHub approvals, conflicts, and mergeability satisfy repository rules. Pending or unknown is not success. If no checks are configured, assess the review's validation evidence.

In `architect` mode, merge based on a report meeting these requirements within existing authorization, without asking again each time. In `user` mode, present the PR link, SHA, specification revision, verdict, validation summary, and remaining risks. Wait for explicit approval of that PR/SHA/revision, then recheck current state. A changed head or specification requires re-review and renewed approval.

Example GitHub CLI operations; verify supported options with the installed CLI's help:

```sh
gh pr view NUMBER --repo OWNER/REPO --json number,url,state,isDraft,baseRefName,baseRefOid,headRefOid,mergeable,mergeStateStatus,reviewDecision,statusCheckRollup
gh pr checks NUMBER --repo OWNER/REPO --required
gh pr merge NUMBER --repo OWNER/REPO --squash --match-head-commit REVIEWED_SHA
gh pr view NUMBER --repo OWNER/REPO --json state,mergedAt,mergeCommit
```

Use the repository's permitted merge method; squash above is only an example. Use an operation conditioned on the reviewed head SHA to avoid merging newer code under a stale review. Follow the normal merge queue if required, and verify actual merge rather than treating queue admission as done. Do not bypass protections with `--admin` or equivalent. Return conflict resolution and base integration to the implementer, then obtain an updated review.

After failure or timeout, refetch PR state before deciding what to do next. Do not mark Issues complete until merge is confirmed or retry without addressing the cause. A partial implementation must not close the entire parent Issue.

Do not infer GitHub authentication failure solely from sandboxed `gh auth status`. DNS failures are not expired credentials. Recheck `gh auth status --json hosts` with network access permitted by the host; request `gh auth login` only if that check establishes an authentication error. If network access cannot be obtained, report connectivity as unresolved. For multiline CLI bodies, use a temporary file and `--body-file`.
