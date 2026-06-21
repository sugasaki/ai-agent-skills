# AI Agent Skills

PC 間で共有する AI エージェント用スキルリポジトリ。`AGENTS.md` 規約に対応した各種 AI エージェントで再利用できるスキル/ガイドを集約する。スキルの内容はエージェント非依存で記述しており、Claude Code には slash command 対応のスキルとして直接インストールできる。

## 含まれるスキル

| スキル名 | 説明 |
|---|---|
| `maintainable-project-guide` | 長期的な保守性を最優先にしたプロジェクトのための `.claude/CLAUDE.md` / `.hermes/AGENTS.md` 生成ガイド |

## セットアップ

まず各 PC でこのリポジトリを clone する。

```bash
git clone https://github.com/sugasaki/ai-agent-skills.git ~/.ai-agent-skills
```

自分の fork で管理する場合は、clone URL の `sugasaki` を fork owner に置き換える。

その後、利用するエージェントに合わせて以下のいずれかを設定する。

### Claude Code の場合

personal skills ディレクトリにシンボリックリンクを作成すると、slash command として呼び出せる。

```bash
mkdir -p ~/.claude/skills
ln -s ~/.ai-agent-skills/maintainable-project-guide ~/.claude/skills/maintainable-project-guide
```

### その他の AGENTS.md 規約対応エージェントの場合

スキル本体は各スキルディレクトリの `SKILL.md` にエージェント非依存で記述されている。skills の自動読み込みに対応していないエージェントでは、`SKILL.md` の内容を指示として参照・引用するか、エージェント固有の指示ファイル（`AGENTS.md` など）に取り込んで利用する。

## 使い方

### Claude Code

```
/maintainable-project-guide
```

または「このプロジェクトの CLAUDE.md と AGENTS.md を生成して」のように依頼する。

### その他のエージェント

対象スキルの `SKILL.md` を読み込ませたうえで、同様の依頼（例: 「保守性重視のエージェント向けガイドを生成して」）を行う。

## 更新方法

```bash
cd ~/.ai-agent-skills
git pull
```

`git pull` で最新のスキルを取得できる。Claude Code の場合、既に skills ディレクトリを認識している環境では更新後の再起動は不要。初回セットアップで `~/.claude/skills` を新しく作成した場合は、必要に応じて Claude Code を再起動する。

## 新規スキルを追加する場合

1. `~/.ai-agent-skills/{skill-name}/SKILL.md` を作成する（エージェント非依存の記述を心がける）
2. Claude Code で使う場合は `~/.claude/skills/{skill-name}` へシンボリックリンクを作成する
3. commit / push する
