# AGENTS.md — ai-agent-skills

`.claude/CLAUDE.md` から派生した要約版。矛盾する場合は `.claude/CLAUDE.md` を優先し、上位指示ファイル（あれば `refactor-instructions.md` 等）を最優先する。

## プロジェクトの本質

PC 間で共有する AI エージェント用スキルリポジトリ。`AGENTS.md` 規約対応の各種エージェントで再利用できるスキルを `{skill-name}/SKILL.md` 単位で集約する。保守対象はコードではなく Markdown のスキル定義。

## 保守性ルール

| ルール | 詳細 | 影響 |
|---|---|---|
| **worktree で作業** | 作業前に専用 worktree を作成し、その中だけで編集する | 並行作業の事故防止・作業ツリーの汚染防止 |
| ブランチ + PR | `main` へ直接 push しない | 履歴の安全性・レビュー担保 |
| force-push 禁止 | 既存履歴を上書きしない | 共有履歴の保護 |
| 1 スキル 1 ディレクトリ | `{skill-name}/SKILL.md` 構造を維持 | 一貫した発見性 |
| SSOT | スキルの正は `SKILL.md`。README / ガイドは参照のみ | 二重管理の排除 |
| ガイド同期 | `CLAUDE.md` と `AGENTS.md` は同時に更新 | 指示の不整合防止 |
| 日本語 | コミット / PR は日本語 `type: 要約` | 規約統一 |

## 実装フェーズ（スキルのライフサイクル）

1. **worktree 作成** → ブランチ作成
2. `SKILL.md` 追加 / 編集（`name` = ディレクトリ名、`description` = トリガー文）
3. README / 両ガイドの整合を取る
4. commit → push → PR（必要なら `Closes #n`）
5. マージ後に worktree を削除

## 禁止事項

- **worktree を作らず `main` チェックアウトで直接作業すること**
- `main` への直接 push
- force-push
- 確認なしの構造変更（ディレクトリ / 命名規約の破壊的変更）
- 他者が投稿した GitHub コメントの編集・署名

## 技術ガイド

- Markdown + YAML frontmatter。ビルド / テスト / パッケージ管理なし。
- 各スキルは `{skill-name}/SKILL.md`。`name` と `description` は必須。
- 記述はエージェント中立。エージェント固有手順は節で分離する。

## ファイル構成

```
README.md
LICENSE
.claude/CLAUDE.md
.hermes/AGENTS.md
{skill-name}/SKILL.md
```

## 検証戦略（テスト戦略の読み替え）

- 自動テストは無い。`SKILL.md` 変更時は `description` のトリガー意図が崩れていないか目視確認する。
- Markdown の表・コードブロック・リンクが壊れていないか確認する。

## 変更に注意すべき挙動

- スキルの `name` / `description`: 起動トリガーに直結。意図せず変更しない。
- セットアップ手順（symlink パス）: 利用者環境に影響するため、変更時は README も更新する。

## PR テンプレート

```markdown
## 概要
（何を・なぜ）

## 変更点
-

## 確認
- [ ] SKILL.md の frontmatter（name / description）を確認
- [ ] README / CLAUDE.md / AGENTS.md の整合を確認
- [ ] worktree で作業した
```

## レビュー対応ルール

- PR review / PR comment を投稿する際は `gh-review-signature` に従い、本文先頭に `レビュアー: {自身のエージェント名}`（例: `レビュアー: Claude Code`）を入れ、空行後に本文を書く。
- レビュー指摘は同一 PR のブランチ（worktree）で対応し、対応内容を返信する。
- 自分のレビュー署名が誤っている場合は、新規コメントせず既存を編集する。
