# GitHub Project Orchestrator

GitHubでタスクと進捗を管理し、実装・レビューを独立したエージェントに委譲して、開発を完了まで進めるCodexスキル。

## 使う

```text
$github-project-orchestrator このリポジトリの対象Issueを整理し、実装とレビューを委譲して完了まで進めて。
```

マージ判断は既定でArchitect。ユーザー判断にする場合は開始時に指定する。

- Issue：目的、設計、受け入れ条件、仕様版
- Projects：進行状態、優先順位、予定、ロードマップ
- Milestones：リリースや段階ごとの到達目標
- Sub-issues / Issue dependencies：親子関係と着手順序
- Discussions：横断的な設計判断と理由
- PR：実装、検証証拠、独立レビュー

小さな作業に全機能を強制しない。権限やツールが使えない場合のIssueへの代替運用も含む。ローカルTODO台帳は作らない。

## 配置と更新

編集元はこのリポジトリの `skills/github-project-orchestrator/`。インストール先の `~/.codex/skills/github-project-orchestrator/` は実行用コピーとし、直接編集しない。

リポジトリルートで以下を実行する（配置先への書き込み許可が必要な環境では承認を受ける）。

```sh
mkdir -p "${CODEX_HOME:-$HOME/.codex}/skills/github-project-orchestrator"
cp -R skills/github-project-orchestrator/. "${CODEX_HOME:-$HOME/.codex}/skills/github-project-orchestrator/"
diff -r skills/github-project-orchestrator "${CODEX_HOME:-$HOME/.codex}/skills/github-project-orchestrator"
```

削除したファイルはコピーでは消えないため、差分に残った旧ファイルは用途を確認して整理する。別環境ではこのprivateリポジトリをcloneしてから同じ配置手順を使う。

## 保守

改善要求と抜け漏れはGitHub Issueで管理する。複数タスクにまたがる方針はDiscussion、具体的な仕様と受け入れ条件はIssueへ残す。スキルの変更はcommitしてGitHubへpushし、検証後にインストール先へ同期する。

スキル更新時はCodexに同梱された `skill-creator/scripts/quick_validate.py` で形式を検証する（PyYAMLが必要）。復旧、仕様変更、権限不足など判断に関わる変更は、実際の外部操作をしない独立エージェントのケース検証も行う。机上検証と実環境での確認を区別して報告する。

スキルは常駐サービスではない。利用可能なサブエージェントAPI・モデル・GitHub権限に従って動作する。
