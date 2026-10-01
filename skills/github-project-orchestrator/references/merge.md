# レビュー報告に基づくマージ

Architectはコード・diff・PR本文を読み直さず、担当の報告とGitHubの機械的状態を照合して判断する。

マージ前に確認する情報:

- 報告のrepository/PRが対象と一致し、判定が `merge_ready` で、受け入れ条件・検証・残リスクが明確である。
- 現在のIssueの仕様版がレビュー報告の版と一致する。設計・受け入れ条件が実質的に変更されていれば、SHAが同じでも再判定を依頼する。単なる進捗欄更新は対象外。
- 現在のPRがopenかつ非draftで、base branchが意図どおりである。
- 現在のhead SHAがレビュー済みSHAと一致する。更新されていれば承認を持ち越さず再レビューする。
- base SHAがレビュー時から変わっていれば、レビュー担当へ影響確認と必要な追加検証を依頼する。Architectが差分を読む必要はない。
- 必須チェック、必要なGitHub承認、競合、マージ可否を確認する。判定がpending/unknownなら待機・再取得し、成功扱いにしない。必須チェックが設定されていない場合はレビュー報告の検証証跡で判断する。

`architect` モードでは、上記を満たした報告に基づきArchitectがマージする。都度ユーザーの許可を求め直さない。`user` モードではPRリンク・対象SHA・仕様版・レビュー結論・検証要約・残リスクを提示し、そのPR・SHA・仕様版に対する明示的な承認を待つ。承認後も最新状態を照合する。対象headまたは仕様版が変わったら再レビューと再承認が必要。

GitHub CLI利用時の例（実環境のhelpで対応オプションを確認する）:

```sh
gh pr view NUMBER --repo OWNER/REPO --json number,url,state,isDraft,baseRefName,baseRefOid,headRefOid,mergeable,mergeStateStatus,reviewDecision,statusCheckRollup
gh pr checks NUMBER --repo OWNER/REPO --required
gh pr merge NUMBER --repo OWNER/REPO --squash --match-head-commit REVIEWED_SHA
gh pr view NUMBER --repo OWNER/REPO --json state,mergedAt,mergeCommit
```

マージ方式はリポジトリの規則と有効な方式に従う。上のsquashは例。head一致条件を付けられる方法を使い、古いレビューのまま更新後commitをマージする競合を避ける。merge queueが必要なら通常のキューを利用し、投入はdoneにせず実際のマージまで確認する。`--admin`等で保護規則を迂回しない。競合解消やbase取り込みは実装担当へ戻し、更新後のレビューを受ける。

マージ操作が失敗・タイムアウトした場合はPR状態を再取得してから次を判断する。成功を確認するまでIssueを完了にしない。失敗原因を解消せず反復しない。親Issueの一部だけを実装したPRで親全体を閉じない。

GitHub CLIの認証確認はサンドボックス内の `gh auth status` だけで判断しない。DNS失敗は認証切れではない。ネットワーク利用を許可した実行で `gh auth status --json hosts` を再確認し、それでも認証エラーの場合だけ `gh auth login` を依頼する。CLIで複数行の本文を渡す場合は一時ファイルと `--body-file` を使う。
