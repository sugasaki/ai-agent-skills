# AI Agent Skills

PC 間で共有する AI エージェント用スキルリポジトリ。Claude Code を中心に、`AGENTS.md` 規約に対応した各種 AI エージェントで再利用できるスキル/ガイドを集約する。

## 含まれるスキル

| スキル名 | 説明 |
|---|---|
| `maintainable-project-guide` | 長期的な保守性を最優先にしたプロジェクトのための `.claude/CLAUDE.md` / `.hermes/AGENTS.md` 生成ガイド |

## セットアップ

各 PC で以下を実行してください。

```bash
# 1. このリポジトリをclone
git clone git@github.com:sugasaki/ai-agent-skills.git ~/.ai-agent-skills

# 2. Claude Code の personal skills ディレクトリにシンボリックリンクを作成
mkdir -p ~/.claude/skills
ln -s ~/.ai-agent-skills/maintainable-project-guide ~/.claude/skills/maintainable-project-guide
```

自分の fork で管理する場合は、clone URL の `sugasaki` を fork owner に置き換えてください。

## 使い方

Claude Code 上で以下のように呼び出します。

```
/maintainable-project-guide
```

または、新規プロジェクトで「このプロジェクトの CLAUDE.md と AGENTS.md を生成して」と依頼してください。

## 更新方法

```bash
cd ~/.ai-agent-skills
git pull
```

既に Claude Code が skills ディレクトリを認識している環境では、更新後の別途再起動は不要です。
初回セットアップで `~/.claude/skills` を新しく作成した場合は、必要に応じて Claude Code を再起動してください。

## 新規スキルを追加する場合

1. `~/.ai-agent-skills/{skill-name}/SKILL.md` を作成
2. 同様に `~/.claude/skills/{skill-name}` へシンボリックリンクを作成
3. commit / push
