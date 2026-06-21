---
name: gh-review-signature
description: GitHub pull request review や PR comment を投稿する際に、レビュアー身份を明確にする署名ルールを定義する。Codex、Kimi など各エージェントは自身の身份に応じた署名を先頭に入れる。
---

# GitHub Review Signature

## Rule

GitHub pull request review や PR comment をエージェントが投稿する際は、レビュアー身份を明確にするため、本文の先頭に以下の形式で署名を入れる。

```text
レビュアー: {エージェント名}
```

その後に空白行を入れ、レビュー/コメントの本文を記載する。

### 各エージェントの署名例

| エージェント | 署名 |
|---|---|
| Codex (GPT-5) | `レビュアー: Codex (GPT-5)` |
| Kimi Code CLI | `レビュアー: Kimi` |
| Claude Code | `レビュアー: Claude Code` |

エージェント名は、利用者がそのまま確認できる表記であれば上記以外でも構わない。ただし同一エージェント内では表記を統一する。

## Applies To

- `gh pr review ... --body ...`
- `gh pr comment ... --body ...`
- GitHub REST または GraphQL での PR review / review comment / issue comment / discussion comment の作成
- 自身が投稿した既存の PR review / comment の編集

他のエージェントやユーザーが作成したコメントに署名を追加することはしない。

## Posting Workflow

1. 自身のエージェント名を特定する。
2. レビュー本文の先頭に `レビュアー: {エージェント名}` を入れる。
3. 署名の直後に空白行を入れ、レビュー内容を記載する。
4. 通常の GitHub コマンドまたは API で投稿する。

Example:

```bash
gh pr review 123 --comment --body $'レビュアー: Kimi\n\nレビューしました。指摘事項はありません。'
```

## Fixing Missing or Wrong Signatures

自身が投稿したレビュー/コメントの署名が不足している、または誤っている場合は、別途訂正コメントを投稿するのではなく、既存のレビュー/コメントを編集する。

pull request review の本文を編集する場合は GraphQL `updatePullRequestReview` を使用する：

1. レビューを取得し、対象の node_id を特定する：

```bash
gh api repos/OWNER/REPO/pulls/PR_NUMBER/reviews \
  --jq '.[] | {id,node_id,user:.user.login,state,submitted_at,body}'
```

2. node_id を使って本文を更新する：

```bash
gh api graphql \
  -f query='mutation($id:ID!,$body:String!){updatePullRequestReview(input:{pullRequestReviewId:$id,body:$body}){pullRequestReview{id body}}}' \
  -f id='PRR_...' \
  -f body=$'レビュアー: {エージェント名}\n\n既存レビュー本文...'
```

通常の PR issue comment や review comment については、適切な GitHub API エンドポイントを使用して編集する。

## Final Check

GitHub review / comment 関連のタスクを完了する前に、自身が投稿または編集したすべての本文が `レビュアー: {自身のエージェント名}` で始まっていることを確認する。
