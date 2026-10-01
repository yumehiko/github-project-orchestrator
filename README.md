# GitHub Project Orchestrator

Coordinate GitHub development with separate implementation and review agents, from planning to merge.

## Install

With Node.js and npm installed, run:

```sh
npx skills add yumehiko/github-project-orchestrator
```

Choose your agent and installation scope when prompted. To install for a specific agent across all projects:

```sh
npx skills add yumehiko/github-project-orchestrator -g -a codex
# or
npx skills add yumehiko/github-project-orchestrator -g -a claude-code
```

See the [skills CLI documentation](https://github.com/vercel-labs/skills) for more options.

## Use

Open your target repository and invoke the skill:

```text
$github-project-orchestrator Work on Issues #12 and #13. Delegate implementation and independent review. Use user mode for merge decisions.
```

In Claude Code, use `/github-project-orchestrator` instead of `$github-project-orchestrator`.

**Merge decisions default to `architect`:** the coordinating agent may merge within the authorized scope after review and required checks. Choose `user` mode to approve each merge yourself.

## What it does

- Breaks goals into Issues with acceptance criteria and dependencies, and tracks progress on GitHub.
- Delegates implementation and review to different agents in separate worktrees.
- Prefers lower-cost models for routine implementation and stronger reasoning for review when available. Works with a single model too.
- Preserves decisions and handoffs so work can resume later.

Requires Git, authenticated GitHub access, and a host capable of starting subagents without inheriting the parent conversation. Designed for Codex, Claude Code, and compatible hosts; full live workflows have not yet been verified on every host. Conversations follow your preferred language.

See [SKILL.md](skills/github-project-orchestrator/SKILL.md) for the workflow and detailed rules. Feedback and contributions are welcome through [Issues](https://github.com/yumehiko/github-project-orchestrator/issues).

## License

[MIT](LICENSE).
