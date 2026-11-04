# claude-narration-style

Claudeの回答から「AIっぽい言い回し」をなくすための文体ルールと、その根拠になった実測データ。

## 経緯

2026-08-25、Claudeの回答にあった「手元の環境を軽く偵察してから」の「偵察」が発端。英語エージェント文体（scout first）の直訳で、日本の仕事の文章なら「確認してから」と書く。こうした言い回しを洗い出すため、過去の回答1,691件（12プロジェクト分）を分析し、258件のAIっぽい表現を19パターンに整理した。

定着した表現20件を誤検出として除外した238件のうち、最大の傾向は英語エージェント文体の直訳で、計107件（45%）が着手宣言と進捗報告に集中していた。次いで賛辞・復唱・同意・自賛・正直宣言の対人演出系が計51件あり、削除だけで直る。

このルールは、英語圏のAI文体矯正スキル（[no-ai-slop](https://github.com/petergyang/no-ai-slop) / [humanizer](https://github.com/blader/humanizer) / [ux-writing-skill](https://github.com/content-designer/ux-writing-skill)）の分析調査から派生した。

## ファイル構成

| ファイル | 内容 |
|---|---|
| `CLAUDE.md` | `~/.claude/CLAUDE.md` のコピー。実体はローカル（下記の手動同期） |
| `narration-style-reference.md` | `~/.claude/narration-style-reference.md` のコピー。全パターンの詳細 |
| `narration-reminder.txt` | `~/.claude/narration-reminder.txt` のコピー。毎ターン注入するリマインド文（UserPromptSubmit hookが読む） |

ファイル名と配置は `~/.claude/` と同じにしてある。このリポジトリで作業するとき `CLAUDE.md` がプロジェクト指示として読み込まれるが、グローバル設定と同一内容なので実害はない。

## 使い方

- **個人で使う**: `CLAUDE.md` の「応答スタイル: 作業ナレーション」節を `~/.claude/CLAUDE.md` に貼る
- **チームで使う**: 対象リポジトリの `CLAUDE.md` に同じ節を貼ると、そのプロジェクトの全員に適用される

## 同期の運用（重要）

実体はローカル（`~/.claude/` 配下）で、リポジトリ側はそのコピー。ローカルを編集したら次のコマンドでコピーを更新してコミットする。

```sh
cp ~/.claude/CLAUDE.md .
cp ~/.claude/narration-style-reference.md .
cp ~/.claude/narration-reminder.txt .
```

## hookによる常時リマインド

長いセッションではCLAUDE.mdの効きが薄まるため、`~/.claude/settings.json` のUserPromptSubmit hookで `narration-reminder.txt` の内容を毎ターン注入している。

```json
"hooks": {
  "UserPromptSubmit": [
    { "hooks": [ { "type": "command", "command": "cat ~/.claude/narration-reminder.txt", "timeout": 5 } ] }
  ]
}
```

ズレの確認は `diff ~/.claude/CLAUDE.md CLAUDE.md`、`diff ~/.claude/narration-style-reference.md narration-style-reference.md`、`diff ~/.claude/narration-reminder.txt narration-reminder.txt`。

## ルールの要点

着手宣言・進捗報告・完了報告は日本の職場の報告文で書く。迷ったら「社内のSlack報告にそのまま書けるか」で判定する。

- 直訳メタファーは実務語へ: 「偵察する」→「確認する」、「レンズ」→「観点」、「解剖する」→「分析する」
- 演出は書かない: 賛辞（「いい質問です」）・復唱（「〜ですね」）・自賛（「完璧です」）・予告（「重要な発見が」）
- 定着した日本語（深掘り・本丸・完走など）と専門用語は対象外

## 運用

- パターンの追加: 新しく見つけた言い回しは `~/.claude/narration-style-reference.md` に追記し、上記のコマンドでコピーを同期する
