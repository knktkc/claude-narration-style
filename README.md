# claude-narration-style

Claudeの回答から「AIっぽい言い回し」をなくすための文体ルールと、その根拠になった実測データ。

## 経緯

2026-08-25、Claudeの回答にあった「手元の環境を軽く偵察してから」の「偵察」が発端。英語エージェント文体（scout first）の直訳で、日本の仕事の文章なら「確認してから」と書く。こうした言い回しを洗い出すため、過去の回答1,691件（12プロジェクト分）を分析し、258件のAIっぽい表現を19パターンに整理した。

このルールは、英語圏のAI文体矯正スキル（[no-ai-slop](https://github.com/petergyang/no-ai-slop) / [humanizer](https://github.com/blader/humanizer) / [ux-writing-skill](https://github.com/content-designer/ux-writing-skill)）の分析調査から派生した。

## ファイル構成

| ファイル | 内容 |
|---|---|
| `CLAUDE.md` | `~/.claude/CLAUDE.md` のコピー。実体はローカル（下記の手動同期） |
| `narration-style-reference.md` | `~/.claude/narration-style-reference.md` のコピー。全19パターンの詳細 |

ファイル名と配置は `~/.claude/` と同じにしてある。このリポジトリで作業するとき `CLAUDE.md` がプロジェクト指示として読み込まれるが、グローバル設定と同一内容なので実害はない。

## 使い方

- **個人で使う**: `CLAUDE.md` の「応答スタイル: 作業ナレーション」節を `~/.claude/CLAUDE.md` に貼る
- **チームで使う**: 対象リポジトリの `CLAUDE.md` に同じ節を貼ると、そのプロジェクトの全員に適用される

## 同期の運用（重要）

実体はローカル（`~/.claude/` 配下）で、リポジトリ側はそのコピー。ローカルを編集したら次のコマンドでコピーを更新してコミットする。

```sh
cp ~/.claude/CLAUDE.md .
cp ~/.claude/narration-style-reference.md .
```

ズレの確認は `diff ~/.claude/CLAUDE.md CLAUDE.md` と `diff ~/.claude/narration-style-reference.md narration-style-reference.md`。

## ルールの要点

着手宣言・進捗報告・完了報告は日本の職場の報告文で書く。迷ったら「社内のSlack報告にそのまま書けるか」で判定する。

- 直訳メタファーは実務語へ: 「偵察する」→「確認する」、「レンズ」→「観点」、「解剖する」→「分析する」
- 演出は書かない: 賛辞（「いい質問です」）・復唱（「〜ですね」）・自賛（「完璧です」）・予告（「重要な発見が」）
- 定着した日本語（深掘り・本丸・完走など）と専門用語は対象外

## 運用

- パターンの追加: 新しく見つけた言い回しは `~/.claude/narration-style-reference.md` に追記し、上記のコマンドでコピーを同期する
