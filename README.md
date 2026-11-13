# GitHub Actions workflow lockfile experiment

2026-11-13 に、公開リポジトリで **Require lockfile** ポリシーを Active にして検証しました。
各実行はその時点のコミットを参照します。

| ケース | 結果 | 実行 |
| --- | --- | --- |
| ロックファイルなし | 開始前に失敗。「Workflow is not in the repository's lockfile」 | [実行](https://github.com/shinodogg/actions-lockfile-experiment/actions/runs/43355181650) |
| ロックファイル生成後 | 成功 | [実行](https://github.com/shinodogg/actions-lockfile-experiment/actions/runs/43355284451) |
| action を追加し、ロックファイルを更新しない | 開始前に失敗。「The lockfile is missing `actions/setup-node@v4`」 | [実行](https://github.com/shinodogg/actions-lockfile-experiment/actions/runs/43355331758) |
| action のタグを変更し、ロックファイルを更新しない | 開始前に失敗。「The lockfile is missing `actions/checkout@v5`」 | [実行](https://github.com/shinodogg/actions-lockfile-experiment/actions/runs/43355364368) |

最後のケースは、ワークフローの `actions/checkout@v4.4.0` を `@v5` に変更し、
ロックファイルの `@v4.4.0` をそのまま残して検証しました。ローカルの
`gh actions-lock --verify` も `Ref changed` を報告しました。
その後、ロックファイルを再生成し、ワークフローを正常な状態に戻しています。

追加の整合性チェックがなくてもタグ変更は検出されました。PR で早期に検出するには、
公式の [`verify-lockfile.yml` の例](https://github.com/github/gh-actions-lock/blob/main/.github/workflows/verify-lockfile.yml)のように
`gh actions-lock --verify` を実行できます。検証用ワークフロー自体もロックファイルに登録してください。
ロックファイルの変更は内容をレビューしてからマージしてください。

`Require lockfile` を有効にする前の非公開リポジトリでは、ロックファイルなしの
[実行が成功](https://github.com/shinodogg/actions-lockfile-experiment/actions/runs/43354915580)しました。
ポリシーが強制されない場合、ロックファイルがないことだけでは実行はブロックされません。

参考: [GitHub Actions workflow lockfiles are generally available](https://github.blog/changelog/2026-10-28-github-actions-workflow-lockfiles-are-generally-available/)
