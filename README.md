# Claude Skills

PC 間で共有する Claude Code 用スキルリポジトリ。

## 含まれるスキル

| スキル名 | 説明 |
|---|---|
| `maintainable-project-guide` | 長期的な保守性を最優先にしたプロジェクトのための `.claude/CLAUDE.md` / `.hermes/AGENTS.md` 生成ガイド |
| `gh-review-signature` | GitHub PR review / comment 投稿時のレビュアー署名ルール |

## セットアップ

各 PC で以下を実行してください。

```bash
# 1. このリポジトリをclone
git clone git@github.com:YOUR_GITHUB_ACCOUNT/claude-skills.git ~/.claude-skills

# 2. Claude Code の personal skills ディレクトリにシンボリックリンクを作成
mkdir -p ~/.claude/skills
ln -s ~/.claude-skills/maintainable-project-guide ~/.claude/skills/maintainable-project-guide
```

## 使い方

Claude Code 上で以下のように呼び出します。

```
/maintainable-project-guide
```

または、新規プロジェクトで「このプロジェクトの CLAUDE.md と AGENTS.md を生成して」と依頼してください。

## 更新方法

```bash
cd ~/.claude-skills
git pull
```

Claude Code はスキルディレクトリの変更を検知するため、別途再起動は不要です。

## 新規スキルを追加する場合

1. `~/claude-skills/{skill-name}/SKILL.md` を作成
2. 同様に `~/.claude/skills/{skill-name}` へシンボリックリンクを作成
3. commit / push
