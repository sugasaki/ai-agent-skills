# CLAUDE.md — ai-agent-skills

このリポジトリで作業する Claude Code 向けのガイド。**長期的な保守性を最優先**する。各ルールは「なぜ」を理解した上で従うこと。

> `.hermes/AGENTS.md` は本ファイルから派生した要約版。両者が矛盾する場合は本ファイルを優先し、さらに上位の指示ファイル（存在すれば `refactor-instructions.md` 等）があればそれを最優先する。

## 1. プロジェクト概要

- **名称**: ai-agent-skills
- **目的**: PC 間で共有する AI エージェント用スキルリポジトリ。`AGENTS.md` 規約に対応した各種 AI エージェント（Claude Code / Codex / Kimi / Hermes など）で再利用できるスキル/ガイドを集約する。
- **価値**: スキルを 1 箇所で管理し、各 PC・各エージェントから symlink や参照で再利用できる。

## 2. 長期ビジョン

- スキルは今後増えていく前提。**1 スキル = 1 ディレクトリ**（`{skill-name}/SKILL.md`）の構造を崩さない。
- 特定エージェントに依存しない「エージェント中立」な記述を基本とし、エージェント固有の手順は明示的に切り分ける。
- README はスキルのカタログ兼セットアップ手引きとして維持する。

## 3. リファクタリング方針

- スキルが軽量なうちに、共通ルール・重複記述を早めに整理する。
- スキルの `description`（トリガー文）は起動精度を左右する最重要部分。変更は意図を持って行い、トリガー挙動が変わらないか確認する。
  - このリポジトリには自動テストが無いため、「テスト前提のリファクタ」は **description / トリガーの動作確認** に読み替える。
- 破壊的な構造変更（ディレクトリ構成・命名規約の変更）は単独の PR で行い、レビューを通す。

## 4. 判断基準（新規追加 vs 既存改善）

- 既存スキルで表現できるなら、新スキルを増やさず既存を改善する。
- スキルの責務が明確に異なる場合のみ新ディレクトリを切る。
- README / 両ガイドの整合が崩れる変更は、同じ PR 内で必ず追従する。

## 5. 技術スタック

- Markdown + YAML frontmatter（`SKILL.md`）。Claude Code の Agent Skills 形式に準拠。
- ビルド・パッケージマネージャ・テストランナーは無し（純粋なドキュメント / スキル定義リポジトリ）。
- git / GitHub（`gh` CLI）。

## 6. ファイル構成

```
.
├── README.md                  # スキルのカタログとセットアップ手引き
├── LICENSE
├── .claude/CLAUDE.md          # 本ファイル（Claude Code 向けガイド）
├── .hermes/AGENTS.md          # AGENTS.md 規約対応エージェント向け要約版
└── {skill-name}/SKILL.md      # 各スキル本体（例: maintainable-project-guide）
```

## 7. スキル記述の規約

- 各スキルは `{skill-name}/SKILL.md` の 1 ファイルに収める。
- frontmatter は最低限 `name` と `description` を持つ。
  - `name`: ディレクトリ名と一致させる。
  - `description`: いつ起動すべきかを具体的に書く（トリガー精度に直結）。
- 本文はエージェント中立に書く。特定エージェント固有の手順は「〜の場合」として節を分ける。
- テンプレート系スキルでは、例示を `{EXAMPLE_*}` プレースホルダにし、利用側で差し替える前提にする。

## 8. 単一の情報源（SSOT）

- 各スキルの正は `SKILL.md`。README や本ガイドは `SKILL.md` を参照・要約するに留め、ロジックを二重管理しない。
- `.claude/CLAUDE.md` を正とし、`.hermes/AGENTS.md` はその要約。**両者は常に同期させる**（片方だけ更新しない）。

## 9. 開発ルール

### 9.1 worktree 必須（最重要）

作業を始める前に**必ず専用の git worktree を作成し、その中で作業する**。`main` チェックアウトや既存の作業ツリーで直接編集しない。

- **理由**: 複数タスク / 複数エージェントの並行作業でも互いの作業ツリーを汚さず、ブランチ切り替えによる未コミット変更の事故を防ぐため。

```bash
# 作業開始
git worktree add ../ai-agent-skills-{summary} -b {type}/{summary} origin/main
cd ../ai-agent-skills-{summary}
# 作業 → commit → push → PR
# 完了（マージ）後にクリーンアップ
git worktree remove ../ai-agent-skills-{summary}
```

- このリポジトリには `.env` 等の機密・マシン依存ファイルが無いため、worktree 作成後に手動でコピーが必要なものは無い。

### 9.2 ブランチ / コミット / PR

- **ブランチ**: `feat/` `fix/` `docs/` `chore/` + 内容の要約。Issue があれば PR 本文で `Closes #n` により紐付ける。
- **`main` へ直接 push しない**。必ずブランチ + PR を経由する。
- **force-push しない**。
- **コミット / PR は日本語**。コミットは `type: 要約` 形式（例: `docs: プロジェクトガイドを追加`）。
- PR review / PR comment を投稿する場合は、`gh-review-signature` スキルに従い本文先頭に `レビュアー: Claude Code` を入れ、空行後に本文を書く。
- このリポジトリに lint / build / test は無い。変更前に該当 `SKILL.md` / Markdown を目視確認する（表・コードブロック・リンク崩れ）。

## 10. セキュリティ / 注意

- セットアップは `~/.claude/skills/{skill-name}` への symlink 方式。スクリプトで利用者の `~/.claude` を自動改変しない。
- マシン固有の絶対パスや個人情報を、スキル本文・コミットに含めない。
- 他エージェント / ユーザーが投稿した GitHub コメントを勝手に編集・署名しない（`gh-review-signature` 準拠）。

---

_このガイドと `.hermes/AGENTS.md` は時間経過とともに同期を保つこと。_
